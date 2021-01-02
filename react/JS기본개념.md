## 웹팩

### 개념

> 프로젝트의 구조를 분석하고 자바스크립트 모듈을 비록한 리소스들을 찾은 다음 이를 브라우저에서 이용할 수 있는 번들로 묶고 패킹하는 **모듈 번들러**
>
> > - 모듈 번들러란?
> >   지정한 단위로 파일들을 하나로 만들어서 요청에 대한 응답할 수 있는 환경을 만들어줌
> >   -> 웹의 HTTP Request 시간이 줄어든다는 장점
>
> 하나의 파일을 여러 파일로 분할하고 구성할 수 있는 **자바스크립트 모듈**

### 특징

- 파일이름에 해시값 추가 -> 효율적으로 브라우저 캐싱 이용 ex ) {}

- 사용되지않는 코드 제거

- 자바스크립트 압축

- css,json,text file을 일반 모듈처럼 불러오기

- 환경 변수 주입

### 왜 사용하는지

> 모듈 시스템(ESM, Common JS)을 사용하고 싶어서

- ESM

![img](https://lh6.googleusercontent.com/BOTz_QB-5d9Og79OMh36igwxw0ZgSCe27NksjuNrhrVcDeZBpzDyQH2U5tRG1kTpQ8e61cKdLNRJHi3sRiP7j3LJ1m64917Fi3CYl8HwS76FZdd5taDu98wfEYyoCrnN6PQD7_gm)

> 각자 파일로 저장한 후, import 하여 사용할 수 있다
>
> ESM을 사용하지 않으면 script을 import해야 사용할 수 있다
>
> ![image-20200907102417828](C:\Users\juyeunkim\AppData\Roaming\Typora\typora-user-images\image-20200907102417828.png)



## 바벨

> ES6 -> ES5 로 변환
>
> 코드 압축 용도

~~~shell
npm init -y
npm install @babel/cor @bable/cli @bable/preset-react
~~~





## CRA

> 웹팩을 설치하지 않고도 사용할수 있도록 
>
> ES6 자바 스크립트를 브라우저가 이해할 수 있는 ES5 코드로 변경해주는 툴

- 설치 방법

~~~shell
npm install create-react-app
~~~



- CRA vs Next.js

|                   | CRA                                            | Next.js       |
| ----------------- | ---------------------------------------------- | ------------- |
| 서버사이드 렌더링 | X                                              | O             |
| config 변경       | X                                              | O             |
| 장점              | 간편하게 React를 만들수 있음                   | 커스텀이 가능 |
| 단점              | 설정을 거의 변경할 수 없다초기에 설정이 제공됨 |               |



## HTTPS로 실행하는 방법

> 기존에 `npm start` 로 실행하면 http로 실행된다.
> https로 실행하기 위해서는 2가지 방법이 존재

- 맥

  ~~~shell
  HTTPS = true npm start
  ~~~

- 윈도우

  ~~~shell
  set HTTPS=true && npm start
  ~~~



## 환경 변수 (.env)

> develop, build 환경을 다르게 구성 ==> profile설정같은 개념
>
> 깃허브와 같은 오픈 소스로 정보를 공개하지 않을때에도 이용





