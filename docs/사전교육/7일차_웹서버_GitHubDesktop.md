# 07일차: 웹서버 만들어보기 및 GitHub Desktop을 활용한 코드 동기화

---

## 핵심개념

### 웹 서버의 역할

웹 서버는 클라이언트(브라우저)의 요청을 받아 웹 페이지, 이미지, 데이터 등을 전송하는 소프트웨어입니다.

**웹 통신의 기본 흐름:**

```
┌──────────────┐         HTTP 요청          ┌──────────────┐
│              │  ───────────────────────>  │              │
│   클라이언트   │   GET /index.html          │   웹 서버    │
│   (브라우저)   │                            │  (Apache,    │
│              │  <───────────────────────  │   Nginx 등)  │
│              │         HTTP 응답           │              │
└──────────────┘   HTML, CSS, JS, 이미지     └──────────────┘
```

**웹 서버의 주요 역할:**

| 역할 | 설명 |
|------|------|
| 정적 파일 제공 | HTML, CSS, JS, 이미지 파일 전송 |
| 요청 처리 | URL에 따라 적절한 리소스 반환 |
| 보안 | HTTPS 암호화, 접근 제어 |
| 로깅 | 접속 기록, 에러 로그 저장 |
| 리버스 프록시 | 백엔드 서버로 요청 전달 |

**정적 웹 vs 동적 웹:**

| 구분 | 정적 웹 | 동적 웹 |
|------|---------|---------|
| 콘텐츠 | 미리 만들어진 파일 | 요청 시 생성 |
| 예시 | HTML, 이미지 | 검색 결과, 로그인 페이지 |
| 서버 부하 | 낮음 | 높음 |
| 기술 | HTML, CSS, JS | Python, PHP, Node.js + DB |

**주요 웹 서버 소프트웨어:**

| 웹 서버 | 특징 |
|---------|------|
| **Apache** | 가장 오래된 웹 서버, 모듈 확장성 |
| **Nginx** | 고성능, 리버스 프록시, 로드 밸런싱 |
| **IIS** | Windows 전용, Microsoft 제품 |
| **Python http.server** | 개발/테스트용 간단한 서버 |
| **Node.js (Express)** | JavaScript 기반 서버 |

**로컬 웹 서버란:**
- 본인 컴퓨터에서만 접속 가능한 웹 서버
- 개발 및 테스트 용도
- 주소: `http://localhost` 또는 `http://127.0.0.1`
- 포트: 기본 80, 개발용으로 8000, 3000 등 사용

---

### GUI 기반의 Git 관리

Git은 명령어(CLI)로도 사용할 수 있지만, GUI 도구를 사용하면 시각적으로 쉽게 관리할 수 있습니다.

**Git의 핵심 개념 복습:**

```
┌─────────────────────────────────────────────────────────────┐
│  Working Directory    Staging Area       Local Repo        │
│  (작업 디렉토리)        (스테이징)         (로컬 저장소)      │
│                                                             │
│    파일 수정    ──git add──>  스테이징  ──git commit──> 커밋  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
                              │
                              │ git push
                              ▼
                    ┌─────────────────┐
                    │  Remote Repo    │
                    │  (GitHub 등)    │
                    └─────────────────┘
```

**주요 Git 작업:**

| 작업 | CLI 명령어 | 설명 |
|------|-----------|------|
| **Commit** | `git commit` | 변경사항을 로컬에 저장 |
| **Push** | `git push` | 로컬 커밋을 원격 저장소에 업로드 |
| **Pull** | `git pull` | 원격 저장소의 변경사항을 로컬로 가져오기 |
| **Fetch** | `git fetch` | 원격 변경사항 확인 (병합 전) |
| **Branch** | `git branch` | 독립적인 작업 공간 생성 |
| **Merge** | `git merge` | 브랜치 병합 |

**GUI Git 도구 비교:**

| 도구 | 특징 | 가격 |
|------|------|------|
| **GitHub Desktop** | GitHub 공식, 초보자 친화적 | 무료 |
| **Sourcetree** | 고급 기능, Atlassian 제품 | 무료 |
| **GitKraken** | 세련된 UI, 크로스플랫폼 | 무료/유료 |
| **VS Code Git** | 에디터 내장, 가벼움 | 무료 |
| **Fork** | 빠르고 직관적 | 유료 |

**GitHub Desktop의 장점:**
- GitHub와 완벽한 통합
- 직관적인 인터페이스
- 변경사항 시각적 확인 (diff)
- 충돌 해결 도우미
- 드래그 앤 드롭으로 커밋

---

## 실습활동

### 실습 1: 파이썬으로 로컬 웹 서버 구동하기

**목표:** 파이썬 내장 모듈을 사용하여 간단한 웹 서버를 실행하고 웹 페이지를 제공합니다.

#### Step 1: 프로젝트 폴더 생성

```bash
# 프로젝트 폴더 생성
mkdir my-web-server
cd my-web-server
```

#### Step 2: HTML 파일 작성

`index.html` 파일 생성:

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>나의 첫 웹서버</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Malgun Gothic', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .container {
            background: white;
            padding: 40px;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            text-align: center;
            max-width: 500px;
        }
        h1 {
            color: #333;
            margin-bottom: 20px;
        }
        .status {
            background: #4CAF50;
            color: white;
            padding: 10px 20px;
            border-radius: 25px;
            display: inline-block;
            margin-bottom: 20px;
        }
        p {
            color: #666;
            line-height: 1.8;
        }
        .info {
            background: #f5f5f5;
            padding: 15px;
            border-radius: 10px;
            margin-top: 20px;
            text-align: left;
        }
        .info code {
            background: #333;
            color: #0f0;
            padding: 2px 8px;
            border-radius: 4px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="status">Server Running</div>
        <h1>웹 서버 실행 성공!</h1>
        <p>축하합니다! 파이썬으로 웹 서버를 구동했습니다.</p>
        <p>이 페이지는 로컬 웹 서버에서 제공되고 있습니다.</p>

        <div class="info">
            <p><strong>서버 정보:</strong></p>
            <p>주소: <code>http://localhost:8000</code></p>
            <p>서버: Python http.server</p>
            <p>상태: 정상 작동 중</p>
        </div>
    </div>
</body>
</html>
```

#### Step 3: 파이썬 내장 웹 서버 실행

**방법 1: 명령어로 실행 (가장 간단)**

```bash
# 프로젝트 폴더에서 실행
python -m http.server 8000
```

**방법 2: 파이썬 스크립트로 실행**

`server.py` 파일 생성:

```python
# server.py
import http.server
import socketserver

PORT = 8000

Handler = http.server.SimpleHTTPRequestHandler

with socketserver.TCPServer(("", PORT), Handler) as httpd:
    print(f"서버가 시작되었습니다!")
    print(f"브라우저에서 http://localhost:{PORT} 접속하세요")
    print("종료하려면 Ctrl+C를 누르세요")
    httpd.serve_forever()
```

실행:
```bash
python server.py
```

#### Step 4: 브라우저에서 확인

1. 웹 브라우저 열기
2. 주소창에 `http://localhost:8000` 입력
3. index.html 페이지가 표시되면 성공!

#### Step 5: 여러 페이지 추가

`about.html` 파일 추가:

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>About</title>
    <style>
        body { font-family: sans-serif; padding: 40px; }
        a { color: #667eea; }
    </style>
</head>
<body>
    <h1>About 페이지</h1>
    <p>이것은 두 번째 페이지입니다.</p>
    <a href="index.html">홈으로 돌아가기</a>
</body>
</html>
```

접속: `http://localhost:8000/about.html`

#### 추가: Flask로 더 강력한 웹 서버 만들기

```bash
# Flask 설치
pip install flask
```

`app.py` 파일 생성:

```python
from flask import Flask, render_template_string

app = Flask(__name__)

@app.route('/')
def home():
    return '''
    <h1>Flask 웹 서버</h1>
    <p>Flask로 만든 동적 웹 서버입니다!</p>
    <a href="/hello/홍길동">인사하기</a>
    '''

@app.route('/hello/<name>')
def hello(name):
    return f'<h1>안녕하세요, {name}님!</h1>'

@app.route('/api/data')
def api_data():
    return {
        'status': 'success',
        'message': 'JSON 데이터 반환',
        'data': [1, 2, 3, 4, 5]
    }

if __name__ == '__main__':
    print("Flask 서버 시작: http://localhost:5000")
    app.run(debug=True, port=5000)
```

실행:
```bash
python app.py
```

---

### 실습 2: GitHub Desktop 설치 및 코드 동기화

**목표:** GitHub Desktop을 설치하고 코드 변경 이력을 시각적으로 관리합니다.

#### Step 1: GitHub Desktop 설치

1. [GitHub Desktop 다운로드](https://desktop.github.com/) 접속
2. "Download for Windows" 클릭
3. 설치 파일 실행 및 설치 완료

#### Step 2: GitHub 계정 연결

1. GitHub Desktop 실행
2. **"Sign in to GitHub.com"** 클릭
3. 브라우저에서 GitHub 로그인
4. "Authorize GitHub Desktop" 승인
5. Configure Git:
   - Name: 본인 이름
   - Email: GitHub 이메일
6. Finish 클릭

#### Step 3: 기존 저장소 Clone (가져오기)

1. File → **Clone repository**
2. GitHub.com 탭 선택
3. 목록에서 저장소 선택 (예: `pre-course-log`)
4. Local path: 저장할 폴더 선택
5. **Clone** 클릭

#### Step 4: 새 저장소 생성

1. File → **New repository**
2. 정보 입력:
   - Name: `my-web-server`
   - Description: "웹 서버 실습 프로젝트"
   - Local path: 실습 폴더 선택
   - Initialize with README 체크
3. **Create repository** 클릭

#### Step 5: 변경사항 확인 및 커밋

**파일 수정 후 변경사항 확인:**

1. 실습 1에서 만든 파일들을 저장소 폴더에 복사
2. GitHub Desktop에서 **Changes** 탭 확인
3. 왼쪽: 변경된 파일 목록
4. 오른쪽: 변경 내용 비교 (diff)
   - 초록색: 추가된 줄
   - 빨간색: 삭제된 줄

**커밋 만들기:**

1. 하단의 **Summary** 입력 (필수)
   - 예: "feat: 웹 서버 초기 설정"
2. Description 입력 (선택)
   - 예: "index.html, about.html, server.py 추가"
3. **Commit to main** 클릭

#### Step 6: GitHub에 Push

1. 상단 **Push origin** 버튼 클릭
2. 또는 Repository → Push
3. GitHub 웹사이트에서 업로드 확인

#### Step 7: 변경 이력 확인

1. **History** 탭 클릭
2. 모든 커밋 목록 확인
3. 각 커밋 클릭하면 변경 내용 표시
4. 커밋 간 차이점 시각적으로 비교

#### Step 8: 원격 변경사항 Pull

다른 컴퓨터나 GitHub 웹에서 수정한 경우:

1. **Fetch origin** 클릭 (변경사항 확인)
2. **Pull origin** 클릭 (변경사항 가져오기)

---

### 실습 3: 코드 수정하며 이력 추적하기

**목표:** 코드를 여러 번 수정하며 Git 이력이 쌓이는 것을 확인합니다.

#### 단계별 수정 및 커밋

**1차 수정: CSS 색상 변경**

`index.html`에서 배경색 수정:
```css
background: linear-gradient(135deg, #ff6b6b 0%, #feca57 100%);
```

커밋 메시지: `style: 배경 그라데이션 색상 변경`

**2차 수정: 콘텐츠 추가**

```html
<p>현재 시간: <span id="time"></span></p>
<script>
    document.getElementById('time').textContent = new Date().toLocaleString();
</script>
```

커밋 메시지: `feat: 현재 시간 표시 기능 추가`

**3차 수정: 새 페이지 추가**

`contact.html` 생성

커밋 메시지: `feat: 연락처 페이지 추가`

#### History에서 확인

1. History 탭에서 3개의 커밋 확인
2. 각 커밋의 변경사항 비교
3. 특정 커밋의 파일 상태 확인

---

## GitHub Desktop 주요 기능 정리

| 기능 | 설명 | 위치 |
|------|------|------|
| **Clone** | 원격 저장소 복제 | File → Clone |
| **Changes** | 변경된 파일 확인 | 왼쪽 탭 |
| **History** | 커밋 이력 확인 | 왼쪽 탭 |
| **Commit** | 변경사항 저장 | 하단 버튼 |
| **Push** | GitHub에 업로드 | 상단 버튼 |
| **Pull** | GitHub에서 가져오기 | 상단 버튼 |
| **Branch** | 브랜치 생성/전환 | 상단 드롭다운 |
| **Stash** | 임시 저장 | Branch → Stash |
| **Discard** | 변경 취소 | 파일 우클릭 |

**유용한 단축키:**

| 단축키 | 기능 |
|--------|------|
| `Ctrl + Shift + N` | 새 저장소 |
| `Ctrl + Shift + O` | 저장소 추가 |
| `Ctrl + Enter` | 커밋 |
| `Ctrl + P` | Push |
| `Ctrl + Shift + P` | Pull |
| `Ctrl + B` | 브랜치 목록 |

---

## 완료 체크리스트

- [ ] 웹 서버의 역할을 이해했는가?
- [ ] 파이썬 http.server로 로컬 웹 서버를 실행했는가?
- [ ] 브라우저에서 localhost 페이지에 접속했는가?
- [ ] GitHub Desktop을 설치하고 계정을 연결했는가?
- [ ] 저장소를 Clone하거나 생성했는가?
- [ ] 파일 변경 후 Commit을 만들었는가?
- [ ] GitHub에 Push했는가?
- [ ] History에서 커밋 이력을 확인했는가?

---

## 참고 자료

- [Python http.server 문서](https://docs.python.org/3/library/http.server.html)
- [Flask 공식 문서](https://flask.palletsprojects.com/)
- [GitHub Desktop 문서](https://docs.github.com/en/desktop)
- [Git 기초 (Pro Git Book)](https://git-scm.com/book/ko/v2)
