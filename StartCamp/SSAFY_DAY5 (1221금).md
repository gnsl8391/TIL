# SSAFY_DAY5 (12/21/금)

## Chat_bot 기초 완성하기

### chat_bot code

```py
from flask import Flask, request
from pprint import pprint as pp
import os
import requests
import random
from datetime import datetime, timedelta

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"
    
api_url = "https://api.hphk.io/telegram"
token = os.getenv('TELE_TOKEN')

@app.route(f"/{token}", methods=['POST'])
def telegram():
    # naver api를 사용하기 위한 변수
    naver_client_id = os.getenv('NAVER_ID')
    naver_client_secret = os.getenv('NAVER_SECRET')
    
    # tele_dict = 데이터 덩어리
    tele_dict = request.get_json()
    pp(request.get_json())
    
    # 유저 정보
    chat_id = tele_dict['message']['from']['id']
    # print(chat_id)
    # 유저가 입력한 데이터
    # text = tele_dict['message']['text']
    # None 으로 반환하기 위해 형식 변경
    text = tele_dict.get('message').get('text')
    # print(text)
    
    tran = False
    img = False
    # 사용자가 이미지를 넣었는지 체크
    # if tele_dict['message']['photo'] is not None:
    # text를 넣었을때 error나는 부분을 변경
    if tele_dict.get('message').get('photo') is not None:
        img = True
        
    # 번역 "번역하고 싶은 문장" = 번역작업실행
    # text(유저가 입력한 데이터) 제일 앞 두글자가 '번역' 인지 확인
    else:
        if text[:2] == "번역":
            # 번역 안녕하세요
            tran = True
            text = text.replace(text[:3], "")
            # 안녕하세요
    
    if tran:
        papago = requests.post("https://openapi.naver.com/v1/papago/n2mt",
                    headers = {
                        "X-Naver-Client-Id":naver_client_id,
                        "X-Naver-Client-Secret":naver_client_secret
                    },
                    data = {
                        'source':'ko',
                        'target':'en',
                        'text':text
                    },
        )
        trans = papago.json()
        text = trans['message']['result']['translatedText']
    
    elif img:
        text = "사용자가 이미지를 넣었어요"
        # 텔레그램에게 사진 정보 가져오기
        file_id = tele_dict['message']['photo'][-1]['file_id']
        file_path = requests.get(f"{api_url}/bot{token}/getFile?file_id={file_id}").json()['result']['file_path']
        file_url = f"{api_url}/file/bot{token}/{file_path}"
        # print(file_url)
        # 사진을 네이버 유명인 인식 api로 넘겨주기
        file = requests.get(file_url, stream=True)
        clova = requests.post("https://openapi.naver.com/v1/vision/celebrity",
                    headers = {
                        "X-Naver-Client-Id":naver_client_id,
                        "X-Naver-Client-Secret":naver_client_secret
                    },
                    files = {
                        'image':file.raw.read()
                    },
        )
        # 가져온 데이터 중에서 필요한 정보 빼오기
        pp(clova.json())
        # 인식이 되었을때
        if clova.json().get('info').get('faceCount'):
            text = clova.json()['faces'][0]['celebrity']['value']
        # 인식이 되지않았을때
        else:
            text = "사람 얼굴이 아니에요"
        
    elif text == "메뉴":
        menu_list = ["한식1","중식","양식","분식","한식2"]
        text = random.choice(menu_list)
    elif text == "로또":
        num_list = range(1,46)
        text = random.sample(num_list,6)
        text.sort()
    elif text == "집에가고싶어":
        n1 = datetime.now()
        after_9h = timedelta(hours=9)
        text = n1 + after_9h
    
    # 유저에게 그대로 돌려주기
    requests.get(f'{api_url}/bot{token}/sendMessage?chat_id={chat_id}&text={text}')
    
    return '', 200
    
app.run(host=os.getenv('IP','0.0.0.0'), port=int(os.getenv('PORT',8080)))
```

- 미리 경험해본 로또 추출하는 코드와 점심메뉴 뽑아주는 코드 chat_bot 안에 넣기
- 오늘 시간 띄워주는 코드 작정해서 넣기



- chat_bot 안에 한글을 영어로 번역해주는 기능과 유명인 사진을 보고 누군지 분석하는 기능을 집어넣어보기
- Naver developer 에서 Clova Face Recognition 기능과 Papago NMT 번역 기능 가져오기
- .bashrc 에 환경변수로 비밀번호같은 숨김 정보를 저장
- 받은 API를 이용하여 변역기능과 사진분석기능 구현해보기
- 간단한 몇가지 기능을 더해 Telegram chat_bot 완성하기
- Heroku 사이트를 통해 배포(deploy) 해보기
- 어디서나 Telegram 어플을 통해 접속 가능한 상태로 완성

