스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술

----
# DispatcherServlet

- 이전에 만들어본 FrontController와 비슷한 역할을 한다.

---

## DispatcherServlet 서블릿 등록

- `DispatcherSetvlet`는 부모 클래스에서 `HttpServlet`을 상속 받아서 사용하고 서블릿으로 동작한다.
- 스프링부트가 `DispatcherServlet`을 서블릿으로 자동으로 등록하면서 모든 경로(`urlPatterns ="/"`)에 대해서 매핑한다.



## 요청 흐름

- 서블릿이 호출되면 `HttpServlet`이 제공하는 `service()`가 호출된다.
- 이후 여러 메서드가 호출되면서 최종적으로 `DispatcherServlet.doDispatch()`가 호출된다.
1. **핸들러 조회** - getHandler()
    - 핸들러 매핑을 통해 요청 URL에 매핑된 핸들러(컨트롤러)를 조회한다.
2. **핸들러 어댑터 조회** - getHandlerAdapter()
    - 핸들러를 처리할 수 있는 어댑터를 조회한다.
3. **핸들러 어댑터 실행**
    - 핸들러 어댑터를 통해 **핸들러 실행** - HandlerAdapter.handle()
    - **ModelAndView 반환** - mv
4. **뷰 렌더링 호출** - render(mv, request, response)
    - mv에서 **ViewName**을 가져온다. 이후 **ViewResolver**를 통해서 View의 논리 이름을 물리 이름으로 바꾸고 **View 객체를 반환**한다.
5. **뷰 렌더링** - view.render()
    - View를 통해서 View를 렌더링한다.






