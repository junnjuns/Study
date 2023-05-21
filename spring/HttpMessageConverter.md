
**JSON 데이터를 HTTP message body에서 직접 읽거나 쓰는 경우** **HttpMessageConverter**를 사용하면 편리하다.

- **아래와 같은 경우에 HttpMessageConverter 적용**
    - HTTP 요청: ``@RequestBody`` , ``@HttpEntity(RequestEntity)`` 
    - HTTP 응답:`@ResponseBody` , ``@HttpEntity(ResponseEntity)``


---

## ResponseBody 사용 원리

- ``@ResponseBody`` 사용
- HTTP body에 문자 내용을 직접 반환
- ``viewResolver`` 대신 ``HttpMessageConverter`` 가 동작
    - 기본 **문자** 처리: StringHttpMessageConverter가 동작
    - 기본 **객체** 처리: MappingJackson2HttpMessageConverter
    - byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음.

----

## HttpMessageConverter 인터페이스

스프링 부트는 다양한 메시지 컨버터를 제공한다. 대상의 클래스 타입, 미디어 타입 이 두 가지를 체크해서 사용여부를 결정한다. 만약 사용여부가 만족되지 않으면 다음 메시지 컨버터로 우선순위가 넘어간다.

- ``canRead()``, ``canWrite()``:: 메시지 컨버터가 해당 클래스, 미디어타입을 지원하는지 체크
- ``read()`` , ``write()`` : 메시지 컨버터를 통해서 메시지를 읽고 쓰는 기능

----

## HTTP Request 데이터 읽기

- HTTP 요청이 오고, 컨트롤러에서 ``@RequestBody`` , ``HttpEntity`` 파라미터를 사용
- 메시지 컨버터가 메시지를 **읽을 수 있는지 확인**하기 위해 ``canRead()`` 를 호출
    - 대상 **클래스 타입**을 지원하는가?
        - 예) ``@RequestBody`` 의 대상 클래스 ( ``byte[]`` , ``String`` , ``HelloData`` )
    - HTTP 요청의 **Content-Type 미디어 타입**을 지원하는가?
        - 예) ``text/plain`` , ``application/json`` , ``*/*``
- ``canRead()`` 조건을 만족하면 ``read()`` 를 호출해서 객체를 생성하고 반환한다.



## HTTP Response 데이터 생성

- 컨트롤러에서 ``@ResponseBody`` , ``HttpEntity`` 로 값이 반환된다.
- 메시지 컨버터가 메시지를 **쓸 수 있는지 확인**하기 위해 ``canWrite()`` 를 호출한다.
    - 대상 **클래스 타입**을 지원하는가?
        - return의 대상 클래스 ( ``byte[]`` , ``String`` , ``HelloData`` )
    - HTTP 요청의 **Accept 미디어 타입**을 지원하는가?
        - 예) ``text/plain`` , ``application/json`` , ``*/*``
- ``canWrite()`` 조건을 만족하면`` write()`` 를 호출해서 HTTP Response body에 데이터를 생성한다.