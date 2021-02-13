# JPA



## 기본키(PK) 매핑 방법 및 생성 전략

### 기본 키 매핑

- 직접 할당
  `@Id`

- 자동 생성
  `@Id` 와 `@GeneratedValue` 

  - 자동 생성 전략

    1. `IDENTITY`

    - 기본키 생성을 데이터 베이스에 위임

    - id 값을 `null` 로 하면 DB가 알아서 `AUTO_INCREMENT` 를 해준다

    ~~~java
    @Entity
    public class 객체명 {
      @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    }
    ~~~

    2. `SEQUENCE`

    



[기본키 매핑 참고](https://gmlwjd9405.github.io/2019/08/12/primary-key-mapping.html)