# 4일차: 실시간 데이터 수집 및 결과물 Git에 기록하기

---

## 핵심개념

### HTTP 통신 (GET 요청)

HTTP(HyperText Transfer Protocol)는 웹에서 데이터를 주고받는 규약입니다. 클라이언트(브라우저, 프로그램)가 서버에 요청을 보내면, 서버가 응답을 반환합니다.

**HTTP 요청 방식:**

| 메서드 | 용도 | 설명 |
|--------|------|------|
| `GET` | 조회 | 데이터를 요청하여 받아옴 (가장 많이 사용) |
| `POST` | 생성 | 서버에 새 데이터를 전송 |
| `PUT` | 수정 | 기존 데이터를 업데이트 |
| `DELETE` | 삭제 | 데이터를 삭제 |

**GET 요청의 흐름:**

```
[클라이언트]  ──── GET 요청 ────>  [서버]
     │                              │
     │      (예: 날씨 정보 요청)      │
     │                              │
     │  <──── JSON 응답 ────        │
     │      (날씨 데이터 반환)        │
```

**HTTP 응답 상태 코드:**

| 코드 | 의미 | 설명 |
|------|------|------|
| `200` | OK | 요청 성공 |
| `400` | Bad Request | 잘못된 요청 |
| `401` | Unauthorized | 인증 필요 (API 키 오류 등) |
| `404` | Not Found | 리소스를 찾을 수 없음 |
| `500` | Server Error | 서버 오류 |

---

### API의 개념

API(Application Programming Interface)는 프로그램 간 데이터를 주고받기 위한 인터페이스입니다.

**일상 비유:**
- 식당에서 손님(클라이언트)이 웨이터(API)에게 주문하면
- 웨이터가 주방(서버)에 전달하고
- 요리(데이터)를 가져다 줍니다

**API의 종류:**

| 유형 | 설명 | 예시 |
|------|------|------|
| 오픈 API | 누구나 사용 가능 (무료/유료) | 날씨, 지도, 뉴스 API |
| 내부 API | 조직 내부에서만 사용 | 사내 시스템 연동 |
| 파트너 API | 제휴 관계에서만 사용 | 결제, 인증 시스템 |

**API 구성 요소:**

```
https://api.example.com/v1/weather?city=Seoul&lang=ko
└─────── 기본 URL ──────┘└ 버전 ┘└ 엔드포인트 ┘└── 쿼리 파라미터 ──┘
```

- **Base URL**: API 서버 주소
- **Endpoint**: 특정 기능/리소스 경로
- **Query Parameter**: 추가 조건 (?key=value&key2=value2)
- **API Key**: 인증을 위한 고유 키 (보통 필수)

**API 문서 읽는 법:**

```
GET /weather/current

Parameters:
- city (required): 도시명
- units (optional): 온도 단위 (metric/imperial)

Headers:
- Authorization: Bearer YOUR_API_KEY

Response:
{
  "city": "Seoul",
  "temperature": 15.5,
  "description": "맑음"
}
```

---

### 데이터 파싱 (Parsing)

파싱은 받아온 데이터에서 필요한 정보만 추출하는 과정입니다.

**파싱이 필요한 이유:**
- API 응답은 대량의 데이터를 포함
- 필요한 정보만 골라서 사용해야 함
- 데이터 구조를 이해하고 접근해야 함

**JSON 파싱 예시:**

```python
# API에서 받은 원본 데이터
response_data = {
    "cod": "200",
    "message": 0,
    "cnt": 40,
    "city": {
        "id": 1835848,
        "name": "Seoul",
        "country": "KR"
    },
    "list": [
        {
            "dt": 1704067200,
            "main": {
                "temp": 275.15,
                "humidity": 65
            },
            "weather": [
                {"description": "clear sky"}
            ]
        }
    ]
}

# 필요한 정보만 파싱
city_name = response_data["city"]["name"]           # "Seoul"
temperature = response_data["list"][0]["main"]["temp"]  # 275.15
humidity = response_data["list"][0]["main"]["humidity"] # 65
description = response_data["list"][0]["weather"][0]["description"]  # "clear sky"
```

**중첩된 데이터 접근:**

```python
data = {
    "users": [
        {
            "name": "홍길동",
            "contacts": {
                "email": "hong@email.com",
                "phone": "010-1234-5678"
            }
        }
    ]
}

# 단계별 접근
users = data["users"]           # 리스트
first_user = users[0]           # 첫 번째 사용자 딕셔너리
contacts = first_user["contacts"]  # 연락처 딕셔너리
email = contacts["email"]       # "hong@email.com"

# 한 줄로 접근
email = data["users"][0]["contacts"]["email"]
```

---

## 실습활동

### 실습 1: requests 라이브러리로 오픈 API 호출하기

**목표:** 파이썬 requests 라이브러리를 사용하여 날씨 API를 호출하고 데이터를 받아옵니다.

#### Step 1: requests 라이브러리 설치

터미널에서 실행:

```bash
pip install requests
```

#### Step 2: 간단한 GET 요청 테스트

먼저 공개 테스트 API로 연습합니다.

```python
# test_api.py
import requests

# 테스트용 공개 API (JSONPlaceholder)
url = "https://jsonplaceholder.typicode.com/posts/1"

# GET 요청 보내기
response = requests.get(url)

# 응답 확인
print(f"상태 코드: {response.status_code}")  # 200이면 성공
print(f"응답 데이터: {response.json()}")
```

**예상 출력:**
```
상태 코드: 200
응답 데이터: {'userId': 1, 'id': 1, 'title': '...', 'body': '...'}
```

#### Step 3: OpenWeatherMap API 사용하기

**API 키 발급:**
1. [OpenWeatherMap](https://openweathermap.org/) 접속
2. 회원가입 후 로그인
3. API Keys 메뉴에서 키 확인 (무료 플랜 사용)

**날씨 데이터 가져오기:**

```python
# weather_api.py
import requests
import json

# API 설정
API_KEY = "your_api_key_here"  # 발급받은 API 키 입력
CITY = "Seoul"
URL = f"https://api.openweathermap.org/data/2.5/weather?q={CITY}&appid={API_KEY}&units=metric&lang=kr"

# API 호출
response = requests.get(URL)

# 응답 확인
if response.status_code == 200:
    data = response.json()

    # 데이터 파싱
    city = data["name"]
    temp = data["main"]["temp"]
    feels_like = data["main"]["feels_like"]
    humidity = data["main"]["humidity"]
    description = data["weather"][0]["description"]

    # 결과 출력
    print(f"=== {city} 날씨 정보 ===")
    print(f"현재 온도: {temp}°C")
    print(f"체감 온도: {feels_like}°C")
    print(f"습도: {humidity}%")
    print(f"날씨: {description}")
else:
    print(f"오류 발생: {response.status_code}")
    print(response.text)
```

**예상 출력:**
```
=== Seoul 날씨 정보 ===
현재 온도: 15.2°C
체감 온도: 14.8°C
습도: 62%
날씨: 맑음
```

#### Step 4: 뉴스 API 사용하기 (대안)

**NewsAPI 사용:**
1. [NewsAPI](https://newsapi.org/) 접속
2. 회원가입 후 API 키 발급

```python
# news_api.py
import requests
import json

# API 설정
API_KEY = "your_news_api_key"
URL = f"https://newsapi.org/v2/top-headlines?country=kr&apiKey={API_KEY}"

# API 호출
response = requests.get(URL)

if response.status_code == 200:
    data = response.json()
    articles = data["articles"]

    print(f"=== 오늘의 뉴스 (총 {len(articles)}건) ===\n")

    for i, article in enumerate(articles[:5], 1):  # 상위 5개만
        print(f"{i}. {article['title']}")
        print(f"   출처: {article['source']['name']}")
        print(f"   링크: {article['url']}\n")
else:
    print(f"오류: {response.status_code}")
```

---

### 실습 2: 데이터를 JSON 파일로 저장하고 GitHub에 기록하기

**목표:** API에서 받은 데이터를 파일로 저장하고 Git으로 버전 관리합니다.

#### Step 1: 데이터를 JSON 파일로 저장

```python
# save_weather.py
import requests
import json
from datetime import datetime

# API 설정
API_KEY = "your_api_key_here"
CITY = "Seoul"
URL = f"https://api.openweathermap.org/data/2.5/weather?q={CITY}&appid={API_KEY}&units=metric&lang=kr"

# API 호출
response = requests.get(URL)

if response.status_code == 200:
    raw_data = response.json()

    # 필요한 데이터만 정리
    weather_data = {
        "collected_at": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
        "city": raw_data["name"],
        "country": raw_data["sys"]["country"],
        "weather": {
            "temperature": raw_data["main"]["temp"],
            "feels_like": raw_data["main"]["feels_like"],
            "humidity": raw_data["main"]["humidity"],
            "description": raw_data["weather"][0]["description"]
        },
        "wind": {
            "speed": raw_data["wind"]["speed"]
        }
    }

    # JSON 파일로 저장
    filename = f"weather_{datetime.now().strftime('%Y%m%d_%H%M%S')}.json"

    with open(filename, "w", encoding="utf-8") as f:
        json.dump(weather_data, f, ensure_ascii=False, indent=2)

    print(f"데이터가 '{filename}'에 저장되었습니다.")
    print(json.dumps(weather_data, ensure_ascii=False, indent=2))
else:
    print(f"API 호출 실패: {response.status_code}")
```

**저장된 파일 예시 (weather_20240115_143022.json):**
```json
{
  "collected_at": "2024-01-15 14:30:22",
  "city": "Seoul",
  "country": "KR",
  "weather": {
    "temperature": 15.2,
    "feels_like": 14.8,
    "humidity": 62,
    "description": "맑음"
  },
  "wind": {
    "speed": 2.5
  }
}
```

#### Step 2: Git 초기화 및 원격 저장소 연결

```bash
# 프로젝트 폴더로 이동
cd your-project-folder

# Git 초기화 (처음 한 번만)
git init

# 원격 저장소 연결 (GitHub에서 미리 저장소 생성)
git remote add origin https://github.com/username/pre-course-log.git
```

#### Step 3: 변경사항 커밋 및 푸시

```bash
# 현재 상태 확인
git status

# 파일 추가
git add weather_20240115_143022.json
# 또는 모든 JSON 파일 추가
git add *.json

# 커밋
git commit -m "feat: 날씨 데이터 수집 (2024-01-15)"

# GitHub에 푸시
git push origin main
# 또는 첫 푸시인 경우
git push -u origin main
```

#### Step 4: 전체 자동화 스크립트

```python
# collect_and_commit.py
import requests
import json
import os
from datetime import datetime

def collect_weather():
    """날씨 데이터 수집"""
    API_KEY = "your_api_key_here"
    CITY = "Seoul"
    URL = f"https://api.openweathermap.org/data/2.5/weather?q={CITY}&appid={API_KEY}&units=metric&lang=kr"

    response = requests.get(URL)

    if response.status_code != 200:
        raise Exception(f"API 오류: {response.status_code}")

    raw_data = response.json()

    return {
        "collected_at": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
        "city": raw_data["name"],
        "temperature": raw_data["main"]["temp"],
        "humidity": raw_data["main"]["humidity"],
        "description": raw_data["weather"][0]["description"]
    }

def save_to_json(data, folder="data"):
    """JSON 파일로 저장"""
    # 폴더 생성
    if not os.path.exists(folder):
        os.makedirs(folder)

    # 파일명 생성
    filename = f"{folder}/weather_{datetime.now().strftime('%Y%m%d_%H%M%S')}.json"

    with open(filename, "w", encoding="utf-8") as f:
        json.dump(data, f, ensure_ascii=False, indent=2)

    return filename

def git_commit_and_push(filename):
    """Git 커밋 및 푸시"""
    date_str = datetime.now().strftime("%Y-%m-%d %H:%M")

    os.system(f'git add {filename}')
    os.system(f'git commit -m "data: 날씨 데이터 수집 ({date_str})"')
    os.system('git push origin main')

# 실행
if __name__ == "__main__":
    try:
        print("1. 날씨 데이터 수집 중...")
        weather_data = collect_weather()
        print(f"   수집 완료: {weather_data['city']} {weather_data['temperature']}°C")

        print("2. JSON 파일 저장 중...")
        filename = save_to_json(weather_data)
        print(f"   저장 완료: {filename}")

        print("3. Git 커밋 및 푸시 중...")
        git_commit_and_push(filename)
        print("   푸시 완료!")

        print("\n모든 작업이 완료되었습니다!")

    except Exception as e:
        print(f"오류 발생: {e}")
```

---

## Git 명령어 정리

| 명령어 | 설명 |
|--------|------|
| `git init` | 새 Git 저장소 초기화 |
| `git status` | 현재 상태 확인 |
| `git add <파일>` | 스테이징 영역에 추가 |
| `git commit -m "메시지"` | 변경사항 커밋 |
| `git push origin main` | 원격 저장소에 푸시 |
| `git pull origin main` | 원격 저장소에서 가져오기 |
| `git log --oneline` | 커밋 히스토리 확인 |

---

## 완료 체크리스트

- [ ] requests 라이브러리를 설치했는가?
- [ ] 테스트 API 호출에 성공했는가?
- [ ] 날씨 또는 뉴스 API 키를 발급받았는가?
- [ ] API에서 데이터를 받아왔는가?
- [ ] 데이터를 JSON 파일로 저장했는가?
- [ ] Git으로 커밋을 생성했는가?
- [ ] GitHub에 푸시했는가?

---

## 참고 자료

- [requests 라이브러리 공식 문서](https://requests.readthedocs.io/)
- [OpenWeatherMap API 문서](https://openweathermap.org/api)
- [NewsAPI 문서](https://newsapi.org/docs)
- [Git 간편 안내서](https://rogerdudler.github.io/git-guide/index.ko.html)
- [HTTP 상태 코드 목록](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)
