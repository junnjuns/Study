

----

# 텍스트 - text, utext

- 태그 안의 텍스트를 서버에서 전달받은 값에 따라 표현하고자 할 때 사용
- th:text="${data}"
    - data라는 변수를 보여줌
    - **utext**
        - 기존 text는 출력할 문자에 태그가 포함되어 있는 경우, 문자에 태그를 반영하지 않고 **태그와 문자를 그대로 출력**한다.
        - utext는 출력할 문자에 태그가 포함되어 있는 경우 **태그를 반영한 문자를 출력**한다.
        


----


# 변수 - SpringEL

- 타임리프에서 변수를 사용할 때 변수 표현식을 사용한다. -> ${. . .}
- 변수 표현식은 SpringEL이라는 스프링에서 제공하는 표현식을 사용한다.
    - 단순 변수가 아닌 Object, List, Map 같은 객체를 사용하기 위해 활용
## Object
- user.username
- user['username']
- user.getUsername()

## List
- users[0].username
- users[0]['username']
- users[0].getUsername()

## Map
- userMap['userA'].username
- userMap['userA']['username']
- userMap['userA'].getUsername()

## 지역변수 선언 th:with

- th:with 를 사용하면 지역 변수를 선언해서 사용할 수 있다. 지역 변수는 선언한 테그 안에서만 사용할 수 있다.
```html
<div th:with = "name = 'hello Spring'">
    <p>지역변수 보여주기 <span th:text="${name}"></span></p>
</div>
```
-> 지역변수 보여주기 hello Spring

----
# URL 링크

- URL 생성할때 @{. . .} 문법을 사용한다.
- @{/. . .} : 절대 경로
- @{. . .} : 상대 경로

## 단순 URL
- @`{/hello}`
    - -> `hello`

## 쿼리 파라미터를 포함한 URL
- @`{/hello(param1 = ${Param1}, param2 = ${param2})}`
    - -> `/hello?param1=data1&param2=data2`
- () 에 있는 부분을 쿼리 파라미터로 처리한다.

## 경로 변수가 포함된 URL
- @`{hello/{param1}/{param2}(param1 = ${param1}, param2 = ${param2})}`
    - -> `{hello/data1/data2}`
- URL 경로상에 변수가 있으면 () 부분은 경로 변수로 처리된다.
- ----
# 리터럴 - Literals

- 리터럴이란? : 소스 코드상에 고정된 값

## 문자 리터럴 사용 시 주의

- 타임리프에서 문자 리터럴은 항상 **`**(작은 따옴표)로 감싸야 한다.
- 문자가 공백없이 쭉 이어진다면 작은 따옴표 생략 가능하다.
- `<span th:text="hello world!"></span>`  -> 오류
- `<span th:text="'hello world!'"></span>` -> 오류 수정
    -  공백이 존재하기 때문에 작은 따옴표 추가

## 리터럴 대체

- `<span th:text="|hello ${data}|">`
    - | | 로 리터럴 대체 문법을 사용하면 작은따옴표도 더하기도 필요 없이 대체 문법 안에 있는 리터럴과 변수들을 편리하게 사용할 수 있다.

----
# 반복 - th:each

타임리프에서 반복은 th:each 로 사용한다.

## 반복 기능
`th:each="user : ${users}"`
- 반복 시 오른쪽 컬렉션 ${users}의 값을 하나씩 꺼내서 왼쪽 변수 user에 담고 태그를 반복한다.
- List 뿐만 아니라 배열, java.util.Iterable, java.util.Enumeration을 구현한 모든 반복에 사용 가능하다.

## 반복 유지
`th:each=user, userStat : ${users}`
- 반복의 두번째 파라미터(userStat)를 설정해서 반복의 상태를 확인 할 수 있다.
- 두번째 파라미터 생략가능하다.
    - 만약 생략하면 **첫번째 파라미터 + Stat **로 사용 가능
- `index` : 0부터 시작하는 값
- `count` : 1부터 시작하는 값
- `size` : 전체 사이즈
- `even`   , `odd` : 홀수, 짝수 여부 (boolean)
- `first` , `last`: 처음, 마지막 여부 (boolean)
- `current` : 현재 객체

----
# 조건부 평가 - if, unless

if , unless(if의 반대)

```html
<tr th:each="user, userStat : ${users}"> 
    <td> 
        <span th:text="'미성년자'" th:if="${user.age lt 20}"></span> 
        <!-- 10 < 20 = true --> 미성년자 출력 --> 
        <!-- 20 < 20 = false --> 
        <!-- 30 < 20 = false --> 
    </td> 
</tr>

```
- 타임리프는 해당 조건이 맞지 않으면 태그자체를 렌더링 하지 않는다.
- if 조건이 false 인 경우 span 태그가 사라진다.

```html
<tr th:each="user, userStat : ${users}"> 
    <td th:switch="${user.age}"> 
        <span th:case="10">10살</span> <!-- 10 == 10 = true 일때 10살 출력 --> 
        <span th:case="20">20살</span> <!-- 20 == 20 = true 일때 20살 출력 --> 
        <span th:case="*">기타</span> <!-- 만족하는 조건이 없을 때 기타 출력 --> 
    </td> 
</tr>
```
- *은 만족하는 조건이 없을 때 사용하는 디폴트이다.
----
# 주석

## 표준 HTML 주석
- 자바 스크립트의 표준 HTML 주석은 타임리프가 렌더링 하지 않고 그대로 남겨둔다.
- 웹 페이지에서 안 보이고 소스코드에서 보인다.

## 타임리프 파서 주석
- 타임리프 파서 주석은 타임리프의 진짜 주석이다. 렌더링에서 주석 부분을 제거한다.
- 웹 페이지에서 안 보이고 소스코드에서도 안 보인다.
## 타임리프 프로토타입 주석
- HTML 파일을 그대로 열어보면 주석처리가 되지만, 타임리프를 렌더링 한 경우에만 데이터가 보인다.
- 웹 페이지에서 열었을 때 소스코드에서 안보이고 값도 안 나온다.
- 서버에서 렌더링 된 값이 들어올 경우 웹페이지에서 보이고 소스코드에서도 보인다.
----
# 블록 - th:block

- th:block 은 HTML 태그가 아닌 타임리프 유일한 자체 태그이다.
- 타임리프 특성상 HTML 태그 안에 속성으로 기능을 정의해서 사용한다.
- 만약 태그가 따로 없을 때 블록을 사용한다.

```html
<th:block th:each="user : ${users}"> 
    <div> 
        사용자 이름1 <span th:text="${user.username}"></span> 
        사용자 나이1 <span th:text="${user.age}"></span> 
    </div> 
    <div> 
    요약 <span th:text="${user.username} + ' / ' + ${user.age}"></span> 
    </div> 
</th:block>
```
- th:block 은 렌더링시 제거된다.
    - 렌더링 시 th:block은 제거되고 블록안에 있는 내용만 남는다.

