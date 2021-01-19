# ui-router

> AngularJS SPA 라우팅 프레임 워크



### Get Started

~~~~sh
npm install --save @uirouter/angularjs
~~~~



### Add script tags

~~~html
<head>
  <script src="lib/angular.js"></script>
  <script src="lib/angular-ui-router.js"></script>
  <script src="helloworld.js"></script>
</head>
~~~

> 이때, `angular.js` 가 `angular-ui-router` 보다 위에있어야한다 



### 예제

> `template` 사용

- index.html

~~~html
<html>
  <head>
    <script src="lib/angular.js"></script>
    <script src="lib/angular-ui-router.js"></script>
    <script src="helloworld.js"></script>

    <style>.active { color: red; font-weight: bold; }</style>
  </head>

  <body ng-app="helloworld">
    <a ui-sref="hello" ui-sref-active="active">Hello</a>
    <a ui-sref="about" ui-sref-active="active">About</a>

    <ui-view></ui-view> <!-- 해당 부분에 url이 활성화된 template이 보여진다 -->
  </body>
</html>
~~~

- helloworld.js

~~~js
var myApp = angular.module('helloworld', ['ui.router']);
// helloworld는 main module
// ui.router는 helloworld에 속해있는 module

myApp.config(function($stateProvider) { // module method 의존성 주입
  var helloState = {
    name: 'hello',
    url: '/hello',
    template: '<h3>hello world!</h3>'
  }

  var aboutState = {
    name: 'about',
    url: '/about',
    template: '<h3>Its the UI-Router hello world app!</h3>'
  }

  $stateProvider.state(helloState);
  $stateProvider.state(aboutState);
});
~~~



### 예제2

> `component` 사용

- hellosolarsystem.js

~~~js
var myApp = angular.module('hellosolarsystem', ['ui.router']);

myApp.config(function($stateProvider) {
  var helloState = {
    name: 'hello',
    url: '/hello',
    component: 'hello' // template 대신에 component
  }

  var aboutState = {
    name: 'about',
    url: '/about',
    component: 'about'
  }

  states.forEach(function(state) {
    $stateProvider.state(state);
  });
});
~~~

- about.js

~~~js
angular.module('hellosolarsystem').component('about', {
  template:  '<h3>Its the UI-Router<br>Hello Solar System app!</h3>'
})
~~~

> `.component` 으로 template을 정의한다



### 예시 3

> `parent-child` 관계로 작성

~~~js
var personState = { 
  name: 'people.person', // people의 child
  url: '/{personId}', 
  component: 'person',
  resolve: {
    person: function(people, $stateParams) {
      return people.find(function(person) { 
        return person.id === $stateParams.personId;
      });
    }
  }
}
~~~

![Changes to Person state definition](https://ui-router.github.io/assets/tutorial/ss-to-galaxy-diff.png)

## 용어 정리

### $stateProvider

> url, template, controller를 정의



### $stateChangeStart

> URL의 상태가 변경되면 `$stateChangeStart` 이벤트 발생

- 예제

~~~js
// app.js
(function(){
  'use strict';

  angular.module('app', [
    'ui.router'
  ])
  .run(function ($rootScope, $state) {

    $rootScope.$on('$stateChangeStart', function (event, toState, toParams, fromState, fromParams) {
      // 이동할 페이지에 authenticate 값이 있는지 확인해서 라우팅한다.
      if( toState.authenticate ){
        $state.transitionTo('signin');
        event.preventDefault();
      }

    });
  });

})();
~~~



### resolve

> SPA 내에서 서버 API에서 데이터를 가져 올 때,  `promise`로 라우터가 활성화 되기 전에 필요한 데이터를 가져온다

- 예제 - 데이터 리스트 가져오기

~~~js
var peopleState = {
  name: 'people',
  url: '/people',
  component: 'people',
  resolve: {
    people: function(PeopleService) {
      return PeopleService.getAllPeople();
      // data를 가져 올 때, return promise로 위임한다
    }
  }
},
~~~

~~~js
angular.module('hellogalaxy').component('people', {
  bindings: { people: '<' },

  template: '<h3>Some people:</h3>' +
            '<ul>' +
            '  <li ng-repeat="person in $ctrl.people">' +
            '    <a ui-sref="person({ personId: person.id })">' +
            '      {{person.name}}' +
            '    </a>' +
            '  </li>' +
            '</ul>'
    // $ctrl.people 에 데이터 리스트가 들어있음
})
~~~

> 데이터를 다 가져온 후, `people` 이라는 `name`에 바인딩해서 데이터 리스트를 보여준다



### $transitions$

- 예제 - 파라미터 설정

~~~~js
{
  name: 'person',
  url: '/people/{personId}',
  component: 'person',
  resolve: {
    person: function(PeopleService, $transition$) {
      return PeopleService.getPerson($transition$.params().personId);
    }
  }
}
~~~~





[ui-router 공식 문서](https://ui-router.github.io/)
