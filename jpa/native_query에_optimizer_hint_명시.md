```sql
SELECT 
  * 
FROM followers f
WHERE f.business_account_id=1286318 
  AND f.id>20 
ORDER BY f.id 
LIMIT 20;
```

실제로 `ix_businessaccountid_id` 형태의 index가 있음에도 이런 형태의 쿼리가 오래 걸렸음 (특히 `f.id > ?` 이 포함된 경우)  
DB 직접 물고 실행해도 대략 2.4s 정도 걸림; 단골 수가 17만정도. (170K)

TOBE
> 요거 예전에도 비슷한 케이스가 몇번 있었는데, MySQL server optimizer가 좀 잘못된 판단을 하는 버그가 있는 것으로 보입니다. Optimizer의 변화는 나름 큰 변화라, Oracle에서도 쉽게 변경하지 못하고 있는 듯 한 느낌이기도 해서, 조만간 버그가 개선될 것 같진 않아 보입니다.

라는 이유로 business_account_id, id 로 된 인덱스를 잘 타지 못하는 것 같아서 (종종?)
직접 네이티브 쿼리로 인덱스를 쿼리에 명시해주도록 수정했어요

```kotlin
 // 단골 수가 많은 경우 인덱스를 제대로 타지 못해, 직접 인덱스를 명시해줘요.
@Query(
        value = """
        SELECT /*+ INDEX(f ixx_businessaccountid_id) */
          *
        FROM followers f
        WHERE f.business_account_id = :businessAccountId
          AND f.id > :afterId
        ORDER BY f.id
        LIMIT :size
""", nativeQuery = true
)
fun findByBusinessAccountIdAndIdGreaterThanWithHint(
        @Param("businessAccountId") businessAccountId: Long,
        @Param("afterId") afterId: Long,
        @Param("size") size: Int,
    ): List<FollowEntity>

// 원래는 JPARepository에서 지원해주는 거 썼었어요
// 발생되는 쿼리는 같은데 힌트만 추가!
interface FollowRepository : JpaRepository<FollowEntity, Long> {
fun findByBusinessAccountIdAndIdGreaterThan(
        businessAccountId: Long,
        afterId: Long,
        pageable: Pageable
    ): List<FollowEntity>
}
```

기존에 3~4s -> 0.05s로 줄었음;;

