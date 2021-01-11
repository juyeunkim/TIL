# Angular Directive

> 사용자가 지정한 DOM 요소를 컨트롤 할 때 사용하는 컴포넌트
>
> -> DOM을 직접 컨트롤 할 수 있음



### AngularJS HTML Compiler 절차

- compile 단계

HTML의 DOM 엘리먼트들을 돌면서 디렉티브를 찾는다. (attribute name, tage name, comments, class name 으로 디렉티브 매칭)

결과로 link function을 리턴한다

- link 단계

디렉티브와 HTML이 상호작용(동적인 view) 할 수 있도록 디렉티브에 event listener를 등록하며 scope와 DOM 엘리먼트간에 2-way data binding 을 위한 $watch 설정



## 작명법

- JavaScript 생성 시 - `camelCase`

~~~html
<my-example></my-example>
<my:example></my:example>
<my_example></my_example>
~~~

- HTML에서 AngularJS 의 디렉티브 이용시 - `snake-case`

~~~js
angular.module.(....)
   .directive('myExample', function() {
   -- [생략] directive 내용 작성 --
   )};
~~~



## 생성 규칙

~~~js
var myModule = angular.module(...);
myModule.directive('directiveName', function (injectables) {
  return {
    restrict: 'A',
    template: '<div></div>',
    templateUrl: 'directive.html',
    replace: false,
    priority: 0,
    transclude: false,
    scope: false,
    terminal: false,
    require: false,
    controller: function($scope, $element, $attrs, $transclude, otherInjectables) { ... },
    compile: function compile(tElement, tAttrs, transclude) {
      return {
        pre: function preLink(scope, iElement, iAttrs, controller) { ... },
        post: function postLink(scope, iElement, iAttrs, controller) { ... }
      }
    },
    link: function postLink(scope, iElement, iAttrs) { ... }
  };
});
~~~



### restrict

> HTML에서 디렉티브를 사용하기 위한 DOM 엘러먼트 속성

| 옵션  | 매칭      | 예제                          |
| ----- | --------- | ----------------------------- |
| **E** | element   | <my-example>                  |
| **A** | atrribute | <div my-example>              |
| **C** | class     | <div class=my-example>        |
| **M** | comment   | <!- directive: my-example --> |



### template

> html의 디렉티브를 사용한 부분에 보여줄 내용
>
> In-Line value 설정 
>
> ​	-> template의 내용이 많아질수록 코드관리가 어렵기 때문에 `templateUrl` option으로 html파일을 분리하는것이 좋다

~~~html
<!-- index.html -->
<!doctype html>
<html ng-app="exampleDirective">
  <head>
    <script src="http://code.angularjs.org/1.2.10/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div ng-controller="Ctrl">
      <my-example></my-example> 
      <!-- 	Name: nextreeMember
			Address: Gasan
		-->
    </div>
  </body>
</html>
~~~

~~~js
// script.js
angular.module('exampleDirective', [])
  .controller('Ctrl', function($scope) {
    $scope.person = {
      name: 'nextreeMember',
      address: 'Gasan'
    };
  })
  .directive('myExample', function() {
    return {
      restrict: 'E',
      template: 'Name: {{person.name}} </br> Address: {{person.address}}'
    };
});
~~~



### replace

> 디렉티브의 template or templateUrl에 포함된 태그 내용을 추가할지 교체할지 결정
>
> `true` - 교체



### priority

> 디렉티브 별로 compile과 link의 우선 순위를 지정 ( `default 0` )
>
> 값이 클수록 우선순위가 높고 먼저 호출



### scope

- 옵션

1. scope: false - `default`

   새로운 scope 객체를 생성하지 않고 부모가 가진 같은 scope 객체를 공유

2. scope: true

   새로운 scope를 생성하고 부모 scope 객체를 상속

3. scope : {}

   isolate / isolated scope를 새롭게 생성

   재사용 가능한 컴포넌트를 만들 때, parent scope의 값을 read/write 하지 못하게 막기 위해서 사용

   parent scope에 접근하고 싶을 경우 `=, @, &` 사용

- Binding 전략

  - `=`  : 부모 scope의 property와 디렉티브의 property를 바인딩하여 부모 scope에 접근

    ~~~
    
    ~~~

    

  - `@` : 디렉티브의 attribute value를 `{{ }}` 방식을 이용하여 부모 scope에 접근

    ~~~html
    <div ng-controller="Ctrl">
          <nextree-directive company-info={{name}}>
          {{locate}}
          </nextree-directive>
    </div>
    ~~~

    ~~~js
    angular.module('testDirective', [])
      .controller('Ctrl', function($scope) {
        $scope.name = 'Nextree';
        $scope.locate = 'Gasan';
      })
      .directive('nextreeDirective', function() {
        return {
          restrict: 'E',
          scope: {
            name: '@companyInfo'
          }
        };
      });
    ~~~

    





[참고 - 넥스트트리 ](https://www.nextree.co.kr/p4850/)

