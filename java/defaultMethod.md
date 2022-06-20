# default 메서드와 static 메서드

> 자바 8 이후 인터페이스에 대한 정의 변경

1. 정적 메소드 : 인터페이스 내부
2. 디폴트 메서드 : 인터페이스의 기본 구현을 제공



## default 메소드

- 인터페이스에 새로운 기능이 추가 or 변경 된다면? 
  - 해당 메소드를 모든곳에서 구현해야하는 문제 발생 !
  - 
  -  `default method` 가  <u>인터페이스에 메소드를 구현해 놓을수 있도록 함</u>
    - 인터페이스의 기본 구현을 그대로 상속하므로 인터페이스에 자유롭게 새로운 메서드를 추가할 수 있다
- 기존의 코드 구현을 바꾸지 않으면서 인터페이스를 바꿀수 있다.
  - 클래스에서 이 메서드를 구현할 필요 없이 즉시 접근이 가능하다.



~~~
참고]
- 함수형 인터페이스는 오직 하나의 추상 메서드만을 포함
  - default 메소드 는 몇개를 선언해도 상관이 없다.
    - 추상 메서드에 default 메서드는 포함하지 않음
~~~





### 활용

- 선택형 메서드
  - 구현하는 클래스에서 메서드를 구현하였지만 body가 비어있는 경우
    - default 메서드에서 기본 구현을 제공하면 구현하는 클래스에서 빈 구현을 할 필요가 없다.
- 다중상속 가능





### defult 메서드의 세가지 규칙

1. 클래스가 항상 이긴다. 
   클래스나 슈퍼 클래스에서 정의한 메서드가 디폴트 메서드보다 항상 우선권을 갖는다.
2. 1번 규칙 이외의 상황에서는 서브 인터페이스가 이긴다.
   상속관계를 갖는 인터페이스에서 같은 메서드를 정의할 때는 서브 인터페이스가 이긴다.
3. 여전히 데폴트 메서드의 우선순의가 결정되지 않았다면 여러 인터페이스를 상속받는 클래스가 명시적으로 디폴트 메서드를 오버라이드하고 호출해야한다.





#### 예제1

~~~java
public interface Calculator {
  public int plus (int i, int j);
  public int muliple (int i, int j);
  default int exec(int i, int j){
    return i+j;
  }
  public static int exec(int i, int j){
    return i*j;
  }
}


// 인터페이스에서 정의한 static 메소드는 반드시 인터페이스명.메소드 형식으로 호출해야한다.
public class MyCalculationExam {
  public static void main(String[] args){
    Calculator cal = new MyCalculator();
            int value = cal.exec(5, 10);
            System.out.println(value);

            int value2 = Calculator.exec2(5, 10);  //static메소드 호출 
            System.out.println(value2);
    
  }
}
~~~

> 인터페이스에 static 메소드를 선언함으로써, 인터페이스를 이용하여 간단한 기능을 가지는 유틸리티성 인터페이스를 만들 수 있게 되었다.



### 예제 2

> 인터페이스 A, B 
>
> - Hello() 디폴트 메서드를 가짐
>
> 클래스 C
>
> 이때, C 에서 hello() 메서드를 호출한다면 A,B 중에서 어떤 메서드를 실행해야할까?
>
> ==> 직접 명시

~~~java
public class C implements B, A {
  void hello() {
    B.super.hello(); // B.hello 를 사용한다고 명시해야한다.
  }
}
~~~

> 직접 메서드를 명시해줘야한다.







[자바8의 defulat 메서드 등장](https://devfunny.tistory.com/350)