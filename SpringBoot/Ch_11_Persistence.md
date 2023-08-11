# **스프링 서버 Persistence, ORM, JPA, Hibernate, JDBC 이란**



스프링이나 스프링 부트를 사용하면서 프로그래밍을 하면 **Persistence(영속성), ORM, JPA, Hibernate, JDBC** 에 대한 이야기를 많이 들을 수 있다. 이러한 개념들에 대하여 간단하게 이해할 수 있게 한 포스팅에서 정리해보았다.

![image](https://melonicedlatte.com/assets/images/2022/2023-01-22-17-19-39.png)



## 1. Persistence(영속성)

- 프로그램이 데이터를 생성하고 종료되었을 때, 메모리에만 가지고 있으면 데이터가 보존되지 않다.
- **데이터를 생성한 프로그램이 종료되더라도 데이터가 사라지지 않는 특성**을 `Persistence(영속성)`이라고 한다.
- 대부분의 프로그램들은 지속적으로 데이터를 보유하고 있어야 되는 필요성이 있어, Persistence(영속성)을 가지려고 한다.
- 데이터는 **파일, RDBMS(관계형 테이터베이스), 비관게형 데이터베이스** 등에 저장하여 Persistence(영속성)을 유지할 수 있다.



## 2. ORM (Object Relational Mappping)

- 프로그래밍 언어에서는 주로 Object(객체)를 사용하고, 데이터베이스에서는 하나의 데이터를 Table(테이블) 내부에서 로우(row, 행) 단위로 사용한다.
- 데이터베이스의 데이터를 읽어와서 객체에 넣어주, 프로그래밍을 하면서 손쉽게 처리할 수 있다.
- `ORM (Object Relational Mappping)`은 **Java Object(객체)와 데이터베이스의 Table 데이터를 자동으로 매핑(연결)**해주는 것을 말한다.
- java를 사용하여 프로그래밍을 하는 중이라고 가정해 보겠다. 아래와 같은 table 이 있을 때, 데이터는 id, name, address 컬럼 3개가 존재하고 타입도 java에서 사용하는 타입과 다르게 integer, varchar를 사용합니다. java에서 사용하기 위해서는 java 에서 사용하는 형태의 객체를 만들어서 데이터를 넣어주는 것이 데이터를 효율적으로 다룰 수 있는 방법이다.

![image](https://melonicedlatte.com/assets/images/2022/2023-01-22-17-01-03.png)

```
class User{
  int id;
  String name;
  String address;
}
```



## 3. JPA(Java Persistence API)

- ORM은 객체와 데이터베이스의 연관입니다. JPA는 자바에서 사용하는 ORM이라고 할 수 있다.
- **JPA(Java Persistence API)는 자바 Application 에서 RDBMS(관계형 테이터베이스)를 사용하는 방식을 정의한 인터페이스** 이다.
- JPA는 라이브러리가 아닌 인터페이스. JPA를 구현하는 구현체는 따로 존재한다.



## 4. Hibernate

- JPA 인터페이스를 구현하는 실제 구현체는 매우 여러 종류가 있다. **구현체 중에서 가장 많이 사용하는 것이 Hibernate 이다.**
- Hibernate를 사용하면 직접 SQL을 호출하지 않아도, Java에서 메소드를 호출하는 것만으로 DB에 데이터를 CRUD 할 수 있다.
- JPA/Hibernate 는 아래와 같이 손쉽게 사용할 수 있습니다. UserRepository의 메소드를 호출하는 것만으로 쿼리를 호출하지 않고도 데이터베이스를 접근할 수 있다.

```
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    User findById(String name);
    List<User> findByName(String name);
}
```



## 5. JDBC

- 자바에서 DB 프로그래밍을 하기 위해 사용되는 API 이다.
- 각 데이터베이스에 맞는 JDBC 드라이버를 사용하여, 데이터베이스와 연결하고 영속성 데이터를 저장할 수 있다.
- JDBC 내부적으로는 아래와 같은 과정을 수행한다.
  - JDBC 드라이버 로드 -> DB 연결 -> DB 데이터 CRUD -> DB 연결 종료

------



**reference**

- https://docs.jboss.org/hibernate/orm/6.1/userguide/html_single/Hibernate_User_Guide.html