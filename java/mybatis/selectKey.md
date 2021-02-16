# selectKey

> 어떤 키값을 가져와서 증가시켜서 입력
> or 입력후에 증가된 키값을 가져올때
> ==> 별도의 쿼리 로직을 등록할 필요없이 해당 메소드에서 일괄처리 가능

- 입력하기 전 특정키값을 가져온 후, 그값을 이용하여 처리

~~~xml
<insert id="insertBoard" parameterType="Board">
    <selectKey resultType="string" keyProperty="boardID" order="BEFORE">
        SELECT MAX(boardID)+1 FROM board        
    </selectKey>    
    INSERT INTO board(boardID, title, content)
    VALUES(#{boardID}, #{title}, #{content})
</insert>  
~~~

> `keyProperty` : 컬럼명 
> 	이때, getter,setter가 있어야한다
> `order` : 쿼리 실행순서 
>
>   - befo : insert 쿼리 수행 전 selectKey 수행
>   - after :                             후 



- 방금 입력한 값의 특정값을 리턴

~~~xml
<insert id="insertSurveyInfo" parameterType="Board">
    INSERT INTO board(boardID, title, content)
    VALUES(#{boarID}, #{title}, #{content})
    <selectKey resultType="int" keyProperty="iq" order="AFTER">
        SELECT LAST_INSERT_ID()
    </selectKey>        
</insert>
~~~

>`LAST_INSERT_ID()` : 방금 입력한 테이블의 AI로 증가된 컬럼값을 가져오는 쿼리
>
>

