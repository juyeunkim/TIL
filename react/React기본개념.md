# React 기본 개념

> React는 라이브러리 ! - 오직 뷰만 담당
>
> 라우터와 같이 웹을 만드는게 필요한 도구들이 포함되어있지 않는다

- 특징 3 가지

1. Component
2. JSX
3. Virtual DOM



## Component

> UI를 구성하는 개별적인 뷰 단위
>
> 여러가의 컴포넌트를 통해 하나의 웹 페이지 완성



## JSX

> 자바 스크립트의 확장 문법, XML과 유사
>
> React 을 위한 자바스크립트 문법
>
> 브라우저에서 실행되기 전에 코드가 번들링되는 과정에서 바벨을 사용하여 일반 자바스크립트의 형태 코드로 변환

- 특징

1. 닫혀야 하는 태그
   단일 태그시 < /> 사용

2. 감싸져있어야하는 엘리먼트
   두개 이상의 엘리먼트는 하나의 엘리먼트로 감싸야한다

   - div로 감싸는 경우

   ~~~jsx
   class App extends Component {
     render() {
       return (
           <!-- 안되는 경우 -->
   		<div> Hello  </div>
   		<div> World !</div> 
   
   		<!-- 되는 경우 -->
   		<div>
       		<div> Hello  </div>
			<div> World !</div>
   		</div>
 	);
     }
   }
   ~~~
   
   - Fragment 사용
   
     > 여러개의 요소를 반환할 때 일종의 key 역할을 한다
     >
     > 실제 돔에는 반영 X
     >
     > <> </>  과 동일
   
   ~~~jsx
   import React, { Component, Fragment } from ‘react’;
   
   class App extends Component {
     render() {
       return (
         <Fragment>
           <p> Hello world</p>
        <p> Have a Nice day :)</p>
         </Fragment>
       );
     }
   }
   ~~~
   
3. JSX 안에 자바스크립트 사용

   ~~~jsx
   import React, { Component } from 'react';
   class App extends Component {
     render() {
       let name = 'react';
       return (
         <div>
           <p>Hi {name}!</p>
         </div>
       );
     }
   }
   export default App;
   ~~~

4. CSS : 인라인 스타일

   ~~~jsx
   // 이때, 카멜케이스 사용
   const styles = {
   	        backgroundColor: 'black',
   	        padding: '16px',
   	        color: 'white',
   	        fontSize: '12px'
   	    };
   
   const Temp = () => {
       return (
           <h1 style = {{ style }}> HI </h1>
       )
   }
   ~~~

   ~~~jsx
   <Card className="homgraph-div"
              style={{
                margin: "0 auto",
                display: "block",
                width: "50%",
                marginBottom:"30px"
              }}
   >
   ~~~

   

5. html의 class 는 `className`

   ~~~html
   <div className="App">
       Hello World
    </div>
   ~~~
   
6. 조건부 렌더링 `{}` 
   

## Virtual DOM

### DOM (Document Object Model)

> 객체로 문서 구조를 표현하는 방법
>
> 트리 형태라서 특정 노드를 원하는 곳에서 수정, 삭제, 삽입이 좋다
>
> 웹 브라우저는 DOM을 이용하여 객체에 자바스크립트와 CSS를 적용

- 단점 : 
  - 동적 UI에 최적화 되어있지 않다. (HTML - OK , Javascript - BAD )
    규모가 큰 애플리케이션에서 DOM에 직접 접근하여 변화를 주면 성능 이슈가 발생



### Virtual DOM

> 실제 DOM에 접근하여 조작하는 대신에 추상화한 자바스크립트 객체를 구성

- 절차
  1. 데이터를 업데이터 하면 전체 UI를 Virtual DOM에 리렌더링
  2. 이전 Virtual DOM에 있던 내용과 현재 내용 비교
  3. 바뀐 부분만 실제 DOM에 적용



## React.memo

> 속성값이 변할때만 렌더링

~~~jsx
import React from 'react'

function Title({title}){
    return <p> {title} </p>
}

export default React.memo(Title);
~~~

> title 값이 변할때만 화면이 렌더링



## createPortal

> 분리된 dom을 만들어 주고 싶을때 사용 -> 주로 모달

~~~jsx
export default function App() {
    return (
        <>
        	{ReactDOM.createPortal(
             <div>
                 <p>Hello</p>
             </div>,
             document.getElementById('somthing')
             )}
        </>
    )
}
~~~

![img](https://lh6.googleusercontent.com/BiF7Y9MXJm9YYfARuxV5ehNwyVLK82_oFeVMss6L_AZ_kTJyPNjtJfHceNFcJTWYRAuO3wh6e6nd72byWA5BZVjdikmrHpAbETF4VAZHMzSh7P2sJCgXWqfCupUADVk9vSDleyZm)

![img](https://lh3.googleusercontent.com/LVGkrtkrClnuonSDYiM6BLLQV1ABGabsKO7aVcELCtBhRMtbD0QdW-QgjiBUf3cKpzMgFDDJzBFGSdI3aMf8lX0790wO8XpJO5w2tqVNj7N7P5uKkIFb8NrXDPGNMZSdhH7P9QsA)





## Props와 State

단방향 데이터 흐름을 가진다 : 
부모에서 자식에게로만 데이터 전달가능 - 부모에게 받은 데이터를 자식이 수정할 수 없다.![Image for post](https://miro.medium.com/max/600/0*rM4xTWZQXWXUngBJ)

#### props 

> 부모 컴포넌트가 자식 컴포넌트에 데이터를 전달

~~~jsx
// app.js
class App extends Component {
    render(){
        return (
            <MyComponent name = "리액트">
        )
    }
}
~~~

~~~jsx
// MyComponent.js
class MyComponent extends Component {
    render(){
        const {name} = this.props;
        return (
            <>
            	부모에서 온 데이터 : {name}
            </>
        )
    }
}
~~~



#### state

> 컴포넌트 내에서 동적으로 변동되는 데이터 관리
> 기본 값을 미리 설정해야 사용 가능

- state vs setState()

  > setState는 비동기적으로 state를 변화시킨다 (콜백함수)
  > 콜백함수가 실행된 후 리렌더링

- 주의할점

  1. 직접 state 수정 X -- setState 을 이용하여 수정
     state는 constructor에서 지정

     ~~~js
     // Wrong
     this.state.comment = 'Hello';
     
     // Correct
     this.setState({comment: 'Hello'});
     ~~~

  2.  state 업데이트는 비동기적

     ~~~js
     // Wrong
     this.setState({
       counter: this.state.counter + this.props.increment,
     });
     
     // Correct
     // 첫번째 state를 수행하고 나서 props를 수행한다.
     this.setState((state, props) => ({
       counter: state.counter + props.increment
     }));
     ~~~

  3. state 업데이트는 병합
     state 안에 여러개의 인자가 있을수 있고, 독립적으로 setState 사용 가능



#### constructor

> Component를 생성할 때, state 값을 초기화 시키거나 메서드를 바인딩할 때 사용
> 해당 Component가 마운트 되기 전 호출

~~~js
constructor(props){
    super(props); 
	// 자식 Component에서 this.props 사용시 생성자 내에서 정의되지 않아 
    // 버그 발생가능성이 있기에 이 포맷사용
    this.state = {} // 초기화 시키고 싶은 내용 
}
~~~

- 구독 작업이나, 외부 API 호출 금지



## 라이프 사이클

#### ![img](https://media.vlpt.us/post-images/bclef25/47401c40-2939-11ea-8dbe-d351c8f8dc69/image.png)1. mount

- contructor
- componentWillMount : DOM 노드에 추가하기 직전에 호출되는 메소드
  render가 호출되기 이전이므로 setState를 사용해도 render가 호출하지 않는다
- render
- componentDidMount

#### 2. update

- componentWillReceiveProps : 컴포넌트 생성후 첫 렌더링 마친후 호출되는 메소드
  props를 받아서 state를 변경해야하는 경우에 유용
- shouldComponentUpdate : 컴포넌트 변경전에 호출되는 메소드
  props 또는 state가 변경되었을때, 재렌더링 여부를 return 값으로 결정
- componentWillUpdate :
  새로운 props 또는 state가 반영되기 직전 변경 된 값들을 받는다

#### 3. unmount



## Component와 Props

### 함수 컴포넌트와 클래스 컴포넌트

#### 함수 컴포넌트

~~~jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
~~~

- Hook 사용

#### 클래스 컴포넌트

~~~jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
~~~

- constructor에서 state 설정
- render를 통해서 화면 렌더링

  > 순수 함수로 작성
  >
  > 랜덤 함수 사용 X
  >
  > 외부 상태 변경 X



### 비교

- 함수 컴포넌트 
  선언하기 쉽고, 메모리 자원도 덜 사용
  빌드 후 배포할 때도 메모리 자원

### 함수 컴포넌트 -> 클래스 컴포넌트로 변경

![image-20200805171015818](C:\Users\juyeunkim\AppData\Roaming\Typora\typora-user-images\image-20200805171015818.png)

1. function -> class 로 변경 : extends React.Component
2. render 함수 안에 return 내용 저장
3. props -> this.props로 변경





## 이벤트

#### bind

> this를 가르키는 context를 변경하여 바로 실행시켜주는 메소드
> 메소드의 재사용과 공유 그리고 중복을 방지함

- 언제 써야하는지
  다른 컴포넌트로 pass 할때 메소드만 bind

- bind 방법

  1. `render` 안에서 

     ~~~jsx
     render() {  
         return <div
             onClick={ this.update.bind( this ) }
         />
     }
     ~~~

  2. `constructor` 안에서

     ~~~jsx
   class Home extends React.Component {
     
         constructor() {
             super()
             this.update = this.update.bind(this);
         }
     }
     ~~~
  
  3. `autobind-decorator` 

     ~~~jsx
   import autobind from 'autobind-decorator'
     
     class Home extends React.Component {  
         @autobind
         update() {
             ...
         }
     }
     ~~~
  
     





## Context API

>   props로 데이터를 전달하는 대신에, 전역 변수를 선언해서 사용
>
> ==> 간단하다

### createContext을 통해 contextAPI 호출

~~~jsx
import React, {createContext} from "react"

const UserContext = createContext('unknown') // 초기값 넣어서 호출
// UserContext는 Provider와 Consumer 사용가능

export default function App() {
    return (
        <>
        <UserContext.Provider value = "mike">
            <Profile />
        </UserContext.Provider>
        </>
    )
} 

function Profile () {
    return (
       <Greeting />
    )
}

function Greeting () {
    return (
        <UserContext.Consumer>
            {username => <p> {`${username}님 안녕하세요`} </p>} // render props 패턴
        </UserContext.Consumer>
    )
}
~~~

> Provider에서 value로 넣어주면
> Consumer에서 그 값을 받아서 처리할 수 있다. -> 가장 가까운 Provider에서 value를 가져온다
> 																					 만약 Provider를 찾기 못한다면 초기값으로 사용

- render props 패턴이란?

  > children을 함수형태로 작성





### CORS vs Proxy

CORS는 모든 도메인을 허용

proxy : 특정 도메인만 허용



![img](https://i.imgur.com/8qNJaoI.png)





### localStorage vs redux-persist

localStorage는 String 형태만 저장 가능



## 참고 사이트

[React 공식 문서](https://ko.reactjs.org/docs)

