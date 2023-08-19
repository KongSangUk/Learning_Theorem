![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnRsUv%2FbtqDmVAJZmn%2FinVKefvwsHVQ0Z9CJ3z23K%2Fimg.png)

### 리눅스 명령어

---

**- pwd :** 본인의 현재 위치 확인

**- cd :** change directory : 일루 들어갑니다~ 라는 의미

**- mkdir : make directory : 폴더 생성**

**- ls :** 이 명령어는 다음과 같은 옵션과 함께 사용할 수 있다.

 	**- a (all) : 숨김 파일을 포함한 모든 목록 출력**

 	**- l : 각 목록의 세부 정보까지 함께 출력**

​	 **- d : 특정 파일이나 디렉토리를 지정해서 정보를 확인 (ls -ld의 형태로도 사용 가능)**

 	**- n : 파일 및 디렉토리 정보 출력시 UID, GID 형태로 출력 (ls -ln의 형태로도 사용 가능)**

​	 **- F : 파일 및 디렉토리(즉, 목록)의 형식을 표시**

​	 **- R : 디렉토리의 하위 정보까지 모두 확인**

**#** Git Bash에서 복붙하는 방법 : **Insert 키.**

</br>



### 기본 용어

---

 

**- 커맨트 라인(Command Line):**

깃 명령어를 입력할 때 사용하는 컴퓨터 프로그램. 맥에선 터미널이라고 한다. PC에선 기본적인 프로그램이 아니어서 처음엔 깃을 다운로드해야 한다(다음 섹션에서 다룰 것이다). 두 경우 모두 마우스를 사용하는 것이 아닌 프롬프트로 알려진 텍스트 기반 명령어를 입력한다.

 

**- 저장소(Repository):**

프로젝트가 거주(live)할 수 있는 디렉토리나 저장 공간. 깃허브 사용자는 종종 “repo”로 줄여서 사용한다. 당신의 컴퓨터 안의 로컬 폴더가 될 수도 있고, 깃허브나 다른 온라인 호스트의 저장 공간이 될 수도 있다. 저장소 안에 코드 화일, 텍스트 화일, 이미지 화일을 저장하고, 이름붙일 수 있다.

 

**- 버전관리(Version Control):**

기본적으로, 깃이 서비스되도록 고안된 목적. MS 워드 작업할 때, 저장하면 이전 화일 위에 겹쳐쓰거나 여러 버전으로 나누어 저장한다. 깃을 사용하면 그럴 필요가 없다. 프로젝트 히스토리의 모든 시점의 “스냅샷”을 유지하므로, 결코 잃어버리거나 겹쳐쓰지 않을 수 있다.

 

**- 커밋(Commit):**

깃에게 파워를 주는 명령이다. 커밋하면, 그 시점의 당신의 저장소의 “스냅샷”을 찍어, 프로젝트를 이전의 어떠한 상태로든 재평가하거나 복원할 수 있는 체크포인트를 가질 수 있다.

 

**- 브랜치(Branch):**

여러 명이 하나의 프로젝트에서 깃 없이 작업하는 것이 얼마나 혼란스러울 것인가? 일반적으로, 작업자들은 메인 프로젝트의 브랜치를 따와서(branch off), 자신이 변경하고 싶은 자신만의 버전을 만든다. 작업을 끝낸 후, 프로젝트의 메인 디렉토리인 “master”에 브랜치를 다시 “Merge”한다.

</br>



### **주요 명령어**

---

 

**- git init:**

깃 저장소를 초기화한다. 저장소나 디렉토리 안에서 이 명령을 실행하기 전까지는 그냥 일반 폴더이다. 이것을 입력한 후에야 추가적인 깃 명령어들을 줄 수 있다.



**- git config:** “configure”의 준말, 처음에 깃을 설정할 때 가장 유용하다.



**- git help:** 명령어를 잊어버렸다? 커맨드 라인에 이걸 타이핑하면 21개의 가장 많이 사용하는 깃 명령어들이 나타난다. 좀 더 자세하게 “git help init”이나 다른 용어를 타이핑하여 특정 깃 명령어를 사용하고 설정하는 법을 이해할 수도 있다.



**- git status:** 저장소 상태를 체크. 어떤 화일이 저장소 안에 있는지, 커밋이 필요한 변경사항이 있는지, 현재 저장소의 어떤 브랜치에서 작업하고 있는지 등을 볼 수 있다.



**- git add:** 이 명령이 저장소에 새 화일들을 추가하진 *않는다*. 대신, 깃이 새 화일들을 지켜보게 한다. 화일을 추가하면, 깃의 저장소 “스냅샷”에 포함된다.



**- git commit:** 깃의 가장 중요한 명령어. 어떤 변경사항이라도 만든 후, 저장소의 “스냅샷”을 찍기 위해 이것을 입력한다. 보통 “git commit -m “Message hear.” 형식으로 사용한다. -m은 명령어의 그 다음 부분을 메시지로 읽어야 한다는 것을 말한다.



**- git branch:** 여러 협업자와 작업하고 자신만의 변경을 원한다? 이 명령어는 새로운 브랜치를 만들고, 자신만의 변경사항과 화일 추가 등의 커밋 타임라인을 만든다. 당신의 제목이 명령어 다음에 온다. 새 브랜치를 “cats”로 부르고 싶으면, git branch cats를 타이핑한다.



**- git checkout:** 글자 그대로, 현재 위치하고 있지 않은 저장소를 “체크아웃”할 수 있다. 이것은 체크하길 원하는 저장소로 옮겨가게 해주는 탐색 명령이다. master 브랜치를 들여다 보고 싶으면, git checkout master를 사용할 수 있고, git checkout cats로 또 다른 브랜치를 들여다 볼 수 있다.



**- git merge:** 브랜치에서 작업을 끝내고, 모든 협업자가 볼 수 있는 master 브랜치로 병합할 수 있다. git merge cats는 “cats” 브랜치에서 만든 모든 변경사항을 master로 추가한다.



**- git push:** 로컬 컴퓨터에서 작업하고 당신의 커밋을 깃허브에서 온라인으로도 볼 수 있기를 원한다면, 이 명령어로 깃허브에 변경사항을 “push”한다.



**- git pull:** 로컬 컴퓨터에서 작업할 때, 작업하고 있는 저장소의 최신 버전을 원하면, 이 명령어로 깃허브로부터 변경사항을 다운로드한다(“pull”).

</br>





# Git 사전 준비



> Git을 사용하기 위해서는 Git의 commit을 남기는 사람에 대한 정보를 설정합니다.
>
> 한번 설정하면 컴퓨터에 자격증명 방식으로 계속 남아있어서 또 할 일은 잘 없습니다.



```
$ git config --global user.name '{이름}'
$ git config --global user.email '{이메일}'
```

- 추후에 commit을 작성한 사람(author)로 저장된다.
- email 정보는 github에서 활용하고 있는 email과 동일하게 설정하는 것을 추천한다.
- 설정 내용을 확인하려면 아래의 명령어를 입력한다.

</br>



# Git 기초 흐름

## 0. 저장소 설정

```
$ git init
```

- git 저장소를 만들게 되면 해당 디렉토리 내에 `.git/` 폴더가 생성된다.
- `git bash` 에서는 `(master)` 라는 표기가 같이 등장한다.
- 현재 작업 중인 브랜치를 의미한다.

</br>

### 1. `add`

- 작업 위치(Working Directory 이하 WD) 폴더에 작업한 파일이 있을 경우 `add`를 통해서 `staging Area`로 옮길 수 있습니다. `stagin Area`는 `commit`을 진행하기 전의 임시 저장된 상태 정도로 생각하면 될 것 같습니다.
- 해당 폴더에서 작업 후 상황
- `# a.txt를 만든 상황 $ git status On branch master No commits yet # 트래킹 되고 있지 않은 파일들 # => git으로 버전을 남긴적이 없는 파일 Untracked files: # staging area에 포함시키려면, git add # (커밋될 파일 목록) (use "git add <file>..." to include in what will be committed) a.txt # 커밋할 내용 없지만, 트래킹 되지 않는 파일은 존재한다. # => WD O, Staging Area X nothing added to commit but untracked files present (use "git add" to track)`
- staging area에 추가
- `$ git add a.txt # a.txt 파일 $ git add myfolder/ # myfolder 폴더 $ git add . # 현재 디렉토리`
- 추가 후 상태 : `status`를 자주 살펴보면서 상태를 확인하는 것이 좋습니다.
- `$ git status # git의 상태를 확인 On branch master No commits yet # 새로 생성괸 것이 없으면 나오는 메세지 # 커밋될 변경사항들 (staging area) Changes to be committed: (use "git rm --cached <file>..." to unstage) # a.txt 새로 생성됨 new file: a.txt`

</br>

### 2. `commit`

> 커밋 메시지는 현재 버전에 대한 내용을 명확하게 작성해야합니다.
>
> 현재 어느정도 commit 메세지에 대해서 사람들끼리 정해진 메세지 작성법이 있습니다. (참고 : https://meetup.toast.com/posts/106)

```
$ git commit -m '커밋메시지'

[master (root-commit) 41a723a] Init
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 a.txt
```

- `git log` : 커밋 이력을 확인하기 위해서는 아래의 명령어를 입력한다.
- `$ git log commit 41a723a3626a9737a7eb822441f4ba0aede37d9d (HEAD -> master) Author: edutak <edutak.ssafy@gmail.com> Date: Fri Apr 10 10:45:22 2020 +0900 Init $ git log -1 # 최근 한개 커밋 $ git log --oneline # 간략한 로그 $ git log --oneline -1 # 최근 한개의 커밋을 간략하게`

</br>

## Git 상태 메시지

```
$ git status
# branch 정보
On branch master
Your branch is up to date with 'origin/master'.

# 커밋될 변경사항
# Staging area
Changes to be committed:
  # unstage => add를 취소하는 명령어(staging area -> WD)
  (use "git restore --staged <file>..." to unstage)
        modified:   cli.txt

# stage 상태가 아닌 변경사항
# Working directory
Changes not staged for commit:
  # WD => staging area
  (use "git add <file>..." to update what will be committed)
  # WD 작업 내용을 모두 삭제(되돌릴 수 없음.)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   a.txt
```

</br>

## Git 원격저장소 활용(Github)

- Github에 repository를 생성한다.

</br>

### 0. 원격 저장소 설정

- 원격 저장소 설정
  - 원격저장소(`remote`)로 `origin` 이름으로 `url` 을 추가(`add`)
- `$ git remote add origin {__url__}`
- 원격 저장소 목록
- `$ git remote -v origin https://github.com/edutak/test-repo.git (fetch) origin https://github.com/edutak/test-repo.git (push)`
- 원격 저장소 삭제
  - `origin` 이름의 원격 저장소 설정을 삭제(remove - `rm` )
- `$ git remote rm origin`

</br>

### 1. `push`

> `commit`한 이력이 repository로 저장됩니다.

```
$ git push origin master
```

- 현재 폴더를 그대로 업로드 하는 것이 아니라, 지금까지의 이력/버전(commit)을 `push` 하는 것이다.
- Working directory, Staging area의 변경사항들은 원격저장소로 `push` 되지 않는다.
- 따라서, `push` 전에 `$ git status` , `$ git log` 를 통해서 확인하는 습관을 가져야합니다.

</br>

### 2. pull

```
$ git pull origin master
```

- 원격저장소 변경 사항(이력)을 받아옵니다.
- 다른 작업 환경이나 위치에서 작업할 때, 혹은 공동 작업에서 타인이 `commit`해서 이력이 변경되었을 경우 등의 경우가 있습니다. 따라서, `pull`을 통해서 가져온 후, 작업을 진행하는 것이 좋습니다.