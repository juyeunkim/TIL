# Angular

## 시작하기

- Angular CLI 설치

~~~shell
npm install -g @angular/cli
~~~

- 애플리케이션 생성

~~~shell
ng new ///이름 
~~~

- 실행하기

~~~shell
ng serve --open
~~~

> open : 빌드한 후에 브라우저가 자동으로 생성



## 특징

- 타입스크립트로 작성
- 

### 타입스크립트

> = JavaScript + Type
>
> 기존의 자바스크립트는 인터프리터 언어
> 타입스크립트는 컴파일 언어 - 코드 수준에서 미리 타입을 체크하여 오류를 체크

#### 	특징

- 컴파일 단계에서 오류를 잡아낼 수 있고, 코드 어시스트 기능 지원

- 암묵적 형변환, 호이스팅, 복잡성 문제 해결

  > - 호이스팅 
  >
  >   > 모든 선언 (var, let, const, function 등 )을 가장 위로 끌어올린다.
  >   >
  >   > - 변수 호이스팅
  >   >
  >   >   ~~~js
  >   >   var hoisting;
  >   >   console.log(hoisting); // undefined
  >   >   hoisting = "success";
  >   >   console.log(hoisting); // 'success'
  >   >   ~~~
  >   >
  >   > - 함수 호이스팅 **(함수 선언문)**
  >   >
  >   >   ~~~js
  >   >   var a;
  >   >   a();
  >   >   a = function() {
  >   >     console.log("a is not a function");
  >   >   };
  >   >   ~~~
  >   >
  >   >   





# Angular 버전 1.x

### 특징

- 하나 이상의 모듈로 구성

  ~~~html
  <html ng-app="todoApp">
      <script>
      var todoApp = angular.module("todoApp", []);
  	</script>
  </html>
  <!-- todoApp이라는 모듈을 호출 -->
  ~~~

  ~~~
  angular.module( 생성할 모듈명 , 필요한 추가 모듈 배열 )
  ~~~

  

- MVC 모델 지원

- 바인딩 표현식



### ng-app

> html 엘리먼트 중 AngularJS가 컴파일하고 처리해야 할 모듈이 들어있음을 알려주는 역할

- ng-app vs data-ng-app 차이는?
  동작 원리는 동일
  But, `data`를 붙이면 오류가 발생하지 않는다



### MVC

#### M (model)

- 선언

~~~html
<script>
    var model = {
            user: "Adam"
   };
</script>
~~~

- 사용 ( 바인딩 표현식 - `{{ }}`)

~~~html
<html>
    {{todo.user}}
</html>
~~~



#### V (view)



#### C (controller)

- 선언

~~~html
<script>
    todoApp.controller("ToDoCtrl", function ($scope) {
            $scope.todo = model;

            $scope.incompleteCount = function () {
                var count = 0;
                angular.forEach($scope.todo.items, function (item) {
                    if (!item.done) { count++ }
                });
                return count;
            }

            $scope.warningLevel = function () {
                return $scope.incompleteCount() < 3 ? "label-success" : "label-warning";
            }

            $scope.addNewItem = function (actionText) {
                $scope.todo.items.push({ action: actionText, done: false });
            }

        });
</script>
~~~

> - `$scope` 란?
>   뷰에게 데이터 및 기능을 노출하는데 사용 

- 사용

~~~html
<html ng-controller="ToDoCtrl">
    
</html>
~~~



### 바인딩 표현식

> {{ }} 을 이용하여 view에서 데이터 사용

1. controller에서 model 지정

   ~~~js
   $scope.todo = model;
   // model을 todo라고 지정
   ~~~

2. view에서 data 사용

   ~~~html
   <html> {{todo.user}}'s To Do List </html>
   ~~~

※ 주의 : 복잡한 로직을 수행하거나 모델을 조작하는데 사용되서는 안된다



#### 양방향 모델 바인딩

~~~html
<input type="checkbox" ng-model="item.done" />
~~~










