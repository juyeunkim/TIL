## Blocking / Non-Bloking

### Blocking

> 유저 프로세스가 커널에게 I/O 요청을 할 때,
> 커널이 작업을 완료하면 작업 결과를 반환
>
> > IO 작업이 진행되는 동안에 자신의 작업은 중단한 채 **대기**
> > IO 작업은 CPU를 거의 사용하지 않기 때문에 리소스 낭비가 심하다

![img](https://t1.daumcdn.net/cfile/tistory/2371EC4955160B8714)

### Non-Blocking

