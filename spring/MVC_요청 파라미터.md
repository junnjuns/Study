HTTP 요청 메시지를 통해 **클라이언트에서 서버로 데이터를 전달하는 방법 3가지**

- **GET - 쿼리 파라미터**
    - /url?username=hello&age=20
    - **message body 없이**, URL의 쿼리 파라미터에 데이터를 포함해서 전달

- **POST - HTML Form**
    - content-type : application/x-www-form-urlencoded
    - **message body에 쿼리 파라미터 형식**으로 전달 username=hello&age=20

- **HTTP message body** 에 데이터를 직접 담아서 요청
    - HTTP API에서 주로 사용, JSON, XML, TEXT
    - 데이터 형식은 주로 JSON
    - POST, PUT, PATCH
----

# @RequestParam

@RequestParam의 ``name(value)`` 속성이 파라미터 이름으로 사용
- @RequestParam(**"username"**) String **memberName** 
- => reqeust.getParameter("username")
- 파라미터 이름이 변수 이름과 같으면 생략가능
    - @RequestParam("userId") String userId
    - => @RequestParam String userId


## 파라미터 필수 여부

- @RequestParam.required
- 파라미터 필수 여부 결정
- 기본값이 파라미터 필수(true)이다.
- ``@RequestParam(required = true) String username`` 

## 파라미터 Map으로 조회

- @RequestParam Map<key, value>
- @RequestParam MultiValueMap (key=[value1, value2])
    - ex - (key=userIds, value=[id1, id2])


---

# @ResponseBody

- View 조회를 무시하고, HTTP message body에 직접 해당 내용을 입력한다.
- **@Controller + @ResponseBody = @RestController**

----

# @ModelAttribute

**요청 파라미터를 받아서 필요한 객체를 만들고 그 객체에 값을 넣어주는 과정을 자동화 해주는 어노테이션**

```java

@ResponseBody @RequestMapping("/model-attribute-v1") 
public String modelAttributeV1(@ModelAttribute HelloData helloData) { 

log.info("username={}, age={}", helloData.getUsername(), helloData.getAge()); 

return "ok"; 

  }
```
@ModelAttribute를 이용해서 HelloData 객체가 자동으로 생성된다.

- HelloData 객체 생성
- 요청 파라미터의 이름으로 HelloData 객체의 프로퍼티를 찾는다. 그리고 해당 프로퍼티의 setter를 호출해서 파라미터의 값을 입력한다.
    - 예) 파라미터 이름이 ``username`` 이면 ``setUsername()``메서드를 찾아서 호출하면서 값을 입력

- **프로퍼티**
객체에 ``getUsername()`` , ``setUsername()``  메서드가 있으면 이 객체는 username이라는 프로퍼티를 가지고 있다. 



