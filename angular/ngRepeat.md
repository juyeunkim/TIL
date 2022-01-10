# ngRepeat



## Object의 값을 반복

~~~html
<div class="form-group" ng-repeat="(key, value) in extraInfo">
	{{key}} - {{value}}
</div>
~~~



### 예시

~~~js
$scope.type = {
  a : {
      name : "A",
      value : 1
  }, 
  b : {
      name : "B",
      value : 2
  },
  c : {
      name : "C",
      value : 3
  },
};
~~~

~~~html
<select ng-model="t" ng-options="k as v.name for (k,v) in type"></select>
~~~

> t의 값은 type의 key값이 들어가고, display는 key.name



### ng-repeat-start, ng-repeat-end

반복의 시작과 끝을 알려줌





### 개수 제한 : limitTo

~~~html
<span ng-repeat="country in selectedCountries | limitTo:5">
          {{ country.name }} 
</span>
~~~

> 전체 리스트에서 5개만 나옴
