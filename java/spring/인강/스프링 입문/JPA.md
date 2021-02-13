# JPA

## 특징

- 기존의 반복코드, 기본적인 SQL로 JPA가 직접 만들어서 실행해준다
- SQL 중심 설계 -> 객체 중심의 설계로 패러다임을 전환할 수 있다
- 개발 생산성을 크게 높일 수 있다 (작성할 코드 감소)
- 자바 구현 표준 인터페이스
  Hibernate : 구현체



### 환경 설정

- Build.gradle

~~~
implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
~~~

- Application.properties

~~~properties
spring.jpa.show-sql=true
# JPA가 생성한 SQL을 볼 수 있는 기능
spring.jpa.hibernate.ddl-auto=none
# 객체를 보고 자동으로 테이블을 생성해주는 기능
~~~

> - ddl-auto : `create` 를 사용하면 엔티티 정보를 바탕으로 테이블도 직접 생성



### 엔티티 매핑

~~~java
@Entity
public class 객체명 {
  @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
}
~~~

> `@Id` 와 `@GeneratedValue` 를 같이 사용하면 기본키를 자동 생성해준다
>
> > `@Id` 만 사용하면 직접 할당



### EntityManager

> JPA를 사용하려면 주입받아야한다
>
> 데이터베이스를 접근할 수 있는 역할





### createQuery

> 객체를 대상으로 쿼리를 날린다

~~~java
em.createQuery("select m from Member m", Member.class)
                .getResultList();
// 객체를 매핑해서 쿼리의 리스트 결과를 가져온다
~~~



### JPA 사용시 주의사항

사용할때 `@Transaction` 를 이용해야한다

> JPA는 조인시 transaction 안에서 실행이 되어야한다

~~~java
// in Service

@Transactional
public class MemberService {
  ....
}
~~~



# Spring data JPA

> 이미 구현된 쿼리문을 제공, 인터페이스를 상속받아서 쿼리를 사용한다

- 인터페이스를 통한 기본적인 CRUD 제공
- 페이징 기능 자동 제공

> 참고) 복잡한 동적쿼리는 `Querydsl` 라이브러리 사용

##  예제

~~~java
// Spring data JPA
public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository {

    @Override
    Optional<Member> findByName(String name);
}
~~~

> SpringDataJpaMemberRepository가 자동으로 객체 생성

~~~java
@Configuration
public class SpringConfig {
  private final MemberRepository memberRepository;

    public SpringConfig(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }


    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepository);
    }
}
~~~

> 스프링 데이터 JPA가 `SpringDataJpaMemberRepository` 를 스프링 빈으로 자동 등록

