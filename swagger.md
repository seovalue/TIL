## Swagger2
* what  
간단한 설정으로 프로젝트에서 지정한 url을 화면으로 확인할 수 있게 해 준다.  
  swagger ui가 적용된 페이지는 [여기](https://anne-subway.r-e.kr/swagger-ui.html#/)에서 확인할 수 있다.  
  swagger를 프로젝트에 적용할 경우 설정된 API URL 리스트들의 목록을 바로 확인할 수 있다.  
  또한 Postman과 같이 TEST할 수 있는 ui를 제공한다.
  

  
* how  
1. build.gradle에 아래 의존성을 추가한다.
```text
compile group: 'io.springfox', name: 'springfox-swagger2', version: '2.5.0'
compile group: 'io.springfox', name: 'springfox-swagger-ui', version: '2.5.0'
```
2. Swagger 설정 Bean을 프로젝트에 등록한다.
```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.basePackage("wooteco.subway"))
                .paths(PathSelectors.ant("/**"))
                .build();
    }
}
```

### 참고 자료
- https://jojoldu.tistory.com/
- https://woowacourse.github.io/javable/post/2020-08-31-spring-swagger/
- https://stoplight.io/blog/difference-between-open-v2-v3-v31/