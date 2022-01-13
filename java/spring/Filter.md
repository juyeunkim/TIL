# Filter



## 개념

디스패처 서블릿에 요청이 전달되기 전/후에 url 패턴에 맞는 모든 요청에 대해 부가작업을 처리할 수 있는 기능을 제공

ex) Sptring Security, CORS Filter







![img](https://blog.kakaocdn.net/dn/bZQx9K/btq9zEBsJ75/dEAKj1HEymcKyZGZNOiA80/img.png)

### 설정 방법

1. `web.xml` 등록 방식

2. Java Config 등록 방식 -> FilterRegistration Bean을 정의하여 추가할 Filter를 정의

3. Java Config 등록 방식  -> AbstractAnnotationConfigDispatcherServletInitializer에서 getServletFilter에 추가

4. `@WebFilter` Annotation 등록 방식

   - `@Component`, `@WebFilter` , `@Order` 사용으로 필터 등록

   - ~~~java
     package com.example.springstudy.filter;
     
     import org.springframework.core.annotation.Order;
     import org.springframework.stereotype.Component;
     
     import javax.servlet.*;
     import javax.servlet.annotation.WebFilter;
     import java.io.IOException;
     
     @Component
     @WebFilter(
             description = "1번째 필터",
             urlPatterns = "/*",
             filterName = "Test-Filter1"
     )
     @Order(2)
     public class TestFilter implements Filter {
         @Override
         public void init(FilterConfig filterConfig) throws ServletException {
     
         }
     
         @Override
         public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
             System.out.println("start testFilter1");
             filterChain.doFilter(servletRequest, servletResponse);
             System.out.println("finish testFilter1");
         }
     
         @Override
         public void destroy() {
     
         }
     }
     ~~~

   - 다른 설정 파일 없이 Filter Class 파일 하나만 가지고 Filter 설정 가능
   - `@Component`
     - Component-scan시 Spring Bean으로 등록
   - `@WebFilter`
     - Filter 등록에 필요한 interface를 제공
   - `@Order`
     - Filter chain에 대한 순서를 지정



### 메소드

~~~java
public interface Filter {

    public void init(FilterConfig filterConfig) throws ServletException;
		
    public void doFilter(ServletRequest request, ServletResponse response,
                         FilterChain chain)
            throws IOException, ServletException;

    public void destroy();
}
~~~

- init
  - 필터 객체를 초기화하고 서비스에 추가하기 위한 메소드
  - 웹 컨테이너가 1회 `init `메서드를 호출하여 필터 객체를 초기화하면 이후의 요청들을 `doFilter`를 통해 처리

- doFilter
  - url-pattern에 맞는 모든 HTTP 요청이 디스패처 서블릿으로 전달되기 전에 웹 컨테이너에 의해 실행되는 메소드
  - `FilterChain` 의 `doFilter` 를 통해 다음 대상으로 요청을 전달
    - chain.doFilter() 전/후에 우리가 필요한 처리 과정을 넣어줌으로써 원하는 처리를 진행
- destory
  - 필터 객체를 서비스에서 제거하고 사용하는 자원을 반환하기 위한 메소드
  - 웹 컨테이너에 의해 1번 호출되며 이후에는 doFilter에 의해 처리되지 않는다





[[Spring] 필터 vs 인터셉터 차이 및 용도](https://mangkyu.tistory.com/173)

[Spring filter 와 Interceptor](https://jaehun2841.github.io/2018/08/25/2018-08-18-spring-filter-interceptor/#filter)

