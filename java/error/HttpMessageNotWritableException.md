# HttpMessageNotWritableException

- 에러 내용

  ~~~
  org.springframework.http.converter.HttpMessageNotWritableException: No converter for [VO Class] with preset Content-Type ‘null’]
  ~~~

- 해결 방법
  VO에 `@JsonInclude(JsonInclude.Include.NON_NULL)` 추가



