
- BindingResult: 검증과 바인딩의 결과를 저장하고 발생할 수 있는 오류를 담고 있는 스프링에서 제공하는 인터페이스 

----

# BindingResult 사용 시 특징

-  `BindingResult bindingResult` 파라미터의 위치는 `@ModelAttribute Item item` 다음에 와야 한다.
- `BindingResult`가 있으면 `@ModelAttribute`에 데이터 바인딩 시 타입 오류가 발생해도 400 에러 페이지가 보이지 않고 컨트롤러가 호출된다. 
- `BindingResult`는 사용자가 입력한 값을 보관하고 있어서 검증 오류 메시지와 함께 다시 입력 폼으로 보낼 수 있다.

##  BindingResult 적용 시 Controller가 무사히 호출되는 이유

스프링은 `@ModelAttribute`가 적용된 객체에 오류 등으로 인해 에러가 발생한 경우에 `FieldError`를 생성하여 `BindingResult`에 넣어준다. 때문에  **타입 오류가 발생해도** 400 에러 페이지가 나오는 것이 아니라 BindingResult가 에러를 처리하여 **Controller가 무사히 호출되고** View에서 에러 정보가 보이는 것을 볼 수 있다.

----

# addError()

검증 로직 조건문에 걸리면, addError를 통해 View로 넘겨줄 bindingResult안에 에러 메시지들을 담을 수 있다.

`rejectedValue` 가 오류 발생시 사용자 입력 값을 저장하는 필드이기 때문에 이 필드를 통해 **오류 발생 시 사용자 입력 값을 유지할 수 있다**
## FieldError

-  FieldError는 어떤 객체의 어떤 필드에 어떤 오류가 발생했는지를 저장하고 있다.
- 특정 필드와 관련된 에러 1: `FieldError("객체 이름", "필드 이름", "에러 메시지")`
- 특정 필드와 관련된 에러 2: `FieldError(String objectName, String field, @Nullable Object rejectedValue, boolean bindingFailure, @Nullable String[] codes, @Nullable Object[] arguments, @Nullable String defaultMessage)`
    - `objectName` : 오류가 발생한 객체 이름 
    - `field` : 오류 필드
    - `rejectedValue` : 사용자가 입력한 값(거절된 값) 
    - `bindingFailure` : 타입 오류 같은 바인딩 실패인지, 검증 실패인지 구분 값
    - `codes` : 메시지 코드 
    - `arguments` : 메시지에서 사용하는 인자 
    - `defaultMessage` : 기본 오류 메시지

## ObjectError

-  ObjectError는 어떤 객체에 어떤 오류가 발생했는지를 저장하고 있다.
- 전체와 관련된 에러1: `ObjectError("객체 이름", "에러 메시지")`
- 전체와 관련된 에러2: ` ObjectError(String objectName, String code, Object[] args, String defaultMessage)`
    - `objectName`: 오류가 발생한 객체의 이름
    - `code`: 기본 오류 코드. 오류 메시지를 찾을 때 사용
    - `args`: 오류 메시지에 들어갈 인자의 배열. 인자는 {0}, {1} 등으로 메시지에 표시됨. 예를 들어, args가 [“name”, 10]이고 메시지가 "{0}은 {1}글자 이상이어야 합니다."라면 "name은 10글자 이상이어야 합니다." 라고 표시
    - `defaultMessage`: 기본 오류 메시지

----

# hasErrors()

- `hasErrors()`: bindingResult에 에러가 있는지 검사하는 메서드
- bindingResult는 model에 errors 객체를 담는 작업을 자동으로 처리해주므로, 따로 model 객체에 bindingResult를 담을 필요가 없다.