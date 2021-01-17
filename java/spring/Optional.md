# Optional

> java 8에 추가된 내용
>
> Null 을 처리하는 방법



### NULL 과 관련된 문제

1. 런타임에 NPE(NullPointException) 예외를 발생시킬 수 있다
2. NPE 방어를 위해 들어간 null 체크 로직 (if로 null 체크하고 return) 때문에 코드 가독성과 유지 보수성이 떨어진다



# Optional

### 개념

존재할 수도 있지만 안할 수도있는 객체

null이 될 수도 있는 객체



### 효과

1. NPE를 유발할 수 있는 null을 직접 다루지 않아도 된다

   Optional 클래스에 위임한다

2. Null 체크를 직접 안해도된다

3. 명시적으로 해당 변수가 null이 될 수 도있다는 가능성을 표현할 수 있음 ( 불필요한 방어 로직을 줄일 수 있다)



### 사용법

- 변수 선언

~~~java
Optional<Member> optMember;
~~~

> Member 타입의 객체를 감쌀 수 있는 Optional 타입의 변수
>
> 변수명은 그냥 클래스 명을 사용하기도 하지만 `maybe` `opt` 와 같은 접두어를 붙여서 사용

- 객체 생성

Optional 클래스는 간편하게 객체 생성을 할 수 있도록 3가지 정적 팩토리 메소드 제공

1. Empty()

   ~~~java
   Optional<Member> maybeMember = Optional.empty();
   ~~~

   > Null 을 담고 있는 비어있는 Optional 객체를 얻어온다

2. of(value)

   ~~~java
   Optional<Member> maybeMember = Optional.of(aMember);
   ~~~

   > null 이 아닌 객체를 담고 있는 Optional 객체를 생성
   >
   > null이 넘어올 경우, `NPE` 을 던진다

3. ofNullable(value)

   ~~~java
   Optional<Member> maybeMember = Optional.ofNullable(aMember);
   Optional<Member> maybeNotMember = Optional.ofNullable(null);
   ~~~

   > null 인지 아닌지 확신 할 수 없는 객체를 담고있는 Optional 객체를 생성
   >
   > > null이 넘어올 경우, NPE를 던지지 않고 `Optional.empty()` 와 동일하게 비어있는 Optional 객체를 얻어옴
   >
   > ▶︎ 해당 객체가 null 인지 아닌지 자신이 없는 상황에서 사용



