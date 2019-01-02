# SSAFY-DAY2 (12/18/화)

## Python 심화과정

### Git for Windows 설치

- Unix tools 체크하고 설치하기



### Python 설치

- 3.6.7 버전으로 설치
- All Realease 에서 Web Based Installer로 설치
- 설치과정에서 환경변수 등록 누르고 설치



### VSCode 설치

- 확장탭에서 korea 검색 후 언어팩 설치
- Ctrl + ` : 터미널 여는 단축키
- Ctrl + Shift + P 눌러서 Shell 검색 후 Git Bash 로 설정하기



### Git Bash 명령어

- pwd : 현재의 위치(디렉토리)를 나타냄
- mkdir 폴더명 : 폴더 생성하기
- ls : 현재 위치에 있는 파일들을 나타냄
- ls -a : 현재 위치에 있는 모든 파일들을 나타냄
- clear : 창에 있는 메시지를 모두 지우기
- touch 파일명.확장자 : 파일생성
- cd 폴더명 : 폴더로 위치를 이동
- pip install 모듈이름 : 모듈을 다운로드 하는 명령어
- python 파이썬파일명 : 파이썬파일 실행



### 실습

크롬에서 코드 복사하기

- 원하는 정보에 우클릭 후 검사
- 원하는 코드에 우클릭 copy - copy selector



import webbrowser

webbrowser.open() : 웹 브라우저 주소를 열어주는 명령어

webbrowser.open_new() : 새창으로 열기

webbrowser.open_new_tap() : 새탭으로 열기



인터넷 주소는?

- ? 뒤에는 옵션
- & 로 옵션값을 나눈다

ex) 구글에서 list에 있는 단어들을 검색하는 창을 띄우는 코드

```python
import webbrowser

q_list = ["BTS","아이유","블랙핑크","Winner"]

url = "https://www.google.com/search?&q="

for q in q_list:
    webbrowser.open(url+q)
```



import requests

requests.get() : 

requests.get().text : 

requests.get().status_code : 



from bs4 import BeautifulSoup

BeautifulSoup.select('') : 문서 안에 있는 특정 내용을 뽑아줘 (list 형식으로 반환)

BeautifulSoup.select_one('') : 문서 안에 있는 특정 내용을 하나만 뽑아줘 (객체 하나만 반환)



ex) 다음 실시간 검색어 1위를 출력하는 코드

```python
import requests
from bs4 import BeautifulSoup

url = "https://www.daum.net/"
res = requests.get(url).text

soup = BeautifulSoup(res,'html.parser')
pick = soup.select_one('#mArticle > div.cmain_tmp > div.section_media > div.hotissue_builtin > div.realtime_part > ol > li:nth-of-type(1) > div > div:nth-of-type(1) > span.txt_issue > a')
print(pick.text)
```

#bs4에서는 child는 인식하지 못하기 때문에 똑같은 의미인 of-type으로 수정해 줌

ex) 다음 실시간 검색어를 모두 출력하는 코드

```python
import requests
from bs4 import BeautifulSoup

url = "https://www.daum.net/"
res = requests.get(url).text

soup = BeautifulSoup(res,'html.parser')
picks = soup.select('#mArticle > div.cmain_tmp > div.section_media > div.hotissue_builtin.hide > div.realtime_part > ol > li > div > div:nth-of-type(1) > span.txt_issue > a')
for p in picks:
    print(p.text)
```

ex) 코인 이름과 가격 표시하는 코드

```python
import requests
from bs4 import BeautifulSoup

url = "https://www.bithumb.com/"
res = requests.get(url).text
soup = BeautifulSoup(res,'html.parser')
coins = soup.select('tbody.coin_list tr')
for coin in coins:
    print(coin.select_one("td:nth-of-type(1) a strong").text)
    print(coin.select_one("td:nth-of-type(2) strong").text)
```

ex) 멜론으로부터 승인받는 코드

```python
import requests
from bs4 import BeautifulSoup

url = "https://www.melon.com/index.htm"
headers = {
    'User-Agent': 'My User Agent 1.0',
    'From': 'youremail@domain.com'  # This is another valid field
}
response = requests.get(url, headers=headers)
print(response)
```



### zzu.li / bit.do



ex) 멜론차트 50위 불러오는 웹 크롤링 코드

```python
import requests
from bs4 import BeautifulSoup

url = "https://www.melon.com/chart/index.htm"
headers = {
    'User-Agent': 'My User Agent 1.0',
    'From': 'youremail@domain.com'  # This is another valid field
}
response = requests.get(url, headers=headers).text
soup = BeautifulSoup(response,'html.parser')


music_table = soup.select("table tr.lst50")

for music in music_table:
    number = music.select_one("span.rank").text
    title = music.select_one("div.wrap_song_info a").text
    print(number +"위 : "+ title)
```



import os 모듈 사용해보기

ex) 폴더안에 있는 파일 이름바꾸기

```python
import os

os.chdir(r'C:\Users\student\Hoon\SSAFY지원자')

for filename in os.listdir("."):
    # os.rename(filename, "SAMSUNG_" + filename)
    if filename.endswith("txt"):
        new_filename = filename.replace("SAMSUNG","SSAFY")
        os.rename(filename, new_filename)
```



### Ctrl + / : 주석처리



### Git 사용하기

git init

git add 파일명 (. 은 현재폴더에 있는 모든 파일과 폴더를 뜻함)

git commit -m "메세지"

git log

git status

.gitignore : 안넣고 싶은 파일명 기입하면 git으로 업로드가 안됨

. : 현재폴더

.. : 상위폴더

폴더 앞에 . : 숨겨져 있는 폴더



### Github에 등록하기

아이디 등록하고 들어가서 우측 상단에 + 모양 클릭해서 New repository 클릭

Repository name 적고 Description 적고 Create repository로 등록

첫번째 칸의 5번째 복붙, 6번째 복붙 하고 로그인창 뜨면 로그인 하고 등록



### 웹 사이트 바꾸기