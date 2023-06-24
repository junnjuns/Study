MVC 패턴으로부터 파생되었으며 MVC에서 Controller가 View Model로 바뀐 패턴

----

# MVP 패턴의 단점

-  MVP 패턴에서는 View와 Presenter가 1:1로 매칭이 되어야 하기 때문에 View 와Presenter가 강하게 결합되어 있다는 문제가 있다.

----

# View Model이란 무엇인가?

- View는 ViewModel에 관한 참조를 가지고 있다
    - View는 비즈니스 로직을 포함하지 않고 데이터 바인딩을 통해 ViewModel과 상호 작용
-  ViewModel은 View에 관한 정보를 모른다.
    -  ViewModel은 View와 Model 사이의 매개체 역할을 하며 비즈니스 로직을 처리

=>  View와 ViewModel사이의 n:1의 의존관계가 생기며 다수의 View는 하나의 ViewModel에 매핑 =>  ViewModel은 다수의 View에 대해 완전히 **독립적**

----

# Command 패턴

-  View에서 발생하는 이벤트를 ViewModel에 전달하는 방식
- 요청을 객체로 만들어서 나중에 사용하거나 취소하거나 로깅할 수 있게 하는 패턴
- View에서 발생하는 이벤트를 ViewModel에 전달
-  ViewModel에서 이벤트를 처리
- 각각의 역할과 책임이 분명하고, 의존성이 낮다.

----

# Data Binding

- Data Binding은 View와 ViewModel 사이의 데이터를 연결한다.
- ViewModel을 변경하면 View가 변경된다.


----

# MVVM

1. 사용자의 Action들은 View를 통해 들어온다.
2. View에 Action이 들어오면 Command 패턴으로 ViewModel에 Action을 전달한다.
3. ViewModel은 Model에게 데이터를 요청한다.
4. Model은 ViewModel에게 요청받은 데이터를 응답한다.
5. ViewModel은 응답 받은 데이터를 가공하여 저장한다.
6. View는 Data Binding을 이용해 UI를 갱신시킨다.

---

# 장점

-  Command 패턴 또는 데이터 바인딩을 사용하여 View와 ViewModel사이의 의존성을 없앤다.
    -  View는 ViewModel의 상세 구현을 알 필요가 없고, ViewModel은 View의 UI 요소를 직접 조작할 필요가 없다 ->  View와 ViewModel은 서로 독립적으로 작성하고 테스트 가능
    -  ViewModel의 값이 변하면 자동으로 UI가 업데이트 된다. ->  ViewModel은 View에 데이터를 전달하기 위한 별도의 코드를 작성할 필요가 없다.

---

# 단점

- MVVM 패턴의 설계가 다른 디자인 패턴에 비해 복잡하다.