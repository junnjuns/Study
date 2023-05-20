
----

# @RestController

- **@Controller**는 반환 값이 String이면 **뷰 이름**으로 인식된다. 즉 **뷰를 찾고 뷰가 랜더링** 된다
- @**RestController**는 반환값으로 뷰를 찾는 것이 아니라, **HTTP message body 바로 입력**한다.

----

# @RequestMapping("/hello-basic")

- /hello-basic URL 호출이 오면 이 메서드가 실행되도록 매핑한다.
- 대부분의 속성을 배열[]로 적용하므로 다중 설정이 가능하다.
    - {"/hello-baisc", "/hello-go"}


-----

# HTTP 메서드 매핑

- @RequestMapping에 method 속성으로 HTTP 메서드를 지정하지 않으면 HTTP 메서드와 무관하게 호출된다.
    - GET, POST, PUT, PATCH, DELETE, HEAD 모두 허용


---

# @PathVariable(경로 변수) 사용

```java

@GetMapping("/mapping/{userId}") 
public String mappingPath(@PathVariable("userId") String data) { 

log.info("mappingPath userId={}", data); 
return "ok";
}
```
- 리소스 경로 : /mapping/**userA** 
- @PathVariable의 이름과 파라미터 이름이 같으면 생략할 수 있다.
    - @PathVariable("userId") String userId  --> @PathVariable String userId


----


## PathVariable 사용 - 다중
```java

@GetMapping("/mapping/users/{userId}/orders/{orderId}") 
public String mappingPath(@PathVariable String userId, @PathVariable Long orderId) { 

log.info("mappingPath userId={}, orderId={}", userId, orderId); 
return "ok"; 
}
```
- 리소스 경로 : /mapping/users/userA/orders/5

----

# 미디어 타입 조건 매핑

## **Content-Type 헤더 기반 :  consume**

- Content-Type : HTTP 메시지(요청과 응답 모두)에 담겨 보내는 데이터의 형식을 알려주는 헤더

```java
    /**
     * Content-Type 헤더 기반 추가 매핑 Media Type
     * consumes="application/json"
     * consumes="!application/json" 
     * consumes="application/*" 
     * consumes="*\/*" 
     * MediaType.APPLICATION_JSON_VALUE
     **/
    
@PostMapping(value = "/mapping-consume", consumes = "application/json") 
public String mappingConsumes() {

log.info("mappingConsumes");

return "ok"; 
}
```

- HTTP 요청의 Content-Type 헤더를 기반으로 미디어 타입을 매핑한다.
- 만약 맞지 않으면 HTTP 415 상태코드를 반환한다.

예시) consumes 

```  text
consumes = "text/plain"
consumes = {"text/plain", "application/*"}
consumes = MediaType.TEXT_PLAIN_VALUE
```

----

## Accept 헤더 기반 :  produce

- Accept 헤더의 경우에는 브라우저(클라이언트) 에서 웹서버로 요청시 요청메시지에 담기는 헤더
    - 쉽게 말해 자신은 이러한 데이터 타입만 허용하겠다는 뜻

```java
/** 
* Accept 헤더 기반 Media Type 
* produces = "text/html" 
* produces = "!text/html" 
* produces = "text/*" 
* produces = "*\/*" 
*/

@PostMapping(value = "/mapping-produce", produces = "text/html")
public String mappingProduces() {

log.info("mappingProduces");

return "ok"; 
}
```
- HTTP 요청의 Accept 헤더를 기반으로 미디어 타입을 매핑한다.
- 만약 맞지 않으면 HTTP 406 상태코드를 반환한다.

예시) produces
```text
produces = "text/plain" 
produces = {"text/plain", "application/*"} 
produces = MediaType.TEXT_PLAIN_VALUE 
produces = "text/plain;charset=UTF-8
```

----

## Content-Type 헤더와  Accept 헤더의 차이점

**Content-Type 헤더**는 현재 전송하는 데이터가 어떤 타입인지 명시한다. **Accept 헤더**는 클라이언트가 서버에게 자신이 처리할 수 있는 데이터 타입을 명시한다. 
- Content-Type : **내가 보낼 데이터의 형식을 서버에게 알려주는 것**
- Accept : **요청하고 내가 받아볼 데이터의 형식을 지정해주는 것**


----

# 요청 매핑 - API 예시

## 회원 관리 API

- 회원 목록 조회: GET   ```/users```
- 회원 등록: POST  ```/users```
- 회원 조회: GET  ```/users/{userId}```
- 회원 수정: PATCH  ```/users/{userId}```
- 회원 삭제: DELETE  ```/users/{userId}```



