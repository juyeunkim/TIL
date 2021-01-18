# ui-router

> AngularJS SPA 라우팅 프레임 워크



### Get Started

~~~~sh
npm install --save @uirouter/angularjs
~~~~



# ocLazyLoad

> 초기에 모든 리소스를 로드하는 SPA의 문제점을 해결할 수 있는 방법
>
> `ui-router`와 함께 사용됨
>
> ▶리소스를 미리 준비해 놓는 것이 아닌 필요할 때 로딩



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



