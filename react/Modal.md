# Modal

> ✅TODO : angular의 modal에서 react의 modal로 만들기
>
> ​                   CommonModal.js로 만들어서 



# Angular의 Modal

~~~html
<div ng-controller="ModalDemoCtrl as $ctrl" class="modal-demo">
        <!-- MODAL START -->
    	<div class="modal-header">
            <h3 class="modal-title" id="modal-title">I'm a modal!</h3>
        </div>
        <div class="modal-body" id="modal-body">
            <ul>
                <li ng-repeat="item in $ctrl.items">
                    <a href="#" ng-click="$event.preventDefault(); $ctrl.selected.item = item">{{ item }}</a>
                </li>
            </ul>
            Selected: <b>{{ $ctrl.selected.item }}</b>
        </div>
        <div class="modal-footer">
            <button class="btn btn-primary" type="button" ng-click="$ctrl.ok()">OK</button>
            <button class="btn btn-warning" type="button" ng-click="$ctrl.cancel()">Cancel</button>
        </div>
    	<!-- MODAL END -->
    
    <!-- main -->
    <button type="button" class="btn btn-default" ng-click="$ctrl.open()">Open me!</button>
</div>
~~~



~~~js
// in controller.js
$ctrl.open = function (size, parentSelector) {
    var parentElem = parentSelector ? 
      angular.element($document[0].querySelector('.modal-demo ' + parentSelector)) : undefined;
    var modalInstance = $uibModal.open({
      animation: $ctrl.animationsEnabled,
      ariaLabelledBy: 'modal-title',
      ariaDescribedBy: 'modal-body',
      templateUrl: 'myModalContent.html',
      controller: 'ModalInstanceCtrl',
      controllerAs: '$ctrl',
      size: size,
      appendTo: parentElem,
      resolve: {
        items: function () {
          return $ctrl.items;
        }
      }
    });

    // 모달 닫기 눌렀을때
    modalInstance.result.then(function (selectedItem) {
      $ctrl.selected = selectedItem;
    }, function () {
      $log.info('Modal dismissed at: ' + new Date());
    });
  };
~~~



## Angular -> React

> angular 에서는 modal 열고 닫는 로직이 controller.js에 화면은 html에 보여지는데
>
> react는 화면에 합쳐져있다

