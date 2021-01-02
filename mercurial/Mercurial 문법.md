# Mercurial 문법



![How Mercurial does it](https://i.stack.imgur.com/4lmqV.png)

포트는 기본적으로 8000





## Terminal



- COMMIT

  ~~~
  hg commit -m "커밋내용"
  ~~~

- PUSH

  ~~~shell
  hg push
  ~~~

- PULL

  ~~~
  hg pull // 서버 저장소의 변경 내용을 자신의 저장소로 가져오는 것
  hg update // 로컬 저장소의 최신 버전을 복사
  ~~~

  - pull 과 update 차이는?
    

- CHECKOUT

  ~~~
  hg update 이동하려는_목적지
  ~~~

- MERGE

  ~~~
  $ hg update default
  $ hg merge 원하는 브랜치
  $ hg commit -m "내용"
  $ hg push
  ~~~

- 브랜치

  - 현재 브랜치 위치

    ~~~
    hg branch
    ~~~

  - 저장소 브랜치 리스트

    ~~~
    hg branches
    ~~~

  - 브랜치 삭제

    ~~~
    hg commit --close-branch
    hg push
    ~~~

- 저장소 상태

  ~~~
  hg status // 작업중인 파일의 현재상태
  ~~~

  M : 수정
  A : 추가
  R : 삭제
  ! : 저장소에 포함된 파일이나, hg 명령어로 삭제되지 않은 파일
  ? : 저장소에 포함되지 않은 파일
  I : ignored 된 파일

- GRAFT : 하나의 브랜치에서 하나의 커밋을 가져온다

  ~~~
  hg graft -r Rev(커밋번호)
  hg push // 이미 커밋을 했기 때문에 커밋내용 작성할 필요가 없음 
  ~~~

  

- tag



- rebase vs merge
  merge : 병합하면서 기존에있던 커밋의 내용이 존재
  rebase : 병합하면서 기존에있던 커밋의 내용이 사라진다
  ![image-20200806145733135](C:\Users\juyeunkim\Desktop\doc\mercurial\img\git_merge.png)

## 에러 유형

### abort: outstanding uncommitted merge

~~~
hg update --clean
~~~

> 원본 merge - parents의 깨끗한 복사복을 확인하여 모든 변경 내용을 잃게됨

## TortoiseHG

- commit
  ▶ Hg commit
  변경사항을 저장, 로컬 저장소에 저장된 상태 ( push 를 진행해야 원격 저장소에도 저장 )

- push

  ▶ Synchronize -> push 버튼 클릭

- pull

  ▶ Synchronize -> pull 버튼 클릭

- local repo 접속방법
  TortoisHG -> WebServer

- 브랜치 만들기





## 참고 사이트

[TortoiseHg 문서](https://mcmblog.azurewebsites.net/how-to-use-tortoisehg/#Graft_/_Transplant)

[Mercuriral 공식 문서](https://www.mercurial-scm.org/wiki/Tutorial)



