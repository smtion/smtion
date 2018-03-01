---
layout: single
title:  Aurora RDS의 장점
date:   2018-03-01 22:38:00 +0900
tags: AWS RDS Aurora
---

RDS 사용 시 MySQL, PostgreSQL 등에 비해 Aurora를 쓰면 무엇이 좋을까?

## Database on EC2 vs RDS

기본적으로 RDS가 사용하는 인스턴스의 종류는 EC2와 다르다

| RDS | EC2 |
|---
| t, r | t, m, c, ...|
|---

r타입의 인스턴스는 RDS에서만 사용되는 Database 최적화 타입이다.
동일한 인스턴스 크기(e.g. large)의 경우 RDS의 인스턴스가 EC2보다 비용이 비싸다.

- (vCPU, 네트워크 성능 등 동일한 전제) r타입의 메모리가 m, c등의 EC2 인스턴스 타입보다 크다.
- Database 운영을 위한 관리, 유지보수 기능들이 포함되어 있다.

SE나 DBA가 없이 개발자로만 이루어진 소규모 팀이라면 RDS를 선택하는 것을 추천한다.
그래고 비용이 걱정되서 EC2를 쓴다면 어쩔 수 없지만 필자는 말리고 싶다.
관리 능력과 편리함 비용 차이를 가볍게 넘어 선다.
그리고 RDS 사용 시 MySQL 등의 DBMS가 아니라 **Aurora를 사용하면 비용적인 이점** 이 있다. (물론 비용 이외의 이점도 있다.)

### RDS의 단점

- 제한된 운영
  - super privilege가 없다.
  - 대신 RDS에서 제공하는 procedure로 제한적 사용이 가능

그러나 서비스를 운영하면서 별다른 어려움은 없었기 때문에 RDS가 가지는 단점은 크게 작용하진 않는다.

### MySQL vs Aurora

Aurora는 AWS에서 기존 DBMS를 자체적으로 개량한 제품이라고 볼 수 있다.
AWS에서 말하는 MySQL 대비 **장점만** 요약하면 아래와 같다.

- Replica의 수 : 5 -> **15**
- Replication latency : 초 -> **밀리초**
  - 솔직히 초단위라고 표시한 것은 좀 과하다...수십~수백 밀리초 정도
  - Aurora의 replication lag은 경험상 20ms 이하였다. (평균 15ms 정도) **정말 빠르다!**
- Failover 시 데이터 손실 : 몇 분의 손실 가능 -> **손실**
  - RDS의 MySQL은 5분 단위로 스냅샷을 저장

RDS가 아닌 EC2에 DBMS를 운영하면 여러 장점들이 있지만, 대부분의 소규모 팀은 그 장점을 활용할 수 없으므로 이 부분은 생략한다.

그러나 더 중요한 장점이 하나 있다.

서비스 운영 시 Master-Slave 구조로 보통 이중화를 하게 된다. MySQL의 경우 Slave는 **stand-by** 상태로 존재한다.
그러나 Aurora는 **Slave임과 동시에 Replica가** 된다. (엄밀히 말하면 Aurora의 **모든 Replica는 Slave가** 된다.) 즉 stand-by가 아니라 **active** 상태로 존재하는 것이 큰 차이다.
유휴 중인 인스턴스가 없으므로 MySQL로 이중화+복제본 구조로 운영할 때 보다 인스턴스를 효율적으로 사용하게 되므로 비용에서도 절감이 된다.

{% capture aurora56 %}
MySQL 5.6버전 베이스의 Aurora는 서비스 운영 시 Alter와 같은 DDL 시 서비스에 문제를 야기할 수 있다.

- Altering이 아닌 copy to temp table의 경우 해당 테이블의 write lock이 발생
- Alter 시 lock을 회피하기 위한 쿼리 불가 (5.7에서 해당 내용 추가됨)
- Write가 빈번한 테이블의 경우 서비스 안정성을 위해 번거로운 작업 필요
{% endcapture %}
<div>{{ aurora56 | markdownify }}</div>
{: .notice--warning}

현재 Aurora를 MySQL 5.7버전 베이스로 생성할 수 있다.
{: .notice--info}

## 결론

실제 서비스에서 운영해보니 개발자만 있는 팀이면 Aurora가 좋더라.
