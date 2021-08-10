# ENUM



## valueOf

- enum의 값을 가져올 수 있다

- 사용

  ~~~java
  public enum Test{
      a("test1"),b("test2"),c("test3");
      
      private String name;
  
  	public String getName() {
  		return name;
  	}
  }
  
  Test.valueOf(a); // Test의 a를 가져온다
  ~~~

  



### IllegalArgumentException

- enum에 없는값을 `valueOf` 로 접근하면 `IllegalArgumentException` 에러가 발생한다

- 예외처리

  ~~~java
  static public String lookup(String id) {
          try {
              return Test.valueOf(id).getName();
          } catch (IllegalArgumentException ex) {
              return null;
          }
  }
  ~~~

  > valueOf의 값을 접근해서 없는값이 나오면 null 로 리턴