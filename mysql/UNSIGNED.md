# UNSIGNED 란?



모든 Integer 타입들은 속성으로 `UNSIGNED` 를 가지고 있는데

- 이 타입은 칼럼내에 음수를 포함하지 않거나

- range를 양수쪽으로 더 넓게 가지고 싶을때 사용

  - unsigned를 사용하지 않는 경우

    - ~~~~
      -2147483648 ~ 2147483647
      ~~~~

  - unsigned를 사용

    - ~~~
      0 ~ 4294967295
      ~~~

- 해당 칼럼이 음수가 될일이 없는 경우에 사용

  - ex) auto-increment









[[MySQL] UNSIGNED 의미, 언제 사용할까?](https://donggu1105.tistory.com/28)