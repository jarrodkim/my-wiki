# 11일차: 파이썬 앱 제작 및 GitHub 연동 자동 배포

---

## 핵심개념

### Streamlit 프레임워크 기초

Streamlit은 파이썬으로 웹 애플리케이션을 빠르게 만들 수 있는 프레임워크입니다. HTML, CSS, JavaScript 없이 파이썬 코드만으로 인터랙티브한 웹 앱을 만들 수 있습니다.

**Streamlit의 특징:**

| 특징 | 설명 |
|------|------|
| 간단함 | 파이썬만 알면 웹 앱 제작 가능 |
| 빠른 개발 | 코드 저장 시 자동 새로고침 |
| 인터랙티브 | 버튼, 슬라이더, 입력 필드 등 위젯 제공 |
| 데이터 시각화 | Matplotlib, Plotly 등 통합 |
| 무료 배포 | Streamlit Cloud 무료 호스팅 |

**Streamlit vs 기존 웹 개발:**

```
기존 웹 개발:
┌─────────────────────────────────────────────┐
│  HTML + CSS + JavaScript + Python(Flask)   │
│  프론트엔드 + 백엔드 분리                    │
│  많은 파일, 복잡한 구조                      │
└─────────────────────────────────────────────┘

Streamlit:
┌─────────────────────────────────────────────┐
│  Python 단일 파일 (app.py)                  │
│  UI와 로직이 하나의 코드에                   │
│  간단하고 빠른 개발                          │
└─────────────────────────────────────────────┘
```

**기본 구조:**

```python
import streamlit as st

# 제목
st.title("나의 첫 Streamlit 앱")

# 텍스트
st.write("안녕하세요!")

# 입력 위젯
name = st.text_input("이름을 입력하세요")

# 버튼
if st.button("인사하기"):
    st.write(f"안녕하세요, {name}님!")
```

**주요 컴포넌트:**

| 컴포넌트 | 설명 | 코드 |
|----------|------|------|
| 제목 | 큰 제목 | `st.title("제목")` |
| 헤더 | 중간 제목 | `st.header("헤더")` |
| 텍스트 | 일반 텍스트 | `st.write("텍스트")` |
| 마크다운 | 마크다운 렌더링 | `st.markdown("**굵게**")` |
| 코드 | 코드 블록 | `st.code("print('hi')")` |
| 입력 | 텍스트 입력 | `st.text_input("라벨")` |
| 숫자 | 숫자 입력 | `st.number_input("숫자")` |
| 슬라이더 | 범위 선택 | `st.slider("값", 0, 100)` |
| 선택 | 드롭다운 | `st.selectbox("선택", [...])` |
| 체크박스 | 체크박스 | `st.checkbox("동의")` |
| 버튼 | 클릭 버튼 | `st.button("클릭")` |
| 차트 | 라인 차트 | `st.line_chart(data)` |
| 테이블 | 데이터 테이블 | `st.dataframe(df)` |

---

### 클라우드 자동 배포 (CD) 개념

CD(Continuous Deployment/Delivery)는 코드 변경 시 자동으로 배포하는 방식입니다.

**전통적 배포 vs 자동 배포:**

```
전통적 배포:
코드 작성 → 수동 빌드 → 수동 서버 업로드 → 수동 재시작
           (시간 소요, 실수 가능)

자동 배포 (CD):
코드 작성 → Git Push → 자동 빌드 → 자동 배포 → 서비스 반영
           (빠르고 안정적)
```

**Streamlit Cloud 배포 흐름:**

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  로컬 개발   │ ──> │   GitHub    │ ──> │  Streamlit  │
│  (app.py)   │push │  저장소      │감지  │   Cloud    │
└─────────────┘     └─────────────┘     └─────────────┘
                                              │
                                              ▼
                                        ┌─────────────┐
                                        │  웹 서비스   │
                                        │ (자동 배포) │
                                        │ app.streamlit.io
                                        └─────────────┘
```

**Streamlit Cloud 특징:**
- GitHub 연동만으로 배포
- Push 시 자동 재배포
- 무료 플랜 제공 (공개 저장소)
- HTTPS 자동 적용
- 고유 URL 제공

---

## 실습활동

### 실습 1: Streamlit 설치 및 기본 앱 만들기

**목표:** Streamlit을 설치하고 로컬에서 첫 앱을 실행합니다.

#### Step 1: Streamlit 설치

```bash
# 가상환경 생성 (권장)
python -m venv venv

# 가상환경 활성화 (Windows)
venv\Scripts\activate

# 가상환경 활성화 (Mac/Linux)
source venv/bin/activate

# Streamlit 설치
pip install streamlit

# 설치 확인
streamlit --version
```

#### Step 2: 첫 번째 앱 작성

`app.py` 파일 생성:

```python
import streamlit as st

# 페이지 설정
st.set_page_config(
    page_title="My First App",
    page_icon="🚀",
    layout="centered"
)

# 제목
st.title("🚀 나의 첫 Streamlit 앱")
st.write("Streamlit으로 만든 웹 애플리케이션입니다!")

# 구분선
st.divider()

# 사용자 입력
name = st.text_input("이름을 입력하세요", placeholder="홍길동")
age = st.slider("나이를 선택하세요", 1, 100, 25)

# 버튼 클릭 시 결과 표시
if st.button("제출"):
    st.success(f"안녕하세요, {name}님! ({age}세)")
    st.balloons()  # 풍선 효과
```

#### Step 3: 앱 실행

```bash
streamlit run app.py

# 브라우저에서 자동으로 열림
# 주소: http://localhost:8501
```

---

### 실습 2: 설문조사 앱 만들기

**목표:** 다양한 입력 위젯을 사용한 설문조사 앱을 만듭니다.

`survey_app.py` 파일 생성:

```python
import streamlit as st

# 페이지 설정
st.set_page_config(page_title="설문조사", page_icon="📋")

# 제목
st.title("📋 사전교육 설문조사")
st.write("아래 설문에 응답해주세요!")

st.divider()

# 기본 정보
st.header("1. 기본 정보")

col1, col2 = st.columns(2)
with col1:
    name = st.text_input("이름")
with col2:
    email = st.text_input("이메일")

# 경험 수준
st.header("2. 프로그래밍 경험")

experience = st.radio(
    "프로그래밍 경험이 있으신가요?",
    ["전혀 없음", "조금 있음", "중급", "상급"],
    horizontal=True
)

languages = st.multiselect(
    "사용해본 프로그래밍 언어를 선택하세요",
    ["Python", "JavaScript", "Java", "C/C++", "기타"],
)

# 학습 목표
st.header("3. 학습 목표")

goals = st.text_area(
    "이 과정에서 배우고 싶은 것은?",
    placeholder="예: 웹 개발, 데이터 분석, AI/ML 등"
)

motivation = st.slider(
    "현재 학습 의지는 몇 점인가요?",
    min_value=1,
    max_value=10,
    value=7
)

# 동의
st.header("4. 동의")

agree = st.checkbox("개인정보 수집 및 이용에 동의합니다")

# 제출 버튼
st.divider()

if st.button("설문 제출", type="primary", disabled=not agree):
    if name and email:
        st.success("설문이 제출되었습니다! 감사합니다 🎉")

        # 결과 요약
        st.subheader("📊 응답 요약")
        st.json({
            "이름": name,
            "이메일": email,
            "경험": experience,
            "언어": languages,
            "목표": goals,
            "의지": f"{motivation}/10"
        })
    else:
        st.error("이름과 이메일을 입력해주세요!")
```

실행:
```bash
streamlit run survey_app.py
```

---

### 실습 3: 데이터 그래프 앱 만들기

**목표:** 데이터를 시각화하는 대시보드 앱을 만듭니다.

`chart_app.py` 파일 생성:

```python
import streamlit as st
import pandas as pd
import numpy as np

# 페이지 설정
st.set_page_config(page_title="데이터 대시보드", page_icon="📊", layout="wide")

# 제목
st.title("📊 데이터 시각화 대시보드")

# 사이드바
st.sidebar.header("설정")
chart_type = st.sidebar.selectbox(
    "차트 유형",
    ["라인 차트", "바 차트", "영역 차트"]
)
data_points = st.sidebar.slider("데이터 포인트 수", 10, 100, 30)

# 샘플 데이터 생성
@st.cache_data
def generate_data(n):
    dates = pd.date_range(start="2024-01-01", periods=n, freq="D")
    data = pd.DataFrame({
        "날짜": dates,
        "매출": np.random.randint(100, 500, n),
        "방문자": np.random.randint(50, 200, n),
        "전환율": np.random.uniform(1, 10, n)
    })
    return data

df = generate_data(data_points)

# 메트릭 표시
st.header("📈 주요 지표")
col1, col2, col3, col4 = st.columns(4)

with col1:
    st.metric("총 매출", f"₩{df['매출'].sum():,}", "+12%")
with col2:
    st.metric("평균 방문자", f"{df['방문자'].mean():.0f}명", "+5%")
with col3:
    st.metric("평균 전환율", f"{df['전환율'].mean():.1f}%", "-2%")
with col4:
    st.metric("데이터 기간", f"{data_points}일")

st.divider()

# 차트 표시
st.header("📉 데이터 추이")

chart_data = df.set_index("날짜")[["매출", "방문자"]]

if chart_type == "라인 차트":
    st.line_chart(chart_data)
elif chart_type == "바 차트":
    st.bar_chart(chart_data)
else:
    st.area_chart(chart_data)

# 데이터 테이블
st.header("📋 원본 데이터")

show_data = st.checkbox("데이터 테이블 보기")
if show_data:
    st.dataframe(df, use_container_width=True)

    # 다운로드 버튼
    csv = df.to_csv(index=False).encode('utf-8-sig')
    st.download_button(
        label="📥 CSV 다운로드",
        data=csv,
        file_name="data.csv",
        mime="text/csv"
    )
```

실행:
```bash
streamlit run chart_app.py
```

---

### 실습 4: Streamlit Cloud 배포

**목표:** GitHub에 올려서 Streamlit Cloud로 자동 배포합니다.

#### Step 1: 프로젝트 구조 만들기

```
my-streamlit-app/
├── app.py              # 메인 앱
├── requirements.txt    # 의존성 목록
└── README.md           # 프로젝트 설명
```

#### Step 2: requirements.txt 작성

```
streamlit
pandas
numpy
```

#### Step 3: GitHub 저장소 생성 및 푸시

```bash
# 프로젝트 폴더에서
git init
git add .
git commit -m "Initial commit: Streamlit app"

# GitHub에서 저장소 생성 후
git remote add origin https://github.com/username/my-streamlit-app.git
git branch -M main
git push -u origin main
```

#### Step 4: Streamlit Cloud 연결

1. [Streamlit Cloud](https://streamlit.io/cloud) 접속
2. **Sign in with GitHub** 클릭
3. GitHub 계정 연동 승인
4. **New app** 클릭
5. 설정 입력:
   - Repository: `username/my-streamlit-app`
   - Branch: `main`
   - Main file path: `app.py`
6. **Deploy!** 클릭

#### Step 5: 배포 확인

- 배포 완료 시 URL 제공: `https://username-app-name.streamlit.app`
- 이후 GitHub에 Push하면 자동으로 재배포됨

#### Step 6: 코드 수정 및 자동 배포 테스트

```bash
# app.py 수정 후
git add app.py
git commit -m "feat: 새 기능 추가"
git push

# Streamlit Cloud에서 자동으로 재배포됨!
```

---

## Streamlit 주요 기능 정리

| 기능 | 코드 |
|------|------|
| 텍스트 입력 | `st.text_input("라벨")` |
| 숫자 입력 | `st.number_input("라벨")` |
| 텍스트 영역 | `st.text_area("라벨")` |
| 슬라이더 | `st.slider("라벨", min, max)` |
| 선택박스 | `st.selectbox("라벨", options)` |
| 다중선택 | `st.multiselect("라벨", options)` |
| 라디오 | `st.radio("라벨", options)` |
| 체크박스 | `st.checkbox("라벨")` |
| 버튼 | `st.button("라벨")` |
| 파일 업로드 | `st.file_uploader("라벨")` |
| 날짜 선택 | `st.date_input("라벨")` |
| 컬럼 레이아웃 | `st.columns(n)` |
| 사이드바 | `st.sidebar.xxx` |
| 성공 메시지 | `st.success("메시지")` |
| 에러 메시지 | `st.error("메시지")` |
| 경고 메시지 | `st.warning("메시지")` |

---

## 완료 체크리스트

- [ ] Streamlit을 설치했는가?
- [ ] 로컬에서 Streamlit 앱을 실행했는가?
- [ ] 다양한 입력 위젯을 사용해보았는가?
- [ ] 차트/그래프를 표시하는 앱을 만들었는가?
- [ ] GitHub 저장소에 코드를 푸시했는가?
- [ ] Streamlit Cloud에 배포했는가?
- [ ] 코드 수정 후 자동 배포를 확인했는가?

---

## 참고 자료

- [Streamlit 공식 문서](https://docs.streamlit.io/)
- [Streamlit 갤러리](https://streamlit.io/gallery)
- [Streamlit Cloud 가이드](https://docs.streamlit.io/streamlit-community-cloud)
- [Streamlit Cheat Sheet](https://docs.streamlit.io/library/cheatsheet)
