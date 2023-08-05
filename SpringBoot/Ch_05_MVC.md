![post-thumbnail](https://velog.velcdn.com/images/h220101/post/3760a982-6996-41bd-905d-144b53354103/image.png)

## 🍃스프링부트

> 스프링 프레임워크를 기반으로 한 개발 플랫폼
> 단독실행이 가능한 스프링 어플리캐이션 생성
> 프로젝트 환경을 구출 할 때 필요한 톰캣, 제티, 언더토우 내장
> XML 기반설정이나 코드없이 환경설정을 자동화 기능
> 스프링 프레임워크 개발 접근성 용이



### 스프링 프레임워크 개발단계

1. 사용할 스프링 MVC, JPA, 하이버네이트의 버전을 결정한다.
2. 모든 다른 레이어를 연결해주는 스프링 콘텍스트를 설정한다.
3. 스프링 MVC로 웹 레이어를 설정한다.
4. 데이터레이어에서 하이버네이트를 설정한다.
5. 단위테이스, 트랜잭션전략, 로깅, 모니터링 방법 결정을 구현한다.
6. 웹 어플리케이션 서버에 배포하는 방법 결정 구현
   ![img](https://velog.velcdn.com/images/h220101/post/aaa01b97-2e73-4432-a731-d17ce953873f/image.png)



### 스프링 부트 구성요소

- 빌드도구 (Maven)
- 스프링 프레임워크 (5.X)
- 스프링 부트 (v2.0)
- 스프링 부트 스타터 (spring-boot-starter)



## MVC

> 소프트웨어 디자인 패턴 중 하나
> M (Model) / V (view) / C (Controller)

Model : 어플리케이션 정보, 데이터, DB 등 이다.
View : 사용자에게 보여지는 화면, UI를 말한다,
모델로부터 정보를 얻고 표시한다.
Controller : 데이터와 비즈니스 로직사이 상호동작을 관리한다.
모델과 뷰를 통제하며, MVC패턴에서 View와 Model이 직접적인 소통을 하지않도록 관리한다.



### MVC 1

![img](https://velog.velcdn.com/images/h220101/post/6ad25d01-e560-4eec-ade3-1e8da3645cbb/image.png)

JSP -> View, Controller 모두 담당
하나의 JSP로 유저 요청, 응답 처리 (구현의 난이도는 쉽다)

단순한 작은규모의 프로젝트에는 이 방법이 빠르고 쉽지만,
큰 프로젝트에서는 확실히 나눠주는 게 좋다.
JSP 하나에서 MVC가 모두 이루어지게되면 재사용성이 떨어지고,
읽기도 힘들다. = 유지보수가 힘들다.

그래서 주소요청, 화면구현 따로따로 하기위해 MVC가 나온것이다.

- 컴포넌트 기반으로 필요한 요소들을 묶어 놓았다!
  (* 자바를 사용해서 웹을 만들기 위한 기술이 핵심)



### 컴포넌트

재사용이 가능한 최소단위, 독립적인 소프트웨어 모듈 (교체가능한 부품)
컴포넌트는 인터페이스를 통해서만 접근이 가능하다.
컴포넌트 내 정보는 외부로부터 모두 숨겨진다. (정보 은닉)



### 📌 MVC 2

![img](https://velog.velcdn.com/images/h220101/post/4e72a5c3-bcb6-49a7-a9f1-0d610d6d847f/image.png)

요청을 하나의 컨트롤러(Servlet)가 먼저 받는다.
MVC 1과 다르게 역할이 분리되어있으며, M,V,C 중 수정할 부분이 생기면 그것만 꺼내어 수정하면된다. = 유지보수에 있어 장점이 있다.



## spring MVC 패턴

![img](https://velog.velcdn.com/images/h220101/post/a623a580-eae5-43ab-8388-07940c837724/image.png)
![img](https://velog.velcdn.com/images/h220101/post/d73bb10d-7769-416d-b189-b7588faac4b4/image.png)

스프링에서는 유저의 요청을 받는 DispathcerServlet이 핵심이다.
이것이 Front Controller의 역할을 맡는다.

Front Controller -> 우선 먼저 유저(클라이언트)의 모든 요청을 받고, 그 요청을 분석하여 세부 컨트롤러들에게 필요한 작업을 나눠주게 된다.



1. 요청
   브라우저 특정 URL에 요청을 DispathcerServlet이 받는다.



2. 요청에 매핑되는 컨트롤러를 검색요청

- DispathcerServlet이 URI를 보고 controller를 식별하기위해
  핸들러 매핑과 통신한다.
- 핸들러 매핑은 요청을 처리하는 특정 핸들러메서드를 반환(return)한다.



3. 컨트롤러에 처리요청

- DispathcerServlet은 특정 핸들러 메서드를 호출한다.
  (public String 특정메서드 (Model model)호출)
- 핸들러 메서드는 모델과 뷰를 반환(return)한다.
  (public String 특정메서드 (Model model) {return "view";})



4. 컨트롤러의 처리결과를 생성할 뷰를 결정한다.

- DispathcerServlet은 논리적 뷰를 결정하는 뷰 리졸버를 찾아 호출한다. -> 논리적 뷰 이름을 입력한다.
- 뷰 리졸버는 논리적 뷰 이름 -> 물리적 뷰 이름에 매핑하는 로직을 실행한다. (/template/view.html 으로 변환)
- 예를들어,
  return "/hongView"; 를 아래와 같이 변경한다.
  src/main/resources/template/+return(논리이름)+.html



5. 

- DispathcerServlet은 뷰를 요청해 실행한다.
  -> 뷰에서 model 객체를 사용할 수 있게 한다.
- DispathcerServlet은 응답을 다시 브라우저로 보낸다.
  -> 결과화면 return



## Servlet?

java언어로 웹을 구현하는 기술이며,
클라이언트의 요청에 제일 먼저 반응한다.
(주소요청을 받고 응답해주는 역할이다.)

ex) 브라우저에서 사용자가 입력하고, 전송하면 다시 결과값을 돌려주는데 여기가 서블릿이라 생각하면된다.

MVC 에서는 model(비즈니스로직), view(화면), controller(주소분기) 중 controller의 역할을 담당한다.



## 🍃Spring Tools 4.2.1 설치

------

1. C 드라이브 sts4.zip 압축풀기

![img](https://velog.velcdn.com/images/h220101/post/f257da75-0d3c-4276-9b17-99fc06219bb0/image.png)



2. D 드라이브 springboot 폴더 내 workspace 로 선택하기



3. 인코딩 설정하기
   ![img](https://velog.velcdn.com/images/h220101/post/9716c0d2-82ce-487d-9c96-efa3c91f3ae2/image.png)
   -스프링 프로퍼티 -> 어플리케이션 프로퍼티 + add (*properties)
   -UTF-8
   -텍스트 -> UTF-8
   -나머지 workspace/웹/xml = UTF-8 바꿔주기



4. new spring starter project
   ![img](https://velog.velcdn.com/images/h220101/post/0ac48275-0da7-4ef6-9ad2-d340617d2ade/image.png)
   ✏️패키지가 가장 중요하다. (그룹.네임)을 무조건 지켜줘야한다.
   다시 수정하면 나중에 못찾을 수 있다!

-원래 그룹이름을 도메인의 역순으로 적지만, 길어지니 지금은 짧게hong까지만 한 것임!
-Type : Maven
-pakaging : War
-Java Version : 8
-lang : java



5. 라이브러리 선택
   ![img](https://velog.velcdn.com/images/h220101/post/0ead5477-bbae-4770-a1aa-91287f0b1afc/image.png)
   -spring boot devtools
   -thymeleaf
   -spring web

![img](https://velog.velcdn.com/images/h220101/post/d8e3969d-fd9d-4c28-bb3f-4166c40e4062/image.png)
스냅샷 X (안정화 버전으로 택)





### 어플리케이션 프로퍼티 : 프로젝트 구성 시 모든 설정을 구성한다.

![img](https://velog.velcdn.com/images/h220101/post/acbc136f-c09c-4203-a7a2-9a04b8aed84d/image.png)

![img](https://velog.velcdn.com/images/h220101/post/e495ca42-03db-4edd-acb8-270cc68ca9ab/image.png)



### controller 패키지 생성

- 새로운 패키지를 만들 때, 최초 생성했던 규칙은 변경하지 않는다.
  @Controller -> 스프링 MVC의 컨트롤러 객체임을 명시하는 어노테이션

![img](https://velog.velcdn.com/images/h220101/post/4fcd1df1-35c0-4ae4-bac7-37d6fc653001/image.png)



### index.html 생성

- 템플릿 하위 html 파일을 생성한다.
  ![img](https://velog.velcdn.com/images/h220101/post/692d3242-3822-4620-a5fc-880b5aa5cb2f/image.png)



### 실행(1) (프로젝트 - Run As - 9 SpringBootApp)

- 주소창에 localhost 입력한다.
  ![img](https://velog.velcdn.com/images/h220101/post/1a686fac-e73c-4efe-817a-0ce8a4db575b/image.png)![img](https://velog.velcdn.com/images/h220101/post/a34b0bf9-8ec3-4505-80e2-2765c27b550e/image.png)

### 실행 (2)

![img](https://velog.velcdn.com/images/h220101/post/585e5980-7ae2-4e3d-afa5-2e705e213cf4/image.png)![img](https://velog.velcdn.com/images/h220101/post/bf3863c7-f912-4b3d-aae6-4142f7ce3d57/image.png)

![img](https://velog.velcdn.com/images/h220101/post/d4ffa80e-3b78-4532-a746-0d750ec11d71/image.png)
멤버에 대한 멤버컨트롤러 생성,
localhost -> /hongView 직접 입력 후 받아온 값 확인

------

### MAVEN

> 자바 프로젝트의 빌드를 자동화 해주는 빌드 툴 + 라이브러리 관리

![img](https://velog.velcdn.com/images/h220101/post/1baeb700-3a02-4a1f-bb8a-ee4f24311f2d/image.png)

### pom.xml

![img](https://velog.velcdn.com/images/h220101/post/02f66a14-f310-47b8-90b6-e9bb822bc7f5/image.png)![img](https://velog.velcdn.com/images/h220101/post/9a5ac8c2-a3dd-4232-9268-8d91f1371cb5/image.png)![img](https://velog.velcdn.com/images/h220101/post/1e337aed-b710-43a6-bbce-4e559c67e757/image.png)![img](https://velog.velcdn.com/images/h220101/post/4714b6a4-7f46-445b-bbab-3cb14e672104/image.png)

pom.xml-> maven 에 대한 설정정보가 들어가있다.
프로젝트 정보가 표시되며, 스프링에서 사용되는 여러 라이브러리를 설정하여 다운로드가 가능하다.

라이브러리는 추가하다가 빨간색이 뜨면
오른쪽키 - Maven- add dependency 해도 되긴하는데
프로젝트에서 오른쪽키 - 업데이트가 더 낫다. (Alt+F5)

만약에 그것도 안되면
C 폴더 - 사용자 - repository 폴더까지 들어가서
aopall~ 여기 폴더에서 삭제를 해준다.
하지만 최후의 방법인 것으로, 업데이트가 더 빠르다!

### src/main/java

자바 소스파일이 위치한다.

### src/main/resource

프로퍼티 파일이나 xml 파일 등 리소스 파일이 위치한다.

### src/main/webapp

WEB_INF 등 웹 어플리케이션 리소스 파일이 위치한다.

### src/test/java

Junit 등 테스트 파일이 위치한다.

### src/resource

테스트 시에 필요한 resource 파일이 위치한다.