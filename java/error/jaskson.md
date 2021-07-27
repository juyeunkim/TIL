## InvalidDefinitionException

### Direct self-reference leading to cycle

- 에러 내용

  ~~~
  com.fasterxml.jackson.databind.exc.InvalidDefinitionException: Direct self-reference leading to cycle
  ~~~

  자기 자신을 참조하는 멤버변수가 존재할 때 발생하는 에러

- 해결방법
  `@JsonIgnore` 을 사용



### No serializer found for class

- 에러 내용

  ~~~
  com.fasterxml.jackson.databind.exc.InvalidDefinitionException: No serializer found for class com.google.protobuf
  ~~~

- 해결 방법
  pom.xml 에 dependency 추가

  ~~~xml
  		<dependency>
  			<groupId>com.google.protobuf</groupId>
  			<artifactId>protobuf-java</artifactId>
  			<version>3.12.4</version>
  		</dependency>
  		<dependency>
  		    <groupId>com.google.protobuf</groupId>
  		    <artifactId>protobuf-java-util</artifactId>
  		    <version>3.14.0</version>
  		    <scope>test</scope>
  		</dependency>
  ~~~

  





