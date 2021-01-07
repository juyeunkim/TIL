#  자식 -> 부모 object 전달

> 리액트는 단방향 데이터 바인딩으로 부모 -> 자식으로 props를 전달한다





https://www.pluralsight.com/guides/how-to-pass-props-object-from-child-component-to-parent-component



## Example

~~~jsx
// parent

const Parent = () => {
    const [data, setData] = useState('init data');
    return (
        <>
        <h1>{data}</h1>
        <Child setData={setData}>
        </>
    );
};
~~~

~~~jsx
// child

const Child = () => {
    return (
        <>
        <button onClick={ () => setData("data from child")}> Click</button>
        </>
    );
    
};
~~~

> button을 클릭하면 view에서 `init data` -> `data from child` 로 수정됨

