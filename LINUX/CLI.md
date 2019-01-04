# CLI

- CLI (Command Line Interface) - 키보드 만으로 컴퓨터를 조작
- GUI (Graphic User Interface) - 키보드와 마우스로 조작

### witch OS ?

- Unix based OS (Linux, Mac OS 등등)
- Unix => 서버를 관리하는 시점에서 무조건 사용하게 됩니다.

### Prompt

- $ => 컴퓨터가 명령을 받을 준비가 됨
- 뭔가 막혔을땐 => Ctrl + c 를 통해서 탈출

### 명령어

|        명령어         |                             설명                             |    사용법    |
| :-------------------: | :----------------------------------------------------------: | :----------: |
|        `echo`         |                 다음에 들어오는 내용을 리턴                  | echo "Hello" |
|      `ctrl + c`       |                           작업중지                           |   ctrl + c   |
|         `yes`         | 다음에 들어오는 내용을 무한하게 리턴 (CPU의 상태를 체크할 때) |  yes python  |
|    방향키 `↑`, `↓`    |                   이전 명령어, 다음 명령어                   |   '↑', '↓'   |
| `ctrl + l` or `clear` |                     터미널 깔끔하게 정리                     |    clear     |

### 파일 조작

- `-` 다음에 오는 명령어는 조합할 수 있음

|      명령어      |                          설명                          |           사용법            |
| :--------------: | :----------------------------------------------------: | :-------------------------: |
|       `>`        |         왼쪽의 출력물을 오른쪽 파일로 전송하기         |  echo "Hello" > Hello.txt   |
|       `>>`       |          왼쪽의 출력물을 오른쪽 파일로 붙이기          |   echo "Hi~" >> Hello.txt   |
|   `cat <file>`   |             `<file>` 의 내용을 화면에 출력             |        cat hello.txt        |
|       `ls`       |           파일/ 디렉토리 들의 목록을 보여줌            |             ls              |
|     `ls -a`      |           숨김파일을 포함해서 목록을 보여줌            |            ls -a            |
|     `ls -l`      |          파일의/ 디렉토리 들의 정보를 보여줌           |            ls -l            |
| `mv <old> <new>` | `<old>` 의 이름을 `<new>` 로 변경 (나중에 이동도 시킴) |     mv Hello Hello.txt      |
| `cp <old> <new>` |               `<old>` 를 `<new>` 로 복사               | cp Hello.txt copy_Hello.txt |
|   `rm <file>`    |                   `<file>` 을 지우기                   |      rm copy_Hello.txt      |
|  `rm -i <file>`  |           `<file>` 을 지울지 물어보고 지우기           |    rm -i copy_Hello.txt     |
|  `rm -r <dir>`   |     `<dir>` 와 그 하위 폴더와 파일까지 전부 지우기     |         rm -r .git          |
|  `rm -f <file>`  |               `<file>` 을 강제로 지우기                |    rm -f copy_Hello.txt     |
|        탭        |                        자동완성                        |                             |
|      두번탭      |                     목록을 보여줌                      |                             |
|       `*`        |                         모든것                         |                             |
|  `touch <file>`  |                    `<file>` 을 생성                    |                             |

### 파일 검사

|    명령어     |               설명               |       사용법       |
| :-----------: | :------------------------------: | :----------------: |
|    `curl`     |          url과 상호작용          | curl -O http:// ~~ |
| `head <file>` |        파일의 앞부분 출력        | head bohemian.txt  |
| `tail <file>` |        파일의 뒷부분 출력        | tail bohemian.txt  |
|  `wc <file>`  | 파일의 줄, 단어, 바이트를 카운트 |  wc bohemian.txt   |

### 디렉토리

| 명령어        | 설명                                            | 사용법 |
| ------------- | ----------------------------------------------- | ------ |
| `ls`          | 파일/폴더 내용을 출력                           |        |
| `pwd`         | print working directory / 현재 폴더 내용을 출력 |        |
| `mkdir <dir>` | `<dir>` 폴더를 생성                             |        |
| `cd <dir>`    | `<dir>` 위치로 이동                             |        |
| `cd ..`       | 상위 폴더로 이동                                |        |

- `/` : 최상위 루트
- `~` : 홈 디렉토리