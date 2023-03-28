# [Oracle 데이터베이스] 용어 및 문법

- 목차

# 1. 데이터베이스와 오라클 설치

## 1) 데이터베이스란

- 데이터의 효율적인 관리와 검색을 위한 데이터 집합을 뜻한다.
- 프로그램을 화면에 출력한 것은 휘발성을 지녔기 때문에 종료하면 데이터가 사라지는 특징을 갖는다. 따라서 이를 데이터베이스에 저장하면서 다수의 사람들과 함께 데이터를 공유하면서 사용할 수 있게 된다.
- 데이터베이스 관리 시스템은 보통 DBMS(Data Base Management System) 이라하며 오라클, Mongo, MySQL 등 다양한 종류가 있다.
- 계층형, 네트워크형, 객체지향형 데이터 모델이 있지만, 현대의 DBMS는 주로 데이터를 일정 기준으로 나누어 관리하고 각 개체의 속성과 관계를 보여주는 관계형 데이터베이스 모델을 주로 사용한다.

## 2) 오라클 설치 및 환경설정

### 오라클의 자료형

| 자료형 | 설명 |
| --- | --- |
| VARCHAR2(길이) | 1~4000byte 사이의 가변 길이 문자열 데이터를 저장할 수 있다. |
| NUMBER | +- 38자릿수의 숫자를 저장할 수 있다. |
| DATE | 날짜 형식을 저장할 수 있다. 세기, 연, 월, 일, 시, 분, 초 저장이 가능하다. |
| CHAR(길이) | 1~4000byte만큼의 고정 길이 문자열 데이터를 저장할 수 있다. |
| NVARCHAR2(길이) | 1~4000byte만큼의 가변 길이 국가별 문자 세트 데이터를 저장할 수 있다. |
| BLOB | 최대 4GB의 대용량 이진 데이터를 저장할 수 있다. |
| CLOB | 최대 4GB의 대용량 텍스트 데이터를 저장할 수 있다. |
| BFILE | 최대 4GB의 대용량 이진 데이터 파일을 저장할 수 있다. |

### 오라클의 객체

| 객체 | 설명 |
| --- | --- |
| 테이블(table) | 데이터를 저장하는 장소 |
| 인덱스(index) | 테이블의 검색 효율을 높이기 위해 사용하는 것 |
| 뷰(view) | 하나 또는 여러 개의 선별된 데이터를 논리적으로 연결하여 하나의 테이블처럼 사용하게 해주는 것 |
| 시퀀스(sequence) | 일련 번호를 생성해주는 것 |
| 프로시저(procedure) | 반환 값이 없는 형태로 프로그래밍의 연산 및 기능 수행을 가능하게 하는 것 |
| 펑션(function) | 반환 값이 있는 형태로 프로그래밍의 연산 및 기능 수행을 가능하게 하는 것 |
| 트리거(trigger) | 데이터 관련 작업의 연결 및 방지 관련 기능을 제공 |

### 설치와 환경설정

[Oracle 소프트웨어 다운로드](https://www.oracle.com/kr/downloads/)

[[Oracle] Windows 10 오라클 19c 다운로드 및 설치 방법 [Windows]](https://source-factory.tistory.com/11)

- 설치 과정에서 비밀번호를 입력하는데 잘 기억해야한다. (일단 아이디 system, 비번 manager1로 설정해놓음)


- 설치 후엔 다음과 같은 명령어로 설치된 것을 확인할 수 있다.
- DB를 관리하는 클라이언트 파일은 sql developer 등을 설치하면 된다.

# 2. SQL 명령어

## 1) DML (Data Manipulation Language) 데이터 조작어

- 자료를 추가, 수정, 삭제, 조회하는 명령어

### SELECT

```sql
select [ALL | DISTINCT] 컬럼이름
from 테이블이름
[where 검색조건]
[group by 컬럼이름]
[having 검색조건]
[order by 컬럼이름 [asc(오름차순) | desc(내림차순)]]

[] : 선택적 사용 / | : 문법 중 하나만 사용 가능 
```

- select : 데이터를 조회하는 명령어.
- [ALL(*) | Distinct]
    - ALL : 해당 테이블의 모든 레코드를 중복을 제거하지 않고 조회
    - Distinct : 중복을 제거하고 데이터를 조회
- [where 검색조건] : 조건을 통해 레코드를 추릴 수 있는 기능. 다양한 조건어들이 사용된다.
    - where절에 사용되는 조건술어들
    
    | 술어 | 연산자 | 예시 | 예시 의미 |
    | --- | --- | --- | --- |
    | 비교 | =,<,>,≤,≥ | select * from book where price > 1000 | price가 1000보다 큰 book |
    | 범위 | BETWEEN <조건> AND <조건> | select * from book where price between 1000 and 2000 | price가 1000에서 2000 사이의 book |
    | 집합 | IN, NOT IN | select * from book where publisher in (’민음사’, ‘열린책들’) | publisher가 ‘민음사’ 혹은 ‘열린책들’인 book |
    | 패턴 | LIKE ‘<패턴>’ | select * from book where bookname like ‘%축구%’ | bookname에 축구가 들어가는 book |
    | NULL | IS NULL, IS NOT NULL | select * from book where bookname IS NOT NULL | bookname이 NULL이 아닌 book |
    | 복합조건 | AND, OR, NOT | select * from book where price > 1000 AND bookname like ‘%축구’ | price가 1000보다 크고, bookname의 마지막 두 글자가 ‘축구’인 book |
    - like에서 쓰이는 와일드 문자들
    
    | 와일드 문자 | 의미 |
    | --- | --- |
    | + | 문자열을 연결시키는 문자 |
    | % | ‘%문자’ 문자를 0개 이상 포함하는 문자열 |
    | [] | ‘[0-10]%’ 0~10 사이 숫자로 시작하는 문자열 |
    | [^] | ‘[^0-10]%’ 0~10 사이 숫자로 시작하지 않는 문자열 |
    | _ | 아무 한 글자. ‘_성%’ 두 번째 글자가 ‘성’인 모든 문자열 |
- 집계함수 : 컬럼별로 총합, 평균, 최대값, 최소값, 개수 등을 나타낼 수 있는 함수
    - 총합 : sum
    - 평균 : avg
    - 최대값 : max
    - 최소값 : min
    - 개수 : count
    - 순위 : rank
    
    ```sql
    1. select sum(saleprice) from orders where custid=1;
    => 고객번호가 1번인 사람이 주문한 총 금액
    2. select count(bookname) from book where publisher='이상미디어';
    => 출판사가 이상미디어인 책의 개수
    3. select max(price) from book where publisher='이상미디어';
    => 출판사가 이상미디어인 책 중 가장 비싼 금액
    4. select avg(price) from book where publisher='이상미디어';
    => 출판사가 이상미디어인 책의 평균 금액
    5. select count(*), sum(saleprice), avg(saleprice), max(saleprice), min(saleprice) from orders;
    => 주문 테이블에서의 모든 레코드 숫자, saleprice의 총액, 평균, 최대값, 최소값.
    ```
    
- GROUP BY : 데이터들을 원하는 그룹으로 나눌 수 있게 하는 명령어.

```sql
select sum(saleprice) from orders group by custid;
-> 고객 id에 따라 주문한 금액의 총액
```

- HAVING : 집계함수를 활용하여 GROUP BY에서 추린 결과물을 다시 추리는 조건 명령어.
- ORDER BY : 오름차 혹은 내림차순으로 레코드를 정렬할 때 쓰는 명령어.
    - 기준을 여러 개로 적용할 수 있다.

```sql
select custid, count(orderid), sum(saleprice) from orders where bookid between 1 and 8 group by custid having sum(saleprice)>=20000 order by count(orderid) desc;

=> 도서번호가 1번에서 8번 사이의 도서를 구매한 고객번호별로 총 주문건수와 총 주문금액을 출력하되 총 주문금액이 2만원 이상인 것만 나오게 하셈. 단, 총 주문건수가 높은 순으로 출력함.
```

### INSERT

- 테이블에 레코드를 추가하는 명령어.

```sql
insert into 테이블명(열1, 열2, 열3 ...)
values (열1의 데이터, 열2의 데이터 ...);
```

### DELETE

- 테이블의 레코드 데이터를 삭제하는 명령어.

```sql
delete 테이블명 where 조건식;

ex) delete member where name = '장원영';
```

### UPDATE

- 테이블의 레코드 데이터를 수정하는 명령어.

```sql
update 테이블명 set 변경하고자하는컬럼명 = 값 ... where 조건식;

ex) update member set phone='0000', addr='서울' where name = '장원영';
```

## 2) DCL (Data Control Language) 데이터 제어어

- 권한을 부여하거나 뺏는 명령어

### GRANT

### REVOKE

## 3) DDL (Data Definition Language) 데이터 정의어

- 테이블, 뷰, 인덱스를 생성하거나 수정하는 명령어

### CREATE

- 테이블을 생성하는 명령어.

```sql
create table 테이블명 (컬럼명1 자료형 [제약조건], 컬럼명2~);
```

- 여러가지 제약조건을 통해 컬럼의 성격을 정의할 수 있다.
    - not null : 값을 생략할 수 없다.
    - unique : 데이터 값이 유일한 값이어야 한다.
    - primary key : not null + unique 생략할 수 없고, 유일해야한다.
        - 다른 레코드와 구별하기 위해 기본키로 설정된다.
        - 테이블 당 하나만 있어야 한다.
        - 두 개의 컬럼을 합쳐서 주식별자로 설정할 수도 있다.
    - default : 값이 생략될 경우 기본값이 설정된다.
    - foreign key : 이미 있는 다른 테이블의 값을 참조하는 참조키.
        - 두 테이블 간의 관계를 설정하기 위해 사용한다.
        - 참조키는 부모 테이블의 primary key를 설정해야 한다.
        - 참조키를 설정하면 부모 테이블과 자식 테이블로 설정되고, 이 때 부모 테이블의 레코드가 없으면 자식 테이블의 레코드도 생성할 수 없다.
        - 만약 primary key가 두 컬럼으로 합쳐진 형태라면 참조키도 두 가지 모두 설정해야 한다.
        - 만약 관계 상태에 있는 테이블을 삭제할 때는 자식 테이블 먼저 삭제해야 한다.
        - 참조 관계에 있을 때 부모 테이블 레코드와 자식 테이블 레코드가 동시에 지워지게 하려면 테이블 생성시 참조키 설정 뒤에 on delete cascade를 넣으면 된다.

- 다른 테이블을 복사하여 테이블을 생성하기

```sql
create table 만들어지는테이블이름
as select * from 복사대상테이블이름;

ex)
create table dept_copy
as select * from dept;
```

### 테이블 생성 시 대표적인 오류 (개체무결성, 참조무결성)

- 개체무결성
    - 테이블 생성 시 모든 레코드가 주식별자 (primary key)에 의해 다른 레코드와 구별이 가능한 것을 뜻한다.
    - 만약 primary key에 의해 구별되지 않는 레코드가 있을 경우, ‘개체무결성에 위배된다’고 한다.
    - primary key에 중복되는 값을 넣으려하거나, null 값을 넣으려 할 때 발생한다.
- 참조무결성
    - 참조키(foreign key)가 부모 테이블의 주식별자(primary key)의 값을 참조하는 것을 뜻한다.
    - 참조키로 설정된 컬럼값이 부모 테이블에 존재하지 않을 때, ‘참조무결성에 위배된다’고 한다.

### 인덱스 (Index)

- 테이블에 존재하는 특정 컬럼에 검색 속도를 향상시키기 위해 인덱스를 생성할 수 있다.
- 레코드 수가 많을수록 더욱 효율적이다.
- 테이블 내용이 자주 변경되는 경우 비효율적이다.

```sql
1. 인덱스 생성
create index 인덱스명 on 테이블명(컬럼명1, 컬렴명2 ...); 

2. 인덱스 삭제
drop index 인덱스명;
```

### 뷰(View)

- 실제로 존재하지는 않는 가상의 테이블.
- 뷰를 활용하는 두 가지 이유 (편의성, 보안성)
    - 자주 사용하는 select문을 뷰 형태로 저장하여 간략한 코드로 불러오기 위해서.
    - 테이블에 있는 특정 데이터가 보안상 공개되서는 안 될 경우, 공개하고자 하는 컬럼만 조회하는 뷰를 만들어 이것만 볼 수 있게 할 수 있다.

```sql
1. 뷰 생성
create [or replace] [force | noforce] view 뷰이름 (컬럼이름1, 컬럼이름2 ...)
as (저장할 select문)
[with check option [제약조건]]
[with read only [제약조건]];

2. 뷰 삭제
drop view 뷰이름

3. 뷰에 대한 권한 부여하기
grant select on 뷰이름 to 사용자이름;

4. 뷰에 대한 권한 삭제하기
revoke select on 뷰이름 from 사용자이름;

```

- 뷰를 통해서도 원본 테이블에 데이터를 추가, 수정, 삭제할 수 있다.
- 뷰를 읽기 전용으로 만들고 싶다면 with read only를 붙인다.

### 시퀀스(Sequence)

- 레코드를 다른 행과 구별하기 위해 발행하는 번호.

```sql
create sequence 시퀀스명
```

### ALTER

- 새로운 컬럼을 추가하거나, 필요 없는 컬럼을 삭제하거나, 컬럼의 자료형이나 크기, 제약을 수정할 수 있다.

```sql
1. 새로운 컬럼 추가
alter table 테이블명 add 컬럼명 자료형;

2. 컬럼 삭제
alter table 테이블명 drop column 컬럼명;

3. 컬럼 자료형 수정
alter table 테이블명 modify 컬럼명 자료형;

4. 제약 만들기
alter table 테이블명 add 제약(컬럼명);

5. 참조키(foreign key) 설정
alter table 테이블명 add foreign key(컬럼명) references 부모테이블명(컬럼명);

6. 테이블 이름 변경
alter table 원래이름 rename to 바꿀이름;
```

### DROP

- 테이블 삭제할 때 사용하는 명령어.

```sql
drop talbe 테이블명;
```

## 4) Join 조인

- 두 개 이상의 테이블을 연결하여 하나의 테이블처럼 출력할 때 쓰는 방식.
- 두 개의 테이블에 공통적으로 들어가는 컬럼을 조건식으로 표현하여 하나의 테이블로 나타낼 수 있으며, 어느 테이블의 컬럼을 출력할지 구체화하여 표현해야 한다.
- 예시
    
    ```sql
    select bookname, price, saleprice, orderdate from customer,book,orders
      2  where customer.custid = orders.custid and
      3  book.bookid = orders.bookid and
      4  name = '박지성';
    
    -> 박지성 고객이 주문한 모든 도서명과 도서 가격, 구매 가격, 주문일을 출력.
    
    모든 테이블을 활용하기 위해 join 문을 쓸 수 있다. 공통된 컬럼을 연결시키면 됨.
    
    select name, address, phone, orderdate from customer, book, orders
      2  where customer.custid = orders.custid and
      3  book.bookid = orders.bookid and
      4  bookname = '축구의 이해';
    
    -> 축구의 이해를 구매한 고객의 이름, 주소, 전화, 구매일자를 출력
    
    select customer.custid, name, book.bookid, bookname, publisher, orderdate from book, customer, orders
      2  where customer.custid = orders.custid and
      3  book.bookid = orders.bookid and
      4  bookname like '%축구%'
      5  and price >=8000
      6  order by orderdate, name;
    
    -> 축구 관련 도서 중에 가격이 8천원 이상인 도서를 구매한 고객의 고객번호, 고객이름, 도서번호, 도서명, 출판사명, 구매일자를 출력. 단 최근의 구매일 순으로 출력하되 동일할 때는 고객이름 순으로 출력.
    
    select name, count(*), sum(saleprice) from customer, orders
      2  where customer.custid = orders.custid and
      3  address like '대한민국%'
      4  group by name having sum(saleprice) >=10000;
    
    -> 대한민국에 거주하는 고객 이름별로 총 주문건수, 총 주문금액을 출력하시오. 단, 총 주문금액이 10000원 이상인 것만 출력.
    
    select bookname, count(*) from book, orders
      2  where book.bookid = orders.bookid and
      3  orderdate between '2022/08/01' and '2022/08/31'
      4  group by bookname having count(*)>=2;
    
    -> 2022년 8월 1일부터 31일 사이에 판매된 도서명별로 판매건수를 출력. 단, 판매건수가 2건 이상인것만.
    
    select name, count(*), sum(saleprice) from book b, customer c, orders o
      2  where c.custid = o.custid and
      3  b.bookid = o.bookid and
      4  publisher like '%미디어'
      5  group by name having sum(saleprice)>=20000
      6  order by sum(saleprice) desc, name;
    
    -> 출판사명이 '미디어'로 끝나는 출판사의 도서를 구매한 고객이름별로 총 주문건수, 총 주문금액을 출력. 단 총 주문금액이 20000원 이상인 것을, 총 주문금액이 높은 순으로 출력하되 동일할 시 고객 이름 순으로 출력.
    ```
    

### Inner Join 이너 조인

- 일반적인 형태의 조인문을 뜻한다.
- 양 테이블의 조건을 동시에 만족하는 레코드들을 출력한다.

### Outer Join 아우터 조인

- 조인할 때 두 개의 테이블 중 하나가 조건을 만족하지 않더라도 모든 레코드를 포함시키고자 할 때 활용하는 조인문.
- 테이블을 from절을 기준으로 왼쪽과 오른쪽으로 나눠 어느 쪽의 테이블 결과를 모두 포함시킬지에 따라 left outer join과 right outer join으로 나뉜다.
    
    ```sql
    - left outer join의 예시
    
    select name, saleprice from customer c left outer join orders o on c.custid = o.custid order by name;
    
    -> 고객 이름과 구매한 책의 가격을 출력하되 책을 구매하지 않은 고객의 이름도 나오게 하기.
    
    select dname, ename from dept left outer join emp on dept.dno = emp.dno order by dname;
    
    -> 부서명과 그 부서에 근무하는 직원 이름을 출력. 부서명 순으로 출력하고 근무자가 없는 부서명도 출력하기.
    ```
    
- 만약 두 테이블 모두의 결과를 포함시키고자 한다면 full outer join을 활용한다.
- 3개의 테이블을 outer join 할 때는 다음과 같이 표현한다.
    - select 컬럼1, 컬럼2 from 테이블1 left outer join 테이블2 on 조건식
        
        left outer join 테이블3 on 조건식
        
    - select 컬럼1, 컬럼2 from 테이블1 left outer join 테이블2 on 조건식
        
        left outer join 테이블 3 on 조건식 where 조건식
        
    

### Self Join 셀프 조인

- 하나의 테이블에서 어떤 컬럼이 자신의 또 다른 컬럼 값을 참조하는 경우에 사용한다.
- 물리적으로 테이블은 하나이지만 마치 2개인 것처럼 별칭을 주는 형태로 표현하는 조인문.

```sql
select e.ename 사원명, m.ename 관리자명
2  from emp e, emp m
3  where e.mgr = m.eno;

-> 

select m.ename from emp e, emp m where e.mgr = m.eno and e.ename = '권현욱';

-> 권현욱의 관리자 직원 이름을 출력

select e.ename from emp e, emp m where e.mgr = m.eno and m.ename='권현욱';

-> 권현욱의 부하 직원 이름들을 출력.

select e.ename, e.hiredate, m.ename, m.hiredate, dname, dloc
  2  from emp e, emp m, dept where e.mgr = m.eno and e.dno = dept.dno and
  3  e.addr in ('신림동', '서교동') and m.hiredate > e.hiredate
  4  order by e.hiredate, e.ename;

-> 서교동이나 신림동에 거주하는 직원들 중에서 관리자보다 입사일이 더 빠른 직원에 대하여 사원명, 사원 입사일, 관리자명, 관리자 입사일, 부서명, 부서위치를 출력. 단 사원 입사일 순으로 출력하되 동일할 때는 사원명 순으로 출력.
```

## 5) 서브쿼리

- SQL문을 실행하는데 필요한 데이터를 추가로 조회하기 위해 SQL 내부에서 사용하는 SELECT문.
- Join 식과 비슷한 결과를 낳지만 자료의 수가 많을 때에는 서브쿼리가 더 효율적이다.
- select 혹은 from 혹은 where 뒤에 올 수 있다.
    - select 컬럼, (select~~)
    - from 테이블1, 테이블2, (select~~)
    - where 컬럼 ≥ (select ~~)

```sql
- 서브쿼리를 활용하여 가장 비싼 책의 이름을 출력

select bookname from book where price = (select max(price) from book); 
```

### 단일행 서브쿼리(single-row subquery)

- 실행 결과가 단 하나의 행으로 나오는 서브쿼리.
- >, <, = 같은 단일행 연산자를 써서 비교할 때, 서브쿼리 값이 날짜형일 때, 집계함수를 써서 비교할 때의 결과를 뜻한다.

```sql

1. 서브쿼리 값이 날짜형일 때
-> '장원영' 보다 입사일이 빠른 사람 조회하기
select * from emp where hiredate <
(select hiredate from emp where ename='장원영')

2. 집계함수를 썼을 때
-> 20번 부서에 속한 사원 중 전체 사원의 평균 급여보다 높은 급여를 받는 사원 정보와 소속 부서 정보를 함께 조회
select e.empno, e.ename, e.job, e.sal, d.deptno, d.dname, d.loc
from emp e, dept d
where e.deptno = d.deptno and e.deptno = 20
and e.sal > (select avg(sal) from emp);

```

### 다중행 서브쿼리(multiple-row subquery)

- 실행 결과 행이 여러 개로 나오는 서브쿼리.
- 다중행 연산자를 썼을 때의 결과를 뜻한다.
    
    
    | 다중행 연산자 | 설명 |
    | --- | --- |
    | IN | 메인쿼리 데이터가 서브쿼리의 결과 중 하나라도 일치하는 데이터가 있다면 TRUE |
    | ANY, SOME | 메인쿼리의 조건식을 만족하는 서브쿼리의 결과가 하나 이상이면 TRUE |
    | ALL | 메인쿼리의 조건식을 만족하는 서브쿼리의 결과 모두가 만족하면 TRUE |
    | EXISTS | 서브쿼리의 결과가 존재하면 (행이 1개 이상이면) TRUE |

```sql
1. IN 연산자를 활용한 다중행 서브쿼리
-> 부서 번호가 20이거나 30인 사원의 정보를 추출
select * from emp where deptno IN (20,30);

2. ANY 연산자를 활용한 다중행 서브쿼리
-> 각 부서별 최고 급여와 동일한 급여를 받는 사원 정보 추출
select * from emp where sal = any (select max(sal) from emp group by deptno)

3. ALL 연산자를 활용한 다중행 서브쿼리
-> 부서 번호가 30번인 사원들의 최소 급여보다 더 적은 급여를 받는 사원 출력
select * from emp where sal < ALL (select sal from emp where deptno=30);
```

### From 절에서 사용하는 서브쿼리와 WITH절

- From 절에서 사용하는 서브쿼리는 인라인 뷰(inline view)라 한다.
- 가독성을 위해 WITH을 활용하여 표현할 수도 있다.
    - WITH
        
        별칭1 as (select ~),
        
        별칭2 as (select ~)
        
        select from 별칭1, 별칭2
        

```sql
1. from 절에 서브쿼리를 활용하기
-> 부서 번호가 10번인 사원들의 empno, ename, deptno과 dname, loc을 출력하시오
select e10.empno, e10.ename, e10.deptno, d.dname, d.dloc
from (select * from emp where deptno = 10) e10,
(select * from dept) d
where e10.deptno = d.deptno;

2. 위의 sql문을 with절로 나타내기
WITH
e10 as (select * from emp where deptno = 10),
d as (select * from dept)
select e10.empno, e10.ename, e10.deptno, d.dname, d.dloc
from e10, d
where e10.deptno = d.deptno;
```

### SELECT절에서 사용하는 서브쿼리

- 스칼라 서브쿼리 (scalar subquery) 라고 함.

```sql
select empno, ename, job, sal,
(select grade from salgrade where e.sal between losal and hisal) as salgrade, 
deptno,
(select dname from dept where e.deptno = dept.deptno) as dname
from emp e;
```

## 6) 오라클 함수

### 문자 데이터를 가공하는 문자 함수

- 대소문자로 바꿔주는 UPPER, LOWER, INITCAP
    - UPPER(문자열) : 괄호 안의 데이터를 모두 대문자로 변환.
    - LOWER(문자열) : 괄호 안의 데이터를 모두 소문자로 변환.
    - INITCAP(문자열) : 괄호 안의 데이터를 첫 글자는 대문자로, 나머지는 소문자로 변환.
- 문자열 길이를 구하는 LENGTH, 문자열의 바이트 수를 구하는 LENGTHB
    - LENGTH(문자열) : 데이터가 몇 글자인지 출력.
    - LENGTHB(문자열) : 데이터가 몇 byte인지 출력.
- 문자열 일부를 추출하는 SUBSTR
    - SUBSTR(문자열 데이터, 시작위치, 추출길이) : 문자열 데이터의 시작위치부터 추출 길이만큼 추출.
    - substr(ename, 1, 5) : ename 문자열을 첫번째 글자부터 5글자 추출하기
- 문자열 데이터 안에서 특정 문자 위치를 찾는 INSTR
    - INSTR(’찾으려는 대상 문자열’, ‘찾고 싶은 특정 문자’, [위치 찾기를 시작할 문자열의 데이터 위치], [시작위치로부터 찾으려는 문자가 몇 번쨰인지 지정])
    
    ```sql
    select instr('hello, oracle!', 'l')
    => 결과값 : 3
    => 가장 처음 나오는 'l'의 위치를 반환함.
    
    select instr('hello, oracle!', 'l', 5)
    => 결과값 : 12
    => 5번째 글자인 'o'부터 탐색을 시작하여 그 다음부터 'l'의 위치를 반환함.
    
    select instr('hello, oracle!', 'l', 2, 2)
    => 결과값 : 4
    => 2번째 글자인 'e'부터 탐색을 시작하여 그로부터 'l'이 두 번째 나온 위치를 반환.
    ```
    

- 특정 문자를 다른 문자로 바꾸는 REPLACE
    - REPLACE(’문자열 데이터 혹은 컬럼이름’, ‘찾는 문자’, ‘대체할 문자’)
- 데이터의 빈 공간을 특정 문자로 채우는 LPAD, RPAD
    - LPAD(’문자열 데이터 혹은 컬럼이름’, 데이터의 자릿수, ‘빈공간을 채울 문자’)
    - RPAD(’문자열 데이터 혹은 컬럼이름’, 데이터의 자릿수, ‘빈공간을 채울 문자’)
- 두 문자열 데이터를 함치는 CONCAT
    - CONCAT(’문자열 혹은 컬럼이름’, ‘문자열 혹은 컬럼이름’)
- 특정 문자를 지우는 TRIM
    - TRIM([삭제옵션], ‘삭제할 문자’ from ‘원본 문자열’)
        - ‘삭제할문자’를 지정하지 않으면 공백을 제거한다.
    - 삭제옵션 종류
        - LEADING : 왼쪽 글자를 지우기
        - TRAILING : 오른쪽 글자를 지우기
        - BOTH : 양쪽 모두 지우기 (기본값)
    
    ```sql
    select trim(both, '_' from '_oracle_')
    => 결과값 : 'oracle'
    => both를 썼기 때문에 둘 다 지워진다.
    ```
    

### 숫자 데이터를 연산하고 수치를 조정하는 숫자 함수

- 특정 위치에서 반올림하는 ROUND
    - ROUND(숫자, 반올림 위치)
    - 반올림 위치의 기본값은 0이다. 즉 소수점 첫째자리에서 반올림한 정수값이 나온다.
- 특정 위치에서 버리는 TRUNC
    - TRUNC(숫자, 버림 위치)
- 지정한 숫자와 가장 가까운 정수를 찾는 CEIL, FLOOR
    - CEIL(숫자) : 숫자에서 가장 가까운 큰 정수
    - FLOOR(숫자) : 숫자에서 가장 가까운 작은 정수
- 숫자를 나눈 나머지 값을 구하는 MOD
    - MOD(나눗셈의 대상 숫자, 나눌 숫자)

### 날짜 데이터를 다루는 날짜 함수

- 몇 개월 이후 날짜를 구하는 ADD_MONTH
    - ADD_MONTH(날짜 데이터, 더하는 개월수)
- 두 날짜 간의 개월 수 차이를 구하는 MONTHS_BETWEEN
    - MONTHS_BETWEEN(날짜데이터1, 날짜데이터2)
- 돌아오는 요일, 달의 마지막 날짜를 구하는 NEXT_DAY, LAST_DAY
    - NEXT_DAY(날짜데이터, 요일)
    - LAST_DAY(날짜데이터)

### 자료형을 반환하는 형 변환 함수

- 날짜, 숫자 데이터를 문자 데이터로 반환하는 TO_CHAR
    - TO_CHAR(날짜데이터, ‘문자열 형태’)
    
    ```sql
    select to_char(sysdate, 'yyyy/mm/dd')
    => 현재 시간을 yyyy/mm/dd 형태로 나타내기
    ```
    
    | 문자열 형식 | 설명 |
    | --- | --- |
    | CC | 세기 |
    | YYYY | 4자리로 나타낸 연도 |
    | YY | 2자리로 나타낸 연도 |
    | MM | 월 |
    | MON | 월 이름을 영어 약자로 |
    | MONTH | 월 이름을 영어로 |
    | DD | 일 |
    | DDD | 1년 중 몇번째 날인지 |
    | DY | 요일을 영어 약자로 |
    | DAY | 요일을 영어로 |
    | W | 1년 중 몇번째 주 |
- 문자 데이터를 숫자 데이터로 변환하는 TO_NUMBER
    - TO_NUMBER(’문자열데이터’, ‘숫자형태’)
    
    ```sql
    select to_number('1300', '999,999') - select to_number('1500', '999,999')
    = -200
    ```
    

- 문자 데이터를 날짜 데이터로 변환하는 TO_DATE
    - TO_DATE(’문자열데이터’, ‘날짜형태’)
    
    ```sql
    1981년 6월 1일 이후에 입사한 사원 정보 출력
    select * from emp where hiredate > to_date('1981/06/01', 'yyyy/mm/dd')
    ```
    

### NULL 처리 함수

- NULL일 경우 특정 데이터를 반환하는 NVL
    - NVL(’NULL인지 검사할 데이터 혹은 컬럼’, ‘데이터가 NULL일 경우 반환할 데이터’)
- NULL일 경우와 아닐 경우 특정한 데이터를 반환하는 NVL2
    - NVL2(’NULL인지 검사할 데이터 혹은 컬럼’, ‘데이터가 NULL이 아닐 경우 반환할 데이터’, ‘데이터가 NULL일 경우 반환할 데이터’)

```sql
1. 급여 comm이 null일 경우 0으로 출력하기
select comm, nvl(comm,0) from emp

2. 급여 comm이 null일 경우 X으로, null이 아닐 경우 O로 출력하기
select comm, nvl2(comm, 'O', 'X') from emp
```

# 3. 데이터모델링

# 4. PL/SQL

## 1) 프로시저

## 2) 트리거

## 3) 펑션

- 오라클 설치 및 사용자 계정 만들기
    
    [사용자 계정 만들기](%5BOracle%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%5D%20%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%8B%E1%85%A5%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%2085d6da75bef94283a6d15f166d131124/%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%8C%E1%85%A1%20%E1%84%80%E1%85%A8%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF%E1%84%80%E1%85%B5%206ac3102078fc4d53abb6b9656057f5b5.md)
    
    [설치](%5BOracle%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%5D%20%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%8B%E1%85%A5%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%2085d6da75bef94283a6d15f166d131124/%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8E%E1%85%B5%201cf448ecdba0463fb083c0d0f83cad35.md)
    

- SQL 명령어
    - 데이터베이스 명령어 종류
        - DDL(Data Definition Language) 데이터 정의어 : 테이블, 뷰, 인덱스를 생성하거나 수정하는 명령어
        - DCL (Data Control Language) 데이터 제어어 : 특정 사용자에게 권한을 부여하거나 뺏는 명령어.
        - DML (Data Manipulation Language) 데이터 조작어 : 자료를 추가, 수정, 삭제, 조회하는 명령어
    
    1) DDL
    
    - Create
    - Alter
    - Rename
    - Truncate
    - Drop
    - 제약조건 (unique, primary key, foreign key, default)
    - 개체무결성과 참조무결성
    
    2) DCL
    
    [사용자 생성 및 권한 부여](%5BOracle%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%5D%20%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%8B%E1%85%A5%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%2085d6da75bef94283a6d15f166d131124/%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%8C%E1%85%A1%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%80%E1%85%AF%E1%86%AB%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%87%E1%85%AE%E1%84%8B%E1%85%A7%20336ed24cd44147ce90415a3b076ed704.md)
    
    3) DML
    
    - Select
    - Update
    - Delete
    - Join
    - 서브쿼리
    - 연산자
    - 정렬
    - 집계함수
    - 수학과 관련된 함수
    - 인덱스
    
- 데이터모델링
- PL/SQL
    - 프로시저
    - 트리거
    - 펑션
