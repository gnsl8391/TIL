# SSAFY-DAY4 (12/20/목)

크롬에서 json viewer 확장프로그램 다운받음

### Flask 심화과정

```py
from flask import Flask, render_template
import random
import requests
import json

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"
    
@app.route("/lotto")
def lotto():
    url = "https://www.dhlottery.co.kr/common.do?method=getLottoNumber&drwNo=837"
    res = requests.get(url).text
    lotto_dict = json.loads(res)
    # print(lotto_dict["drwNoDate"])
    week_num = []
    week_format_num = []
    drwtNo = ["drwtNo1","drwtNo2","drwtNo3","drwtNo4","drwtNo5","drwtNo6"]
    for num in drwtNo:
        number = lotto_dict[num]
        week_num.append(number)
        # print(week_num)
        
    for i in range(1,7):
        week_format_num.append(lotto_dict["drwtNo{}".format(i)])
        
    # pick = 우리가 생성한 번호
    # week_num = 이번주 당첨 번호
    # 위의 두 값을 비교해서 로또 당첨 등수 출력해보기
        
    # num1 = lotto_dict["drwtNo1"]
    # num2 = lotto_dict["drwtNo2"]
    # num3 = lotto_dict["drwtNo3"]
    # num4 = lotto_dict["drwtNo4"]
    # num5 = lotto_dict["drwtNo5"]
    # num6 = lotto_dict["drwtNo6"]
    # numB = lotto_dict["bnusNo"]
    # print(type(res))
    # print(type(json.loads(res)))
    
    num_list = range(1,46)
    pick = random.sample(num_list,6)
    pick.sort()
    # return render_template("lotto.html", lotto=pick, num1=num1, num2=num2, num3=num3, num4=num4, num5=num5, num6=num6, numB=numB)
    return render_template("lotto.html", lotto=pick, week_num=week_num, week_format_num=week_format_num)
```

- 로또 추천번호를 뽑고, 이번주 당첨번호도 같이 보여주는 html 문서 작성해보기
- 로또 API를 json 형태로 받아서 이용함



```py
from flask import Flask, render_template, request
import random
import requests
import json
from faker import Faker

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"
    
@app.route("/lotto")
def lotto():
    url = "https://www.dhlottery.co.kr/common.do?method=getLottoNumber&drwNo=837"
    res = requests.get(url).text
    lotto_dict = json.loads(res)
    week_format_num = []
    drwtNo = ["drwtNo1","drwtNo2","drwtNo3","drwtNo4","drwtNo5","drwtNo6"]
    bnusNo = lotto_dict["bnusNo"]
    isBonus = False
 
    for i in range(1,7):
        week_format_num.append(lotto_dict["drwtNo{}".format(i)])
        
    # pick = 우리가 생성한 번호
    # week_num = 이번주 당첨 번호
    # 위의 두 값을 비교해서 로또 당첨 등수 출력해보기
    
    num_list = range(1,46)
    pick = random.sample(num_list,6)
    # pick = [2,6,25,28,30,33]
    pick.sort()
    
    def intersect(a,b):
        return list(set(a)&set(b))
    comp = intersect(pick,week_format_num)
    comp_len = len(comp)
    # print(comp_len)
    
    for i in pick:
        if(i==bnusNo):
            isBonus = True
        
    if comp_len == 6:
        x = "1등입니다."
    elif comp_len == 5 and isBonus:
        x = "2등입니다."
    elif comp_len == 5:
        x = "3등입니다."
    elif comp_len == 4:
        x = "4등입니다."
    elif comp_len == 3:
        x = "5등입니다."
    else:
        x = "꽝입니다."
    
    return render_template("lotto.html", lotto=pick, week_format_num=week_format_num, x=x)
    
@app.route('/lottery')
def lottery():
    # 로또 정보를 가져온다. & 필요한 것만 고른다.
    url = "https://www.dhlottery.co.kr/common.do?method=getLottoNumber&drwNo=837"
    res = requests.get(url).text
    lotto_dict = json.loads(res)
    
    # 1등 당첨 번호를 week 리스트에 넣는다.
    week = []
    for i in range(1,7):
        week.append(lotto_dict["drwtNo{}".format(i)])
        
    # 보너스 번호를 bonus 변수에 넣는다.
    bonus = lotto_dict["bnusNo"]
        
    # 임의의 로또 번호를 생성한다.
    pick = random.sample(range(1,46),6)
    pick.sort()
    
    # 비교해서 몇등인지 저장한다.
    match = len(set(pick) & set(week))
    
    if match == 6:
        text = "1등"
    elif match == 5:
        if bonus in pick:
            text = "2등"
        else:
            text = "3등"
    elif match == 4:
        text = "4등"
    elif match == 3:
        text = "5등"
    else:
        text = "꽝"
    
    # 사용자에게 데이터를 넘겨준다.
    return render_template("lottery.html", pick=pick, week=week, text=text)
    
@app.route('/ping')
def ping():
    return render_template("ping.html")
    
@app.route('/pong')
def pong():
    input_name = request.args.get('name')
    fake = Faker('ko_KR')
    fake_job = fake.job()
    return render_template("pong.html", html_name=input_name, fake_job=fake_job)
    
@app.route('/hoho')
def hoho():
    return render_template("hoho.html")
    
@app.route('/haha')
def haha():
    input_name = request.args.get('name')
    fake = Faker('ko_KR')
    fake_color = fake.hex_color()
    return render_template("haha.html", html_name=input_name, fake_color=fake_color)
```

- 우리가 추천해준 번호와 이번주 당첨 번호를 비교해서 로또 당첨 등수를 출력해보는 작업
- html 을 이용하여 웹페이지에서 유저의 정보를 받아 다시 출력해주는 웹페이지 만들어보기
- Faker 라는 모듈을 사용하여 이미 저장된 리스트를 뽑아올 수 있음
- ex) 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style type="text/css">
       body { background-color : {{fake_color}} ; }
    </style>
</head>
<body>
    <h1> {{html_name}}님이랑 가장 어울리는 색입니다. </h1>
    <a href="/hoho"> 되돌아가기 </a>
</body>
</html>
```



### Dictionary 구조 이해하기

```py
phonebook = {
    "치킨집":"062-000-0000",
    "피자집":"062-000-0000"
}
# print(phonebook["치킨집"])

# 가수 그룹의 딕셔너리를 만들어주세요
# 변수명 : 그룹이름
# key : 이름
# value : 나이

IZ_one = {
    "권은비":"24살",
    "미야와키 사쿠라":"21살",
    "강혜원":"20살",
    "최예나":"20살",
    "이채연":"19살",
    "김채원":"19살",
    "김민주":"18살",
    "야부키 나코":"18살",
    "혼다 히토미":"18살",
    "조유리":"18살",
    "안유진":"16살",
    "장원영":"15살"
}

# 변수명 : idol
# 두개의 그룹을 넣어주세요

Blackpink = {
    "지수":"24살",
    "제니":"23살",
    "로제":"22살",
    "리사":"22살"
}

idol = {
    "IZ_one":IZ_one,
    "Blackpink":Blackpink
}

# print(idol)
# print(idol["IZ_one"]["권은비"])

score = {
    "수학":50,
    "국어":70,
    "영어":100
}

# for key, value in score.items():
#     pass
#     # print(key)
#     # print(value)
    
# for key in score.keys():
#     print(key)
    
# for value in score.values():
#     print(value)
    
# average = sum(score.values()) / len(score)
# print(average)

# score_sum = 0
# for value in score.values():
#     score_sum = score_sum + value
# print(score_sum/3)

ssafy = {
    "location": ["서울", "대전", "구미", "광주"],
    "language": {
        "python": {
            "python standard library": ["os", "random", "webbrowser"],
            "frameworks": {
                "flask": "micro",
                "django": "full-functioning"
            },
            "data_science": ["numpy", "pandas", "scipy", "sklearn"],
            "scrapying": ["requests", "bs4"],
        },
        "web" : ["HTML", "CSS"]
    },
    "classes": {
        "gj1":  {
            "lecturer": "ChangE",
            "manager": "pro1",
            "class president": "서희수",
            "groups": {
                "두드림": ["구종민", "김녹형", "윤은솔", "이준원", "이창훈"],
                "런치타임": ["문영국", "박나원","박희승", "서희수", "황인식"],
                "Friday": ["강민지", "박현진", "서상준", "안현상", "최진호"],
                "TMM": ["김훈", "송건희", "이지선", "정태준", "조호근"],
                "살핌": ["문동식", "이중봉", "이지희", "차상권", "최보균"]
            }
        },
        "gj2": {
            "lecturer": "teacher2",
            "manager": "pro2"
        },
        "gj3": {
            "lecturer": "teacher3",
            "manager": "pro3"
        }
    }
}

# ssafy를 진행하는 지역(location)은 몇개 인가요?
print(len(ssafy["location"]))

# python standard library 'requests'가 있나요?
psl = ssafy["language"]["python"]["python standard library"]
# print(psl)
if "requests" in psl:
    text = "있어요~"
else:
    text = "없어요~"
print(text)

# gj 1반의 반장의 이름을 출력하세요
print(ssafy["classes"]["gj1"]["class president"])

# ssafy에서 배우는 언어들을 출력하세요 : dictionary.keys 활용
for lang in ssafy["language"].keys():
    print(lang)
    
# ssafy gj2의 강사와 매니저의 이름을 출력하세요
# 예시) teacher2
#       pro2
for man in ssafy["classes"]["gj2"].values():
    print(man)

# framework들의 이름과 설명을 다음과 같이 출력하세요
# 예시)
# flask는 micro이다.
# django는 full-functioning이다.


# 오늘 당번을 뽑기 위해 '살핌' 조원중 1명을 랜덤으로 뽑아주세요
# 예시)
# 오늘 당번은 문동식입니다.
```

- dictionary 구조에서 원하는 정보를 빼오는 과정을 학습



### 텔레그램을 이용하여 bot 만들어보기

```pyt
import os
import requests
import json

token = os.getenv('TELE_TOKEN')
method = 'getUpdates'

url = "https://api.hphk.io/telegram/bot{}/{}".format(token, method)

res = requests.get(url).json()

# ID 값 찾아서 넣기
user_id = res["result"][0]["message"]["from"]["id"]
msg = "왜"

method = 'sendMessage'
msg_url = "https://api.hphk.io/telegram/bot{}/{}?chat_id={}&text={}".format(token, method, user_id, msg)

print(msg_url)
requests.get(msg_url)



# print(user_id)
# print(res)
```

- 텔레그램을 이용하여 봇 체험하기 기초
- 봇이 나에게 메세지를 보내는 과정을 학습