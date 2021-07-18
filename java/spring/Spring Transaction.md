# Spring Transaction



## Spring Reactor와 함께 사용하는 경우

Reactor에서 특정 작업 후 `map()` 같은 함수를 활용해서 다른 작업을 하면 일반적으로 그 작업은 map() 를 호출한 쓰레드와는 다른 쓰레드에서 실행

▶ 트랜잭션이 종료된 환경에서 실행

▶~~`@Transactional` 을 지원하지 않음~~ 

### 해결 방법

1. 트랜잭션이 종료된 이후의 사용할 entity를 트랜잭션이 종료되기 전에 미리 loading

2. Mono.map() 안에서 트랜잭션을 열고 새로운 entity 를 가져와서 사용

   ~~~java
   @RestController
   public class TestController extends BaseController{
   	@Autowired
   	WordsMapper WordsMapper;
   	
   	@Autowired
       private TransactionTemplate transactionTemplate;
   	
   	// Mono<Result> 리턴하는 과정에서 트랜잭션이 제대로 걸리는지 체크
   	@RequestMapping(value="/public/transaction/test", method=RequestMethod.POST)
   	@Transactional(propagation=Propagation.REQUIRED, rollbackFor=Exception.class) // <- 해당 기능 동작 X
   	public Mono<Result> transactionTest() throws Exception{
   		return Mono.just(new Result())
   				.map( res -> {
   					// 트랜잭션이 종료된 환경에서 실행
   					// 새로운 트랜잭션 설정					
   					transactionTemplate.execute(new TransactionCallbackWithoutResult() {
   						
   						@Override
   						protected void doInTransactionWithoutResult(TransactionStatus status) {
   							Words word = new Words();
   							word.setWord("테스트test");
   							WordsMapper.insertWord(word);
   							
   							 ServerError.ERR_AUTH_RECOVERY_EMAIL_DUPLICATED.throwSelf();
   							
   						}
   					});
   					
   					
   					return new Result();
   				});
   		
   	}
   
   }
   ~~~

   > `transactionTemplate` 을 통해서 새로운 트랜잭션을 만들어준다

[Spring Transaction 사용시 주의할 점](https://suhwan.dev/2020/01/16/spring-transaction-common-mistakes/)

