# @ComponentScan

- 특정 어노테이션들을 자동으로 스캔

  - @Component

  - @Controller

  - @Service

  - @Repository

  - @Configuration

- 자기가 위치하고 있는 곳부터 스캔

- @SpringBootApplication에 정의되어있음



## ComponentScan 범위

> 스캔의 범위를 지정할 수 있음

- basePackages

  - ~~~java
    @Configuration
    @ComponentScan(basePackages = "com.test.spring")
    public class ApplicationConfig(){
        
    }
    ~~~

  - 직접 패키지 경로 명시하여 스캔할 위치 지정

  - untypesafe - 철자 오류로 scan을 못하는 상황 발생

- basePackageClasses

  - ~~~java
    @Configuration
    @ComponentScan(basePackageClasses = ApplicationConfig.class)
    public class ApplicationConfig(){
        
    }
    ~~~

  - 괄호안에 적힌 Class가 위치한 곳으로 부터 모든 어노테이션이 부여된 Class를 빈으로 등록

  - typesafe