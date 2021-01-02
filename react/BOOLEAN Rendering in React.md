# BOOLEAN Rendering in React

> { item.isSysadmin } 하면 isSysadmin (boolean) 렌더링 시 아무 값도 안나온다

화면에 보여주는 5가지 방법

1. `toString()`

   ~~~jsx
   { ipsumText.toString() }
   ~~~

2. `String()`

   ~~~
   { String(ipsumText) }
   ~~~

3. `''`

   ~~~
   Boolean Value: { '' + ipsumText }
   ~~~

4. ` ${}`

   ~~~
   {`Boolean Value: ${ipsumText}`}
   ~~~

5. `JSON.stringfy()`

   ~~~
   { JSON.stringify(ipsumText) }
   ~~~

   

