# Input 상태 관리

> form에 input이 여러개 있을때 값을 어떻게 변경할 것인지



### input - onChange

~~~jsx
const onChange = (e) => {
        setState({...state, [e.target.name]: e.target.value});
};
~~~

> object의 값 변경시 `setState({...state, 'object-name': object-value})`



~~~jsx
const InputTest = () => {
    const [state, setState] = useState({
        select: "",
        textarea: "",
        
    }); 
    const options = [
        {value:"1", label:"1"},
        {value:"2", label:"2"},
        {value:"3", label:"3"},
        {value:"4", label:"4"},
    ]; // value, label
    
    const onChange = (e) => {
        console.log('inputTest -- value change', e.target.value);
        setState({...state, [e.target.name]: e.target.value});
        
    };

    return (
        <Card>
            <CardBody>
                <div className="card__title">
                    <h5 className="bold-text">Common Input Test</h5>
                </div>
                
                <Label>select</Label>
                <CommonInput template={"select"} model={state.select} disabled={false} options={options} change={onChange} placeholder={"select test"}/>
                
                <Label>textarea</Label>
                <CommonInput template={"textarea"} model={state.textarea} disabled={false} change={onChange} placeholder={"textarea test"}/>
                
            </CardBody>
        </Card>
    );
}

export default InputTest;
~~~



## useEffect + useCallback + useState

> 만들고자하는 내용
>
> `checkItems` 가 바뀔때 마다 `setParam` 의 내용을 업데이트 해준다

- 초기 코드

~~~jsx
// parent
const Parent = () => {
    const [param, setParam] = useState({});
    
    return(
        <Child param={param} setParam={setParam} />
    )
}
~~~

~~~jsx
// child
const Child = ({param, setParam}) => {
    const [checkItems, setCheckItems] = useState([]);
    
    useEffect(() => {
        setParam({...param, logType: checkItems.toString()});
        // checkItems가 변경될때마다 param의 값을 update 한다
    }, [checkItems])
}
~~~

~~~shell
// js waring
React Hook useEffect has missing dependencies: 'param' and 'setParam'. Either include them or remove the dependency array. If 'setParam' changes too often, find the parent component that defines it and wrap that definition in useCallback  react-hooks/exhaustive-deps
~~~

- 1차 시도

~~~jsx
// child
const Child = ({param, setParam}) => {
    const [checkItems, setCheckItems] = useState([]);
    
    useEffect(() => {
        setParam({...param, logType: checkItems.toString()});
        // checkItems가 변경될때마다 param의 값을 update 한다
    }, [checkItems, param, setParam])
}
~~~

> 무한 렌더링 되는 문제 발생

- 완성 코드

~~~jsx
// parent
const Parent = () => {
    const [param, setParam] = useState({});
    
    const setLogType = useCallback((data) => {
        setParam(prev => ({...prev, logType: data}));
    },[]);
    // prev => 를 사용해서 useCallback의 인자에 값을 안넣어줘도 된다
    
    return(
        <Child param={param} setParam={setParam} setLogType={setLogType}/>
    )
}
~~~

~~~jsx
// child
const Child = ({param, setParam, setLogType}) => {
    const [checkItems, setCheckItems] = useState([]);
    
    useEffect(() => {
        setLogType(checkItems.toString());
    }, [checkItems, setLogType])
}
~~~

> `Parent` 에 logType을 업데이트 하는 함수를 만들어주고, `useCallback` 을 이용한다
>
> `Child` 에서는 param에 넣어줄 값을 `setLogType` 에 넣어주면 된다