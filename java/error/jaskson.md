## InvalidDefinitionException

### Direct self-reference leading to cycle

- 에러 내용

  ~~~
  com.fasterxml.jackson.databind.exc.InvalidDefinitionException: Direct self-reference leading to cycle
  ~~~

  자기 자신을 참조하는 멤버변수가 존재할 때 발생하는 에러

  ~~~
  com.fasterxml.jackson.databind.exc.InvalidDefinitionException: Direct self-reference leading to cycle XXXXX....
  com.google.protobuf.UnknownFieldSet["defaultInstanceForType"])
  ~~~

  protobuf의 내용을 읽어오지못할때 발생

- 해결방법
  1. `@JsonIgnore` 을 사용
  2. `JsonFormat.printer().print(res)` 사용
     `com.google.protobuf.util.JsonFormat`



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

  

[Protobuf 필드가 있는 Java POJO를 JSON으로 변환](https://the-codeslinger.com/2021/02/16/convert-java-pojo-with-protobuf-field-to-json-using-jackson/)

[ObjectMapper로 Enum 컨트롤하기](https://velog.io/@recordsbeat/JacksonObjectMapper%EB%A1%9C-Enum%EC%BB%A8%ED%8A%B8%EB%A1%A4-%ED%95%98%EA%B8%B0)

