# Jackson 어노테이션



# @JsonInclude



## 옵션

- ALWAYS (기본값)

- NON_NULL

  - null은 제외

- NON_ABSENT

  - null은 제외
  - absent 제외(Optional)

- NON_EMPTY

  - null은 제외
  - absent 제외
  - Collection, Map의 isEmpty()가 true이면 제외
  - Array의 length가 0이면 제외
  - String의 length()가 0이면 제외

- NON_DEFAULT

  - empty는 제외
  - primitive 타입이 default이면 제외
  - Date의 Timestamp가 0L이면 제외

  

## @JsonIgnore

- 필드 레벨에서 무시될 수 있는 속성을 표시

### 예제

~~~java
public class ObjectA {
    @JsonIgnore
    private String level;
    private String name;
}
~~~



~~~json
{
    "name" : "kim"
}
~~~

> level값이 null 이면 출력하지 않는다 

