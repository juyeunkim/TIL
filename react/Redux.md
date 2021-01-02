## Redux

> React 의 상태관리 (store)

### 특징

- 컴포넌트 코드로부터 상태코드를 분리

- 미들웨어를 활용한 다양한 기능 추가

  - 미들웨어 라이브러리 ( redux - saga)
  - 로컬 스토리지에 데이터 저장하기 및 불러오기

- SSR 시 데이터 전달이 간편하다

- React Context 보다 효율적인 렌더링 가능

- 데이터 흐름이 단방향

  

1. 단일 스토어

2. 읽기 전용 상태 :

3. 순수한 함수 
   인자값 : state, action
   인자값 이외의 값에는 의존X - 다른 함수를 호출하면 안된다. ex) axios, random 함수

   이전 상태는 건드리지 않고, 새로운 상태 객체를 반환



![Image for post](https://miro.medium.com/max/1121/0*Z18iLsM7Bf1xoNth.) 

> View 에서 상태값을 변경하고 싶으면 Action
>
> Action에서는 middleware or Reducer로 상태값의 로직을 처리
>
> Reducer가 새로운 상태값을 Store에 알려줌
>
> Store에서 변경된 데이터 or 액션이 처리가 되면 View에게 알려준다
>
> View는 이벤트를 받아서 화면을 갱신

### Action

> 프로젝트의 상태에 변화를 일으키는것
>
> 이름은 문자열 형태, 주로 대문자 - 고유한 이름
>
> 타입 속성값을 가지고 있는 객체

- 액션 타입

  ~~~js
  const TOGGLE_SWITCH = 'TOGGLE_SWITCH'
  const INCREASE = 'INCREASE'
  const DECREASE = 'DECREASE'
  ~~~

- 액션 생성 함수

  > 파라미터를 받아와서 액션 객체 형태로 만들어 준다
  >
  > `type` 을 반드시 지정해야한다 - 유니크하게 지정 

  ~~~js
  // { type : TOGGLE_SWITCH} :  액션 객체
  const toggleSwitch = () =>({ type : TOGGLE_SWITCH});
  const increase = () =>({ type : INCREASE, difference});
  const decrease = () =>({ type : DECREASE});
  ~~~

### Reducer

> 변화를 일으키는 함수
>
> 액션을 만들어서 발생시키면 리듀서가 현재상태와 전달받은 액션 객체를 파라미터로 받아온다.
> 두 값을 참고하여 새로운 상태를 만들어서 반환 

- immer 
  객체의 구조가 복잡해질때 사용
- combineReducers : 여러개의 reducer를 만들었을때 사용
  createStore를 사용할때는 한개의 reducer만 사용가능하므로 combineReducer 사용

#### immer

> 상태값을 수정하는 로직을 간편하게 사용가능

~~~js
import produce from 'immmer'

const person = {name: 'mike', age:22}
const newPerson = produce (person, draft => {
    draft.age = 32;
})
// produce (바꾸고싶은 state 값, 상태값 변경 로직)
~~~

#### createReducer

> immutable 자동으로 관리

~~~js
const reducer = createReducer (INITIAL_STATE, {
    [ADD] : (state, action) => state.todos.push(action.todo),
    [REMOVE_ALL] : state => (state.todos = []),
    [REMOVE] : (state, action) => 
    	(state.todos = state.todos.filter(todo => todo.id !== action.id)),
});
// createReducer (초기값, handlerMap)
~~~







### Store

> 프로젝트에 리덕스를 사용하기 위한 store 역할
>
> action 처리가 끝났다는 것을 외부에 알려주는 역할

- dispatch : 액션을 발생시키는 것

- subscribe : 액션이 디스패치되어 상태가 업데이트 될 때마다 호출
  react-redux 가 대신 사용

  ~~~js
  const store = createStore(reducer);
  
  let prevState;
  store.subscribe(() => {
      const state = store.getState();
      if(state === prevState) {console.log('상태값 같음')}
      else { console.log('상태값 변경')}
      
      prevState = state;
  })
  ~~~
  
  
  
- createStore : 스토어 생성

  ~~~js
  // index.js
  import {createStore} from "redux"
  
  const store = createStore(reducer);
// store에 reducer을 넣어서 호출 -> 스토어 생성
  ~~~
  
  

### Redux Code Sytle

1. 일반적인 구조

   > 각 폴더로 나눠서 관리

   - actions
     - counter.js
     - todos.js
   - constants
   - reducers
     - counter.js
     - todos.js

2. Ducks 패턴

   > 하나의 js 파일안에 action, constants, reducer 정의

   - modules
     - counter.js
     - todos.js



##  React-Redux

### Provider

> 리액트 컴포넌트에서 스토어를 사용할수 있도록 제공
>
> `props` 와 `state` 이외에 상태를 관리하는 속성 

~~~jsx
function App() {
  return (
    <Provider store = {store}>
      <Router />
    </Provider>
  );
}
~~~



### Container

> 리덕스 스토어와 연동된 컴포넌트

- connect : 컴포넌트를 리덕스와 연동

  > React의 컴포넌트는 Redux의 흐름을 타는게 불가능한데, `connect` 가 흐름 타는것을 가능하게 해준다

  - mapStateToProps : 리덕스 스토어 안의 상태를 컴포넌트의 props로 넘겨주기 위해 설정
  - mapDispatchToProps : 액션 생성함수를 컴포넌트의 props로 넘겨주기위해 사용

  #### 사용방법 2가지

  - 함수 미리 선언

  ~~~js
  const mapStateToProps = state => ({
      number : state.counter.number
  })
  
  const mapDispatchToProps = dispatch => ({
      increase : () =>{
          console.log('increase')
          dispatch(increase())
      },
      decrease : () =>{
          console.log('decrease')
          dispatch(decrease())
      },
  
  })
  
  
  export default connect(mapStateToProps,mapDispatchToProps)(CounterContainer);
  ~~~

  - 익명 함수

  ~~~js
  export default connect(
      state => ({
          number : state.counter.number,
      }),
      dispatch => ({
          increase: () => dispatch(increase()),
          decrease: () => dispatch(decrease()),
      }),
  )(CounterContainer);
  ~~~

==> connect 는 Old한 버전 

### useSelector 

> state를 가져온다
>
> redux store의 상태가 바뀌지 않으면 리렌더링 하지 않는다

- useSelector 최적화 하기

  1. 여러번  `useSelector` 사용

     ~~~js
     import { useSelector } from "react-redux";
     
     const number = useSelector(state => state.counter.number);
   const diff = useSelector(state => state.counter.diff);
     ~~~

  2. `equlityFn` 사용
  
     - 객체
     
     ~~~js
     import {shallowEqual} from 'react-redux'
     
     const { number, diff } = useSelector(
         state => ({
           number: state.counter.number,
         diff: state.counter.diff
         }),
      shallowEqual
       );
     // 일치하는 state만 찾아서 사용 - 그 부분만 리렌더링
     ~~~
     
     - 배열
     
     ~~~js
     const [friends, freinds2] = useSelector(state => [state.friend.friends,state.friend.friends2], shallowEqual)
     ~~~
     
     



### useDispatch

> actions를 가져온다

~~~jsx
import { useDispatch } from "react-redux";
import {addTodo} from "./module/todo"

function TodosContainer() {
  // useSelector 에서 꼭 객체를 반환 할 필요는 없습니다.
  // 한 종류의 값만 조회하고 싶으면 그냥 원하는 값만 바로 반환하면 됩니다.
  const todos = useSelector(state => state.todos);
  const dispatch = useDispatch();

  const onCreate = text => dispatch(addTodo(text));
  const onToggle = useCallback(id => dispatch(toggleTodo(id)), [dispatch]); // 최적화를 위해 useCallback 사용

  return <Todos todos={todos} onCreate={onCreate} onToggle={onToggle} />;
}
~~~



### reselect

> filter를 적용하고 싶을때
>
> 리덕스에 원본데이터를 저장하고, 컴포넌트에서 필터연산 수행

~~~jsx
// BEFO - filter 연산 계속 수행
const [
    ageLimit, showLimit, 
    friendsWithAgeLimit, friendsWithAgeShowLimit] = useSelector( state => {
        const {ageLimit, showLimit, friends } = state.friend;
        const friendWithAgeLimit = friends.filter( item => item.age <= ageLimit);
        // filter 연산은 불필요하게 계속 호출된다는 단점
        return [
            ageLimit, showLimit,
            friendWithAgeLimit, friendWithAgeLimit.slice(0, showLimit)
        ]
    }, shallowEqal)
~~~

~~~jsx
// AFTER - reslect 이용 (ageLimit이 변경될때만 filter을 이용하도록 적용)
// select.js
import {createSelector} from "reselect"

export const getAgeLimit = state => state.friend.ageLimit;
export const getShowLimit = state => state.friend.showLimit;
const getFriends  = state => state.friend.friends;

export const getFriendWithAgeLimit = createSelector(
    [getFriends, getAgeLimit],
    (friends, ageLimit) => friends.filter(item => item.age <= ageLimit),
);

export const getFriendWithAgeShowLimit = createSelector(
    [getFriends, getShowLimit],
    (friendWithAgeLimit, showLimit) => friendWithAgeLimit.slice(0, showLimit)
);

// 컴포넌트 내
const [
    ageLimit, showLimit, 
    friendsWithAgeLimit, friendsWithAgeShowLimit] = useSelector( state => [
        getAgeLimit(state),
        getShowLimit(state),
        getFriendWithAgeLimit(state),
        getFriendWithAgeShowLimit(state),
    ], shallowEqal)
// or useSelector 이용
const ageList = useSelector(getAgeLimit);
~~~



### redux-actions

> 리덕스 액션들을 관리할 때 유용 - createAction / handleActions

- createAction

  ~~~js
  // BEFORE
  export const increase = (input) => ({ type: CHANGE_INPUT}, input)
  // AFTER
  export const changeInput = createAction(CHANGE_INPUT, (input) => input);
  ~~~

- handleActions

  ~~~js
  // BEFORE
  function counter(state = initialState, action){
      switch(action.type){
          case INCREASE:
              return{
                  number: state.number+1  
              };
          default:
              return state
      }
  }
  
  // AFTER
  const counter = handleActions(
      {
          [INCREASE]: (state, action) => ({
              number: state.number+1
            }),
      },
      initialState
  }
  
      
  ~~~



## Redux 비동기처리

### Redux-thunk

> 비동기 로직이 간단할때, 가장 간단하게 시작 가능

### Redux-observable

> 비동기 코드가 많을때
>
> 진입장벽이 높다 - 리액티브 프로그래밍을 공부

### Redux-saga

> 비동기 코드가 많을때
>
> 제너레이터를 적극적으로 활용
>
> 테스트 코드 작성이 쉽다.





## API 설명

### compose

> 여러개의 함수를 묶는 역할
> 여러개의 redux를 묶어서 사용할 수 있도록 하는 함수

