# 동시성 제어

> 동시성 제어를 위한 2가지 방법

- 목적

동시성 문제 해결하기

InnoDB를 사용하면서 동시성을 고려하지 않고 개발을 하면 큰 문제가 생길 수 있다.

## LOCK IN SHARE MODE

select를 한 후에 트랜잭션이 끝날 때까지 해당 ROW 값이 변경되지 않을 것을 보장

`update, delete` 쿼리는 잠김 상태가 되어 트랜잭션이 끝날때 까지 대기

`select` 는 얼마든지 여러 세션이 동시에 수행하는 것이 가능하다

- 예시

~~~mysql
SELECT NAME, CODE, CNT
FROM STAMP
WHERE id = 10
LOCK IN SHARE MODE;
~~~



## FOR UPDATE

`FOR UPDATE` 로 select를 하면 다른 세션에서 DML(`select, update, delete`) 이 요청와도 잠김 상태가 된다

트랜잭션이 끝나는 시점에 풀린다

- 예시

~~~mysql
SELECT NAME, CODE, CNT
FROM STAMP
WHERE id = 10
FOR UPDATE;
~~~

