## chapter08_지옥에서 온 Git : 버전 만들기 (commit)
- 모든 변화를 version이라고 하지 않음.
- version은 의미있는 변화를 지칭함.
  - 내가 의미있는 변화라고 지정해 주어야함.(add)
- 앞으로 버전을 갱신 할 때, 버전을 갱신한 사람이 나라는 것을 알려주기 위해 이름을 setting 해야함
  (한 번만 해주면 됨)
~~~
git config --global user.name jaebig
git config --global user.email koni114@gmail.com
~~~

- 이상태에서 git commit을 하게 되면 commit 됨
~~~
git commit
~~~
- 커밋 후에 vim이 실행됨
- 이 때 commit에 대한 메세지를 작성(commit 내용에 대한 메세지 입력)
- 수정 방법(순서)
  - i 를 눌러서 insert 
  - 글자 입력
  - esc를 누르면 insert 종료
  - :wq 를 입력(write, quit)

- 버전 확인 방법
~~~
git log
~~~
- 새로운 파일이 생성되었을 때 git에게 버전 관리 명령을
  수행 할 때 add 명령어 사용

## chapter09_git stage area
- 왜 git은 add 라는 과정을 포함하고 있는가?
  - 보통 commit은 하나의 작업 과정이 추가될때 마다
    수행하는 것이 보통인데,
    거의 대부분 그 타이밍을 놓침.
    그러면 보통 10~20개 작업이 수행된 후에야 commit을 해야하는데,
   이 때 add는 내가 원하는 파일만 commit  시킬 수 있음!

- add를 한 file만이 commit 이 됨
- 선택적으로 commit이 가능.

- stage 상태
  - git commit 대기 상태를 말함
  - git add 하면 stage 상태로 올라감

- repository
  - commit된 결과가 저장되는 곳.


## chapter10_ 변경 사항 확인하기(log & diff)
- 차이점 확인하기.
~~~
git log
git log -p # -p를 붙이면 각각의 commit 과 commit 사이의 소스의 차이점 확인
~~~
- 마이너스(-)는 이전버전의 소스 내용, 플러스(+)가 붙은 것들은 최신(이후)버전의 소스 내용
~~~
git diff id1..id2 # git commit id 간의 차이를 보여줌.
~~~
- git diff 명령어
  - 소스의 차이를 통해서 어떤 작업을 했는지 알 수 있음.
  - commit을 하기 전에 작업 내용의 마지막 리뷰를 제공해줌.
  - commit을 해주면 비교안됨.

## chapter11_과거로 돌아기기(reset)
- commit을 취소하는 명령, 주의해서 해야함.
- 사고를 방지하기 위해서는 cp 명령어를 통해 다른 디렉토리에 저장해두고,  
  잘못 reset시 백업해둔 폴더를 다시 복원하면 크게 문제될 일 없음!

- reset vs revert
  - reset
    - 일반적으로 특정 commit 위치로 돌아가고 싶을 때 reset 사용
    - commit의 경계를 잘 생각해야함.
    - ex) version 3의 상태로 돌아가고 싶을때 ,
      --> git reset (version3의 commit Id 입력) -- hard
    - 일반적으로 git은 어떠한 정보도 삭제를 안함.
    - reset 명령어를 통해 지운것처럼 보이지만, 실제로는 남아있음.
    - 필요하면 복구 가능. --> git의 원리를 이해해야함.
    - 자원을 원격저장소에 공유하고 있는 경우에는 절대로! reset을 하면 안됨
  - revert

- 중요한 것은 version을 되돌릴 수 있다!

## chapter12_스스로 공부하는 법
- git에서 어떤 명령어가 많이 사용되는지? 통계는 없음..
- 명령어가 얼마나 많은 검색 결과가 있는가?
  - commit > push > pull > clone > checkout > add > branch...

- 메세지에 대한 도움말 확인 방법
  - --help 입력!
  - 화살표키를 위아래로 누르면 scroll이 됨
~~~
git commit -a 
~~~
- 삭제하거나 추가된 파일을 자동으로 stage에 올려줌. -> add 명령어 생략 가능

## chapter13_git의 원리 소개
- git의 원리를 알아야 하는 이유.
  - 영감을 얻을 수 있음.
  - 명령어들의 수행을 훨씬 더 오래 기억할 수 있음.
  

## chapter14_분석도구 gistory 소개.
- 원리를 파악하는 방법?
  - 리눅스 토발츠가 처음으로 git을 세상에 내놓은 첫 버전을 분석하는 방법
  - 어떠한 명령을 내렸을 때, .git에서 어떠한 일이 일어나는가?   
   --> 이번 강의를 통해 알아보는 것.  

- 이고잉 아저씨가 .git 폴더의 변경사항을 좀 더 쉽게 알기 위하여,  
  gistory라는 프로그램을 만들었다고 함.

## chapter15_GIT : 원리 - git add.
- 파일을 추가한 것에 대해서 .git은 변화가 없음(add가 아님)

- git add f1.txt 명령어 수행시, 2가지가 바뀜
  - index file 수정
    - 변화된 파일의 파일명이 저장되어 있음
  - objects/78/9819~~ file이 추가
    - 들어가서 확인해보면, 내가 add 한 file의 내용이 추가되어 있음

- git은 파일을 저장할 때, 파일의 이름이 달라도,
  파일 안의 내용이 같으면, 같은 object를 가리킨다!  
  -> 아무리 많은 파일이 있다고 하더라도, 그 내용이 같으면 disk를 잡아먹지 않음.

## chapter16_GIT 원리: objects 파일명의 원리
- 내용이 같으면, 파일의 이름이 같다!   
  --> 내용을 기반으로 해서 파일의 이름이 결정되는 매커니즘이 존재

- sha1 online site를 접속해보자.
- 결과적으로 git은 내용을 sha1 이라고 하는 hash algorithm을 통과시켜서 file의 object name명을 만들어냄.
 - 이때 만들어진 hash code의 앞의 두글자 명의 directory를 만들어내고,  
 세번째글자 ~ 마지막글자 명으로 파일명으로 생성함.
 - git은 순수하게 파일 내용을 sha1 알고리즘을 통과시키는 것이 아니라,   
 부가적으로 뒤에 뭔가를 추가시켜 sha1 알고리즘을 통과시킴.  

## chapter17_GIT 원리 - commit의 원리
- commit 수행시
  - 기본적으로 commit은 tree 형태의 구조를 가짐
  - objects 디렉토리 안에 commit message가 저장되어 있음
  - terminal node가 최신 commit 정보
    - commit message 내용
      - 누가 커밋 했는지,
      - tree + object 가 적혀있음. -> f1, f2, f3.txt의 파일의 내용이 적혀있음.
      - commit을 한번 더 하면, 해당 디렉토리에 parent가 생김
        -> 이는 이전 commit의 내용임을 알 수 있음.  
        -> 중요한 것은 해당 커밋 객체들의 tree object가 다르다는 것임.  
        당연하다! 내용 자체가 달라졌기 때문.
    - 각각의 버전은 그 시점의 snapshot을 찍어내는데, 이는 tree라는 구조로 표현.

- object file에는 크게 3가지가 존재  
  - blob : file의 내용이 담겨있는 디렉토리  
  - tree : 디렉토리의 파일명과 파일명에 해당되는 내용(blob)을 담고 있는 것  
  - commit : commit 정보의 object id  

## chapter18_GIT 원리 - status의 원리
- index라는 파일이 무엇인가?   
- git status 명령어를 사용했을 때, 어떻게 git은 status를 알 수 있을까?
  - index file과 commit 시 생성되는 object file을 비교하면, commit할 것이 있는지 없는지 알 수 있음.
  - 즉 file을 수정했을 때, hash name의 차이를 보고 status를 출력해줌.
 - index 과 최신 commit의 tree가 가리키고 있는 값이 다르다면, add는 되었고, commit 대기 상태라는 것을 알 수 있음

- 이미지 참고  
 ![img](./img/workspace_repository.png)
- workspace (add) index (commit) local repository (push) remote repositoy
- working directory - index, staging area, cache - repository


## chapter19_GIT 원리 - branch 소개.
- branch : 나무의 가지에 의미를 가지고 있음.
  - branch를 만든다 --> 분기되는 과정을 가진다.(client용과 update용을 따로 만든다 ! 등 ..)
  - branch 작업을 하였던, 하지 않았던 하나의 branch를 쭉 가지고 있었다고 생각할 수 있음
- branch라는 기능은 이전까지 있어 왔는데,  
  예전까지는 쓸만하지 않았다라고 생각하기 쉬움!  
  git 가지고 온 혁신 중에 하나는 branch 기능을 쓸만한 수준까지 끌고 왔다! 라고 판단할 수 있음
- git은 모든 것이 내부적으로 branch의 개념을 가지고 있기 때문에
  꼭 알고 있어야 함

## chapter20 - branch 만들기
~~~
git commit -am
~~~
- 한번도 add가 되지 않은 파일은 자동으로 add 안됨
- branch를 사용하는 경우
  - 고객사용 source를 따로 준비하는 경우
  - 기능을 만들텐데, 쓸모없어질 것 같은 경우
  - 기능을 server에 올려 test 하는 경우

~~~
git branch
~~~ 
  - master라는 branch를 사용하고 있다는 의미.
  - git을 사용하는 순간부터 기본 branch를 사용하는데, master라는 branch를 사용.
  - 일반적인 branch 명칭
    - exp : test용, 실험용
    - feature : 특정 기능 추가시 
  - '*' 가 표시되어 있는 branch는 해당 branch에 들어가 있다는 의미
~~~
git branch exp
~~~
- exp 라는 branch 생성 
~~~
git checkout exp
~~~
- exp branch로 들어감
 

## chapter22 - branch 정보확인
- 상당한 복잡성을 가지고 있음.
~~~
git log --branches --decorate
~~~
- branch 사이의 차이점 파악 방법.
  - git log --branches
    - checkout 되어있는 branch 말고 저장소에 있는 모든 branch를 보여줌
  - --decorate
    - -> HEAD가 있는 곳이 checkout 되었다는 의미
    - -> HEAD라는 file에 checkout 위치가 기록되어 있음.

~~~
git branch --graph
~~~
- --graph 명령어를 뒤에 붙이면, log 에서 조상이 어딘지를 알 수 있음
  - git log --branches --decorate --graph  
   --> oneline 형태로 log를 확인할 수 있음.

- git 사용시 command line을 사용하는 이유는 가치가 있기 때문

- stree GUI tool
  - 현재 디렉토리의 저장소가 sourcetree GUI tool로 실행됨.
  - command line 에서 확인하기 힘든 graph 를 GUI로 잘 보여줌~
~~~
git log master..exp
~~~
- version 과 version 사이의 차이 확인  
- master에는 있고, exp는 없는 차이를 확인 가능.
~~~
git log master..exp -p
~~~

- -p를 붙이면 source code의 차이를 알 수 있음.


## chapter23 - branch 병합 - merge
- exp의 내용을 master로 옮기는 방법(master 내용을 exp로 옮기는 것이 아님)
  - master로 checkout을 한 후, merge 명령어 수행
~~~
git merge exp
~~~
  - 위의 명령어를 수행하면, Merge branch 'exp' 라는 명령어와 함께 commit 명령어가 뜸
  - graph에서 확인해보면, master의 최신 commit이 됨.

~~~
git branch -d exp
~~~
- branch를 지우는 방법  
  --> exp branch가 삭제가 됨.  


## chapter24 - branch 수련
- branching 방식은 상황에 따라 여러 방식 존재
  - past forward
  - non-past forward
~~~
git checkout -b iss53 
~~~
- iss53이라는 branch 생성과 동시에 checkout  
![img](https://github.com/koni114/Linux_lifeCoding/blob/master/shell_kernel.JPG)

- fast forward(빨리 감기)  
  - master branch에서 hotfix가 독립한 이후에, master branch는 어떠한 commit도 만들지 않음  
  - 중요한건, 별도의 commit을 생성하지 않고, master가 가리키는 commit이 hotfix로 변경됨  
  - 이 떄 hotfix를 지움  
  - 다음으로 issue53을 수정하고, master branch로 merge하려고 함
    - 이 때는 fast forward를 할 수 없음. master branch는 변경되었기 때문
    - 이 떄는 iss53과 master branch의 공통 조상을 찾음
    - 두 개를 합쳤다라는 정보를 담고있는 별도의 commit 을 담고 있음
- 결과적으로 fast forward는 commit id를 생성하지 않음
- non-fast forward는 commit id를 생성함.

## chapter25 - stash
- stash는 감추다, 숨겨두다 라는 뜻을 가지고 있음
- 내가 특정 branch에서 수정을 하고 있는데, 수정을 다 마치지 않은 상태에서   
  다른 branch로 checkout을 해서 작업을 해야하는 경우가 발생  
- 바로 checkout을 하게 되면 경고 문구가 뜸
- 이 때 stash를 사용하여 숨겨둘 수 있음.  
  그렇게 되면 가장 최신의 commit 상태로 적용하여 깔끔하게 branch 상태를 만들어낼 수 있음

- 실습 예제
- 파일 수정 후, 다른 branch로 이동
~~~
vim test.txt         # test.txt file 생성
git checkout -a exp  # exp로 branch 이동 
vim test.txt         # exp * 인 상태에서 test.txt 수정
git checkout master  # master로 checkout
git status           # master로 이동했음에도 불구하고, test.txt가 수정되었다고 메세지 뜸 
~~~
- git stash 예제
~~~
git stash + [내가 행하고자 하는 일의 명령어]
~~~
- exp branch에서 git stash 명령어 수행하면,
  - Saved working directory and index state WIP(working in process) on exp: ~~~  
  --> exp 에 작업중인 변경사항들의 정보들이 save됨을 알 수 있음. 
- 이후에 commit 을 해보면, 다른 message가 안뜨는 것을 알 수 있음.

~~~
git stash apply 
~~~
- stash로 감춰두었던 파일을 다시 복원  
~~~  
git reset --hard HEAD 
~~~
- 가장 최신 commit 상태로 working 상태를 복원.  
- git status 를 해보면, 아무것도 안나오는 것을 확인.  
  그렇다면, stash 내용까지 다 없어진걸까? --> NO! 
~~~
git stash list
~~~ 
- git stash list 명령어를 통해 확인할 수 있고, apply 를 통해 다시 복원 가능  
- git stash list --> stash한 내용을 확인할 수 있고  
  git stash apply 하면 없어짐  

- 즉, 내가 stash 명령어를 통해 숨겨둔 파일 내역은 내가 직접 삭제하지 않는이상, 항상 살아있음  

- 이 떄 내가 한번더 파일을 수정해서 stash를 하면,  
  stash list에 가장 상단에 있는것이 가장 최신에 명령한 stash임  

- git stash apply 명령어를 날리면    
  가장 위의 stash를 복원함.  

- git stash drop --> 가장 최신 stash가 삭제됨.  
- ; 를 붙이면 command line 을 한줄에 여러개 사용 가능.  

- git stash pop --> apply + drop 까지 되는 명령어!  
- git stash 라는 명령어는 최소한 version 관리가 되고 있는 파일에 한해서만 적용됨.  
  즉 working directory 에 file이 올라와 있어야함(add)


## chapter27 - branch 원리
- HEAD 파일 - git init시 최초로 생성되며, 디렉토리는 refs/heads/master 라고 하는 directory가 명시되어 있음.
- 해당 directory는 commit 수행시 해당 directory 가 생성되고, 그 안에는 commit object id가 들어가 있음.
- master branch에서 commit을 수행하면, HEAD 내에 object Id도 마찬가지로 바뀜.

- 즉, HEAD를 통해 commit의 최신 버전을 알 수 있음.  
- branch 명령어를 통해 또다른 branch 생성시, refs/heads/exp 라는 directory가 생성됨.  
  --> 이를 지우면, branch 는 사라짐.  
- 파일은 바이너리가 아니라, 일반 텍스트임!  

- branch가 바뀌면? 
  - HEAD 파일이 바뀜 --> exp를 가리킴  

## chapter27 - branch 충돌 해결
- 각각의 branch에 같은 이름의 파일을 새로 생성한 경우 문제가 발생함.
 - ex) common.txt라는 파일을 exp와 master branch에서 각각 다른 부분을 수정한 경우,
 
   -> 각 branch 수정한 부분이 다른 경우, 자동으로 merge가 됨
   -> 각 branch에서 수정한 부분이 같은 경우, 
     - conflict (content) : Merge conflict in common.txt
     - git status 수행시, Unmerged paths: ~~ 라고 나옴.

- ======= 를 기준으로, <<<<<<<< HEAD 라고 되어 있는 부분은 내가 지금 checkout되어 있는 branch의 수정사항
- '>>>>>>> exp' 라고 되어 있는 부분은 checkout 되어 있지 않은 branch의 수정사항임.  
   -> 즉, git은 자동으로 merge 할 수 없는 부분에 대해서 우리에게 위임함  
   -> 이 부분을 바탕으로 병합을 잘 수정해야한다는 것임.   
   -> 수정을 한 후 다시 commit 하면 정상적이게 merge가 됨!  

 
## chapter28 - reset checkout
- checkout을 통해 과거로 돌아가는법을 알아보자.
- ex) 내가 version 3에 해당되는 commit으로 돌아가고 싶다면? 
  - git reset --hard "commit id 명"
  
