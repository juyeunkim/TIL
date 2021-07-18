# BeanFactory와 ApplicationContext

> BeanFactory와 ApplicationContext를 `스프링 컨테이너` 라고 부른다

BeanFactory(interface) ⬅️  ApplicationContext(interface) ⬅️ AnnotationConfig ApplicationContext

## BeanFactory

- 스프링 컨테이너의 최상위 인터페이스
- 스프링 빈을 관리하고 조회하는 역할
- `getBean()` 을 제공
- 지금까지 우리가 사용했던 대부분의 기능을 BeanFactory가 제공하는 기능



## ApplicationContext

- BeanFactory의 기능을 모두 상속 받아서 제공
- 빈을 관리하고 검색하는 기능을 BeanFactory가 제공
  - 메세지 소스를 활용한 국제화 기능
  - 환경 변수
  - 애플리케이션 이벤트
  - 편리한 리소스 조회



## 다양한 설정 형식 지원 - 자바코드, XML



### 어노테이션 기반 자바 코드 설정 사용

- `new AnnotationConfigApplicationContext(AppConfig.java)`
- `AnnotationConfigApplicationContext`  클래스를 사용하면서 자바 코드로된 설정 정보를 넘긴다



### XML 설정 사용

- 최근에는 스프링 부트를 많이 사용하면서 XML 기반의 설정은 잘 사용하지 않는다.
- `GenericXmlApplicationContext` 를 사용하면서 `xml` 설정 파일을 넘기면 된다

- Xml 기반의 `appConfig.xml` 스프링 설정 정보와 자바 코드로 된 `AppConfig.java` 설정 정보를 비교해보면 거의 비슷하다



## 스프링 빈 설정 메타 정보 - BeanDefinition

- 스프링의 다양한 설정 형식 지원
- **역할과 구현을 개념적으로 나눈 것** (추상화)
  - XML을 읽어서 BeanDefinition을 만든다
  - 자바 코드를 읽어서 BeanDifinition을 만든다
  - 스프링 컨테이너는 자바 코드인지, XML 인지 몰라도 된다. <u>오직 BeanDifnition만 알면된다</u>
- `BeanDefinition` 을 빈 설정 메타정보라고 한다
  - `@Bean`, `<bean>` 당 각각 하나씩 메타 정보가 생성
- 스프링 컨테이너는 이 메타정보를 기반으로 스프링 빈을 생성

 

### BeanDefinition 정보

- BeanClassName : 생성할 빈의 클래스 명 (자바 설정처럼 팩토리 역할의 빈을 사용하면 없음)
- factoryBeanName : 팩토리 역할의 빈을 사용할 경우 이름 ex) appConfig
- factoryMethodName : 빈을 생성할 팩토리 메서드 지정 ex) memberService
- Scope : 싱글톤 (기본값)
- lazyInit : 스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때 까지 최대한 생성을 지연처리 하는지 여부
- InitMethodName : 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명
- DestoryMethodName : 빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드 명
- Contructor arguments, Properties : 의존관게 주입에서 사용 ( 자바 설정처럼 팩토리 역할의 빈을 사용하면 없음 )





