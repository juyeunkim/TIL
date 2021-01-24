# DI



## DI 방법 세가지

> 의존 관계가 실행중에 동적으로 변하는 경우는 거의 없으므로 `생성자 주입`을 권장함

1. 필드 주입

   > 중간에 바꿀수 있는 방법이 없다
   > IntelliJ 에서 사용시 - warning 경고가뜸 (생성자 주입 권장)

   ~~~java
   @Autowired
   private MemberService memberService;
   ~~~

2. setter 주입

   > 누군가가 컨트롤러를 호출할 때, setter 가 `public` 으로 열려있어야한다

   ~~~java
   @Autowired
   public void setMemberService(MemberService memberService){
     this.memberService = memberService;
   }
   ~~~

3. 생성자 주입

   ~~~java
   @Autowired
   public MemberController(MemberService memberService){
     this.memberService = memberService;
   }
   ~~~

   



