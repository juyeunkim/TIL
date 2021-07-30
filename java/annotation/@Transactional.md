# @Transactional

 

@Transactional(value="transactionManagerEvent", propagation=Propagation.REQUIRED, rollbackFor=Exception.class)



transactionManager : 트랜잭션 매니저를 지정



[@Transactional 전파 레벨](https://jsonobject.tistory.com/467)



## ChainedTransactionManager 

- 여러개의 트랜잭션 매니저를 하나로 묶어(Chain) 사용하는 방식

- 완벽한 트랜잭션을 제공하지 않는다 (완벽한 롤백 보장 X)

  - 정상적인 롤백

    ~~~
    - 트랜잭션1 (Tx1)
    - 트랜잭션2 (Tx2)
    
    Tx1 (Start) -> Tx2 (Start) -> Logic -> Tx2 (COMMIT) -> Tx1 (COMMIT) 
    
    
    Tx1 (Start) -> Tx2 (Start) -> Logic -> Tx2 (ROLLBACK) -> Tx1 (ROLLBACK) 
    ~~~

  - 비정상적인 롤백

    ~~~
    Tx1 (Start) -> Tx2 (Start) -> Logic -> Tx2 (COMMIT) -> Tx1 (ROLLBACK) 
    ~~~

    > Tx1이 문제가 생겨 Tx2도 롤백을 해야하지만, 이미 commit된 상황이기때문에 되돌릴 수 없다.

- 트랜잭션의 순서가 중요하다

  - 에러가 날 확률이 높은 트랜잭션을 후순위 chain으로 묶어서 안전한 트랜잭션을 구성한다.

[ChainedTransactionManager 데이터 소스 트랜잭션 연결하기](https://taes-k.github.io/2020/06/09/chained-transaction-manager/)

