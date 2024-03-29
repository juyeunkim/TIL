# MyBatis



## 설정 순서

1. pom.xml dependency 추가

   ~~~xml
   		<dependency>
   			<groupId>org.mybatis</groupId>
   			<artifactId>mybatis</artifactId>
   			<version>3.5.5</version>
   		</dependency>
   
   		<dependency>
   			<groupId>org.mybatis</groupId>
   			<artifactId>mybatis-spring</artifactId>
   			<version>1.3.0</version>
   		</dependency>
   
   		<dependency>
   			<groupId>mysql</groupId>
   			<artifactId>mysql-connector-java</artifactId>
   			<scope>runtime</scope>
   		</dependency>
   
   		<dependency>
   			<groupId>org.mybatis.spring.boot</groupId>
   			<artifactId>mybatis-spring-boot-starter</artifactId>
   			<version>1.3.0</version>
   		</dependency>
   ~~~

2. application.properties 추가 (Mybatis의 global location)

   ~~~properties
   mybatis.config-location=classpath:/config/SqlConfig.xml
   ~~~

3. resources/config/SqlConfig.xml

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
   
   <configuration>
   	<!-- VO 매핑 : vo에 연결되어있는 객체를 사용가능하다 -->
   	<typeAliases>
   		<package name="com.nmfun.web.vo" />
   	</typeAliases>
   
       <!-- 작성한 쿼리 매핑 -->
   	<mappers>
   		<mapper resource="mapper/accountmapper.xml" />
   		<mapper resource="mapper/EventMapper.xml" />
   	</mappers>
   
   </configuration>
   ~~~

4.  쿼리 작성하기 (.xml)

   ~~~xml
   <mapper namespace = "com.nmfun.web.dao.EventDao">
   	<!-- 쿼리 작성 -->
   </mapper>
   ~~~

   

## parameterType vs parameterMap

- parameterMap

- parameterType
  VO가 올수있음

  ~~~xml
  <insert id="insertUser" parameterType="User">
    insert into users (id, username, password)
    values (#{id}, #{username}, #{password})
  </insert>
  ~~~

  

## resultType vs resultMap

- resultType 
  클래스명 전체 또는 alias 입력 (매핑하려는 자바 클래스의 전체 경로를 입력)

- resultMap
  선언 당시 참조로 사용한 이름을 입력
  resultType을 이용하면 자동매핑되기 때문에 편리하지만 제한이 있음,
  resultMap을 사용하면 개발자가 직접 원하는 POJO 클래스에 매핑 가능

  ~~~xml
  <!-- resultMap 객체로 사용 가능-->
  <resultMap id="test" type="com.test.Student">
  
              <result property="name" column="name">
  
               ....
  
    </resultMap>
  
  
  <!-- 타입으로 지정가능 -->
    <select id="selectTest" resultMap="test">
  
     ...
  
    </select>
  
  
  ~~~



## 파라미터 2개 넘기기

- EventDao

  ~~~java
  public List<Event> selectByDate(@Param("start") String start, @Param("end") String end);
  ~~~

- EventMapper.xml

  ~~~xml
  <!-- 두개의 기간이 들어왔을때 이벤트 조회 -->
  	<select id="selectByDate" parameterType="string" resultType = "Event"> 
  		select * from event 
  		<![CDATA[ where start >= #{start} and end <= #{end}]]>
  	</select>
  ~~~

  > Dao의 start와 end 의 parameter를 가지고 값을 넣어준다



## 날짜 범위 검색하기

- `<![CADATA[]]>` 사용

  >  Mybatis에서는 XML로 쿼리를 작성하기 때문에 부등호를 사용하면 에러가 발생

- EventMapper.xml

~~~xml
<!-- 기간 이벤트 조회 -->
	<!-- 두개의 기간이 들어왔을때 이벤트 조회 -->
	<select id="selectByRange" parameterType="string" resultType = "Event"> 
		select * from event 
		<![CDATA[ where start >= #{start} and end <= #{end}]]>
	</select>
~~~



## Mapper

### namespace

> xml 파일이 어떤것에서 오는지 경로를 작성해주기 위한 용도
>
> 인터페이스의 바인딩



## Lombok

[자주쓰는 표현식](https://www.daleseo.com/lombok-popular-annotations/)


