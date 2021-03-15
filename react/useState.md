# useState

- 비동기 - 즉시 반영이 되지않는다

  [값 즉시 업데이트하는 방법](https://linguinecode.com/post/why-react-setstate-usestate-does-not-update-immediately)



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



### useState값 바로 업데이트 하기

> `useState` 는 비동기라서 값을 바로 업데이트하지 않는다
>
> setXXX로 값을 넣었어도 console.log로 출력했을때 원하는 결과가 나오지 않음
>
> ▶ `useEffect` 를 사용한다

- 예시

> model ( `Object`) 저장된 내용을
>
> data 의 param에 string으로 저장하고 싶다

~~~~jsx
setModel({ ...model, [newItem[i].name]: newItem[i].value });
~~~~

~~~jsx
useEffect(() => {
        setData(data => ({ ...data, param: JSON.stringify(model) }));
}, [model])
~~~

> setModel로 Object의 내용을 `Update` 해주면 useEffect 함수가 호출되고,
>
>  data.param에 바뀐 내용이 바로바로 업데이트 된다

