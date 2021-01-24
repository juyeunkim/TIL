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



### git add 취소

~~~sh
git reset
~~~



### git commit 취소

~~~shell
// [방법 1] commit을 취소하고 해당 파일들은 staged 상태로 워킹 디렉터리에 보존
$ git reset --soft HEAD^
// [방법 2] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에 보존
$ git reset --mixed HEAD^ // 기본 옵션
$ git reset HEAD^ // 위와 동일
$ git reset HEAD~2 // 마지막 2개의 commit을 취소
// [방법 3] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에서 삭제
$ git reset --hard HEAD^
~~~



### commit description 추가

~~~shell
git commit -m "내용" -m "설명"
~~~

