
요청 매핑 핸들러 어댑터 구조 정리
- HttpMessageConverter는 스프링 MVC 어디쯤에서 사용되는가?


---

# ArgumentResolver

정확히는 ``HandlerMethodArgumentResolver`` 인데 줄여서 ``ArgumentResolver`` 라고 부른다.

Annotation 기반의 컨트롤러는 **매우 다양한 파라미터를 사용**할 수 있다. ``HttpServletRequest`` , ``Model`` 은 물론, ``@RequestParam`` , ``@ModelAttribute`` 같은 애노테이션 그리고 ``@RequestBody`` , ``HttpEntity`` 같은 HTTP 메시지를 처리하는 부분까지 매우 큰 유연함을 보여준다.

- 스프링은 30개가 넘는 ArgumentResolver 를 기본으로 제공한다.

## 파라미터를 유연하게 처리할 수 있는 이유

- Annotation 기반 컨트롤러를 처리하는 ``RequestMappingHandlerAdapte``r가 ``ArgumentResolver`` 호출함
- ``ArgumentResolver`` 는 컨트롤러(핸들러)가 필요로 하는 다양한 파라미터 값(객체)을 생성함
- 파라미터 값(객체)가 모두 준비되면 컨트롤러를 호출하면서 값(객체)를 넘김

---

# ReturnValueHandler

정확히는 ``HandlerMethodReturnValueHandler`` 인데 줄여서 ``ReturnValueHandler`` 라 부른다.

- ``ArgumentResolver`` 와 비슷한데, 이것은 **응답 값을 변환하고 처리**한다.

---

# HTTP Message Converter 위치

- **요청할 때**
    - ``@RequestBody`` 를 처리하는 ``ArgumentResolver`` 가 있고, ``HttpEntity`` 를 처리하는 ``ArgumentResolver`` 가 있다. 이`` ArgumentResolver`` 들이 HTTP Message Converter를 사용해서 필요한 객체를 생성함
- **응답할 때**
    - ``@ResponseBody`` 와 ``HttpEntity`` 를 처리하는 ``ReturnValueHandler`` 가 있다. 그리고 여기에서 HTTP Message Converter를 호출해서 응답 결과를 만든다.

