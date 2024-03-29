# 💾 GIT 기초

## 사용 목적

`(분산) 버전 관리 프로그램`

### 1. 버전관리

- 컴퓨터 소프트웨어의 특정 상태들을 관리하는 것.
- 지속적으로 수정본만 업데이트 하면 변경사항을 찾기 어려움.
- 그 결과, **가장 최근 파일**과 **변경사항**을 기록한 파일만 남기자!

<br>

### 2.  분산관리 (데이터를 로컬저장소, 원격저장소에 저장)
- 데이터를 한 곳에만 보관하면 메인 hub에 문제가 발생하면 해결이 어려움
- 중앙서버의 문제가 있어도 클라이언트 PC 소스를 통해 복구가 쉬움
- 로컬환경에서 편리하게 사용할 수 있음

<br>

---

##  로컬 저장소

**로컬 저장소**는 3가지의 영역으로 나뉘어진다. 

- `working directory/ working tree` : 작업 공간 
  사용자가 일반적인 작업을 하는 공간
  우리 눈에 보이는 곳 
- `staging area`: 스테이지
  커밋을 할 파일/폴더들이 등록되는 곳
  버전으로 기록하기 위한 파일 변경사항의 목록
  (커밋 전에 git이 변경되는 사항을 관리해야 하는데, 파일을 추적하면서 이전 상태를 관리하는 곳)
- `Repository` : 저장소
  커밋(버전)들이 기록되는 곳

<br>

> **`working directory` => `staging area` => `commits`**
>
> 파일이 위의 흐름으로 이동!

<br>

---

## 로컬 저장소 명령어

```bash
$ git <명령어> <인자> <옵션>
```

<br>

### 1. 초기화

```bash
$ git init
```

- `.git`이라는 숨긴폴더가 생성된다.

- 터미널에 **(master)**가 표시된다. 

- `rm -rf .git`을 통해 `.git` 폴더를 삭제한다면,  git으로 관리되기 전의 상태로 변경된다. 
  => **(master)** 표시가 삭제된다. 

<br>

#### ✔  주의 사항

  > 1. 처음 한번만 실행한다. 
  >    
  >    >  이미 git에 의해 관리되는 폴더 내부에서 다시 `init`을 실행하지 않는다. 
  >    
  > 2. 절대 홈 디렉토리에서 `init`을 실행하지 않는다.
  >
  >    > 터미널에서 현재 위치가 `~`인지 아닌지 확인한다.
  >
  > 3. 이미 Git이 생성된 폴더 내에서 다시 `init`을 하지 않는다.

<br>

### 2. git status

> working directory와 staging area에 대한 정보(파일의 현재 상태)를 나타낸다. 

- 어떤 작업을 하기전에 수시로 status를 확인하는 습관을 가져야 한다. 

- 파일의 상태

  - `untracked`: Git이 관리하지 않는 파일들 (한번도 staging area에 등록되지 않은 파일들)

  - `tracked`: Git이 관리하고 있는 파일들 (Git이 변경사항을 관리 하고 있음)

```bash
$ git satus

on branch master
No commits yet  # 아직 커밋한 것이 없다

Untracked files: # 아직 staging area에 옮겨진 것도 없다.즉, git이 관리하지 않고 있다.
    (use "git rm --cached <files>..." to include in what will be committed)
          first.txt
          
nothing added to nommit but untracked files present (use "git add" to track)

```

=> 현재 어떤 이동도 하지 않았기에 **working directory**에만 저장되어 있음. 

<br>

### 3. git add

> staging area로 이동시키는 명령어. 

- working directory의 파일을 staging area에 등록
- 등록된 파일을 Git이 추적 관리한다.

```bash
$ git add first.txt

$ git status

on branch master
No commits yet  

changes to be committed: => staging area에 있다는 정보 
    (use "git rm --cached <files>..." to unstage)
          new file:   first.txt
```

<br>

#### ✔ 다른 명령어

```bash
$ git add a.txt
$ git add my_folder
$ git add my_folder_a.txt

# 모든 파일을 등록 (현재 디렉토리)
$ git add . 
```

<br>

#### ✔ 파일 수정을 할 경우

만약 first.txt 파일을 수정한다면, 다시 `git add`를 해야한다. 

```bash
$ git status

on branch master
No commits yet  

changes to be committed: 
    (use "git rm --cached <files>..." to unstage)
          new file:   first.txt

changes not staged for commit: 
 (use "git"add <file>..." to update what will be committed")
 (use "git" restore <file>..." to discard changes in working directory")
       modified:     first.txt
```

- 이 상태로 커밋하면 현재 스테이지에 있는 수정 전 파일을 커밋하는 것!
- 현재 working directory의 상태를 수정 전으로 돌아가고 싶으면 
  즉, 스테이지에 있는 파일로 복구 하고 싶으면 `git restore <file>`명령어를 사용!

<br>

### 4. git commit -m 

> commit 시키는 명령어로 m뒤에 버전에 대한 간단 설명을 기입한다. 

- staging area에 등록된 파일의 변경 사항을 하나의 버전(커밋)으로 저장하는 명령어
- `커밋 메시지`를 작성해야 함
  - 변경 사항을 잘 표현할 수 있도록 의미있게 작성한다.
- 최초 커밋 시에는 `root-commit`이 출력된다. 

```bash
$ git commit
# 커밋 메시지 작성을 위한 vim 에디터가 오픈

혹은 

$ git commit -m '첫번째 커밋입니다.'

# 그러나 오류발생 
```



#### ✔작성자 정보 기입 (필수)

- 작성자에 대한 정보가 있어야함 (email, 이름)

- `.git` 폴더에 있는 `config` 문서를 통해 작성자에 대한 정보를 추가 할 수 있음  

- 혹은 명령어를 사용할 수 있음

   ````bash
    git cofing --global user.name '<사용자 이름>'
    git cofing --global user.email <이메일 주소>
   ````

- `.gitconfig` 의 내용을 출력하는 명령어

  ```bash
  $ git config --global --list
  ```

<br>

```bash
$ git cofing --global user.namer 'JK'
$ git coging --global user.email worudp12@gmail.com

$ git commit -m '첫번째 커밋입니다.'
[master (root-commit) 83bcc78] 첫번째 커밋입니다.
1file changed, 1 insertion(+)
create mode 100644 first.txt
```

<br>

### 5. git log

> 커밋 내역을 조회하는 명령어

```bash
$ git log

commit 83bcc78d...(HEAD -> master)
Author: JK <worudp12@gmail.com>
Date: Wed Jan ...

첫번째 커밋입니다.
```
- commit<해쉬값 *#커밋을 구분하기 위한 유닛 값*>(HEAD -> master *#최신 버전을 의미*)

- 명령어 뒤에 옵션을 줄 수 있음

  ```bash
  # 한 줄로 간단하게 요약 
  $ git log --oneline
  
  # 모든 커밋을 열람
  $ git log --all
  
  # 그래프 모양으로 출력
  $ git log --graph
  ```

<br>

### 6. git --help 

> 도움말을 제공한다.

```bash
$ git -help


#특정 명령어에 대한 도움말을 제공
$ git -- help <명령어>

```

<br>

---

## 원격 저장소 명령어

> **🚫 주의사항** 🚫
>
> 원격 저장소에서 수정작업을 하지 않는다.
>
> 반드시 로컬 저장소에서 `git add`  => `git commit` =>`git push` 단계로 업로드한다.  

<br>

### 1. git remote add

> 원격 저장소를 등록한다. 

```bash
$ git remote add origin https://github.com/JaeKP/TIL.git
```

해석: 원격저장소를 <origin> 이라고 부르고 주소는 http://...이다. 

<br>

### 2. git remote -v

> 원격 저장소 정보를 조회한다.

```bash
$ git remote -v

orign  https://github.com/JaeKP/TIL.git (fetch)
otign  https://github.com/JaeKP/TIL.git (push)

```

<br>

### 3. git push

>  로컬 저장소에 있는 데이터를 원격 저장소로 옮기는 명령어

```bash
# origin이라는 이름의 원격저장소의 master 브랜치에 push하기 
$ git push origin master

info: please complete authentication in your browser...
.
.
.
To https://github.com/JaeKP/TIL.git
 * [new branch]      master -> master
 
# 처음 실행시 github와 같은 원격저장소에 자격을 인증하는 창이 뜬다
 

# -u옵션을 사용한 후에는 저장소 이름 (origin), 브랜치 이름(master)를 생략 가능함
$ git push -u origin master

#  그 이후부터는 아래 명령어 사용
$ git push
```

<br>

### 4. git remote rm origin

> 원격 저장소 연결을 삭제

```bash
$ git remote rm origin
```

<br>

### 5.gitignore

> 특정 파일 혹은 폴더에 대해 Git이 버전 관리를 하지 않도록 설정

```bash
$ touch .gitignore

# .gitignore 에 적혀있는 항목들이 무시된다. 
```

<br>

#### ✔.gitignore에 작성하는 내용들 

`Git으로 관리할 필요가 없는 데이터`

- 민감한 개인정보가 담긴 파일 (전화번호, 각종 비밀번호, API KEY 등)
- 운영체제에서 사용되는 파일들
- IDE(통합개발환경) 혹은 Text 에디터등에서 활용하는 파일 
  - pycharm -> `.idea` 폴더
- 개발 언어/ 프레임워크에서 사용되는 파일
  - python 가상환경 

<br>

#### ✔ 주의 사항 

- 반드시 파일 이름을 `.gitignore`로 작성
- `.gitignore`의 위치는 `.git`과 동일한 폴더에 존재 
- 제외하고 싶은 파일들을 **스테이지에 `add`하기 전**에 `.gitignore`에 **작성**
- 그래서 보통 git 초기화 하고   `readme.md`와 `.gitignore`파일을 작성

<br>

#### ✔ 예시

 ```
 # a.txt 파일 제외
 a. txt
 
 # 디렉토리는 /를 붙여서
 user/
 
 # 패턴사용
 *.txt
 
 # 무시하고 싶지 않으면
 !a.txt
 
 # 2개의 asterisk(**) 디렉토리 내부의 디렉토리를 지정
 user/**/b.txt
 # user/startcanp/b.txt  user/b.txt   user/startcamp/c/b.txt
 디렉토리 단계를 고려하지않고 user디렉토리 안에 있는 b.txt를 무시하겠다. 
 ```

<br>

---

## 원격저장소 가져오기

> 원격저장소에 있는 데이터를 로컬 저장소로 가져오는 방법 

- 작업장소가 여러 곳일 경우 원격 저장소에 데이터를 업데이트하고 다른장소에서 그 데이터를 그대로 가져와서 작업할 수 있다.
- 협업 및 편리한 작업환경을 위해 활용  

<br>

### 1. git clone

> 원격 저장소의 커밋 내역을 모두 가져와서, 로컬 저장소에 생성

1. Home1 = `push`=> Remote =`clone`=> Home2 
2. Home2 작업 = `push`=> Remote=`pull`=> Home1
3. Home1 작업 = `push`=> Remote =`pull`=> Home2 

<br>

```bash
$ git clone <원격 저장소 주소>  
$ git clone <원격 저장소 주소> <폴더 이름>  # 복제본을 <폴더이름>에 추가
$ git clone <원격 저장소 주소> . # 복제본을 현재 폴더에 생성
```

- `git clone`를 하게 되면 `git init`과 `git remote add`가 이미 수행된 상태이다.
  (`git init`과 `git remote add`를 안해도 괜찮다는 뜻)

<br>

### 2. git pull

> 원격 저장소의 변경 사항을 가져와서, 로컬 저장소에 반영 (업데이트)

```bash
$ git pull origin master
```

<br>

#### ✔문제상황

>  **Home1에서 `pull`이 아니라` commit`을 먼저한 후 `pull`을 하면 어떻게 될까?**
>
>  1. Home2과 Home1에서 서로 다른 파일을 수정한 경우,
>     **정상 실행 된다.** 
>  2. Home2과 Home1에서 같은 파일을의 서로 다른 라인을 수정한 경우,
>     **정상 실행 된다.** 
>  3. Home2과 Home1에서 같은 파일의 같은 라인을 수정한 경우, 
>     **충돌발생** => 충돌되는 부분을 보여주고 어떤 버전을 사용할 지 선택하여 직접 수정 후 커밋 

<br>

> **Home1에서 `pull`이 아니라` commit`을 먼저한 후 바로`push`를 하면 어떻게 될까?**
>
> 1. 에러 발생. 
> 2. 먼저  `pulll`을 수행한 후에 다시 `push`해야 한다. 

<br>

---

## Branch

>  독립적으로 어떤 작업을 진행하기 위한 개념

- 각각의 브랜치는 다른 브랜치의 영향을 받지 않음
- 그 결과, 여러 작업을 동시에 진행할 수 있음
- 브랜치는 병합함으로, 작업한 내용을 다시 모을 수 있음

### 1. git branch

> 현재의 브랜치 목록 출력

```bash
$ git branch

# 브랜치 생성
$ git branch <브랜치 이름>
```

<br>

### 2. git switch

> 브랜치 변경

```ba
$ git switch <브랜치 이름>
```

<br>

### 3. git merge

> 브런치 병합

```bash
$ git merge <병합할 branch>
```

- 기준이 되는 branch로 이동해서 병합을 수행
- 같은 파일, 같은 줄을 수정하여 충돌이 발생하면 직접 수정해서 merge  

<br>

### 4. git brach -d

> 브런치 삭제 (일반적으로 병합이 끝나고 삭제시킴)

```bash
$ git branch -d <삭제할 브랜치 이름>
```

<br>
