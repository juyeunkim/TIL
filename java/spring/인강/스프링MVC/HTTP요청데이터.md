# HTTP 요청 데이터

> HTTP 요청 메시지를 통해 클라이언트에서 서버로 데이터를 전달하는 방법



## 1. GET - 쿼리 파라미터

- `/url?username=hello&age=20`
- 메시지 바디 없이, URL의 쿼리 파라미터에 데이터를 포함해서 전달
- 예) 검색, 필터, 페이징 등에서 많이 사용하는 방식

## 2. POST - HTML Form

- `content-type: application/x-www-form-urlencoded`
- 메시지 바디에 쿼리 파라미터 형식으로 전달 
  - username=hello&age=20
- 예) 회원가입, 상품 주문, HTML Form 사용
- 서버에서 넘어올때 쿼리파라미터 방식으로 넘어온다
  - 클라이언트(웹 브라우저) 입장에서는 두 방식에 차이가 있지만, 서버 입장에서는 둘의 형식이 동일
  - 서버에서 조회시 `requesr.getParameter()` 로 조회 



> **참고**
>
> Content-type은 HTTP 메시지 바디의 데이터 형식을 지정
>
> **GET URL 쿼리 파라미터 형식**으로 클라이언트에서 서버로 데이터를 전달할 때는 HTTP 메시지 바디를 사용하지 않기 때문에 content-type이 없다
>
> **POST HTML Form 형식**으로 데이터를 전달하면 HTTP 메시지 바디에 해당 데이터를 포함해서 보내기 때문에 바디에 포함된 데이터가 어떤 형식인지 content-type을 꼭 지정해야 한다.
>
> > 이렇게 폼으로 데이터를 전송하는 형식을 `application/x-www-form-urlencoded`



## 3. HTTP message body

- HTTP API에서 주로 사용

  - **JSON**
  - XML
  - TEXT

  

