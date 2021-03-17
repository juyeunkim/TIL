# 파일 import

> 파일 내에서 다른 경로의 파일을 import 하는 방법



## import 방법 2가지

1. export const 사용

   > `{ }` 사용 

   ~~~js
   // Export a variable
   export const App = () => { ... }
   ~~~

   ~~~js
   // Import App in another file
   import { App } from '...'
   ~~~

2. export default 사용

   > `default` 사용

   ~~~js
   const App = () => { ... }
   export default App          
   ~~~

   ~~~js
   // Import App in another file
   import App from '...'
   ~~~

   