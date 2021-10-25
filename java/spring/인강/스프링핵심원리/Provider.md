# 프로토타입 스코프 - 싱글톤 빈과 함께 사용시 Provider로 문제 해결

> 싱글톤 빈안에 프로토 타입빈을 생성하면, 
>
> 생성시점에서 의존관계를 주입받기 때문에 프로타입 빈이 생성되긴하지만 공유가 된다는 문제가 생긴다

1. 스프링 컨테이너에 요청
2. ObjectFactory, ObjectProvider
3. JSR-330 Provider



## 스프링 컨테이너에 요청

- 싱글톤 빈이 프로토타입을 사용할 때마다 스프링컨테이너를 새로 요청
  - 싱글톤 빈 내에서 applicationContext 으로 프로토타입을 생성

~~~java
@Autowired
private ApplicationContext ac;
  
public int logic() {
  PrototypeBean prototypeBean = ac.getBean(PrototypeBean.class);
  prototypeBean.addCount();
  int count = prototypeBean.getCount();
  return count;
}	
~~~



### 특징

- 의존관계를 외부에서 주입(DI) 받는게 아니라 직접 필요한 의존관계를 찾는(DL) 사용
- 애플리케이션 컨텍스트 전체를 주입받게 되면, 스프링 컨테이너에 종속적인 코드가되고, 단위테스트가 어려워진다



## ObjectFactory, ObjectProvider

- 지정한 빈을 컨테이너에서 대신 찾아주는 DL 서비스를 제공하는 것이 `ObjectProvider` 

- 과거에는 `ObjectFactory` 가 있었는데 여기에 편의 기능을 추가하여 ObjectProvider가 만들어짐

~~~java
public interface ObjectProvider<T> extends ObjectFactory<T>, Iterable<T> {
  ///
}
~~~

~~~java
@Autowired
  private ObjectProvider<PrototypeBean> prototypeBeanProvider;
  public int logic() {
      PrototypeBean prototypeBean = prototypeBeanProvider.getObject();
      prototypeBean.addCount();
      int count = prototypeBean.getCount();
      return count;
}
~~~

- `getObject` 를 통해서 항상 새로운 프로토타입 빈이 생성
  - 내부에서 스프링 컨테이너를 통해 해당 빈을 찾아서 반환
  - 스프링이 제공하는 기능을 사용하지만, 기능이 단순하므로 단위테스트를 만들거나 mock 코드를 만들기 쉬워짐



### 특징

- ObjectFactory : 기능이 단순, 별도의 라이브러리 필요없음, 스프링에 의존
- ObjectProvider : ObjectFactory 상속, 옵션, 스트림 처리등 편의 기능이 많고, 별도의 라이브러리 필요없음, 스프링에 의존



## JSR-330 Provider

- JSR-330 자바 표준
- 라이브러리를 추가

~~~
dependencies {
	implementation 'javax.inject:javax.inject:1'
}
~~~



~~~java
//implementation 'javax.inject:javax.inject:1' gradle 추가 필수 @Autowired
private Provider<PrototypeBean> provider;

public int logic() {
      PrototypeBean prototypeBean = provider.get();
      prototypeBean.addCount();
      int count = prototypeBean.getCount();
      return count;
}
~~~

- `provider.get()` 을 통해 항상 새로운 프로토타입 빈이 생성
  - 내부에서 스프링 컨테이너를 통해 해당 빈을 찾아서 반환 (DL)
- 자바 표준, 기능이 단순하므로 단위테스트를 만들거나 mock 코드를 만들기는 훨씬 쉬워진다
- `Provider` 는 DL 정도의 기능만 제공

### 특징

- `get()` 메서드 하나로 기능이 매우 단순
- 별도의 라이브러리가 필요
- 자바 표준이므로 스프링이 아닌 다른 컨테이너에서도 사용 가능

