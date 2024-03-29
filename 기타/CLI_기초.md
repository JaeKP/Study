# CLI 기초 🖥

## CLI 환경

> CLI: Command Line interface (명령 기반)
>
> GUI: Graphic User interface (그래픽 기반)
>
> **개발자들은 일반적으로 `개발환경`, `성능`때문에 CLI를 사용한다.** 

<br>

- 지금 대부분의 소프트웨어 개발을 CLI 즉, Git bash와 같은 환경에서 작업한다. 
- GUI 환경에 비해 동일한 일을 할 때 적은 리소스를 사용한다는 장점이 있다.
  ex) 파일을 삭제하는 간단한 행동조차 CLI환경에서 삭제하는 것이 더 빠르다. 

<br>

###  1. Git bash

- git bash 터미널: 유닉스 계열(운영체제_리눅스, 맥OS)에서 쓰는 일종의 명령 프롬프트이다. 
- 대부분의 개발은 리눅스 기반의 운영체제에서 사용하기 때문에 윈도우 명령 프롬프트를 사용하지 않는다. 

**우리는 이제 CLI환경에서 커맨드를 통해 컴퓨터 작업을 하는 것에 익숙해져야 한다!** 😀

<br>

###  2. 경로

file 이나 directory는 위치를 나타내는 경로가 존재한다. 

- root (directory) : `/`
- home(사용자 directory): `~`
- 현재 위치: `.`
- 이전 디렉토리: `..`

<br>

#### ✔경로 종류

1. 절대 경로

   시작점부터 그 파일이나 폴더의 위치까지 어떤 directory를 거쳐서 도달할 수 있는가. 

   ```
   $ pwd
   /c/Users/user/startcamp
   ```

2. 상대 경로
   현재 위치를 기준으로 경로를 표현 하는 것

   ```
   user@DESkTOP-E59vf&U MINGW64 ~/startcamp
   $ cd ../..
   
   user@DESkTOP-E59vf&U MINGW64 /c/Isers
   ```

<br>

---

## 명령어

### 1. 터미널 단축키

| `단축키`    | 설명                    |
| ----------- | ----------------------- |
| `tab`       | 폴더 / 파일 명 자동완성 |
| `ctrl` +`l` | reset                   |
| `ctrl` +`e` | 커서를 맨 뒤로          |
| `ctrl` +`a` | 커서를 맨 앞으로        |
| `↑`,  `↓`   | 사용했던 명령어 조회    |

<br>

### 2. 명령어

| `명령어`    | 설명                                                         |
| ----------- | ------------------------------------------------------------ |
| `ls`        | 현재 작업 중인 위치에 포함되어 있는 파일과 폴더들의 리스트를 출력해주는 것 |
| `ls -a`     | 숨김파일까지 확인                                            |
| `ls -l`     | 파일에 대한 추가적인 정보까지 열람 (수정날짜, 권한, 용량 등) |
| `cd` <경로> | change directorty. 작업하는 위치 변경 <br />- `cd ~`: 홈 디렉토리로 이동<br />- `cd /`: 루트 디렉토리로 이동<br />-`cd ..`부모(상위) 디렉토리로 이동 |
| `mkdir`     | make directory. 폴더만들기<br />여러 폴더를 생성하려면 공백 , `''`으로 구분해서 작성 |
| `touch`     | 파일을 생성. 여러 개 생성 가능<br />숨김파일은`.`으로 시작   |
| `rm`        | 파일 삭제 (GUI와 달리 삭제시 휴지통으로 가지 않는다)         |
| `rm -r`     | 디렉토리나 폴더 삭제                                         |
| `rm *`      | 파일명을 표시할 때 `*`을 사용하면 0개 이상의 어떤 문자열을 의미<br />예시)<br />`rm *txt`: txt로 끝나는 파일 전부 삭제<br />`rm ssafy*txt`: ssafy로 시작해서 txt로 끝나는 모든 파일 삭제 |
| `rmdir`     | 디렉토리 삭제                                                |
| `start`     | 폴더/ 파일을 여는 명령어<br />`start .`: 탐색기가 열린다     |

<br>
