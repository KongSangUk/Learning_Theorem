## REST API



### API를 왜 사용하고 어떻게 사용할까?

모놀리식 애플리케이션이 완전히 사라졌다거나 앞으로도 만들어지지 않는다는 말은 아니다. 특희 다음과 같은 환경에서는 다양한 상황에서 단일 패키지로 수많은 기능을 제공하는 모놀리식 애플리케이션이 여전히 유효하다.

- 도메인과 도메인의 경계가 모호할 때
- 제공된 기능이 긴밀하게 결합됐으며, 모듈 간 상호작용에서 유연함보다 성능이 절대적으로 더 중요할 때
- 관련된 모든 기능의 애플리케이셔 확장 요구사항이 알려져 있고 일관적일 때
- 기능이 변동성이 없을때, 즉 변화가 느리거나 변화 범위가 제한적일 때와 둘 다일 때

그 외에는 MAS(마이크로서비스)를 사용한다.

물론 이 요약은 지나치게 단순화됐지만 타당하다. 마이크로서비스에서는 기능을 작고 응집역 있는 **청크**로 분할해 디커플링한다. 따라서 더 빠른 배포롸 용이한 유지보수가 가능한 우연하고 튼튼한 시스템을 구축하게 된다.

어떤 마이크로서비스 분산 시스템에서든 통신이 핵심이다. 어떤 서비스도 섬처럼 고립되어 동작하지 않는다. 애플리케이션/마이크로서비스를 연결하는 수많은 동작 방식이 있지만, 일상생활에서 자주 사용하는 인터넷으로 이야기를 시작해보겠다.

이터넷은 통신을 위한 수단으로 만들었다. 인터넷의 전신인 **AROANET**의 설계자들은 '중대한 장애'가 발생하더라도 시스템 간 통신이 유지되게 하려고 했다. **AROANET과 HTTP** 기반의 접근방식은 유사하다. 오늘날 사용하는 시스템은 유선을 통해 다양한 리소스를 생성, 조회, 업데이트, 삭제하는 등의 기능을 수행한다.

</br>

### REST가 무엇이며, 왜 중요한가?

앞서 말했다 싶이, API는 개발자 작성한 사양/인터페이스이다. 

- API를 통해 라이브러리
- 다른 어플리케이션
- 서비스 같은 코드

들을 사용 할 수있다. 그런데 **REST API**에서 REST의 의미는 무었일까?

REST는 Representational State Transfer의 약어이다. 다소 모호한 표현이지만 다음과 같이 설명한다. 

> 애플리케이션 A가 애플리케이션 B와 통신할 때, A는 B에서 통신 시점의'현재 상태'를 가져온다. A는 B가 통신 호출간 '상태'를 유지하라고 가정하지 않는다. A는 B에 요청할 때마다 관련 '상태'의 표현을 제공한다. 이런 동작 방식은 생존가능성과 회복탄력성을 향상시킨다. 이렇게 처리하는 이유는 통신 문제가 발생하거나 B가 충돌해 재시작돼도, A와 상호작용한 연재상태를 잃어버리지 않기 때문이다. A가 다시 요청해 두 애플리케이션이 중단된 곳에서 통신을 이어간다.



</br>

### API, HTTP 메서드 스타일

**IETF RFC**문서에 수많은 표준 HTTP 메서드가 정의됐다. 그중 API를 만들 때 일반적으로 사용되는 것은 일부이며, 가끔 사용되는 메서드가 몇 개 더 있다. REST API는 주로 다음의 HTTP 메서드를 기반으로 한다.

- POST
- GET
- PUT
- PATCH
- DELETE

이는 생성(POST), 읽기(GET), 업데이트(PUT 및 PATCH), 삭제 (DELETE) 와 같이 리소스에 수행하는 일반적인 작업이다. 때로는 다음의 두 메서드도 사용한다.

- OPTIONS
- HEAD

두 메서드는 요청/응다 쌍에서 사용할 수 있는 통신 옵션을 조회하고(OPTIONS), 응단 메시지에 바디를 뺀 응답 헤더를 조회하는(HEAD)데 사용한다.

</br>

### GET 

#### GET들러가기 전

**@RestController**개요를 살펴보자 스프링 MVA는 뷰가 서버 렌더링된 웹페이지로 제공된다는 가정하에, 데이터와 데이터를 전송하는 부분과 데이터를 표현하는 부분을 분리해 생성한다. 이러한 MVC의 여러 부분을 연결하는데 **@Controller** 어노테이션이 도움 된다.

**@Controller**는 **@Component**의 스테레오타임 별칭이다. 따라서 애플리케이션 실행시, 스프링 빈 **@Controller** 어노테이션이 붙은 클래스로 생성이 된다.

**@Controller**가 붙은 클래스는 Model 객체를 받는다. Model 객체로 표현 계층에 기반 데이터를 제공한다. 또 ViewResolver과 함께 작동해 애플리케이션이 뷰 기술에 의해 랜더링된 특정 뷰를 표시하기도 한다.

간단희 **@ResponseBody**를 클래스나 메서드에 추가해서 JSON이나 XML 같은 데이터 형식처럼 형식화된 응답을 반환하도록 **Controller** 클래스에 지시할 수도 있다. 이렇게 하면 메서드의 객체/Iterable 반환값이 웹 응답의 전체 바디가 된다. 모델의 일부로 반환되지 않는다.

**@RestController** 어노테이션은 **@Controller**와  **@ResponseBody**를 하나의 어노테이션으로 합친 이다. 결합해 하나의 어노테이션을 사용함으로써 코드를 단순하게 만들고 의도를 더욱 명확하게 표현하게 된다. 클래스에  **@RestController**ㄹㅗ 어노테이션을 달아서 REST API를 만들 수 있다.



</br>

# GET 시작

---

> GET method는 클라이언트에서 서버로 어떠한 리소스로부터 정보를 요청하기 위해 사용되는 메서드이다.



좀 더 쉽게 말하자면, 데이터를 **읽거나**(Read), **검색**(Retrieve)할 때에 사용되는 method라고 할 수 있다.
GET은 요청을 전송할 때 URL 주소 끝에 파라미터로 포함되어 전송되며, 이 부분을 **쿼리 스트링(QueryString)이라고** 부른다.



> e.g.) www.example-url.com/resources?name1=유재석&name2=무한도전
>
> 위 예는 앞서 말한 쿼리스트링을 포함한 URL이다. 파라미터인 name1과 name2를 통해 값을 전달받을 수 있다.
> 만약, 요청 파라미터가 여러 개이면 &로 연결한다.



그리고 GET 요청은 오로지 데이터를 읽을 때만 사용되고 수정할 때는 사용하지 않는다.
따라서 이런 이유로 사용하면 안전하다고 간주되죠. 즉, 데이터의 변형의 위험 없이 사용할 수 있다는 뜻이다.



#### GET 요청에 대한 기타 참고 사항

- GET은 불필요한 요청을 제한하기 위해 요청이 캐시 될 수 있다
- 파라미터에 내용이 노출되기 때문에 민감한 데이터를 다룰 때 GET 요청을 사용해서는 안된다.
- GET 요청은 브라우저 기록에 남는다.
- GET 요청을 북마크에 추가할 수 있다.
- GET 요청에는 데이터 길이에 대한 제한이 있다.
- Get 요청은 성공 시, 200(Ok) HTTP 응답 코드를 XML, JSON뿐만 아니라 여러 데이터(html, txt 등..), 여러 형식의 데이터와 함께 반환한다.
- GET 요청은 **idempotent 합니다.**



### GET API의 특징

- 리소스를 취득하는 작업을 하는 API이다.
- CRUD에서 R에 해당한다.
- 값을 읽어오기만하여 멱등성과 안정성이 있다는 특징이 있다.
- Path Variable을 사용가능하다.
- Query Parameter도 사용가능하다.



### 사용되는 Annotation의 종류

- **@RestController**
  해당 annotation을 추가해주면 해당 class는 REST API를 처리하는 controller로 등록이 된다.
- **@RequestMapping(path)**
  리소스를 설정하는 코드이며 괄호안에 입력하는 값에 따라 URI가 localhost:8080/path로 설정된다.
- **@GetMapping(path)**
  Get API를 해당 uri로 mapping시켜준다.
- **@PathVariable**
  변화하는 구간에 사용하는 annotation이며 URL path를 parsing해준다.
- **@RequestParam**
  URL에 Query문을 추가할 때 parameter를 parshing해준다.

```java
@RestController
@RequestMapping("/api/get")
public class GetApiController {

    // http://localhost:9090/api/get/hello
    @GetMapping(path = "/hello")
    public String Hello(){
        return "get Hello";
    }
}

```



Spring boot가 처음이기에 코드에 대해 처음부터 설명을 하며 정리를 해보려한다.

- 먼저 Class를 생성하는 코드부터 보겠다. 코드의 첫번째 줄인 **@RestController**은 해당 annotation을 추가해주면서 해당 class는 REST API를 처리하는 controller로 등록을 한다.
- 두전째 줄인 **@RequestMapping("/api/get")**은 URI를 지정해주는 annotation이다.
- 이렇게 @RestController, @RequestMapping("/api/get")를 통해 GetApiController class는 기본 uri를 http://localhost:8080/api/get 로 갖으며 REST API를 처리하는 controller로 생성되었다.



다음으로 Method를 확인해보겠다. 

- Method위에 추가된 **@GetMapping(path = "/hello")**라는 annotatin은 GET 요청이 path로 들어오면 해당 method를 mapping시켜주겠다는 의미를 가진다. 
- 위의 코드의 경우는 기본 주소인 http://localhost:8080/api/get에 path인 /hello를 추가하여 http://localhost:9090/api/get/hello 로 GET request가 들어오면 method가 실행되고 결과인 "get Hello"를 return해주게 된다.

- @GetMapping은 기본적으로 괄호안에 ("/hello")라고 입력을 해도 괄호 안의 값을 value값으로 인식을 해서 path = "/hello"와 같은 결과를 내게 된다. 그래서 path는 꼭 써주지 않아도 된다.



```java
    @RequestMapping(path = "/hi", method = RequestMethod.GET)
    public String hi(){
        return "get hi";
    }
```

위의 코드는 과거에 사용하던 방식으로 RequestMapping을 하면 Get, Post, Put, Delete가 모두 동작한다. 위와 같이 Get만 사용하도록 지정하고 싶다면 method parameter를 추가해주면 된다.



### PathVariable 사용하기

```java
    @GetMapping("path-variable/{name}")
    public String pathVariable(@PathVariable String name){
        // 일반적으로 34번,35번,38번 line의 변수명(name)이 같아야한다.
        System.out.println("PathVariable : " + name);
        return name;
    }
```



- 사용자의 이름, 아이디 등 변할 수 있는 정보를 URI에 넣어야 한다면 개발자는 같은 method를 각각의 path를 마다 생성해줘야 할 것이다. 
- 하지만 이것은 매우 비효율적인 개발 방법이며 이러한 일을 해결해 주는 것이 PathVariable이다. 
- 변화하는 구간에 대해서는 PathVariable을 사용해야한다.



PathVariable은 @GetMapping의 Path를 "path-variable/{name}"와 같이 변경될 수 있는 부분을 { }로 지정해주며 사용한다. { }안에는 변수명을 적으며 method의 parameter에 @PathVariable 변수명을 입력하며 { }에 들어가는 값을 읽어온다.

위의 코드의 경우는 http://localhost:8080/api/get/path-variable/seongwon 이라는 URI로 request가 왔다면 name은 seongwon이 되며 return될 것이다.



```java
   @GetMapping("path-variable/{name}")
    public String pathVariable(@PathVariable(name = "name") String pathName){
        System.out.println("PathVariable : " + pathName);
        return pathName;
    }
```

> 프로그래밍을 하다보면 변수가 꼬여 PathVariable의 변수명을 다르게 설정해야하는 상황이 생길 수도 있다. 그러한 경우에는 위의 코드와 같이 method parameter에 @PathVariable에 (name=~~)와 같이 지정을 하며 다른 변수명을 사용할 수도 있다.



### Query Parameter 사용하기

> https://www.google.com/search?q=velog.&oq=velog.&aqs=chrome..69i57j0i512l2j0i30j69i60l4.4182j0j4&sourceid=chrome&ie=UTF-8 다음 주소를 한번 확인해보자. Query parameter는 위의 주소에서 search? 부터의 주소를 의미한다. 즉, 검색을 할 때 여러가지 매개변수들을 의미한다.

위의 주소를 자세히 보면 중간 중간 and 연산자가 있다. 한번 Query parameter를 &로 나눠서 보도록 하자

> search?q=velog.
> &oq=velog.
> &aqs=chrome..69i57j0i512l2j0i30j69i60l4.4182j0j4
> &sourceid=chrome&ie=UTF-8

& 단위로 나눠서 본 URI를 확인해보면 처음에 Search이후 ?로 시작을 하여 key=value의 형식으로 문장이 이어지는 것을 확인할 수 있다. Query parameter는 즉 Key, value값으로 이루어진 연속된 query문으로 이루어진 것을 알 수 있다.

이제 Query parameter를 받는 방법을 알아보도록 하자.

```java
    @GetMapping(path = "query-param01")
    public String queryParam01(@RequestParam Map<String, String> queryParam){
        // key=value값으로 데이터를 받기에 Map으로 구현

        StringBuilder sb = new StringBuilder();

        queryParam.entrySet().forEach( entry -> {
           System.out.println(entry.getKey());
           System.out.println(entry.getValue());
           System.out.println("\n");

           sb.append(entry.getKey()+ " = "+entry.getValue()+"\n");
        });

        return sb.toString();
    }
```



- 위의 코드는 Map을 활용하여 Key, value값을 받아온 코드이다.
- 하지만 위의 코드의 경우는 Key값들이 무엇이 있는지 코드상으로 명확하게 알 수가 없다.
- 아래와 같이 @RequestParam을 여러번 사용하여 Key값을 지정하고 값을 받아오면 Query parameter에서 사용하는 key값들을 미리 알고 지정할 수 있다.



```java
    @GetMapping(path = "query-param02")
    public String queryParam02(@RequestParam String name,
                               @RequestParam String email,
                               @RequestParam int age){
        System.out.println(name);
        System.out.println(email);
        System.out.println(age);

        return name+ "\n" + email+ "\n"+age;
    }
```



- 마지막 방법은 객체를 미리 정의하고 사용하는 방법으로 현재 사람들이 가장 많이 사용하는 방법이다.
- Request DTO(객체)를 미리 만들어 사용하는 방법은 이전 방식과 다르게 @RequestParam을 붙이지 않는다.
- 코드의 동작 원리는 다음과 같다. Spring boot에서는 parameter로 객체가 들어오면 "?user=steve&email=steve@gmail.com&age=30"와 같은 query parameter에 있는 객체들을 spring boot에서 판단을 하고 key에 위치한 값들을 객체의 변수의 이름과 매칭을 해주며 작동을 한다.



```java
    @GetMapping(path = "query-param03")
    public String queryParam03(UserRequest userRequest){
        // 이전 방식과 다르게 @RequestParam을 붙이지 않는다.
        // Spring boot에서는 parameter로 객체가 들어오면
        // "?user=steve&email=steve@gmail.com&age=30"와 같은
        // query parameter에 있는 객체들을 spring boot에서 판단한다.
        // 그리고 key에 위치한 값들을 객체의 변수의 이름과 매칭을 해준다.
        System.out.println(userRequest.getName());
        System.out.println(userRequest.getEmail());
        System.out.println(userRequest.getAge());

        return userRequest.toString();
    }
```



</br>

# POST 시작

---

> POSTmethod는 리소스를 생성/업데이트하기 위해 서버에 데이터를 보내는 데 사용된다.

 

GET과 달리 전송해야 될 데이터를 HTTP 메시지의 Body에 담아서 전송한다. 그리고 그 Body의 타입은 요청 헤더의 Content-Type에 요청 데이터의 타입을 표시 따라 결정된다. (POST로 요청을 보낼 때는 해야 합니다.)

HTTP 메시지의 Body는 길이의 제한 없이 데이터를 전송할 수 있습니다. 그래서 POST 요청은 GET과 달리 대용량 데이터를 전송할 수 있는 이유도 이 때문이다.

이처럼 POST는 데이터가 Body로 전송되고, 내용이 눈에 보이지 않아 GET보다 보안적인 면에서 안전하다고 생각할 수 있지만, POST 요청도 크롬의 **개발자 도구**, **Fiddler**와 같은 툴로 요청 내용을 확인할 수 있기 때문에 민감한 데이터의 경우에는 반드시 암호화해 전송해야 한다.

#### Post 요청에 대한 기타 참고 사항

- POST 요청은 캐시 되지 않는다.
- POST 요청은 브라우저 기록에 남아 있지 않다.
- POST 요청을 북마크에 추가할 수 없다.
- POST 요청에는 데이터 길이에 대한 제한이 없다.
- Post 요청 중 자원 생성은 201(Created) HTTP 응답 코드를 반환한다.
- Post 요청은 **idempotent**하지 않다.



#### POST API의 특징

- 리소스의 생성 및 추가하는 작업을 해주는 API이다.
- CRUD에서 C에 해당한다.
- POST Request를 반복한다면 데이터들은 계속 추가될 것이고 서버는 매번 다른 응답을 나타낼 것이다. 이때 응답이 같을 수도 있지만 응답이 같더라도 서로 다른 데이터기에 POST API는 멱등하지 않다.
- 또한 GET은 데이터를 읽어오기만 하여 안전성이 있었지만 POST의 경우는 데이터를 추가하며 조작하기에 안전성은 없다.
- Path Variable을 사용가능하다.
- Data Body를 사용 가능하다.



#### 사용되는 Annotation의 종류

- **@RestController**
  해당 annotation을 추가해주면 해당 class는 REST API를 처리하는 controller로 등록이 된다.
- **@RequestMapping(path)**
  리소스를 설정하는 코드이며 괄호안에 입력하는 값에 따라 URI가 localhost:8080/path로 설정된다.
- **@PutMapping(path)**
  POST API를 해당 URI로 mapping시켜준다.
- **@RequestBody**
  Request body부분을 Parsing해준다.
- **@PathVariable**
  변화하는 구간에 사용하는 annotation이며 URL path를 parsing해준다.
- **@JsonProperty**
  지정한 변수에 대해 Snake case와 Camel case의 변수를 mapping해준다.
- **@JsonNaming**
  해당 Class 내에 있는 모든 변수에 대해 Snake case와 Camel case의 변수를 mapping해준다.



### POST API사용하기

#### @PostMapping 사용하기

```java
@RestController
@RequestMapping("/api")
public class PostApiController {

    @PostMapping("/post")
    public void post(@RequestBody Map<String, Object> requestData){
        
        requestData.forEach((key, value) -> {
            System.out.println("key : " + key);
            System.out.println("value : " + value);
        });
    }
}
```



- POST API도 GET API과 같이 parameter앞에 무언가를 붙여줘야한다.
- Get은 데이터를 읽어오기 위해 clinet에서 읽어오고자 하는 data의 parameter를 보내고 
- Server는 그것을 받기 위해 @RequestParam을 붙였다면 POST는 데이터를 생성하고 
- 추가하기 위해 아래 사진과 같이 Json 또는 XML타입의 데이터를 Client에서 보내기 때문에 
- Post는 Data block을 받기 위해 **@RequestBody**를 붙여준다.



![img](https://images.velog.io/images/seongwon97/post/b6764d28-f10e-482a-bb72-038e44683e7b/post.PNG)



```java
@RestController
@RequestMapping("/api")
public class PostApiController {

    @PostMapping("/post1")
    public void post1(@RequestBody PostRequestDTO requestData){
        // get과 다르게 객체를 사용하여 받아와도 RequestBody를 입력해줘야한다.
        System.out.println(requestData);
    }
}
```

POST API도 Map형식 말고 DTO(class)를 통해 POST가 가능하다.
POST에서 객체를 사용하여 데이터를 받아올 때는 GET과 다르게 method parameter앞에 @RequestBody를 붙여줘야한다.



#### @JsonProperty, @JsonNaming

>  단어 표현은 Snake case와 Camel Case를 가장 많이 사용한다.
> **Snake case**는 단어의 구분을 _(언더바)를 사용하여 구분하여 snake_case와 같이 표현을 하는 방법이다.
> **Camel case**는 단어의 구분을 단어의 시작을 대문자로 만들어 구분을 하는 방법으로 camelCase와 같이 대문자로 구분을 하는 방법이다.

우리가 공부할 JSON과 spring boot의 Java는 대표적으로 사용하는 표현법이 서로 다르다. JSON의 경우는 Snake case를 주로 사용하며 Java에서는 Camel case를 주로 사용한다.

우리는 데이터를 원활하게 읽어오도록 구현하기 위해서는 snake case뿐만 아니라 camel case로도 parsing이 가능하도록 구현을 해야한다.

Spring boot에서는 **@JsonProperty, @JsonNaming**를 사용하면 쉽게 해결할 수 있다.



#### @JsonProperty사용하기

**PostRequestDTO.java**

```java
public class PostRequestDTO {

    private String account;
    private String email;
    private String address;
    private String password;
    private String phoneNumber;
	
    .....
    
}
```



**Request에 보내는 JSON**

```null
{
  "account" : "user01",
  "email" : "abc123@gmail.com",
  "address" : "Korea",
  "password" : "adcd",
  "phone_number" : "010-1234-5678"
}
```

class내의 phoneNumber는 camel case로 생성되었다 하지만 request body에 담겨 보내지는 json에서 phone_number와 같이 snake case로 key값이 정의되었다. 이러한 경우 request를 한다면 전화번호는 아래와 같이 null값이 출력될 것이다.

> **Result**
> PostRequestDTO{account='user01', email='abc123@gmail.com', address='Korea', password='adcd', phoneNumber='null'}

해결법 중 하나는 @JsonProperty을 이용해 다른 이름도 허용되도록 하는 것이다.
@JsonProperty는 OTP와 같이 camel도 아니고 snake도 아닌 애매한 것들은 JsonProperty("OTP")와 같은 것을 붙여 matching을 시켜주기도 한다.



```java
public class PostRequestDTO {

    private String account;
    private String email;
    private String address;
    private String password;

    @JsonProperty("phone_number")
    private String phoneNumber;
	
    .....
    
}
```

이와 같이 JsonProperty annotation을 통해 phoneNumber를 phone_number도 허용하게 한다면 null이었던 phoneNumber값이 정상적으로 출력되는 것을 확인할 수 있다.



> **Result**
> PostRequestDTO{account='user01', email='abc123@gmail.com', address='Korea', password='adcd', phoneNumber='010-1234-5678'}



#### @JsonNaming 사용하기

@JsonProperty는 변수 하나씩 지정해야하는 번거로움이 있다. @JsonNaming을 사용하면 class전체의 변수들을 camel case에서 snake case로 매칭시켜줄 수 있다.

```java
@JsonNaming(value = PropertyNamingStrategy.SnakeCaseStrategy.class)
public class PostRequestDTO {

    private String account;
    private String email;
    private String address;
    private String password;
    private String phoneNumber;
	
    .....
    
}
```





</br>

# PUT 시작



#### POST API의 특징

- 리소스의 생성 및 추가하는 작업을 해주는 API이다.
- CRUD에서 C에 해당한다.
- POST Request를 반복한다면 데이터들은 계속 추가될 것이고 서버는 매번 다른 응답을 나타낼 것이다. 이때 응답이 같을 수도 있지만 응답이 같더라도 서로 다른 데이터기에 POST API는 멱등하지 않다.
- 또한 GET은 데이터를 읽어오기만 하여 안전성이 있었지만 POST의 경우는 데이터를 추가하며 조작하기에 안전성은 없다.
- Path Variable을 사용가능하다.
- Data Body를 사용 가능하다.



#### 사용되는 Annotation의 종류

- **@RestController**
  해당 annotation을 추가해주면 해당 class는 REST API를 처리하는 controller로 등록이 된다.
- **@RequestMapping(path)**
  리소스를 설정하는 코드이며 괄호안에 입력하는 값에 따라 URI가 localhost:8080/path로 설정된다.
- **@PutMapping(path)**
  POST API를 해당 URI로 mapping시켜준다.
- **@RequestBody**
  Request body부분을 Parsing해준다.
- **@PathVariable**
  변화하는 구간에 사용하는 annotation이며 URL path를 parsing해준다.
- **@JsonProperty**
  지정한 변수에 대해 Snake case와 Camel case의 변수를 mapping해준다.
- **@JsonNaming**
  해당 Class 내에 있는 모든 변수에 대해 Snake case와 Camel case의 변수를 mapping해준다.



### PUT API 사용하기

---

- 반환값 없이 받기만 하는 코드

```java
    @PutMapping("/put1")
    public void put1(@RequestBody PostRequestDto requestDto){
        System.out.println(requestDto);
    }
```

- 반환값으로 받은 데이터를 다시 json으로 보내는 코드

```java
    @PutMapping("/put2")
    public PostRequestDto put2(@RequestBody PostRequestDto requestDto){
        System.out.println(requestDto);
        return requestDto;
    }
```

- pathVariable를 추가한 예시

```java
    @PutMapping("/put3/{userId}")
    public PostRequestDto put3(@RequestBody PostRequestDto requestDto, @PathVariable(name="userId") Long id){
        System.out.println(requestDto);
        return requestDto;
    }
```



</br>

# DELETE 시작

- 리소스를 삭제하는 작업을 한다.

- CRUD에서 D에 해당한다.

- 하나의 리소스에 대해 같은 작업을 반복하여도 리소스의 결과는 삭제되었다는 상태로 동일하기에 멱등하다.

- 리소스를 삭제하는 작업을 하기에 안정성은 없다.

- Path Variable을 사용가능하다.

- Query Parameter도 사용가능하다.

  

### 사용되는 Annotation의 종류

- **@RestController**
  해당 annotation을 추가해주면 해당 class는 REST API를 처리하는 controller로 등록이 된다.

- **@RequestMapping(path)**
  리소스를 설정하는 코드이며 괄호안에 입력하는 값에 따라 URI가 localhost:8080/path로 설정된다.

- **@PathVariable**
  변화하는 구간에 사용하는 annotation이며 URL path를 parsing해준다.

- **@RequestParam**
  URL에 Query문을 추가할 때 parameter를 parshing해준다.

  

### DELETE API사용하기

DELETE API는 GET API와 같이 **@RequestParam**을 사용하여 parameter를 받고 해당 리소스를 삭제하는 작업을 한다.

```java
@RestController
@RequestMapping("/api")
public class DeleteApiController {

    @DeleteMapping("/delete/{userId}")
    public void delete(@PathVariable String userId, @RequestParam String account) {
        
        System.out.println(userId);
        System.out.println(account);
    }
}
```



</br>

#### 마치며

---

스프링 부트로 실제 작동하는 기본적인 애플리케이션을 확인해 보았다. 대부분의 애플리케이션은 프론트 UI를 통해 백엔드 클라우드 리소스를 사용자에게 보여준다. 여기서는 다양한 방식으로 유용하게 쓰이는 REST API를 만들고 발전시켜봤다. 거의 모든 시스템의 중심 기능인 리소스 생성, 읽기, 갱신, 삭제와 같은 CRUD를 일관된 방식으로 할 수 있다.

**@RequestMapping** 어노테이션과 HTTP 메서드와 일치하는 다양하고 편리한 어노테이션을 살펴 보았다.

- @GetMapping
- @PostMapping
- @PutMapping
- @PatchMapping
- @DeleteMapping

어노테이션을 사용한 메서드를 만든 후 코드를 리팩토링해 간소화했고, 필요한 경우에는 HTTP 상태 코드를 반환했다. HTTPpie로 API를 테스트해 정상 동작을 확인했다.
