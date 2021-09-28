# Stream



## IntStream

- IntStream to array

~~~java
int[]arr = IntStream.range(1,5).toArray();
~~~

- IntStream to arrayList

~~~java
IntStream ints = Arrays.stream(new int[] {1,2,3,4,5});       
List<Integer> intsList = ints.map(x-> x*x)
          .collect(ArrayList<Integer>::new, ArrayList::add, ArrayList::addAll);
~~~

~~~java
IntStream.of(1, 2, 3).collect(ArrayList::new, List::add, List::addAll);
~~~

~~~java
MutableIntList list = 
    IntStream.range(1, 5)
    .collect(IntArrayList::new, MutableIntList::add, MutableIntList::addAll);
~~~

~~~java
List<Integer> intList = myIntStream.mapToObj(i->i).collect(Collectors.toList());
~~~





## Collection.forEach vs Stream.forEach

1. Stream 객체 사용 여부

2. parallelStream

3. 동시성 문제
4. 성능



![img](https://pbs.twimg.com/media/B_AmQW7WwAE_Akt.jpg)

### Stream 객체 사용 여부

- Collection.forEach
  - 객체를 따로 생성하지 않고 메서드 호출
  - forEach는 Iterable 인터페이스의 default 메서드
    Collection 인터페이스는 Iterable을 상속 
-  Stream.forEach
  - `stream()` 으로 Stream 객체를 생성해야한다
  - stream()은 Collection 인터페이스의 default 메서드
  - forEach에서 Stream 객체를 생성하고 버려지기 때문에 <u>오버헤드</u>가 존재
    - filter, map등의 Stream 기능을 함께 사용할때만 사용
    - 나머지 단순 반복이 목적이라면 Collection.forEach 사용



### parallelStream

- Stream으로 메서드로 생성한 Stream.forEach를 했을땐 Collection.forEach와 유사하게 동작
- `parallelStream()` 을 사용하면 다르다
  - 여러 스레드에서 스트림을 실행하여, 
    실행 순서가 매번 달라지며 예측 불가능

~~~java
List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5);
System.out.println("Collection.forEach 출력 시작");
nums.forEach(System.out::println);

/*
결과
1
2
3
4
5
*/
~~~

~~~java
List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5);
System.out.println("Stream.forEach 출력 시작");
nums.parallelStream().forEach(System.out::println);

/*
결과
3
4
1
5
2
*/
~~~



### 동시성 문제

- Collection.forEach

  - synchronized O

    - 락이 걸려있어서 멀티스레드에 안전

  - 수정을 감지한 즉시 `ConcurrentModificationException` 을 던지며 프로그램 종료

  - ~~~java
    @Test
    void test() { 
        List<Integer> nums = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6));
        Consumer<Integer> removeIfEven = num -> { 
            System.out.println(num); 
            if (num % 2 == 0) { 
                nums.remove(num); 
            } 
        }; 
        assertThatThrownBy(() -> nums.forEach(removeIfEven))
            .isInstanceOf(ConcurrentModificationException.class); 
    }
    ~~~

    > 1,2 까지 나옴
    > ※ ConcurrentModificationException : 오브젝트에 대해 허가되지 않는 변경이 동시적으로 이루어질때 발생

    

- Stream.forEach

  - synchronized X

    - 반복 도중에 다른 스레드에 의해 수정될 수 있고, 무조건 요소의 끝까지 반복을 돈다
    - 일관성 없는 동작이 발생하고 예상치 못한 에러가 발생할 확률이 높다

  - 무조건 리스트를 다 돌고 나서 예외를 던진다

  - ~~~java
    @Test
    void test() { 
        List<Integer> nums = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6));
        Consumer<Integer> removeIfEven = num -> { 
            System.out.println(num); 
            if (num % 2 == 0) { 
                nums.remove(num); 
            } 
        }; 
        assertThatThrownBy(() -> nums.stream().forEach(removeIfEven))
            .isInstanceOf(ConcurrentModificationException.class); 
    }
    ~~~

    > 1,2,4,6, null





### Stream.forEach

- Stream을 자체를 강제적으로 종료시키는 방법은 없다

  - 강제적인 종료조건 로직이 있다면, 기존 for-loop에 비해 비효율 발생

  ~~~java
  //for-loop
  for (int i = 0; i < 100; i++) {
      if (i > 50) {
        break;
        //50번 돌고 반복 종료
      }
      System.out.println(i);
  }
  
  // Stream
  IntStream.range(1, 100).forEach(i -> {
      if (i > 50) {
        return;
        //각 수행에 대해 다음 수행을 막을 뿐, 100번 모두 조건을 확인한 후 종료
      }
      System.out.println(i);
  });
  ~~~

- 사용 목적

  - **최종 연산**으로 사용

    - 최종 연산중 기능이 가장 적고, 가장 **덜** 스트림답기 때문에, 
      스트림 계산 결과를 보고할때 ( 주로 `print` ) 만 사용하고 계산하는 데는 안쓰는게 좋음
    - 내부에 로직이 하나라도 더 추가되면 동시성 보장이 어려워지고 가독성이 떨어질 위험이 있다
    - 중간 연산(filter, map, sort) 에서 충분히 로직이 수행 가능함

    



[Collection.forEach와 Stream.forEach 비교](https://dundung.tistory.com/247)

[Steam의 foreach와 for-loop는 다르다](https://tecoble.techcourse.co.kr/post/2020-05-14-foreach-vs-forloop/)

[for-loop를 Stream.forEach()로 바꾸지말아야할 3가지 이유](https://homoefficio.github.io/2016/06/26/for-loop-%EB%A5%BC-Stream-forEach-%EB%A1%9C-%EB%B0%94%EA%BE%B8%EC%A7%80-%EB%A7%90%EC%95%84%EC%95%BC-%ED%95%A0-3%EA%B0%80%EC%A7%80-%EC%9D%B4%EC%9C%A0/)









[자바8 Streams API를 다룰때 실수하기 쉬운 10가지](https://hamait.tistory.com/547)
