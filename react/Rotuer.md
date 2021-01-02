## react-router-dom

> 요청에 맞는 컴포넌트를 매칭

- react-router vs react-router-dom vs react-router-native
  react-router(통합)		  -react-router-dom (웹)
  							  			- react-router-native (react-native를 활용한 앱개발)

~~~shell
npm install react-router-dom
~~~



#### BrowserRotuer

- BrowserRouter vs HashRouter
  - BrowserRouter : 히스토리 API
  
    `<Link to="">`
  
  - HashRouter : URL의 해시를 이용, 정적인 페이지에 적합

#### Route

> 요청받은 pathname에 해당하는 컴포넌트를 렌더링

~~~jsx
<Route path = "/aladin" component = {Aladin} />
~~~

==> /aladin이라는 url과 Aladin이라는 컴포넌트로 이동

#### Switch

> path 충돌이 일어나지 않게 Route들을 관리
> 요청에 의해 매치되는 <Route> 들이 다수 있을때 <u>처음 매칭</u>되는 Route만 선별

~~~jsx
<BrowserRouter>
    <Swtich>
    	<!-- 정확하게 / 일때만 매칭 : exact -->
    	<Route exact path="/" component={Home} /> 
    	<Route path="/login" component={Login} />
        <Route path="/Temp/test" component={Temp1} /> <!-- /Temp 호출시 여기로 이동 -->
        <Route path="/Temp" component={Temp2} />
	</Swtich>
</BrowserRouter>
~~~



#### Link

> 해당 URL로 이동





## React Rotuer - Redirect

### Component 이용

> react-router의 `Link` 이용

~~~jsx
import React from 'react'; 
import { Link } from 'react-router'; 

const FilterLink = () => ( 
    <Link to='/filter'> {children} </Link> 
); 

export default FilterLink;
~~~



### history props 이용

> 함수를 만들어서 url 이동

~~~jsx
import React from 'react'; 

class Register extends React.Component { 
    handleNext = () => { 
        this.props.history.push('/'); 
    } 
    
    render() {...} 
             
 } 

export default Register;
~~~



### withRotuer 이용

> 하위컴포넌트에서는 history를 접근 할수 없기 때문에
> 상위 컴포넌트의 Router의 history 객체와 연결

~~~jsx
import React from 'react'; 
import { withRouter } from 'react-router-dom' 

class Register extends React.Component { 
    handleNext = () => { 
        this.props.history.push('/'); 
    } 
    
    render() {...} 
} 

export default withRouter(Register);
~~~



### 제한적 경로 접근

https://velog.io/@public_danuel/Restrict-Route

