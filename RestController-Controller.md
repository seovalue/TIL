## RestController와 Controller의 차이
스프링에서 컨트롤러를 지정해주기 위한 어노테이션에는 다음의 두 가지가 있다.

### @Controller
* what
  주로 View를 반환하기 위해 사용하는 Controller에 다는 어노테이션이다.
  ![](img.png)
* when
  사용자에게 온 요청으로부터 View를 응답할 때 사용한다. 물론 JSON 객체 데이터를 반환할 수 있지만, 이와 같은 경우에는 @ResponseBody 어노테이션을 이용해야한다.

* how
```
@Controller
public class UserController {
    @GetMapping("/user")
    public String userView(){
        return "userView";
    }
}
```


### @RestController
* what
  컨트롤러에서 뷰가 아닌 Data를 반환해야 하는 경우 @ResponseBody 어노테이션이 포함된 @RestController를 사용해 JSON 형태의 객체 데이터를 반환할 수 있도록 도와준다.

* when
  응답으로 객체를 반환할 때 사용한다.

* how
```
@RestController
@RequestMapping("/lines")
public class LineController {
    @PutMapping("/{id}")
    public ResponseEntity<Void> updateLine(@PathVariable Long id, @RequestBody LineRequest lineUpdateRequest) {
        lineService.updateLine(id, lineUpdateRequest);
        return ResponseEntity.ok().build();
    }
}
```

### 참고 자료
* [Spring @Controller와 @RestController 차이 - MangKyu’s Diary](https://mangkyu.tistory.com/49)
