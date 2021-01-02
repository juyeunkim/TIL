# Spring Boot

## JDK 설정

`jdk 11 version`

1. jdk 설치후 경로 설정
   제어판-> 시스템 및 보안 -> 시스템 -> 고급 시스템 설정
   환경변수에서 시스템변수 수정 

   - Path : `%JAVA_HOME%\bin`
   - JAVA_HOME : 다운로드 받은 jdk 경로

   `※ 이때 발생했던 문제`

   > 자바8  이미 설치되어있어서 안바뀌는 점
   >
   > - 해결방법 : 기존에 설치되어있던 자바8를 삭제

2. project->properties->Java Build Path-> Add Library-> 다운로드받은 파일 저장



## pom.xml

- 서버 구동시 멈춤 현상

~~~xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
~~~

- 파일 저장시 부트 재시작

~~~xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
~~~

## 스웨거 설정

- pom.xml

~~~xml
		<dependency>
			<groupId>io.springfox</groupId>
			<artifactId>springfox-swagger2</artifactId>
			<version>2.9.2</version>
		</dependency>
~~~

- SwaggerConfig.java

~~~java
@Configuration
@EnableSwagger2
public class SwaggerConfig {                                    
    @Bean
    public Docket api() { 
        return new Docket(DocumentationType.SWAGGER_2)  
          .select()                                  
          .apis(RequestHandlerSelectors.any())              
          .paths(PathSelectors.any())                          
          .build();                                           
    }
}	
~~~

`※ 이때 발생했던 문제`

> Docket을 찾지 못하고 Error 가 발생 !
>
> - 해결 방법 : pom.xml에서 Swagger의 버전을 2.9.2로 바꿨더니 해결



### Reqeust Body Customize



## Spring Profile 설정

#### 왜 사용해야 하는지

> 개발환경과 리얼 환경에서의 profile을 다르게 설정해야하는 경우가 생기는데
> 환경에 따른 profile을 설정해줄 수 있다
>
> ex) 데이터베이스 설정, 외부 연동 url 등

- cmd 에서 실행시킬때, profile 적용하는 방법

  ~~~shell
  java -Dspring.profiles.active=profile 명  -jar 자바파일명.jar
  ~~~

  

- SpringBoot에서 profile 적용하는 방법 

1. 기능에 맞는 application.properties 만들기
   develop : local 개발
   production : 배포용

   - src/main/resources/develop/application.properties

   ~~~properties
   spring.profile.value: develop
   
   spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
   spring.datasource.url=jdbc:mysql://localhost:3306/mydb?serverTimezone=UTC&characterEncoding=UTF-8
   spring.datasource.username=root
   spring.datasource.password=webadmintool1234
   ~~~

2. application.properties 연결

   - src/main/java/com/nmfun/web/config/ProfileDevelop.java

   ~~~java
   @Configuration
   @Profile(value="develop")
   @PropertySource({"classpath:develop/application.properties"})
   public class ProfileDevelop {
   
   }
   
   ~~~

   

3. ProfileConfig.java  작성

   - src/main/java/com/nmfun/web/config/ProfileConfig.java

     ~~~java
     @Import({ProfileDevelop.class, ProfileProduction.class})
     @Configuration
     public class ProfileConfig {
     
     }
     ~~~

     > 등록한 Profile을 연결한다

4. 스프링 부트의 기본 프로필 설정

   - src/main/java/com/nmfun/web/DefenceWebApplication.java

   ~~~java
   @SpringBootApplication
   public class DefenceWebApplication {
   
   	public static void main(String[] args) {
   		
   		// 스프링 부트의 기본 프로필 설정
   		String profile = System.getProperty("spring.profiles.active");
   		if(profile == null) {
   			System.setProperty("spring.profiles.active", "develop");
   		}
   		
   		SpringApplication.run(DefenceWebApplication.class, args);
   	}
   
   }
   
   ~~~

   































