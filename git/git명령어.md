# Git 명령어



### 커밋되지 않거나 저장되지 않은 모든 변경사항 취소

~~~shell
git reset 
git checkout .
git clean -fdk
~~~

> 1번째 : 모든 stage 파일이 unstage
>
> 2번째 : 모든 변경사항 취소
>
> 3번째 : 추적할 수 없는 모든 파일 제거
> 			x는 ignore된 파일도 모두 제거