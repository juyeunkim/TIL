# RxJava



## doOnComplete

파라미터 - Action

Flowable, Observable 등에서 모든 item을 발행하고 나면 한번만 실행

주로 끝났는지 여부를 로그를 찍기 위해서 사용

onComplete 바로 전에 실행

에러가 발생하면 실행되지 않음

- doOnComplete로 알수있는 세가지
  1. 끝까지 잘 실행이 되었는지
  2. 시작하지 않았는지
  3. 중간에 에러가 났는지



[rxjava란? - Observable, .doOnComplete()](https://krksap.tistory.com/1189)



## doOnNext

파라미터 - Consumer



### doOnNext(), doOnComplete(), doOnError()

- 세 함수는 Observable의 알림 이벤트
- 데이터를 발행할때는 `doOnNext`
- 중간에 에러가 발생하면 `doOnError`
- 모든 데이터를 발행하면 `doOnComplete`



[RxJava 디버깅과 예외처리](https://beomseok95.tistory.com/61)



### doOnNext vs doOnSuccess

- doOnNext : 데이터가 성공적으로 내보내졌다
  데이터가 사용가능하고 존재함
- doOnSuccess : 성공적으로 Mono가 완료되었다
  결과가 T or null 이며, 데이터 상태에 관계없이 처리 자체가 성공적으로 완료되었음을 의미
  데이터를 사용할수 없거나 존재하지 않지만 파이프라인 자체가 성공하더라도 실행





## onErrorResume

에러가 발생하면 다른 시퀀스나 에러로 대체할 수 있다

시퀀스에서 발생한 에러를 입력으로 받아 결과로 Publisher를 리런

> exception handling 같은 느낌인가?



[스프링 리액터 - 에러 처리](https://javacan.tistory.com/entry/Reactor-Start-5-error-handling)



## doOnSubscribe

Observable이 구독될 때 호출되는 콜백 함수를 등록 할 수 있다

