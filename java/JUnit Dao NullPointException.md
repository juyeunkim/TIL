# JUnit Dao NullPointException

> JUnit에서 dao 접근시 `NPE` 발생

### 원인

Spring에 핸들러를 만들어야한다 (종속성 주입을 위하여)

없으면 종속성 주입이 제대로 동작하지X -> dao가 null 이된다



### 해결방법

- `NPE` 를 수정하는 수동 생성 예제

~~~java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations={"Student-servlet.xml"})
@TestExecutionListeners({DependencyInjectionTestExecutionListener.class})
public class TestDAO {
    ...
}
~~~

