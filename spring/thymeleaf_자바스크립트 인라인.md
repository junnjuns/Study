
- 자바스크립트 인라인 : 타임리프 변수나 표현식을 자바스크립트 코드 안에 쉽게 삽입할 수 있도록 해주는 기능

- 자바스크립트 인라인 기능 적용 : `<script th:inline="javascript">`  코드 추가

----

# 텍스트 렌더링
- 타임리프 변수를 자바스크립트 변수에 할당할 때, 문자열의 경우 따옴표를 자동으로 추가해준다.
- 예시) `var username = [[${user.username}]];` 
    - 인라인 사용 전 : `var username = userA;`
    - 인라인 사용 후 : `var username = "userA";` 
- 인라인 사용 후 렌더링 결과를 보면 문자 타입인 경우 " 를 포함해준다.

----

# 자바스크립트 내추럴 템플릿

-  타임리프는 HTML 파일을 직접 열어도 동작하는 내추럴 템플릿 기능을 제공한다.
- 주석 안에 타임리프 변수나 표현식을 삽입하면, 렌더링할 때 주석 부분이 제거되고 타임리프 변수나 표현식이 적용
- 예시)  `var username2 = /*[[${user.username}]]*/ "test username";`
    - 인라인 사용 전 : `var username2 = /*userA*/ "test username";`
    - 인라인 사용 후 : `var username2 = "userA";`
- 인라인 사용 전은 내추럴 템플릿 기능이 동작하지 않고 렌더링 내용이 주석처리 되어 버린다.
- 인라인 사용 후는 주석 부분이 제거되고 타임리프 변수가 적용된다.

## 장점

 - HTML 파일을 직접 열었을 때도 `"test username"` 이라는 임시 값으로 보여지기 때문에 내추럴 템플릿의 장점을 유지할 수 있다.

----

# 객체화

- 타임리프의 자바스크립트 인라인 기능을 사용하면 객체를 JSON으로 자동으로 변환해준다.
- 예시) `var user = [[${user}]];
    - 인라인 사용 전 : `var user = BasicController.User(username=userA, age=10);`
    - 인라인 사용 후 : `var user = {"username":"userA","age":10};`
- 인라인 사용 전은 객체의 toString()이 호출된 값이다.
- 인라인 사용 후는 객체를 JSON으로 변환해준다.

----

# 반복

- 자바스크립트 인라인은 each를 지원한다. (기존의 th:each와 똑같이 작동)
```html
<script th:inline="javascript">
 [# th:each="user, stat : ${users}"]
 var user[[${stat.count}]] = [[${user}]];
 [/]
</script>
```

자바스크립트 인라인 each 결과
```html
<script>
var user1 = {"username":"userA","age":10};
var user2 = {"username":"userB","age":20};
var user3 = {"username":"userC","age":30};
</script>
```