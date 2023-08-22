### **3줄 요약 KeyPoint**

- **데이터베이스(Database, DB)란?** : 데이터의 저장소. 
- **DBMS(Database Management System, 데이터베이스 관리 시스템)란?** 데이터베이스를 운영하고 관리하는 소프트웨어.
  - 계층형, 망형, 관계형 DBMS 중 대부분의 DBMS가 테이블로 구성된 관계형 DBMS(RDMBS)형태로 사용됨. 
- **SQL(Structured Query Language)란?** 구조화된 질의 언어라는 뜻으로 관계형 데이터베이스에서 사용되는 언어. 표준 SQL을 배우면 대부분의 DBMS를 사용할 수 있음.

 

------

 

# 데이터베이스(Database, DB)란?

데이터베이스를 한 마디로 정의하면 **데이터의 집합**이라고 할 수 있다.

데이터베이스에는 일상생활 대부분의 정보가 저장되고 관리됩니다. 오늘 보내거나 받은 카카오톡 메시지, 인스타그램에 등록한 사진, 버스/지하철에서 찍은 교통카드, 카페에서 구매한 아이스 아메리카노 등의 정보가 모두 데이터베이스에 기록된다.

 </br>

### DBMS란?

---

데이터베이스를 ‘데이터의 집합’이라고 정의한다면, 이런 **데이터베이스를 관리하고 운영하는 소프트웨어를 DBMS(Database Management System)**라고 합니다. 다양한 데이터가 저장되어 있는 데이터베이스는 여러 명의 사용자나 응용 프로그램과 공유하고 동시에 접근이 가능해야 한다.

 

가까운 예로 은행의 예금 계좌는 많은 사람들이 가지고 있습니다. 여러 명의 예금 계좌 정보를 모아 놓은 것이 데이터베이스이다. 은행이 가지고 있는 예금 계좌 데이터베이스에는 여러 명이 동시에 접근할 수 있다. 예금 계좌 주인, 은행 직원, 인터넷 뱅킹, ATM 기기 등에서 모두 접근이 가능하다 이러한 것이 가능한 이유는 바로 DBMS가 있기 때문이다.

![DBMS_개념](http://hongong.hanbit.co.kr/wp-content/uploads/2021/11/DBMS_%EA%B0%9C%EB%85%90.png)

</br> 

### DBMS의 종류

---

**DBMS와 같은 소프트웨어는 특정 목적을 처리하기 위한 프로그램** 이다. 

> 예를 들어 문서를 작성하기 위해서는 아래아한글(HWP)이나 워드(Word), 표 계산을 위해서는 엑셀(Excel)이나 캘크(Calc), 멋진 사진을 편집하려면 포토샵(PhotoShop)이나 김프(Gimp)와 같은 소프트웨어를 설치해야 한다.

마찬가지로 데이터베이스를 사용하기 위해서도 소프트웨어, 즉 DBMS를 설치해야 하는데 **대표적으로 MySQL, 오라클(Oracle), SQL 서버, MariaDB** 등이 있다. 소프트웨어 각각의 사용 방법과 특징이 다르지만 특정 목적을 위해서는 어떤 것을 사용해도 무방하다. 대표적인 DBMS의 특징이다. 



| **DBMS**   | **제작사** | **작동 운영체제**         | **기타**                                             |
| ---------- | ---------- | ------------------------- | ---------------------------------------------------- |
| MySQL      | Oracle     | Unix, Linux, Windows, Mac | 오픈 소스(무료), 상용                                |
| MariaDB    | MariaDB    | Unix, Linux, Windows      | 오픈 소스(무료), MySQL 초기 개발자들이 독립해서 만듦 |
| PostgreSQL | PostgreSQL | Unix, Linux, Windows, Mac | 오픈 소스(무료)                                      |
| Oracle     | Oracle     | Unix, Linux, Windows      | 상용 시장 점유율 1위                                 |
| SQL Server | Microsoft  | Windows                   | 주로 중/대형급 시장에서 사용                         |
| DB2        | IBM        | Unix, Linux, Windows      | 메인프레임 시장 점유율 1위                           |
| Access     | Microsoft  | Windows                   | PC용                                                 |
| SQLite     | SQLite     | Android, iOS              | 모바일 전용, 오픈 소스(무료)                         |

</br> 

------

 </br>

### DBMS의 분류

---

DBMS의 유형은 계층형(Hierarchical), 망형(Network), 관계형(Relational), 객체지향형(Object-Oriented), 객체관계형(Object-Relational) 등으로 분류된다. **현재 사용되는 DBMS 중에는 관계형 DBMS가 가장 많은 부분을 차지하며**, MySQL도 관계형 DBMS에 포함된다. 

 </br> 

 

### 계층형 DBMS

---

계층형 DBMS(Hierarchical DBMS)는 처음으로 등장한 DBMS 개념으로 1960년대에 시작되었다. 아래 그림과 같이 각 계층은 트리tree 형태를 갖는다. 사장 1명에 이사 3명이 연결되어 있는 구조이다. 계층형 DBMS의 문제는 처음 구성을 완료한 후에 이를 변경하기가 상당히 까다롭다는 것이다. 또한 다른 구성원을 찾아가는 것이 비효율적입니다. 예를 들어 재무2팀에서 회계팀으로 연결하려면 재무이사 → 사장 → 회계이사 → 회계팀과 같이 여러 단계를 거쳐야 한다. **지금은 사용하지 않는 형태이다.**

![계층형DBMS](http://hongong.hanbit.co.kr/wp-content/uploads/2021/11/%EA%B3%84%EC%B8%B5%ED%98%95DBMS.png)

 

 </br> 

### 망형 DBMS

---

망형 DBMS(Network DBMS)는 계층형 DBMS의 문제점을 개선하기 위해 1970년대에 등장했다. 다음 그림을 보면 하위에 있는 구성원끼리도 연결된 유연한 구조이다. 

> 예를 들어 재무2팀에서 바로 회계팀으로 연결이 가능하다. 하지만 망형 DBMS를 잘 활용하려면 프로그래머가 모든 구조를 이해해야만 프로그램 작성이 가능하다는 단점이 존재한다. 역**시 지금은 거의 사용하지 않는 형태이다.**

![망형DBMS](http://hongong.hanbit.co.kr/wp-content/uploads/2021/11/%EB%A7%9D%ED%98%95DBMS.png)

 

</br>  

### 관계형 DBMS

---

관계형 DBMS(Relational DBMS)는 줄여서 RDBMS라고 부른다. MySQL뿐만 아니라, **대부분의 DBMS가 RDBMS 형태로 사용** 된다. 

RDBMS의 데이터베이스는 테이블(table)이라는 최소 단위로 구성되며, 이 테이블은 하나 이상의 열(column)과 행(row)으로 이루어져 있다.

한글이나 워드에서 표를 만들었던 경험이 있을텐데요, 이 표의 모양이 바로 테이블입니다. 친구의 카카오톡 아이디, 이름, 연락처 등 3가지 정보를 표, 즉 테이블로 만들면 다음과 같다.

![sql_table](http://hongong.hanbit.co.kr/wp-content/uploads/2021/11/sql_table.png)

RDBMS에서는 모든 데이터가 테이블에 저장된다. 이 구조가 가장 기본적이고 중요한 구성이기 때문에 **RDBMS는 테이블로 이루어** 져 있으며, 테이블은 열과 행으로 구성되어 있다는 것을 파악했다면 RDBMS를 어느정도 이해했다고 할 수 있다.

 </br> 

------

 

### SQL: DBMS에서 사용하는 언어

SQL(Structured Query Language)은 관계형 데이터베이스에서 사용되는 언어로, **에스큐엘** 또는 **시퀄**로 읽는다. 관계형 DBMS 중 MySQL를 배우려면 SQL을 필수로 익혀야 한다. SQL이 데이터베이스를 조작하는 ‘언어’이긴 하지만 일반적인 프로그래밍 언어(C, 자바, 파이썬 등)와는 조금 다른 특성을 갖는다.

 

SQL은 특정 회사에서 만드는 것이 아니라 국제표준화기구에서 SQL에 대한 표준을 정해서 발표하고 있다. 이를 표준 SQL이라고 한다. 그런데 문제는 SQL을 사용하는 DBMS를 만드는 회사가 여러 곳이기 때문에 표준 SQL이 각 회사 제품의 특성을 모두 포용하지 못한다는 점이다. 그래서 DBMS를 만드는 회사에서는 되도록 표준 SQL을 준수하되, 각 제품의 특성을 반영한 SQL을 사용한다.

 

다음 그림을 보면 3가지 DBMS 제품(오라클, SQL 서버, MySQL)이 모두 표준 SQL을 포함하고 있다. 
**그래서** **표준 SQL을 익히면 대부분의 DBMS에 공통적으로 적용할 수 있다.** 각 DBMS는 추가로 자신만의 기능도 가지고 있어서 이렇게 변경된 SQL을 오라클은 PL/SQL, SQL서버는 T-SQL, MySQL은 SQL로 부른다. 

 

![DBMS 제품](http://hongong.hanbit.co.kr/wp-content/uploads/2021/11/DBMS-%EC%A0%9C%ED%92%88.png)