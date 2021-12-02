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
>  PK-FK로 묶여있어 id값이 같으면 하위 객체의 값이 비어있음에도 있는거처럼 매핑되는 경우가 있다.
>
>  이럴때 어떻게 처리해야하는지



- `association` `collection` 안에 id와 밖의 id가 같다면

  `association` `collection`안에 정의된 id 를 삭제한다

- 컬럼명이름을 다르게 줘서 다른 컬럼으로 인식하도록
  VO 에 다른 이름으로 컬럼을 추가하고, Map에 매핑한다

- `columnPrefix` 사용

  -  VO의 이름을 변경하지않아도되서 더 편하다



### columnPrefix

- resultMap으로 쿼리를 불러올때 컬럼명이 중복되는 경우가 있는데,  접두어를 추가해서 다른 컬럼명이라는것을 구분

- 쿼리문에서 as 로 접두어도 추가하여야한다

- depth가 깊어지면 상위 prefix와 자신의 prefix도 추가하여야한다

  - ~~~xml
    <resultMap id="testMap" type="com.test.Tester">
    <id column="no" property="no"/>
    <result column="col" property="col" />
    <association property="tester" resultMap="tester1" columnPrefix="t1_"/>
    </resultMap>
    
    <resultMap id="tester1" type="com.test.Tester">
    <id column="no" property="no"/>
    <result column="col" property="col" />
    <association property="tester" resultMap="tester2" columnPrefix="t2_"/>
    </resultMap>
    
    <resultMap id="tester2" type="com.test.Tester">
    <id column="no" property="no"/>
    <result column="col" property="col" />
    </resultMap>
    
    ~~~

  - ~~~mysql
    SELECT
        t.no,
        t.col,
        t1.no as t1_no
        t1.col as t1_col
        t2.no as t1_t2_no
        t2.col as t1_t2_col
    FROM tester t
    left join tester t1 on t.no = t1.no
    left join tester t2 on t.no = t2.no
    ~~~

~~~xml
 <resultMap type="sample.mybatis.Foo" id="fooResultMap">
    <id property="id" column="id" />
    <!-- columnPrefix에서 공용하는 접두어를 지정한다. -->
    <association property="bar" columnPrefix="bar_" resultMap="barResultMap" />
  </resultMap>

<select id="selectFoo" resultMap="fooResultMap">
    select foo.id
          ,bar.id bar_id -- "bar_"를 접두어로 지정
      from foo_table foo
          ,bar_table bar
     where bar.id = foo.bar_id
  order by foo.id asc
  </select>
~~~



[Mybatis, association columnPrefix 중첩 사용시 주의 사항](https://androphil.tistory.com/733?category=423961)





[조건에 따라 다른 클래스에 매핑](http://www.devkuma.com/books/pages/751)

[Mybatis - 검색 결과를 임의의 Java Object에 매핑](https://araikuma.tistory.com/476)

[Mabatis ResultMap이란? ](https://webdevtechblog.com/mybatis-resultmap%EC%9D%B4%EB%9E%80-854a94df1f78)



