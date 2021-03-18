# useEffect



- 화면 렌더링될때 함수를 처음에 한번만 불러오도록 하는방법 2가지

1. `useCallback`과 함께 사용

~~~jsx
const getData = useCallback(() => {
        AxiosRequest.get(`${restapi}`, param)
        .then(function (resp) {
            if (resp.data.code === 0) {
                setData(resp.data.data);
                setCount(resp.data.dataCount);
                CommonUtil.successNoti('데이터 불러오기 성공');
            } else {
                CommonUtil.errorNoti(resp.data.key);
            }
        }).catch(function (e) {
            console.log(e);
            CommonUtil.error('처리 오류 발생');
        });
},[param]);
    
useEffect(() => {
	getData();
},[getData])
~~~

> 문제점 : param의 값이 변동되면 자동으로 getData를 호출한다 - 리소스 낭비가 심해진다

2. `useEffect` 만 사용

~~~jsx
const getData = () => {
        AxiosRequest.get(`${restapi}`, param)
        .then(function (resp) {
            if (resp.data.code === 0) {
                setData(resp.data.data);
                setCount(resp.data.dataCount);
                CommonUtil.successNoti('데이터 불러오기 성공');
            } else {
                CommonUtil.errorNoti(resp.data.key);
            }
        }).catch(function (e) {
            console.log(e);
            CommonUtil.error('처리 오류 발생');
        });
};

useEffect(getData, []);
~~~

> 해당 함수를 먼저 선언해주고, getData를 호출해 준다.

