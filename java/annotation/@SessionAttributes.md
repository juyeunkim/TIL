# @SessionAttributes

- `Model`의 정보를 `HttpSession` 에 저장

- SessionAttributes 어노테이션에 정의된 Key와 동일한 key로 Model에 값을 Set해주는 행위가 있으면 자동으로 Session에 저장

- @SessionAttributes 선언된 클래스 내에서 세션 스코프가 설정

  - ~~~~java
    @SessionAttributes(value = "contentId")
    public class AController {
        @RequestMapping(value = "/test", method = RequestMethod.GET)
        public String test (Model model){
            List<Long> boardSession = (List<Long>) model.getAttribute("contentId");
            // 세션에 저장된 contentId 를 가져옴
        }
    }
    ~~~~

    ~~~java
    public class BController {
        @RequestMapping(value = "/test2", method = RequestMethod.GET)
        public String test (Model model){
            List<Long> boardSession = (List<Long>) model.getAttribute("contentId");
            // null
        }
    }
    ~~~

- `@ModelAttribute` 는 세션에 있는 데이터도 바인딩한다

  - 세션에 같은 이름의 오브젝트가 존재하는지 확인
  - 존재한다면 세션에있는 오브젝트를 가져와 `@ModelAttribute` 파라미터로 전달해 줄 오브젝트 사용
  - 존재하지 않는다면 오브젝트를 새로 만들어서 오브젝트로 사용

- SessionAttributes는 보통 `SessionStatus` 와 함께 사용

  - Session을 사용한 후에는 SessionStatus.setComplete() 메서드를 활용하여 세션을 할당 해지한다

  - ~~~java
    @Controller
    @SesstionAttributes("test")      //SessionAttribute 사용 보통 sessionStatus와 함께 사용한다
    public class ExampleController {
    
    		@GetMapping("/events")
            //SessionStatus를 활용
            public String createEvent(@Validated @ModelAttribute Person person,SessionStatus sessionStatus){
            		Persion person = new Persoon();
                    person.setName = ("gd");
                    model.addAttribute("person",person);
                    
                    sessionStatus.setComplete();        //해당 페이지에서 저장된 세션 값 초기화(완성)
                    return "/person/form";
            }
    }
    ~~~

- 

### 기존 예제 (HttpSession)

> HttpSession에 직접 값을 저장한다

~~~java
@Controller
@RequestMapping
public class SampleController {

    @GetMapping("/events")
    @ResponseBody
    public String hello(Model model, HttpSession httpSession) { // HttpSession을 가져온다.
        Event event = new Event();
        event.setName("goodGid");
        model.addAttribute("event", event);
        httpSession.setAttribute("event", event); // HttpSession 직접 저장
        return "hello";
    }
}
~~~



### 예제

- Controller

~~~java
@Controller
@RequestMapping
@SessionAttributes("event")
public class SampleController {

    @GetMapping("/events")
    @ResponseBody
    public String hello(Model model) {
        Event event = new Event();
        event.setName("goodGid");
        model.addAttribute("event", event); // @SessionAttributes("event") 코드에 의해 
                                            // 동시에 HttpSession에도 저장된다.
        return "hello";
    }
}
~~~

- TC

~~~java
@Test
public void helloTest() throws Exception {
    mockMvc.perform(get("/events"))
            .andDo(print())
            .andExpect(status().isOk())
            .andExpect(request().sessionAttribute("event", notNullValue()));
}
~~~

> Model에 저장하는 동시에 HttpSession에도 저장



[@SessionAttributes](https://goodgid.github.io/Spring-MVC-SessionAttributes/)
