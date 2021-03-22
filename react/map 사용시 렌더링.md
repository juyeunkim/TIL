# map 사용시 렌더링

> map으로 렌더링할때, 데이터가 추가 및 삭제되면 화면이 리렌더링 되는 문제가 발생할 수 있는데 해결방법



- map을 사용할때 key 값을 지정해줘야하는데

  map의 index대신 `unique` 한 값을 넣어준다

  > index로 넣어주면 중간에 삭제되면, 배열에서 뒷부분이 새로운 데이터라고 인식을하게되어 리렌더링을 하게된다

  - 예시

    ~~~jsx
    <Nav tabs>
        {tabs.map((name, index) => (
            <NavItem key={name.link}>
                <NavLink
                    tag={RRNavLink}
    				to={name.link}
    				className={classnames({ active: activeTab === index })}
                    >
                    <b>{name.name}</b>
                    <Button
                        color="info"
    					className="ml-1"
    					size="xs"
    					onClick={() => {
                            removeTab(name.link, name.extraParams)
    					}}> X </Button>
                </NavLink>
            </NavItem>
        ))}
    </Nav>
    ~~~

    > <NavItem> 의 key가 name.link로 들어가서 unique한 값이 들어가서 리렌더링이 되지않는다

- React.memo 사용

  map으로 렌더링되는 부분에 React.memo를 사용하여 중복을 방지한다

  ~~~js
  export default React.memo(Tab);
  ~~~

- useCallBack 사용

  > 배열에서 값을 제거하기 위하여 `filter`를 사용하는데 useCallBack을 사용하여 중복 방지

  ~~~js
  const onRemove = useCallback((link, extraParams) => {
      	/// filter 로직
      }, [tabs])
  ~~~

  

