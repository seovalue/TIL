## ResponseEntity
* what
  클라이언트의 HttpRequest에 대한 응답 데이터를 포함하는 클래스이다. HttpStatus, HttpHeaders, HttpBody를 포함하며 HttpEntity를 상속하여 구현되어있다.
* when
  상태코드, 헤더, 바디를 포함한 응답을 전달하기 위해 사용된다. ResponseEntity를 이용해 반환하게 되면 클라이언트에게는 JSON 또는 xml 형식으로 직렬화되어 반환된다.
* why
  요청에 대한 응답으로 단순히 Object(String과 같은) 를 반환할 수 있다. 예를 들어 String을 반환하게 되면 Spring은 해당 String과 일치하는 view를 찾아 ViewResolver에서 해당 View를 렌더링하도록 한다. 하지만 View가 아닌 HTTP 정보만을 반환해야할 때 HttpStatus, Body, Header로 이루어진 완전한 HTTP 응답인 ResponseEntity를 사용할 수 있다.
* how
  지하철의 노선을 생성한 뒤 제대로 생성된 경우 created라는 201 응답과 함께 생성된 line의 정보를 body에 담아 ResponseEntity로 응답하는 Controller의 다음 메서드를 통해 어떻게 사용하는지 확인할 수 있다.
```
@PostMapping
public ResponseEntity<LineResponse> createLine(@Valid @RequestBody LineRequest lineRequest) {
    LineResponse line = lineService.saveLine(lineRequest);
    return ResponseEntity.created(URI.create("/lines/" + line.getId())).body(line);
}
```
