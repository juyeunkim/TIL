# UPDATE



### concat 사용

~~~mysql
UPDATE 테이블명 SET 필드명 = CONCAT(필드명, '추가내용') 
WHERE 조건들
~~~

- 예제

> A 테이블의 ref_name(컬럼명) 컬럼의 내용을 
>
> `입력받은 내용 + '_' +origin_name(컬럼명)` 으로 수정하고 싶다 방법은?

~~~mysql
update A
set ref_name = concat('입력받은 내용','_', origin_name)
where gid = 2
~~~