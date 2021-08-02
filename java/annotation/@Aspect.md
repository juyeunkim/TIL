# @Aspect



@Aspect 어노테이션을 이용해서 공통으로 적용할 기능을 구현할 경우, XML 설정에서 이를 인식할 수 있도록 `<aop:aspectj-autoproxy />`태그를 추가해야한다

> @Aspect 어노테이션이 있는 클래스를 찾아서 자동으로 aspect로 만들어준다



~~~java
//Aspect 역할을 할 클래스를 선언하기 위해 어노테이션 선언
@Aspect
public class ExeTimeAspect {
  
    @Around("publicTarget()")
    public Object measure(ProceedingJoinPoint joinPoint) throws Throwable {
        try {
            Object result = joinPoint.proceed(); // 핵심 기능 실행
            return result;
        } finally {
            // 공통 기능
    
        }
    }
}
~~~







[AOP구현 (@Aspect)](https://ktko.tistory.com/entry/Spring-AOP%EA%B5%AC%ED%98%84Aspect-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98-%EC%82%AC%EC%9A%A9)

[AOP 개념과 예제 - @Aspect 구현](https://private.tistory.com/44)

## 에러 유형

### error the @annotation pointcut expression is only supported at Java 5 compliance level or above

- 에러 원인

  pom.xml에 있는 Aspectj의 버전이 낮아서 발생하는 에러
  버전을 올려주면 된다



[Spring AOP와 AspectJ 비교하기](https://logical-code.tistory.com/118)

