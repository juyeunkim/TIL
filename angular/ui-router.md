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



## $stateProvider

> url, template, controller를 정의



## $stateChangeStart

> URL의 상태가 변경되면 `$stateChangeStart` 이벤트 발생

### 예제

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





[ui-router 공식 문서](https://ui-router.github.io/)
