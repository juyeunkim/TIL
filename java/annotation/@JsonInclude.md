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

  

