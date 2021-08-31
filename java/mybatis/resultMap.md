# resultMap



- select 쿼리문에서만 사용

- 주로 상속 관계에 있는 클래스를 매핑할 때 사용하는 방법 

  - 1:1
  - 1:N

  



## 1:1 오브젝트 매핑

- `<association >` 사용

### 필드

- property
  해당 VO 내에서 정의된 VO(inside) 의 이름
- `javaType`
  VO(inside) 의 경로



### 예제

- One.java

~~~java
public Class One {
    private int id;
    private String name;
}
~~~

- ObjectBean.java

~~~java
public Class ObjectBean {
    private int id;
    private One one;
    private String desc;
}
~~~

- Mapper.xml

~~~xml
<resultMap id="ObjectMap" type="ObjectBean">
    <id property="id" column="id"/>
    <association property ="one" javaType="com.test.bean.One">
        <id property="id" column="id"/>
        <result column="name" property="name" />
    </association>
    <result column="desc" property="desc" />
</resultMap>
~~~



## 1:N 오브젝트 매핑

- `<collection>` 사용

### 필드

- property

  해당 VO안에 정의된 배열 VO의 이름

- `ofType`

  배열로 지정된 VO의 경로

- autoMapping
  true로 하면 select 문에서 나온 결과를 자동으로 오브젝트로 매핑



### 예제

- ObjectBean.java

~~~java
public Class ObjectBean {
    private int id;
    private List<One> one;
    private String desc;
}
~~~

- Mapper.xml

~~~xml
<resultMap id="ObjectMap" type="ObjectBean">
    <id property="id" column="id"/>
    <collection property ="one" ofType="com.test.bean.One">
        <id property="id" column="id"/>
        <result column="name" property="name" />
    </collection>
    <result column="desc" property="desc" />
</resultMap>
~~~





## 하위 객체의 값을 null or [] 로 설정하기

>  `association` `collection` 을 사용해서 VO를 매핑할때
>
> PK-FK로 묶여있어 id값이 같으면 하위 객체의 값이 비어있음에도 있는거처럼 매핑되는 경우가 있다.
>
> 이럴때 어떻게 처리해야하는지



-  `association` `collection` 안에 id와 밖의 id가 같다면

   `association` `collection`안에 정의된 id 를 삭제한다







[조건에 따라 다른 클래스에 매핑](http://www.devkuma.com/books/pages/751)

[Mybatis - 검색 결과를 임의의 Java Object에 매핑](https://araikuma.tistory.com/476)

[Mabatis ResultMap이란? ](https://webdevtechblog.com/mybatis-resultmap%EC%9D%B4%EB%9E%80-854a94df1f78)

