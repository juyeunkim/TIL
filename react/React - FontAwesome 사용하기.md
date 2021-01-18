# React - FontAwesome 사용하기



### Get Started

~~~shell
npm i --save @fortawesome/fontawesome-svg-core
npm install --save @fortawesome/free-solid-svg-icons
npm install --save @fortawesome/react-fontawesome
~~~



### 예시

~~~jsx
import React from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'
import { faEraser, faList }  from '@fortawesome/free-solid-svg-icons';

const FontAwesomeTest = () => {
    return(
        <FontAwesomeIcon icon={faEraser} />
    );
}
~~~

