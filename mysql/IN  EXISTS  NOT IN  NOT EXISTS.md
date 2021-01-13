# IN / EXISTS / NOT IN / NOT EXISTS





## INSERT + EXISTS

> Insert시 EXISTS 조건을 확인해서 true이면 insert

~~~mysql
INSERT A (컬럼1, 컬럼2, 컬럼3) 
	SELECT 컬럼1, 컬럼2, 컬럼3
	FROM DUAL
	WHERE EXISTS (
    SELECT 컬럼a FROM B WHERE 컬럼a = 컬럼값
  								)
~~~

