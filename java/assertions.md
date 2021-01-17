# Assertions

> 프로그램이 올바르게 동작하거나 동작하지 않는 요구사항을 코드화하는데 사용
>
> condition을 테스트하여 false가 나오면 개발자에게 알려준다
>
> 버그가 발생한 시점에서 어디서 버그가 발생했는지를 알려준다

### 예제

~~~java
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