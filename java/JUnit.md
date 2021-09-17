# JUnit



## @SpringBootTest vs @WebMvcTest 

### @SpringBootTest

- 일반적인 테스트로 slicing을 전혀 사용하지 않기 때문에 전체 응용 프로그램 컨텍스트를 시작한다

  실제 웹 서버와 연결

- 전체 응용 프로그램을 로드하여 모든 bean을 주입하기 때문에 속도가 느리다

### @WebMvcTest 

- Controller layer를 테스트하고 모의 객체를 사용하기 때문에 나머지 필요한 bean을 직정 세팅해줘야한다

  HTTP 요청은 mock을 이용해 가짜로 이뤄지고 실제 연결은 생성되지 않는다

- 가볍고 빠르게 테스트가 가능

- web 레이어 관련 빈(@Controller, @ControllerAdvice, @JsonComponent, Converter, GenericConverter, Filter, WebMvcConfigurer, and HandlerMethodArgumentResolver)들만 등록되므로 Service는 등록되지 않는다 -> `@MockBean` 을 통해서 가짜로 만들어줘야한다



| 버전                | Spring MVC 테스트                  | 단위 테스트(Mockito)                | Spring 통합테스트                            |
| ------------------- | ---------------------------------- | ----------------------------------- | -------------------------------------------- |
| SpringBoot 2.2 이전 | @RunWith(MockitoJUnitRunner.class) | @RunWith(MockitoJUnitRunner.class)  | @RunWith(SpringRunner.class) @SpringBootTest |
| SpringBoot 2.2 이후 | @WebMvcTest                        | @ExtendWith(MockitoExtension.class) | @SpringBootTest                              |



### 단위테스트

- service 단위 예제

~~~java
@ExtendWith(MockitoExtension.class) // 단위테스트
class MemberServiceTest {

    @InjectMocks // service 빈 주입
    private MemberService memberService;

    @Mock // repository 빈 주입 - 주입안하면 DB접근에 관련된 함수 사용불가
    private MemberRepository memberRepository;

    @Test
    public void getMember(){
        // given
        given(memberRepository.findById(anyLong()))
                .willReturn(new Member(1L, "test"));
        // when
        Member member = memberService.getMember(1L);
        // then
        assertEquals(1L, member.getId());
        assertEquals("test", member.getName());
    }
}
~~~



[JUnit5, 단위테스트 참고](https://wan-blog.tistory.com/71)

[스프링부트 테스트](https://yangbox.tistory.com/36)

## @Mock vs @InjectMocks

### @Mock

- Mock 객체를 생성

### @InjectMocks



# JUnit 4

> org.junit

## @Before vs @BeforeClass

### @Before

- @Test  어노테이션이 붙은 메소드 보다 먼저 실행

▶ @Test의 어노테이션의 갯수만큼 실행

▶ 같은 클래스 객체를 생성하더라도 다른 주소를 가진 다른 객체

- 해당 메서드는 `static` 이어야한다

### @BeforeClass

테스트의 실행전 딱 한번 실행

▶ 클래스 내부에서 한 객체를 공유하는게 유리한 경우 사용





# JUnit 5

> org.junit.jupiter

## @BeforeAll vs @BeforeEach

### @BeforeAll

> @BeforeClass 과 동일

### @BeforeEach

> @Before과 동일



[JUnit 5 정리](https://beomseok95.tistory.com/303)





## @Nested

> 중첩된 구조로 테스트 가능

### 예제

~~~java
@DisplayName("중첩 구조 테스트")
public class NestedTest {
    @Nested
    public class Out{
        
        @BeforeEach
        void init(){
            // 
        }
        
        @DisplayName("중첩 테스트 1")
        @Test
        void insideFuc (){
            ///
        }
    }
}
~~~



## @TestInstance

> 테스트 인스턴스의 라이프 사이클을 설정할 때 사용

- PER_METHOD

  test 함수 당 인스턴스가 생성 (default)

- PER_CLASS

  test 클래스 당 인스턴스가 생성

  - @BeforeAll, @AfterAll 어노테이션을 static method가 아닌 곳에서도 사용가능
  - @Nested 테스트 클래스에서 @BeforeAll, @AfterAll 선언할 때 사용

[@TestInstance](https://withhamit.tistory.com/300)



## @TestMethodOrder

> 테스트들의 실행 순서가 반드시 필요할때 사용





## Exception Test

~~~java
ServerException exception = assertThrows(ServerException.class, () -> {
				inquiryService.getUserInquiry(gameId, inquiryType, itemsPerPage, currentPage, notValidUser);
			});
			
			assertEquals(ServerError.ERR_AUTH_CANNOT_FIND_SESSION.toString(), exception.getKey());
~~~

- `assertThrows(Class<> classType, Executable executable)`
  - 인자값
    1. 예외 클래스 타입
    2. 실행할 함수
       executable을 실행하여 예외가 발생할 경우 classType과 발생될 Exception이 같은 타입인지 체크





[예외처리 테스트, assertThrows](https://sas-study.tistory.com/317)



## Transactional Test

> 테스트 수행후 롤백

~~~java
@Transactional
@Rollback
public class TransactionalTest {
    /////
}
~~~

### 주의 사항

- `@BeforeAll` 을 사용하면 트랜잭션이 안먹는다

- 이유는 `@Transactional` 이 정적 메소드가 아닌  `Spring Bean` 의 인스턴스 메소드에서만 작동한다
- `@BeforeEach` 는 인스턴스 메소드
  - 모든 @Test에 대해 실행
- `@BeforeAll` 은 정적 메소드
  - 모든 @Test에 대해 시작시 한번만 실행

- Test 시나리오
  - @BeforeAll
  - @Test
    - transaction
    - @Before
    - @Test
    - @After
    - rollback transaction

▶ 트랜잭션의 범위를 벗어나서 @BeforeAll에서는 롤백이 되지 않는다



[@BeforeAll and @Transaction are not working](https://stackoverflow.com/questions/58657587/beforeall-and-transaction-are-not-working-changes-on-db-side-are-not-rollbac)

