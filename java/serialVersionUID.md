# serialVersionUID 이란?

> 직렬화를 할때 사용 (`implements Serializable`)



- `Serializable`을 상속하는 class의 경우, class의 versioning 용도로 serivalVersionUID 변수를 사용
- serivalVersionUID 를 명시적으로 지정하지않으면 compiler가 계산한 값을 부여
  - Serializeable Class or OuterClass에 변경이 있으면 serivalVersionUID 값이 바뀐다
  - Serialize 할 때와 Deserialize 할 때의 serivalVersionUID 값이 다르면 `InvalidClassExceptions` 발생



## 직렬화

- 자바의 객체를 바이트의 배열로 변환하여 파일, 메모리, 데이터베이스에 저장하는 과정
  - ![img](https://postfiles.pstatic.net/MjAxNzExMTdfODEg/MDAxNTEwODk4OTAzNzU5.UyhhF5HqiTIL1AzorkrZT8sDDEUZ7L7irEMBB5e5C6kg.8GLeHJ571XKw_if1RXDAKlpTbXqcmAKZ2JdKCOKGBuAg.PNG.kkson50/%EC%A7%81%EB%A0%AC%ED%99%94_serialization_%EC%97%AD%EC%A7%81%EB%A0%AC%ED%99%94.png?type=w2)
  - 저장한것을 다시 객체로 변화하는 과정을 역직렬화

- 직렬화를 할때는 `serialVersionUID` 를 저장하여, 역직렬화를 할때 그 값을 체크하는 용도로 사용



[[완벽해설] serialVersionUID에 대한 정확한 설명](https://blog.naver.com/kkson50/220564273220)

[[java] serialVersionUID의 용도](https://m.blog.naver.com/writer0713/220922099055)