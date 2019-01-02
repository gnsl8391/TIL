# SSAFY-DAY1 (12/17/월)

## 첫번째 시간

### 자기소개

- 이름
- 전공
- 지원하게 된 계기

### 파이썬 코드 작성

- 크리스마스까지 남은 날짜 계산하기

```python
from datetime import datetime, timedelta

n = datetime.today().strftime("%Y년 %m월 %d일 %H시 %M분 %S초")
d1 = datetime(2018, 12, 25)
d2 = datetime.now()
print("오늘의 날짜는", n, "입니다.")
print("크리스마스까지 {0}일 남았습니다.".format((d1-d2).days))
```

- 안녕

```python
Hi = [1,2,3,4,5]

for i in Hi:
  print("안녕하세요")
```

- 저녁메뉴

```python
import random
menu = ["치킨","피자","파스타","김치찌개","라면","삼겹살"]
# 랜덤하게 하나를 뽑는 방법은?
pick = random.choice(menu)
print(pick)
```

- 저녁메뉴번호

```python
import random

# 1. menu 리스트를 만들어주세요.
menu = ['영암매력한우수완점','신전떡볶이 광주수완점','양동통닭','광주식당','회마켓','원조나주곰탕50년']

choice = random.choice(menu)
print(choice)

phonebook = {'영암매력한우수완점':'062-383-8118',
            '신전떡볶이 광주수완점':'062-956-2334',
             '양동통닭':'062-471-9277',
             '광주식당':'062-962-8284 ',
             '회마켓':'062-952-2026',
             '원조나주곰탕50년':'062-951-3255'
            }

print(phonebook[choice])
```

- 광주먼지

```python
import requests
from bs4 import BeautifulSoup
url = 'http://openapi.airkorea.or.kr/openapi/services/rest/ArpltnInforInqireSvc/getCtprvnRltmMesureDnsty?serviceKey=' + key + '&numOfRows=10&pageSize=10&pageNo=1&startPage=1&sidoName=%EA%B4%91%EC%A3%BC&ver=1.3'
request = requests.get(url).text
soup = BeautifulSoup(request,'xml')
dong = soup('item')[7]
location = dong.stationName.text
time = dong.dataTime.text
dust = int(dong.pm10Value.text)

if 0 <= dust <= 30:
  print("좋아요 환기합시다")
# 파이썬에서는 위와 같이 부등호를 양옆에 쓸 수 있다.
elif dust <= 80:
  print("그냥 살만해요")
elif dust <= 150:
  print("나가지 마요 중국 나빠요")
else:
  print("폐암주의! 중국 XX")

print("{0} 기준 {1}의 미세먼지 농도는 {2}입니다.".format(time,location,dust))
```

- 로또번호추천

```python
# 완성된 코드 붙여넣음
#import random
#numbers = []
#ran_num = random.randint(1,45)

#for i in range(6):
#  while ran_num in numbers:
#    ran_num = random.randint(1,45)
#  numbers.append(ran_num)
  
#numbers.sort()
#print(numbers)


import random
# numbers에 1부터 45넣기
numbers = list(range(1,46))
# numbers에서 6개를 뽑기
a = random.sample(numbers,6)
# 아래 코드는 오름차순 정렬하는 코드
a.sort()
print(a)
```

- 코스피

```python
import requests
from bs4 import BeautifulSoup

url = 'https://finance.naver.com/sise/'

r = requests.get(url).text
soup = BeautifulSoup(r,'html.parser')
select = soup.select_one("#KOSPI_now")
print(select.text)
```

### 소감

- 프로그래밍 언어를 공부하기 시작한 첫 날
- 보고 따라하는것 만으로도 정신 없었던 날
- 일년동안 열심히 하면 뭔가 많이 알아가지 않을까?
- 배운거 외에 구글링 해서 크리스마스 남은날짜 계산기 만들었을때 기분 신남 뭔가 해낸거같음
- 내일부터도 열심히 공부 해봐야지