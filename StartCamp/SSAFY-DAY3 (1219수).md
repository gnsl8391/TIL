# SSAFY-DAY3 (12/19/수)

### c9.io 사용하기

pyenv 설치하기

pyenv를 github에서 불러오기

pyenv install -list 하면 설치할 수 있는 list를 출력해줌

터미널 창에 pyenv install 3.6.7 하여 설치하기

pyenv global 3.6.7 을 넣어서 버전 적용하기 - pyenv rehash 로 재시작



### python으로 email 보내기

```python
import smtplib
from email.message import EmailMessage

# import getpass
# password = getpass.getpass('비밀번호뭐니 : ')

email_list = ['gnsl8391@gmail.com', 'gnsl8391@daum.net']

for email in email_list:
    
    msg = EmailMessage()
    msg['Subject'] = "곧 점심시간입니다."
    msg['From'] = "gnsl8391@naver.com"
    msg['To'] = email
    msg.set_content('오늘 점심은 뭐가 나올지 너무 궁금해요~')

    smtp_url = 'smtp.naver.com'
    smtp_port = 465
    
    s = smtplib.SMTP_SSL(smtp_url, smtp_port)
    
    s.login('gnsl8391','YQ8S8S8ZCT62')
    s.send_message(msg)
```

```python
import csv

import smtplib
from email.message import EmailMessage

# import getpass
# password = getpass.getpass('비밀번호뭐니 : ')

f = open('pygj.csv', 'r', encoding='utf-8')

smtp_url = 'smtp.naver.com'
smtp_port = 465
        
s = smtplib.SMTP_SSL(smtp_url, smtp_port)
s.login('gnsl8391','YQ8S8S8ZCT62')

read_csv = csv.reader(f)

for line in read_csv:
    
    msg = EmailMessage()
    msg['Subject'] = "저는 김훈 입니다. 곧 점심시간입니다."
    msg['From'] = "gnsl8391@naver.com"
    msg['To'] = line[1]
    msg.set_content('저는' + line[0] + '에게 메일을 보냈습니다. 오늘 점심은 뭐가 나올지 너무 궁금해요~')

    s.send_message(msg)    
    
f.close()
```

- 두번째 코드는 여러명에게 csv파일을 이용하여 일괄 보내기 기능을 담았습니다.



### 플라스크 사용해보기

flask 검색해서 사이트 들어가기

pip install flask 로 설치



### HTML 문서 작성해보기

```py
from flask import Flask, render_template
import random
app = Flask(__name__)

@app.route("/")
def hello():
    return "<h1>서버가 html도 보내주나??</h1>"
    
@app.route("/html_tag")
def html_tag():
    return """
    <h1>첫번째 줄</h1>
    <h2>두번째 줄</h2>
    """
    
@app.route("/html_file")
def html_file():
    return render_template("html_file.html")
    
@app.route("/welcome/<string:name>")
def welcome(name):
    return render_template("welcome.html", people=name)
    
@app.route("/cube/<int:num>")
def cube(num):
    return render_template("cube.html", result=num**3, num=num)
    
@app.route("/lunch")
def lunch():
    menu = ["볶음밥","칼국수","알탕","삼계탕","샤브샤브","삼겹살","스테이크","라면"]
    pick = random.choice(menu)
    return render_template("lunch.html", menu=pick)
    
@app.route("/lotto")
def lotto():
    numbers = list(range(1,46))
    a = random.sample(numbers,6)
    a.sort()
    return render_template("lotto.html", lotto=a)
    
@app.route("/naver")
def naver():
    return render_template("naver.html")
```

- html 문서를 확인할 때는 터미널에 FLASK_APP=app.py flask run --host=$IP --port=$PORT 를 적어준다.
- ctrl + c 로 종료시킬 수 있다.
- ! 이후에 Tab 키를 누르면 html 문서의 기본구조가 나타난다
- tag 키 이후에 Tab 키를 누르면 자동으로 <> 가 채워진다

