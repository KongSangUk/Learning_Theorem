# Redis



![Redis란? 레디스의 기본적인 개념 (인메모리 데이터 구조 저장소)](data:image/png;base64,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)

</br>



## Redis란?

- Key, Value 구조의 비정형 데이터를 저장하고 관리하기 위한 오픈 소스 기반의 비관계형 데이터 베이스 관리 시스템 (DBMS)이다.

- 데이터베이스, 캐시, 메세지 브로커로 사용되며 인메모리 데이터 구조를 가진 저장소이다.

* db-engines.com 에서 key, value 저장소 중 가장 순위가 높다.

</br>

### Redis에 대해서 더 자세하게 알아보기 전 캐시 서버(Cache Server)에 대해

---

데이터 베이스가 있는데도 Redis라는 인메모리 데이터 구조 저장소를 사용하는 이유는 무엇일까?



데이터 베이스는 데이터를 물리 디스크에 직접 쓰기 때문에 서버에 문제가 발생하여 다운되더라도 데이터가 손실되지 않는다. 하지만 매번 디스크에 접근해야 하기 때문에 사용자가 많아질수록 부하가 많아져서 느려질 수 있다.

일반적으로 서비스 운영 초반이거나 규모가 작은, 사용자가 많지 않은 서비스의 경우에는 WEB - WAS - DB 의 구조로도 데이터 베이스에 무리가 가지 않는다.

**하지만** 사용자가 늘어난다면 데이터 베이스가 과부하 될 수 있기 때문에 이때 캐시 서버를 도입하여 사용한다. 그리고 이 캐시 서버로 이용할 수 있는 것이 바로 Redis 이다.

> 
>
> **캐시는 한번 읽어온 데이터를 임의의 공간에 저장하여 다음에 읽을 때는 빠르게 결괏값을 받을 수 있도록 도와주는 공간**이다.
>
> 

같은 요청이 여러 번 들어오는 경우 매번 데이터 베이스를 거치는 것이 아니라 캐시 서버에서 첫 번째 요청 이후 저장된 결괏값을 바로 내려주기 때문에 DB의 부하를 줄이고 서비스의 속도도 느려지지 않는 장점이 있다.

</br>

### Look aside cache 패턴과 Write Back 패턴 

---

#### Look aside cache

1. 클라이언트가 데이터를 요청
2. 웹서버는 데이터가 존재하는지 Cache 서버에 먼저 확인
3. Cache 서버에 데이터가 있으면 DB에 데이터를 조회하지 않고 Cache 서버에 있는 결과값을 클라이언트에게 바로 반환 (Cache Hit)
4. Cache 서버에 데이터가 없으면 DB에 데이터를 조회하여 Cache 서버에 저장하고 결과값을 클라이언트에게 반환 (Cache Miss)

 

#### Write Back 

1. 웹서버는 모든 데이터를 Cache 서버에 저장
2. Cache 서버에 특정 시간 동안 데이터가 저장됨
3. Cache 서버에 있는 데이터를 DB에 저장
4. DB에 저장된 Cache 서버의 데이터를 삭제

 

* insert 쿼리를 한 번씩 500번 날리는 것보다 insert 쿼리 500개를 붙여서 한 번에 날리는 것이 더 효율적이라는 원리다.

* 이 방식은 들어오는 데이터들이 저장되기 전에 메모리 공간에 머무르는데 이때 서버에 장애가 발생하여 다운된다면 데이터가 손실될 수 있다는 단점이 있다.

</br>

### Redis 특징

---

- Key, Value 구조이기 때문에 쿼리를 사용할 필요가 없다.

- 데이터를 디스크에 쓰는 구조가 아니라 메모리에서 데이터를 처리하기 때문에 속도가 빠르다.

- String, Lists, Sets, Sorted Sets, Hashes 자료 구조를 지원한다.

  - **String** </br>
    가장 일반적인 key - value 구조의 형태다.

  -  **Sets**</br>
     String의 집합이다. 여러 개의 값을 하나의 value에 넣을 수 있습니다. 포스트의 태깅 같은 곳에 사용될 수 있다.

  - **Sorted Sets** </br>
    중복된 데이터를 담지 않는 Set 구조에 정렬 Sort를 적용한 구조로 랭킹 보드 서버 같은 구현에 사용할 수 있다.

  -  **Lists** </br>
    Array 형식의 데이터 구조입니다. List를 사용하면 처음과 끝에 데이터를 넣고 빼는 건 빠르지만 중간에 데이터를 삽입하거나 삭제하는 것은 어렵다.

- **Single Threaded** 

  한 번에 하나의 명령만 처리할 수 있다. 그렇기 때문에 중간에 처리 시간이 긴 명령어가 들어오면 그 뒤에 명령어들은 모두 앞에 있는 명령어가 처리될 때까지 대기가 필요. (하지만 get, set 명령어의 경우 초당 10만 개 이상 처리할 수 있을 만큼 빠르다.)

</br>

### Redis 주의 점

---

- 서버에 장애가 발생했을 경우 그에 대한 운영 플랜이 꼭 필요.
  - 인메모리 데이터 저장소의 특성상, 서버에 장애가 발생했을 경우 데이터 유실이 발생할 수 있기 때문.
- 메모리 관리가 중요하.
- 싱글 스레드의 특성상, 한 번에 하나의 명령만 처리할 수 있습니다. 처리하는데 시간이 오래 걸리는 요청, 명령은 피해야 한다.

  

이 외에도 Master-Slave 형식의 데이터 이중화 구조에 대한 **Redis Replication**, 분산 처리를 위한 **Redis cluster,** 장애 복구 시스템 **Redis Sentinel**, **Redis Topology**, **Redis Sharding**, **Redis Failover** 등의 Redis를 더 효율적으로 사용하기 위한 개념들이 존재한다.