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

