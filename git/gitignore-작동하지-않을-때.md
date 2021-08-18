## .gitignore에 등록했는데 add file 목록에 포함되어 있을 때 대처법!

```text
git rm -r --cached .

// 캐시를 지우고 나면 스테이징 된 것들도 모두 취소되므로, 다시 모두 add 해 주어야 한다.
git add --all
git commit -m "chore: fixed untracked files"
```

### 참고 자료
- https://jojoldu.tistory.com/307