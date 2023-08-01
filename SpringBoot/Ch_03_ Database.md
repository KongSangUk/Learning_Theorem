# 데이터베이스 액세스



</br>

## DB 엑세스를 위한 자동 설정 프라이밍

스프링 부트는 개잘바가 계속해서 반복적으로 수행하는 코드와 사용 패턴의 80 ~ 90%를 최대한 단순화하는 것을 목표로 한다. 사용 패턴을 식별하면, 저적한 기본 구성을 사용해 필요한 빈을 자동으로 초기화한다. 간단한 사용자 맞춤 기능으로 사용 패턴에 따라 여러 속성값을 제공하거나 하나 이사의 맞춤형 빈을 제공하는 기능 등이 있다. 만약 자동 설정이 기존에 식별된 것이 아닌 새로운 변경 사항을 감지하면, 개발자의 지시에 따라 이후 과정을 처리한다. DB 엑세스는 자동 설정 프라이밍의 완벽한 예시이다.



### 앞으로 얻게 될 것

ArrayList로 목록을 저장하고 관리가 가능하다. 이 접근 방식은 간단하며 단일 애플리케이션 개발용으로는 충분하지만 단점이 있다.

**첫째**, 회복탄력성이 떨어진다. 애플리케이션 또는 애플리케이션이 실행 중인 플래폿에 장애가 발생하면 애플리케이션이 실행되는 동안 수행된 변경 내용이 모두 사라진다.

**둘째**, 애플리케이션 규모를 확장하기 어렵다. 사용자가 많아져서 애플리케이션을 확장하기 위해 추가로 인스턴스를 만들어 사용하면, 새로 생긴 인스턴스는 해당 인스턴스만의 고유한 목록을 가진다. 인스턴스 간에 데이터가 공유되지 않으므로, 새로운 생성, 수정, 삭제 등의 변경 사항을 다른 인스턴스에 접속하는 사용자는 확인을 못한다.

이렇듯 복잡한 방식으로는 애플리케이션을 제대로 운영할 수 없다.

이제 운영을 수월하게 할 데이터베이스를 확인해 보자.



</br>

## 데이터베이스 시작



- In-Memory 데이터 베이스(H2) 사용하기
- MariaDB 연결하기
- Spring-Data-JPA 사용하기



### In-Memory 데이터 베이스 사용하기

> - 스프링 부트에서 지원하는 In-Memory 데이터베이스 : H2, HSQL, Derby
> - 그 중 H2 의존성 추가해서 사용해보기



#### 1. 부트 프로젝트 파일 설정

##### »> 의존성 추가하기

[pom.xml] : Spring-boot-starter-jdbc 의존성 추가 => 설정이 필요한 bean 자동 설정

```
<dependency>
 <groupId>com.h2database</groupId>
 <artifactId>h2</artifactId>
 <scope>runtime</scope>
</dependency>
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```



##### »> 코드 작성

[DBRunner.java] : Connectionr과 DataSource로 연결정보 받아오기

```
@Component
@Order(1)
public class DBRunner implements ApplicationRunner{

	@Autowired
	private DataSource dataSource;

	private Logger logger = LoggerFactory.getLogger(DBRunner.class);


	@Override
	public void run(ApplicationArguments args) throws Exception {

		logger.info("=====>> connection ===========");
		Connection connection = dataSource.getConnection();
		DatabaseMetaData dbMeta = connection.getMetaData();
		logger.debug("DB URL : " + dbMeta.getURL());
		logger.debug("DB Username : " +dbMeta.getUserName());
		logger.info("=====>> end =============");
	}
}
```

[콘솔]

```
=====>> connection ===========
DB URL : jdbc:h2:mem:testdb
DB Username : SA
=====>> end =============
```



### MariaDB 사용하기

> - **스프링 부트는 기본적으로 제공하는 DBCP** (DataBase Connection Pooling)
>
> 1. HikariDP ( - spring.datasource.hikrai.*)
> 2. Tomcat CP ( - spring.datasource.tomcat.*)
> 3. Commons DBCP2 ( - spring.datasource.dbcp2)
>
> - MySQL 커넥터 의존성과 MySQL Datasource 설정으로 MariaDB에 연결할 수 있음



##### »> 의존성 추가하기

[pom.xml] : MySQL 커텍터 의존성 추가

```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.13</version>
</dependency>
```



##### »> 환경 설정

[application.properties] MySQL DataSource 설정

```
#mysql 설정
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/spring_db?useUnicode=true&charaterEncoding=utf-8&useSSL=false&serverTimezone=UTC
spring.datasource.username=spring
spring.datasource.password=spring
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
```

[터미널로 Maria DB에 접속 및 데이터 추가] : MySQL Database 생성

```
mysql -u root –p
show databases;
use mysql;
create user spring@localhost identified by 'spring';
grant all on *.* to spring@localhost;
flush privileges;
exit;
mysql -u spring -p
create database spring_db;
show databases;
use spring_db;
```



### Spring-Data-JPA 사용하기

#### ORM과 JPA 란?

> - **ORM**(Object-Relational Mapping) : 관계형 DB테이블을 객체지향적으로 사용하기 위해 객체와 관계를 연결해줌 (DB테이블 자체를 객체로 사용)
> - **JPA**(Java Persistance API) : ORM 표준기술로 데이터베이스 정보를 객체 지향으로 쉽게 적용할 수 있게 도와줌
> - 그 중, 스프링부트는 Hibernate로 구현



#### Spring-Data-JPA의 특징

> - JPA를 쉽게 사용하기 위해 스프링에서 제공하는 프레임워크
> - Repository Bean을 자동생성
> - 쿼리 메소드 자동구현
> - @EnalbeJpaRepositories (스프링부트가 자동으로 설정해줌)



##### »> 의존성 추가하기

[pom.xml] : Spring-Data-JPA와 spring-boot-starter-test 의존성 추가

```
<!-- Spring Data JPA -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<!-- Spring Boot Starter Test -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <!-- scope test는 test directory안에서만 의존성이 동작한다는 뜻 -->
    <scope>test</scope>
</dependency>
```



##### »> 코드 작성

[사용 어노테이션 먼저 알기]

| 어노테이션      | 기능                                                         |
| --------------- | ------------------------------------------------------------ |
| @Entity         | - 엔티티 클래스임을 지정하며 **DB 테이블**과 매핑하는 객체를 나타냄 |
| @Id             | - 엔티티의 **기본키**를 나타내는 어노테이션입니다.           |
| @GeneratedValue | - 주 키의 값을 **자동 생성**하기 위해 명시하는 데 사용 -자동 생성 전략 : AUTO, IDENTITY, SEQUENCE, TABLE |



**앤티티(Entity)란** 데이터베이스에서 표현하려고 하는 유형, 무형의 객체로서 서로 구별되는 것을 뜻함. 이 객체들은 DB 상에서는 보통 table로서 나타남

**객체 <=> DB 매핑**

```
- 클래스(엔티티) <=> 테이블
- 객체 <=> Row (행)
- 변수 <=> Column (열)
```



[Account.java] : DB Account 테이블 생성하기

```
@Entity
public class Account {
	@Id @GeneratedValue(strategy = GenerationType.IDENTITY)
	//primary key만들기
	private Long id;

	@Column(unique = true)
	private String username;

	private String password;

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	@Override
	public String toString() {
		return "Account [id=" + id + ", username=" + username + ", password=" + password + "]";
	}
}
```



[AccountRepository.java] : Repository 인터페이스 작성

> - AccountRepository의 구현클래스를 따로 작성하지 않아도 Spring-Data-JPA가 자동적으로 해당 문자열 username에 대한 인수를 받아 자동적으로 DB Table에 매핑
> - JpaRepository<엔티티 클래스, 프라이머리 키 타입>
> - JpaRepository의 상위 객체 CrudRepository가 이미 여러 매소드를 구현해 놓기 때문에 없는것들만 여기에 저장해둠
> - 참조 : https://attacomsian.com/blog/spring-data-jpa-auditing#further-reading



**CrudRepository** 구현 메소드들:

- save(), saveAll()
- findById(), findAll(), findAllById()
- count()
- delete(), deleteById(), deleteAll()



```
public interface AccountRepository extends JpaRepository<Account, Long>{

    //findBy(column명)
    Account findByUsername(String username);
}
```

[applicaion.properties] : JPA로 데이터베이스를 자동 초기화하도록 설정



```
#SQL 설정
spring.jpa.hibernate.ddl-auto=create
spring.jpa.show-sql=true
```



[jpa 설정옵션]

| **spring.jpa.hibernate.ddl-auto=** create | create-drop | update | validate |
| ----------------------------------------- | ----------- | ------ | -------- |
|                                           |             |        |          |

**1. create**

> JPA가 DB와 상호작용할 때 기존에 있던 스키마(테이블)을 삭제하고 새로 만듬

**2. create-drop**

> JPA 종료 시점에 기존에 있었던 테이블을 삭제

**3. update**

> 기존 스키마는 유지하고, 새로운 것만 추가하고, 기존의 데이터도 유지/ 변경된 부분만 반영함 주로 개발 할 때 적합

**4.validate**

> 엔티티와 테이블이 정상 매핑 되어 있는지를 검증

**spring.jpa.show-sql=true**

> JPA가 생성한 SQL문을 보여줄 지에 대한 여부를 알려줌



[src/test/java/AppRepoTest.java] : 실제로 쿼리가 정상적으로 동작하는지 jUnit 확인

```
@RunWith(SpringRunner.class)
@SpringBootTest
public class AppRepoTest {
	@Autowired
	private AccountRepository repository;

	@Test
	public void account() throws Exception {
		System.out.println(repository.getClass().getName());
		//1. Account 객체 생성 -> 등록
		Account account = new Account();
		account.setUsername("Spring2");
		account.setPassword("1234");

		Account addAccount = repository.save(account);
		System.out.println(addAccount.getId() + " " + addAccount.getUsername());
	}
}
```



#### + ddl-auto : validate 확인해보기

[applicaion.properties]

```
#SQL 설정
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.show-sql=true
```

[Account.java] : email값 추가해보기

```
@Entity
public class Account {
	@Id @GeneratedValue(strategy = GenerationType.IDENTITY)
	//primary key만들기
	private Long id;

	@Column(unique = true)
	private String username;

	@Column
	private String password;

	@Column
	private String email;

	public String getEmail() {
		return email;
	}

    (...)
}
```

[src/test/java/AppRepoTest.java] : 다시 사용자 추가 가능한지 확인 / 코드 수정없음

[콘솔 에러] : validate로 변경하고 email추가 시, 콘솔에 에러 발생



[applicaion.properties] : 다시 update로 변경시 정상 작동

```
#SQL 설정
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```



#### + Optional 객체 사용해보기

> - **Optional 객체 반환**
> - Java8은 함수형 언어의 접근 방식에서 영감을 받아 java.util.Optional라는 새로운 클래스를 도입
> - **Optional**: “존재할 수도 있지만 않 할 수도 있는 객체”, ”null이 될 수도 있는 객체” 를 감싸고 있는 일종의 래퍼 클래스
> - 명시적으로 해당 변수가 null일 수도 있다는 가능성을 표현히여 불필요한 NullPointException 방어 로직을 줄일 수 있음
> - Optional에 저장된 객체의 값이 있으면 True 없으면 null(False)



#### username과 id로 테이블 조회 해보기



[AccountRepository.java]

```
public interface AccountRepository extends JpaRepository<Account, Long>{
	Account findByUsername(String username);

    //optional 추가
    // Optional<객체> : 객체는 정상적일수도, null일수도
	Optional<Account> findByEmail(String email);
}
```



[Test.java] :

```
@RunWith(SpringRunner.class)
@SpringBootTest

public class AppRepoTest {

	@Autowired
	private AccountRepository repository;

	@Test
	public void finder() {
        //table에 없는 username 불러보기
        //null값 출력
	Account account = repository.findByUsername("lambda2");
	System.out.println(account);

        //Optional에 저장된 Account객체의 값이 있으면
        // TrueOptional에 저장된 Account객체의 값이 없으면 (null) False

	Optional<Account> optional = repository.findById(1L);
	System.out.println(optional.isPresent());
	}
}
```



[Test.java] : Optional적용 후 없는 Email값 찾아보기

```
@RunWith(SpringRunner.class)
@SpringBootTest

public class AppRepoTest {
	@Autowired
	private AccountRepository repository;

	@Test
	public void finder() {
        //table에 없는 ID 불러보기
        //null값 출력
	Account account = repository.findByUsername("lambda2");
	System.out.println(account);

        //id가 1번인 account객체가 존재하는지 optional이 출력
        //true
	Optional<Account> optional = repository.findById(1L);
	System.out.println(optional.isPresent());


        // <요청한 아이디가 있으면 Account객체 반환, 없으면 예외 발생하도록 설정>

        // optional 설정 후 받아오기
	if(optional.isPresent()) {
	    Account account2 = optional.get();
	    System.out.println(account2);
        //콘솔 Print
        // optional.get() : Account [id=1, username=Spring, password=1234]
	}

        //없는 이메일 주소로 찾아보기
	Optional<Account> optEmail = repository.findByEmail("lambda1@gmail.com");

	//Supplier 함수형 인터페이스추상 메소드 T get()사용
	Account account3 = optEmail.orElseThrow(()-> new RuntimeException("요청한 이메일 주소를 가진 Account가 없습니다. "));
	System.out.println(account3);
	}
}
```



#### Eamil값 업데이트 해보기

[test.java] : 업데이트 메소드 추가 후 실행

```
@Test
public void update() {
    Optional<Account> optFindById = repository.findById(2L);

    //요청한 아이디와 일치하는 객치가 있을 경우
    if(optFindById.isPresent()) {
	Account account = optFindById.get();
	account.setEmail("changed@email.com");
	repository.save(account);
	}
}
```

[콘솔 출력] : Email 값이 null=> changed@email.com으로 변경





---

## 설정

출처 : https://wikidocs.net/161164



## ORM

SQL 쿼리와 ORM을 비교해 보자. 다음과 같은 형태로 구성된 질문 테이블에 데이터를 입력한다고 가정해 보자.

*[question 테이블 구성 예]*

| id   | subject       | content              |
| :--- | :------------ | :------------------- |
| 1    | 안녕하세요    | 가입 인사드립니다 ^^ |
| 2    | 질문 있습니다 | ORM이 궁금합니다     |
| ...  | ...           | ...                  |

> 표에서 id는 각 데이터를 구분하는 고윳값이다. 데이터베이스의 설정을 통해 값이 자동으로 증가되어 저장되도록 할수 있다.

이렇게 구성된 question 테이블에 새로운 데이터를 삽입하는 쿼리는 보통 다음처럼 작성한다.

```sql
insert into question (subject, content) values ('안녕하세요', '가입 인사드립니다 ^^');
insert into question (subject, content) values ('질문 있습니다', 'ORM이 궁금합니다');
```

하지만 ORM을 사용하면 쿼리 대신 자바 코드로 다음처럼 작성할 수 있다.

> 다음코드는 작성할 필요없이 눈으로만 확인하자.

```java
Question q1 = new Question();
q1.setSubject("안녕하세요");
q1.setContent("가입 인사드립니다 ^^");
this.questionRepository.save(q1);

Question q2 = new Question();
q2.setSubject("질문 있습니다");
q2.setContent("ORM이 궁금합니다");
this.questionRepository.save(q2);
```

> 위와 같이 ORM을 이용한 데이터의 삽입 예제는 코드 자체만 놓고 보면 양이 많아 보이지만 별도의 SQL 문법을 배우지 않아도 된다는 장점이 있다.

코드에서 Question은 자바 클래스이며, 이처럼 데이터를 관리하는 데 사용하는 ORM 클래스를 엔티티(Entity)라고 한다. ORM을 사용하면 내부에서 SQL 쿼리를 자동으로 생성해 주므로 직접 작성하지 않아도 된다. 즉, 자바만 알아도 데이터베이스에 질의할 수 있다.



점프 투 스프링부트

**ORM의 장점을 더 알아보자**



ORM을 이용하면 데이터베이스 종류에 상관 없이 일관된 코드를 유지할 수 있어서 프로그램을 유지·보수하기가 편리하다. 또한 내부에서 안전한 SQL 쿼리를 자동으로 생성해 주므로 개발자가 달라도 통일된 쿼리를 작성할 수 있고 오류 발생률도 줄일 수 있다.



## JPA 란?

스프링부트는 JPA(Java Persistence API)를 사용하여 데이터베이스를 처리한다. JPA는 자바 진영에서 ORM(Object-Relational Mapping)의 기술 표준으로 사용하는 인터페이스의 모음이다.

> JPA는 인터페이스이다. 따라서 인터페이스를 구현하는 실제 클래스가 필요하다. JPA를 구현한 대표적인 실제 클래스에는 하이버네이트(Hibernate)가 있다. SBB도 JPA + 하이버네이트 조합을 사용한다.

## H2 데이터베이스

JPA를 사용하기 전에 데이터를 저장할 데이터베이스를 설치해 보자. 개발시에는 Oracle, MSSQL 등의 굵직한 데이터베이스 보다는 설치도 쉽고 사용도 편리한 H2 데이터베이스를 많이 사용한다.



점프 투 스프링부트

**H2 데이터베이스**



H2 데이터베이스는 주로 개발용이나 소규모 프로젝트에서 사용되는 파일 기반의 경량 데이터베이스이다. 개발시에는 H2를 사용하여 빠르게 개발하고 실제 운영시스템은 좀 더 규모있는 DB를 사용하는 것이 일반적인 개발 패턴이다.



다음과 같이 H2 데이터베이스를 설치하자.

```
[파일명: /sbb/build.gradle]
(... 생략 ...)

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    runtimeOnly 'com.h2database:h2'
}

(... 생략 ...)
```

그리고 "Refresh Gradle Project"를 실행하여 필요한 라이브러리를 설치하자.



점프 투 스프링부트

**runtimeOnly**



build.gradle 파일의 runtimeOnly는 해당 라이브러리가 런타임(Runtime)시에만 필요한 경우에 사용한다. 컴파일(Compile)시에만 필요한 경우에는 runtimeOnly 대신 compileOnly를 사용한다.



설치한 H2 데이터베이스를 사용하기 위해서는 설정을 해야 한다. 다음과 같이 `application.properties` 파일을 수정하자.

> 현재 application.properties 파일에는 아무런 내용이 없을 것이다.

```
[파일명: /sbb/src/main/resources/application.properties]
# DATABASE
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
spring.datasource.url=jdbc:h2:~/local
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
```

각각의 항목에 대해서 알아보자.

- spring.h2.console.enabled - H2 콘솔의 접속을 허용할지의 여부이다. true로 설정한다.
- spring.h2.console.path - 콘솔 접속을 위한 URL 경로이다.
- spring.datasource.url - 데이터베이스 접속을 위한 경로이다.
- spring.datasource.driverClassName - 데이터베이스 접속시 사용하는 드라이버이다.
- spring.datasource.username - 데이터베이스의 사용자명이다. (사용자명은 기본 값인 sa로 설정한다.)
- spring.datasource.password - 데이터베이스의 패스워드이다. 로컬 개발 용도로만 사용하기 때문에 패스워드를 설정하지 않았다.

그리고 `spring.datasource.url`에 설정한 경로에 해당하는 데이터베이스 파일을 만들어야 한다. 위에서 `spring.datasource.url`을 `jdbc:h2:~/local` 로 설정했기 때문에 사용자의 홈디렉터리(`~` 에 해당하는 경로) 밑에 `local.mv.db` 라는 파일을 생성해야 한다. 만약 `jdbc:h2:~/test`라고 설정했다면 `test.mv.db` 라는 파일을 생성해야 한다.

사용자의 홈디렉터리는 윈도우의 경우에는 `C:\Users\(사용자명)` 이고 맥OS의 경우에는 `/Users/(사용자명)` 이다. 본인이 사용하는 OS에 맞는 홈디렉터리에 `local.mv.db` 파일을 생성하자. 파일은 내용 없이 빈파일로 생성한다.



점프 투 스프링부트

**맥 OS에서 local.mv.db 파일 생성하기**



```no-highlight
pahkey@mymac ~ % touch local.mv.db
```





여기까지 마무리 되었으면 이제 H2 콘솔을 통해 데이터베이스에 접속할 수 있다. 브라우저에서 다음의 URL 주소로 H2 콘솔에 접속해 보자.

- http://localhost:8080/h2-console

그러면 다음과 같은 H2 콘솔화면을 볼수 있다.

![img](https://wikidocs.net/images/page/161164/O_2-03_2.png)

> 한국어를 지원하기 때문에 언어 설정을 "한국어"로 설정할수 있다.

콘솔 화면에서 JDBC URL 경로를 application.properties 파일에 설정한 `jdbc:h2:~/local`로 변경하고 "연결" 버튼을 눌러보자.

![img](https://wikidocs.net/images/page/161164/C_2-03_3.png)

그러면 다음과 같이 접속된 화면을 볼수 있다.

![img](https://wikidocs.net/images/page/161164/O_2-03_4.png)

## JPA 환경설정

H2 데이터베이스를 사용할 준비가 완료되었다. 이제 자바 프로그램에서 H2 데이터베이스를 사용할 수 있게 해야한다. 자바 프로그램에서 데이터베이스에 데이터를 저장하거나 조회하려면 JPA를 사용해야 한다. 하지만 JPA를 사용하기 전에 JPA를 사용하기 위한 준비 작업이 필요하다.

> JPA를 사용한 데이터 처리는 조금 후에 자세히 알아본다.

다음처럼 build.gradle 파일을 수정하자.

```
[파일명: /sbb/build.gradle]
(... 생략 ...)

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    runtimeOnly 'com.h2database:h2'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
}

(... 생략 ...)
```

그리고 "Refresh Gradle Project"로 변경사항을 적용하면 JPA 라이브러리가 설치된다.



점프 투 스프링부트

**implementation**



build.gradle 파일의 implementation은 해당 라이브러리 설치를 위해 일반적으로 사용하는 설정이다. implementation은 해당 라이브러리가 변경되더라도 이 라이브러리와 연관된 모든 모듈들을 컴파일하지 않고 직접 관련이 있는 모듈들만 컴파일하기 때문에 rebuild 속도가 빠르다.



그리고 JPA 설정을 위해 application.properties 파일을 수정하자.

```
[파일명: /sbb/src/main/resources/application.properties]
# DATABASE
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
spring.datasource.url=jdbc:h2:~/local
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

# JPA
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update
```

추가한 항목을 간단하게 살펴보자.

- spring.jpa.properties.hibernate.dialect - 데이터베이스 엔진 종류를 설정한다.
- spring.jpa.hibernate.ddl-auto - 엔티티를 기준으로 테이블을 생성하는 규칙을 정의한다.



점프 투 스프링부트

**spring.jpa.hibernate.ddl-auto**



위 설정에서 spring.jpa.hibernate.ddl-auto를 update로 설정했다. update와 같은 설정값에 대해서 간단히 알아보자.

- none - 엔티티가 변경되더라도 데이터베이스를 변경하지 않는다.
- update - 엔티티의 변경된 부분만 적용한다.
- validate - 변경사항이 있는지 검사만 한다.
- create - 스프링부트 서버가 시작될때 모두 drop하고 다시 생성한다.
- create-drop - create와 동일하다. 하지만 종료시에도 모두 drop 한다.

개발 환경에서는 보통 update 모드를 사용하고 운영환경에서는 none 또는 validate 모드를 사용한다