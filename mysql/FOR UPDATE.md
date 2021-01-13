# FOR UPDATE

동시성 문제 해결하기

~~~mysql
SELECT * FROM 테이블이름
WHERE id = 10
FOR UPDATE
~~~

SELECT로 가져온 데이터를 변경하려고 할 때 사용

다른 세션에서 DML(`SELECT, INSERT, UPDATE, DELETE`)요청이 와도 잠긴 상태가 된다

`FOR UPDATE` 를 한 세션 외에 다른 세션들은 모두 해당 ROW에 접근할 수 없게되고, 모두 대기 상태가 된다

