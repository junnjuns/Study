
요청 파라미터와는 다르게 HTTP message body를 통해 데이터가 직접 넘어오는 경우는 ``@RequestParam`` , ``@ModelAttribute`` 를 사용할 수 없다.  

----

# 단순 TEXT

## Input, Output 스트림

- **InputStream(Reader)**: HTTP 요청 메시지 바디의 내용을 직접 조회
- **OutputStream(Writer)**: HTTP 응답 메시지의 바디에 직접 결과 출력


## HttpEntity

- HTTP **header**, **body** 정보를 편리하게 조회할 수 있다.
- message body 정보를 직접 조회
- message body 정보를 직접 반환
- view 조회 X

## @RequestBody, @ResponseBody

- **@RequestBody를 사용하여 HTTP message body 내용을 편리하게 조회**할 수 있다. **header 정보를 조회하려면 HttpEntity 또는 @RequestHeader를 사용**해야 한다.
- **@ResponseBody를 사용하여 응답 결과를 HTTP message body에 직접 담아서 전달**한다.


----

# JSON

## @ReqeustBody 문자 변환

- @RequestBody 를 사용해서 HTTP 메시지에서 데이터를 꺼내고 messageBody에 저장한다. 
- 문자로 된 JSON 데이터인 messageBody 를 ``objectMapper`` 를 통해서 자바 객체로 변환한다.

```java
public String requestBodyJsonV2(@RequestBody String messageBody) throws IOException { 

HelloData data = objectMapper.readValue(messageBody, HelloData.class); 

log.info("username={}, age={}", data.getUsername(), data.getAge()); 

return "ok"; 
}
```


## @RequestBody 객체 변환

- 위의 방식처럼 꺼낸 데이터를 문자로 변환하고 다시 JSON으로 변환하는 과정이 불편하다. 더욱 편리하게 객체로 바로 변환해보자.
```java

public String requestBodyJsonV3(@RequestBody HelloData data) 

log.info("username={}, age={}", data.getUsername(), data.getAge());

```
- @Requestbody에 직접 만든 객체를 지정할 수 있다.
    - ``@RequestBody HelloData data``


## 요청, 응답  과정

- **@RequestBody 요청**
    - JSON 요청 ->  HTTP 메시지 컨버터 -> 객체 

- **@ResponseBody 응답**
    - 객체  -> HTTP 메시지 컨버터  -> JSON 응답