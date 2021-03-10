# useState

> 비동기 - 즉시 반영이 되지않는다



## object push

~~~js
setTheObject(prevState => ({ ...prevState, currentOrNewKey: newValue}));
~~~



## array push (맨끝에 추가)

1. element 

   ~~~js
   setTheArray(prevArray => [...prevArray, newValue])
   ~~~

2. object

   ~~~js
   setTheArray(prevState => [...prevState, {currentOrNewKey: newValue}]);
   ~~~

   ~~~js
   setTheArray([...prevState, {currentOrNewKey: newValue}]);
   ~~~

   > 추천 하지 않는 방법 :  deep copy가 안되서 불변성(immutable) 발생할 수 있음

3. element of object

   > object의 특정 element를 수정하는 경우

   ~~~js
   setTheArray(prevState => [...prevState, {[name]: newValue}]);
   ~~~

   

## array index Object변경

~~~js
let newItem = [...params];
newItem[i] = {...newItem[i], [name]:value};
setParam(newItem);
~~~



