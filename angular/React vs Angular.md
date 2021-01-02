# React vs Angular

> React는 라이브러리,
>
> Angular는 프레임워크

![img](https://www.altexsoft.com/media/2019/01/react-angular-compared.png)





## 조건부 렌더링

### React

~~~jsx
const Header = () => {
  return (
    <>
      {!isLogin && <a href="/login"> LOGIN </a>}
      {isLogin && auth === 1 && <a href="/join"> ADD </a>}
      {isLogin && <a onClick={logout}> LOGOUT </a>}
    </>
  );
};

export default Header;
~~~

### AngularJS

~~~html
<html data-ng-app="Header">
    <div ng-if="!isLogin">
        <a href="/login"> LOGIN </a>
    </div>
    <div ng-else-if="auth == 1">
        <a href="/join"> ADD </a>
    </div>
    <div ng-else>
        <a onClick=logout()> LOGOUT </a>
    </div>
</html>
~~~

