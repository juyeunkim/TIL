# HttpServletRequest 



## 기능

### 1. HTTP 요청 메시지 조회

- Start Line
  - HTTP 메소드
  - URL
  - 쿼리 스트링
  - 스키마, 프로토콜
- 헤더
  - 헤더 조회
- 바디
  - form 파라미터 형식 조회
  - message body 데이터 직접 조회



### 2. 임시 저장소 기능

> HTTP 요청이 살아있는 동안 데이터를 임시 저장

- 저장 : `request.setAttribute(name, value)`
- 조회 : `request.getAttribute(name)`



### 3. 세션 관리 기능

 