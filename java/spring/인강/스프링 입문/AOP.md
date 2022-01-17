# AOP

> 공통 관심 사항 vs 핵심 관심 사항 분리





### 객체 생성 2가지 

1. Config 에 등록

   ~~~java
   @Bean
       public TimeTraceAop timeTraceAop(){
           return new TimeTraceAop();
   }
   ~~~

2. `@ComponentScan`

   ~~~java
   // in TimeTraceAop
   @Aspect
   @ComponentScan
   public class TimeTraceAop {
     ///
   }
   ~~~

   

### AOP 동작 원리



빈을 등록하기 전에

프록시 멤버 서비스(가짜 멤버서비스) 를 먼저 세워놓고

joinPoint.proceed()

를 하면 실제 빈을 호출