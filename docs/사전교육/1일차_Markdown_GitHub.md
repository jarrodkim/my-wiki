# 01일차: Markdown 및 첫 GitHub 저장소 생성

---

## 핵심개념

### 구조화된 문서 작성법 (Markdown)

Markdown은 텍스트 기반의 경량 마크업 언어입니다. 복잡한 HTML 태그 없이 간단한 기호만으로 문서의 구조와 서식을 표현할 수 있습니다.

**Markdown의 장점:**
- 배우기 쉽고 직관적인 문법
- 다양한 플랫폼에서 지원 (GitHub, Notion, Obsidian 등)
- 일반 텍스트로 저장되어 버전 관리에 용이
- HTML로 쉽게 변환 가능

**주요 문법:**

| 문법 | 설명 | 예시 |
|------|------|------|
| `#` | 제목 (h1~h6) | `# 제목1`, `## 제목2` |
| `**텍스트**` | 굵게 | **굵은 글씨** |
| `*텍스트*` | 기울임 | *기울임 글씨* |
| `-` 또는 `*` | 순서 없는 목록 | - 항목1 |
| `1.` | 순서 있는 목록 | 1. 첫번째 |
| `[텍스트](URL)` | 링크 | [GitHub](https://github.com) |
| `![대체텍스트](이미지URL)` | 이미지 | 이미지 삽입 |
| `` `코드` `` | 인라인 코드 | `console.log()` |
| ` ``` ` | 코드 블록 | 여러 줄 코드 |

---

### 버전 관리의 필요성

버전 관리(Version Control)는 파일의 변경 이력을 체계적으로 관리하는 시스템입니다.

**버전 관리가 필요한 이유:**
- 코드 변경 이력 추적 가능
- 이전 버전으로 되돌리기 용이
- 여러 사람과 협업 시 충돌 방지
- 백업 및 복구 기능
- 브랜치를 통한 병렬 개발 가능

**버전 관리 없이 작업할 때의 문제점:**
- `최종.txt`, `최종_진짜최종.txt`, `최종_수정본_v2.txt` 같은 파일 난립
- 누가 언제 무엇을 수정했는지 파악 어려움
- 실수로 삭제한 코드 복구 불가

---

### GitHub의 개념

GitHub는 Git을 기반으로 한 웹 호스팅 서비스입니다. 코드 저장, 공유, 협업을 위한 플랫폼입니다.

**Git vs GitHub:**
- **Git**: 로컬에서 동작하는 버전 관리 시스템 (도구)
- **GitHub**: Git 저장소를 온라인에 호스팅하는 서비스 (플랫폼)

**GitHub 주요 기능:**
- Repository (저장소): 프로젝트 파일과 변경 이력 저장
- README.md: 프로젝트 소개 문서
- Issues: 버그 리포트, 기능 요청 관리
- Pull Request: 코드 리뷰 및 병합 요청
- Fork: 다른 사람의 저장소 복제

---

## 실습활동

### 실습 1: 마크다운 문법을 활용한 본인 소개 페이지 작성

**목표:** Markdown 문법을 익히고 자기소개 페이지를 작성합니다.

**실습 단계:**

1. 텍스트 에디터를 엽니다 (메모장, VS Code 등)
2. 새 파일을 만들고 `자기소개.md`로 저장합니다
3. 아래 템플릿을 참고하여 본인만의 소개 페이지를 작성합니다

**예시 템플릿:**

```markdown
# 안녕하세요! 저는 [이름]입니다

## 자기소개

저는 **[직업/학생]**이며, 현재 [하고 있는 일]을 하고 있습니다.

## 관심 분야

- 웹 개발
- 데이터 분석
- 인공지능

## 배우고 싶은 것

1. Python 프로그래밍
2. Git & GitHub 활용
3. 클라우드 서비스

## 연락처

- Email: example@email.com
- GitHub: [내 GitHub](https://github.com/username)

---

> "좋아하는 명언이나 한마디를 적어보세요"
```

**체크리스트:**
- [ ] 제목(`#`)을 사용했는가?
- [ ] 굵은 글씨(`**`)를 사용했는가?
- [ ] 목록(`-` 또는 `1.`)을 사용했는가?
- [ ] 링크(`[텍스트](URL)`)를 사용했는가?
- [ ] 인용문(`>`)을 사용했는가?

---

### 실습 2: GitHub 계정 생성 및 첫 저장소 만들기

**목표:** GitHub 계정을 만들고 첫 번째 저장소에 README.md를 업로드합니다.

**실습 단계:**

#### Step 1: GitHub 계정 생성

1. [github.com](https://github.com)에 접속합니다
2. 우측 상단 **Sign up** 클릭
3. 이메일, 비밀번호, 사용자명 입력
4. 이메일 인증 완료

#### Step 2: 새 저장소(Repository) 생성

1. 로그인 후 우측 상단 `+` 버튼 클릭
2. **New repository** 선택
3. 저장소 정보 입력:
   - Repository name: `pre-course-log`
   - Description: "사전 학습 기록용 저장소" (선택사항)
   - Public 선택 (공개 저장소)
   - **Add a README file** 체크
4. **Create repository** 버튼 클릭

#### Step 3: README.md 수정하기

1. 생성된 저장소에서 `README.md` 파일 클릭
2. 연필 아이콘(Edit) 클릭
3. 실습 1에서 작성한 자기소개 내용 붙여넣기
4. 하단 **Commit changes** 버튼 클릭
5. Commit message 입력 (예: "첫 번째 README 작성")
6. **Commit changes** 확인

**완료 확인:**
- [ ] GitHub 계정이 생성되었는가?
- [ ] `pre-course-log` 저장소가 만들어졌는가?
- [ ] README.md에 자기소개 내용이 표시되는가?

---

## 참고 자료

- [Markdown 공식 가이드](https://www.markdownguide.org/)
- [GitHub Docs - Hello World](https://docs.github.com/en/get-started/quickstart/hello-world)
- [Git 간편 안내서](https://rogerdudler.github.io/git-guide/index.ko.html)
