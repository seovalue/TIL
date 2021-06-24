## 2장

```java
@RunWith(SpringRunner.class)
@WebMvcTest(controllers = HelloController.class)
class HelloControllerTest {

    @Autowired
    private MockMvc mvc;

    @Test
    public void hello가_리턴된다() throws Exception {
        String hello = "hello";

        mvc.perform(get("/hello"))
                .andExpect(status().isOk())
                .andExpect(content().string(hello));
    }
}
```

1. RunWith(SpringRunner.class)  
    * 테스트를 진행할 때 JUnit에 내장된 실행자 외에 다른 실행자를 실행시킵니다.
    * SpringRunner라는 스프링 실행자를 사용합니다.
   * 즉, 스프링 부트 테스트와 JUnit 사이에 연결자 역할을 합니다.
2. @WebMvcTest
   * 스프링 테스트 어노테이션 중 Web(Spring MVC)에 집중할 수 있는 어노테이션
   * 선언할 경우 @Controller, @ControllerAdvice 등을 사용할 수 있습니다.
   * 단, @Service, @Component, @Repository 등은 사용할 수 없습니다.
3. mvc.perform(get("/hello"))
   * MockMvc를 통해 /hello 주소로 HTTP GET 요청을 합니다.
4. mvc.andExpect(status().isOk())
   * mvc.perform의 결과를 검증한다.
5. mvc.andExpect(content().string(hello))
   * mvc.perform의 결과를 검증한다.
   * 응답 본문의 내용을 검증한다.
   

    