# Modal

> ✅TODO : angular의 modal에서 react의 modal로 만들기
>
> ​                   CommonModal.js로 만들어서 사용하기



# Angular의 Modal

~~~html
<!-- in html -->
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
// in ModalUtil.js
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



Modal 안에 props로 component를 넘긴다



~~~jsx
// App.jsx
import React, { useState } from 'react';
const App = () => {
  	const [modal, setModal] = useState(false);
    const modalTest = () => { setModal(!modal);}
    
    return(
      <button onClick={modalTest}>modal test</button> <br/>

      {modal && <Modal modal={modal} title={"모달 제목"} component = {ModalTwo} size={'lg'} toggle={modalTest}/>}
    
  );  
};
~~~

> 이때, component의 값은 모달안에 넣어줄 컴포넌트를 넣어준다

~~~jsx
// Modal.jsx
import React from 'react';
import { Button, Modal, ModalHeader, ModalBody, ModalFooter } from 'reactstrap';

const ModalInstance = ({
    title,
    component : Component,
    size,
    toggle,
    modal
  }) => {
  return (
    <div>
      <Modal isOpen={modal} toggle={toggle} size={size}>
        <ModalHeader toggle={toggle}>{title}</ModalHeader>
        <ModalBody>
            <Component />
        </ModalBody>
        <ModalFooter>
          <Button color="secondary" size="sm" onClick={toggle}>Cancel</Button>
        </ModalFooter>
      </Modal>
    </div>
  );
}

export default ModalInstance;
~~~



### 비동기로 모달안의 결과값 받아오기

