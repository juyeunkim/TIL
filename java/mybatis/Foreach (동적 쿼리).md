# Foreach (동적 쿼리)

property : 파라미터명

prepend : 쿼리로 쓰일 문자

open : 구문이 시잘 될 때 삽입할 문자열

close : 구문이 종료 될 때 삽입할 문자열

conjunction : 반복되는 사이에 출력할 문자열

param : 전달받은 인자 값을 alias 명으로 대체



### 예시

~~~xml
SELECT * FROM APP_STATE_CONFIG
	<where>
		<if test=" params != null"> 
			AND 
			<foreach collection="params" open="(" close=")" separator=" or " item="param"></foreach>
		</if>
	</where>
~~~



### 객체 List로 Insert 하기

- Dao.java

~~~java
int insCustomTypeData(List<CustomTypeData> dataList);
~~~

- Dao.xml

~~~xml
<insert id="insCustomTypeData">
		INSERT INTO CUSTOM_TYPE_DATA ( type_name, type_value, ref_custom_type)
		VALUES 
			<foreach collection="list" item="item" separator=",">
				( #{item.typeName}, #{item.typeValue}, concat(#{groupName},'_', #{item.typeName}))
			</foreach>
		)
	</insert>
~~~

