# Lombok 어노테이션





## @Builder

※ Builder 란?

다수의 필드를 가지는 복잡한 클래스인 경우 생성자 대신 사용



- 클래스로 Builder 생성

  ~~~java
  public class User {
    private String name;
    private int age;
  
    public static UserBuilder builder() {
      return new UserBuilder();
    }
  
    public String getName() {
      return name;
    }
  
    public void setName(String name) {
      this.name = name;
    }
  
    public int getAge() {
      return age;
    }
  
    public void setAge(int age) {
      this.age = age;
    }
  }
  
  //Builder Class
  public class UserBuilder {
    private String name;
    private int age;
  
    public User build() {
      User user = new User();
      user.setName(this.name);
      user.setAge(this.age);
      return user;
    }
  
    public UserBuilder name(String name) {
      this.name = name;
      return this;
    }
  
    public UserBuilder age(int age) {
      this.age = age;
      return this;
    }
  }
  ~~~

- 롬복으로 Builder 생성

  ~~~java
  @Data
  @Builder
  public class User {
    private String name;
    private int age;
  }
  ~~~

- 빌더 예제

  ~~~java
  User user = User.builder()
                    .name("홍길동")
                    .age(19)
                    .build();
  // 클래스와 @Builder 사용방법 동일
  ~~~

[@Builder로 기본값 지정하기](https://tomining.tistory.com/180)

[알아두면 유용한 Lombok 어노테이션](https://www.daleseo.com/lombok-useful-annotations/)

