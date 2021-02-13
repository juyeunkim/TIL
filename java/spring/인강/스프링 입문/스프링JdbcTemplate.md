# 스프링 JdbcTemplate

>  `템플릿 메서드 패턴` 이용

- 순수 JDBC와 동일한 환경설정을 하면 된다
- `JdbcTemplate`과 `MyBatis`는 JDBC의 반복 코드를 대부분 작성
  BUT SQL은 직접 작성해야한다

![img](https://gmlwjd9405.github.io/images/setting-for-dbprogramming/data-access-layer2.png)

### Config

~~~java
@Configuration
public class SpringConfig {

    @Autowired
    private DataSource dataSource;
    
    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
        return new JdbcTemplateMemberRepository(dataSource);
    }

}
~~~



### insert

~~~java
public Member save(Member member) {
          SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
  				  
          jdbcInsert.withTableName("member") // 테이블명 지정
            .usingGeneratedKeyColumns("id"); // 생성된 키의 컬럼명 지정
          
  				// 
  				Map<String, Object> parameters = new HashMap<>();
          parameters.put("name", member.getName());
          
  				// 
  				Number key = jdbcInsert.executeAndReturnKey(new
  MapSqlParameterSource(parameters));
          member.setId(key.longValue());
          return member;
}
~~~



### select

- RowMapper

> SQL 결과를 객체에 매핑하여 결과를 리턴한다

~~~java
 private RowMapper<Member> memberRowMapper(){
        return (rs, rowNum) -> {
            Member member = new Member();
            member.setId(rs.getLong("id"));
            member.setName(rs.getString("name"));
            return member;
        };
 }
~~~

- findById

~~~java
@Override
    public Optional<Member> findById(Long id) {
        List<Member> result = jdbcTemplate.query("select * from member where id = ?", memberRowMapper(), id);
        return result.stream().findAny();

    }
~~~

- findAll

~~~java
@Override
    public List<Member> findAll() {
        return jdbcTemplate.query("select * from member", memberRowMapper());
    }
~~~



### 



