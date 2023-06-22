
MVC 패턴으로부터 파생되었으며 MVC에서 Controller가 Presenter(프레젠터)로 교체된 패턴

----

# MVC 패턴을 사용하는 목적

- View와 Model 사이에 Controller를 두어 View와 Model의 의존성(Dependency)을 없앤다.

## MVC 단점

- View를 하나 만들면 이에 대응되는 Controller도 만들어야 하는가?
- View가 수정되면 Controller도 수정되어야 하는가?
- Model 업데이트는 Controller에서 하지 않아도 가능한가?


-> Controller는 특정 View와 Model에 의존적이며, View도 특정 Model에 의존적이라서 결합도가 높다.

----

# Presenter 란 무엇인가?

- Model과 View 사이의 다리 역할 수행
    - View에서 요청한 정보로 Model을 가공하여 View에 전달해준다.
- View와 Presenter는 1:1 관계


----

# MVP

- View와 Model 중간에 Presenter라는 연결 부분을 두어 MVC 패턴에서의 의존성 문제를 개선해준다.
- 사용자 입력이 오면 MVC 패턴은 Controller부터 왔지만, MVP 패턴은 View에서 발생한다.

1. 사용자의 Action은 View를 통해 들어오게 된다.
2. View는 데이터를 Presenter에 요청한다.
3. Presenter는 Model에게 데이터를 요청한다.
4. Model은 Presenter에게 요청받은 데이터를 응답한다.
5. Presenter가 데이터를 가공하고 다시 View에게 응답한다.
6. View는 Presenter로 부터 데이터를 응답받고 UI 데이터를 갱신한다.

----

# 장점

- MVP 패턴의 장점은 View와 Model의 의존성이 없다는 것이다. MVP 패턴은 MVC 패턴의 단점이었던 View와 Model의 의존성을 해결하였다. (Presenter를 통해서만 데이터를 전달 받기 때문)

---

# 단점

- MVC 패턴의 단점인 View와 Model 사이의 의존성은 해결되었지만, View와 Presenter 사이의 의존성이 높아진다.
- 1:1 관계를 유지해야 해서 View가 많아질수록 Presenter도 자연스럽게 많아진다.
    - 어플리케이션이 복잡해질수록 View와 Presenter 사이의 의존성이 강해진다.