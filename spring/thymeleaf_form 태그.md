
타임리프가 제공하는 입력 폼 기능

-----

# th:object

-  form 태그에서 데이터를 어떤 객체로 받을지 정해주는 속성
    - 컨트롤러와 뷰 사이의 DTO 클래스의 객체
- 선택 변수 식(*{. . .})을 적용할 수 있다.
- 예시) `<form th:object="${item}">` 
    -  form 태그에서 데이터를 item이라는 객체로 받겠다는 뜻



----

# *{. . .}

-  th:object에서 정한 객체의 데이터에 접근할 수 있게 해주는 표현식(선택 변수 식)
- 예시) `*{itemName}` == `${item.itemName}`

----

# th:field

-  input 태그의 id, name, value 속성을 자동으로 설정해주는 속성
    - id : th:field에서 지정한 변수 이름과 같다.
    - name : th:field에서 지정한 변수 이름과 같다.
    - value : th:field에서 지정한 변수의 값을 사용한다.
- 예시) `<input type="text" th:field="*{itemName}"/>` == `<input type="text" id="itemName" name="itemName" value=""/>`
