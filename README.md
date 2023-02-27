# **1. 학습내용**
# 버전 관리 시스템
## VCS 로컬 방식    
RCS의 경우 Patch Set(파일에서 변경되는 부분)을 관리  
## CVCS 중앙 서버 방식  
파일을 관리하는 서버가 별도로 있어, 클라이언트가 중앙 서버에서 파일을 받아 사용(Checkout)하는 방식이다. 하지만 중앙 서버에서 문제가 발생하는 경우 협업, 백업이 모두 이루어질 수 없다. 또 중앙 데이터베이스에 있는 하드디스크에 문제가 발생하면 프로젝트의 모든 히스토리를 잃게 된다.  
## DVCS 분산 저장소 방식  
파일의 마지막 스냅샷을 Checkout 하는 것이 아닌, 히스토리 전부 복제한다. (Clone은 모든 데이터를 갖게 된다.) 서버에 문제가 생기면 이 복제물로 다시 작업을 시작할 수 있다.  
리모트 저장소가 존재하여 많을 수도 있다. 그래서 동시에 다양한 그룹, 다양한 방법으로 협업이 가능하다.     
# git
스냅샷(snapshot) 방식을 이용한다. 스냅샷이란 변경된 파일 전체를 저장하지 않고, 파일에서 변경된 부분을 찾아 수정된 내용만 저장하는 것을 말한다. 변화된 부분만 찾아 사진을 찍는 것과 같다고 하여 스냅샷 방식이라고 한다. 깃의 스냅샷은 HEAD가 가리키는 커밋을 기반으로 사진을 찍는다. 그리고 이를 스테이지 영역과 비교하여 새로운 커밋으로 기록한다.  

### init
* *git init*  
git 저장소를 만드는 명령어로, .git 디렉토리가 생성된다. 이 디렉토리에 Git이 버전관리를 하기 위한 메타정보가 있다.  

remote과 local : 원격 저장소, 로컬 저장소(내 PC)  

### clone
* git clone /로컬/저장소/경로 
* git clone 사용자명@호스트:/원격/저장소/경로  

## 작업의 흐름 (작업 디렉토리, 인덱스(stage), HEAD)
작업중인 실제 파일들은 작업 디렉토리(Working Directory)에 해당한다. *add* 명령을 통해  인덱스로 넘길 수 있으며 인덱스(Index)는 준비 영역(Staging area)을 의미한다. *commit*명령을 통해 HEAD로 넘길 수 있으며, HEAD는 최정 확정본(commit)을 나타낸다.  
### add, commit
* *git add <파일 이름>*
* *git add \**
* *git commit -m "커밋 메시지"*
### push
* *git push origin master*
* *git remote add origin <원격 서버 주소>*
### branch
* *git checkout -b feature_x*  
브랜치 만들고 갈아타기
* *git checkout master*  
master 가지로 돌아가기
* git branch -d feature_x  
feature_x 브랜치 삭제하기
* git push origin <가지 이름>  
브랜치 푸시하기
### merge
* *git pull*  
원격 저장소의 변경 내용이 받아지고(fetch), 병합(merge)된다
* *git merge <브랜치 이름>*  
다른 브랜치와 병합
* *git add <파일 이름>*  
충돌 내용 수정 후 파일 병합
* *git diff <원래 가지> <비교 대상 가지>*  
병합하기 전에 어떻게 바뀌었는지 비교

## 로컬 변경 내용 되돌리기
* `git checkout -- <파일 이름>`  
로컬의 변경 내용을 변겅 전 상태(HEAD)로 되돌린다. 인덱스에 추가된 변경 내용, 새로 생성한 파일은 그대로 남는다.
* `git fetch origin`  
* `git reset --hard origin/master`  
로컬에 있는 모든 변경 내용, 확정본 포기하고 원격 저장소의 최신 이력을 가져온다.  

## **동작에 따른 파일 상태 변화**
***
## Untracked, Tracked

## 세 가지 상태
* Commited : 데이터가 로컬 데이터베이스에 안전하게 저장됨
* Modified : 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않은 것
* Staged : 현재 수정한 파일을 곧 커밋 할 것이라고 표시한 상태

## Git이 하는 기본적인 일
1. 워킹 트리에서 파일을 수정한다.  
2. Staging Area에 파일을 Stage 해서 커밋할 스냅샷을 만든다. 모든 파일을 추가할 수도 있고 선택하여 추가할 수도 있다.  
3. Staging Area에 있는 파일들을 커밋해서 Git 디렉토리에 영구적인 스냅샷으로 저장한다.  

## git add이후 수정한 경우
이후 수정한 것 까지 커밋하고 싶다면 git add를 다시 한다

## 파일 상태 짤막하게 확인하기
`$ git status -s`  
?? : 추적하지 않는 새 파일
A : Staged 상태로 추가한 파일 중 새로 생성한 파일  
M : 수정한 파일  
왼쪽 : Staging Area에서 상태, 오른쪽 : Working Tree에서 상태  
-> MM : add 후 또 수정한 경우  

## 레퍼런스
버전 관리 시스템 종류 : https://git-scm.com/book/ko/v2/시작하기-버전-관리란%3F  
git 간편 안내서 : http://rogerdudler.github.io/git-guide/index.ko.html  
스냅샷 : https://thebook.io/080212/ch04/04/02/  
