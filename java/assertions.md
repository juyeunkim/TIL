# Assertions

> 프로그램이 올바르게 동작하거나 동작하지 않는 요구사항을 코드화하는데 사용
>
> condition을 테스트하여 false가 나오면 개발자에게 알려준다
>
> 버그가 발생한 시점에서 어디서 버그가 발생했는지를 알려준다

### 예제1

~~~java
import org.junit.jupiter.api.Assertions;

@Test
    public void save(){
        Member member = new Member();
        member.setName("spring");

        repository.save(member);

        Member result = repository.findbyId(member.getId()).get();
        Assertions.assertEquals(member, result);
   }
~~~

> member와 result가 같으면 아무런 결과를 제공하지 않지만
>
> 다르면 ![스크린샷 2021-01-17 오후 11.21.19](assertions결과.png)
>
> 이와 같은 결과가 나온다 

### 예제2

~~~java
import static org.assertj.core.api.Assertions.*;

@Test
    public void save(){
        Member member = new Member();
        member.setName("spring");

        repository.save(member);

        Member result = repository.findbyId(member.getId()).get();
        assertThat(member).isEqualTo(result);
   }
~~~



## assertThorws

> assertThrows(Class<> classType, Executable executable)
>
> 발생할 예외 클래스의 Class 타입을 받고, 
>
> executable을 실행하여 예외가 발생할 경우 classType과 발생된 Exception이 같은타입인지 체크

- 예제

~~~java
// in TEST
assertThrows(IllegalStateException.class, () -> memberService.join(member2));
~~~

~~~java
// in SERVICE - join
memberRepository.findbyName(member.getName())
            .ifPresent(m -> {
                throw new IllegalStateException("이미 존재하는 회원입니다");
            });
~~~

> join 함수를 실행했을때 예외가 같은지 확인 - 같으면 테스트 통과 / 다르면 실패