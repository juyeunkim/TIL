# explain 

> 쿼리문의 실행계획을 파악, 최적의 커리문인지 확인할 수 있도록 해준다
>
> ex) 인덱스가 잘 걸려있는지 확인
>
> `type` `filtered`  `Extra` 를 체크해본다



## 항목

### id

> 쿼리안에 있는 각 select 문에 대한 순차 식별자

- 해당 순서대로 select 문이 실행

### select_type

> select 문의 유형

- SIMPLE 
  - 단순 select
  - union 이나 서브쿼리를 사용하지 않음
- PRIMARY
  - 가장 바깥에 있는 select 문
- DERIVED
  - from 절의 서브쿼리
- SUBQUERY
- DEPENDENT SUBQUERY
- UNCACHEABLE SUBQUERY
- UNION
- DEPENDENT UNION

### table

> 참조하고 있는 테이블명

### partitaions

> 어떤 파티션을 사용했는지의 정보
>
> 파티션이 잘되어있는지 판단 가능

### type

> 조인 타입 ==> `속도` 와 밀접한 항목
>
> 인덱스가 잘 걸렸는지 확인 가능

#### 속성 (아래로 내려갈수록 안좋은 형태)

- system
  - 테이블에 단 하나의 행만 존재
  - const 조인의 특별한 형태

- const
  - 하나의 매치되는 행만 존재하는 경우 - row 1
  - 한번만 읽어들이기 때문에 무척 빠르다
  - `PRIMARY KEY` or `UNIQUE`
- eq_ref
  - 조인수행을 위해 각 테이블에서 하나의 행만 읽혀지는 형태
  - `PRIMARY KEY` or `UNIQUE` or `NOT NULL`
  - const 타입 외에 가장 훌륭한 조인
- ref
  - 인덱스로 지정된 컬럼끼리의 `=` , `<=>` 의 연산자를 통한 비교로 수행되는 조인
  - 조인이 키 값을 기반으로 단일 행을 선택할 수 없는 경우
  - NOT `PRIMARY KEY` `UNIQUE` 
- range
  - 특정한 범위의 rows들을 매칭시키는데 인덱스가 사용된 경우
  - `BETWEEN` , `IN`, `>` `>=`
- ALL
  - 이전 테이블과 조인을 위해서 풀스캔 사용
  - type이 이렇게 나오면 수정해야함

### possible_keys

> mysql이 해당 테이블의 검색에 사용할 수 있는 인덱스들을 보여줌

### key

> mysql 이 실제로 사용한 `key (index)`

### key_len

> 사용한 key의 개수
>
> ==> mysql이 실제로 키를 얼마나 사용했는지를 판단할 수 있다

### ref

> 행을 추출하는데 키와 함께 사용된 컬럼이나 상수값

### rows

> mysql이 찾아야하는 데이터 행의 예상값 (정확하지 않다)

### filtered

> 필터링 될 테이블 행의 예상 비율(%)

### extra

> 쿼리를 어떻게 해석하는지에 대한 추가 정보
>
> - `using filesort` `using temporary` 가 붙어있는지 체크
> -  `using index` 가 붙어있으면 인덱싱이 잘되어있다는 뜻

- distinct

  - 조건을 만족하는 레코드를 찾았을때 
    같은 조건을 만족하는 또 다른 레코드가 있는지 검사하지 않음

- not exist

  - `left join` 조건을 만족하는 하나의 레코드를 찾았을 때 같은 조건을 만족하는 또 다른 레코드 검사를 하지 않음

- range checked for each record

- using filesort

  - mysql 이 정렬을 빠르게 하기 위해서 부가적인 일 수행
  - 정렬은 조인 유형에 따라 모든 행을 탐색하고 where절과 일치하는 모든 행에 대한 정렬키와 행에대한 포인터를 저장하여 수행
  - 그런다음 키가 정렬되고 행이 정렬된 순서로 검색

- using index

  - 인덱스 사용

- using where

  - 조건을 사용

- using temporary

  - 가상 테이블 생성
  - 주로 `order by` or `group by` 할때 주로 사용

  





[mysql explain 실행계획 사용법 및 분석](https://nomadlee.com/mysql-explain-sql/)

[Mysql 실행계획(explain) 보는법](https://gradle.tistory.com/4)

[Mysql Exmplain 실행계획 보는법](https://denodo1.tistory.com/306)





## extra 에 다른 값이 들어가는 경우

### group by

기본적으로 정렬을 해서 조회한다. ==> `using filesort` 가 발생

### join

조인을했을때 결과를 임시로 저장을 할 테이블이 필요하다 ==> `using temporary` 발생

[explain 활용 - using temporary, using filesort 제거](https://m.blog.naver.com/pjt3591oo/220832178743)



## 예제

### index 안걸었을때



![image-20210907160629316](C:\Users\juyeunkim\Desktop\doc\mysql\img\image-20210907160629316.png)

> type이 ALL이여서 풀스캔으로 동작



### index 걸었을때

![image-20210907160540293](C:\Users\juyeunkim\Desktop\doc\mysql\img\image-20210907160540293.png)

![image-20210907165433612](C:\Users\juyeunkim\Desktop\doc\mysql\img\image-20210907165433612.png)