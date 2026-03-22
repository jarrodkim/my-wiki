# 09일차: 리눅스 핵심 명령어 및 터미널 환경 Git 숙달

---

## 핵심개념

### CLI (Command Line Interface) 환경

CLI는 텍스트 명령어로 컴퓨터를 조작하는 인터페이스입니다. GUI(그래픽 인터페이스)와 달리 마우스 없이 키보드만으로 모든 작업을 수행합니다.

**CLI vs GUI:**

| 구분 | CLI | GUI |
|------|-----|-----|
| 조작 방식 | 키보드 (명령어 입력) | 마우스 (클릭, 드래그) |
| 학습 곡선 | 높음 (명령어 암기 필요) | 낮음 (직관적) |
| 작업 속도 | 빠름 (숙련 시) | 보통 |
| 자동화 | 용이 (스크립트) | 어려움 |
| 원격 접속 | 가벼움 (SSH) | 무거움 (RDP, VNC) |
| 서버 환경 | 필수 | 거의 사용 안 함 |

**CLI를 배워야 하는 이유:**
- 대부분의 서버는 CLI만 지원
- 자동화 스크립트 작성 가능
- 반복 작업을 빠르게 처리
- 원격 서버 관리 필수
- DevOps, 클라우드 환경의 기본

**터미널 프롬프트 이해:**

```bash
username@hostname:~/projects$
   │        │       │      │
   │        │       │      └── 명령 입력 대기
   │        │       └── 현재 디렉토리 (~ = 홈)
   │        └── 컴퓨터 이름
   └── 사용자 이름

# root 사용자인 경우
root@hostname:/etc#
                 │
                 └── # = root(관리자) 권한
```

---

### 리눅스 파일 시스템 구조

리눅스는 모든 것을 파일로 관리하며, 단일 트리 구조를 가집니다.

**디렉토리 구조:**

```
/                    ← 루트(최상위) 디렉토리
├── bin/             ← 기본 명령어 (ls, cp, mv 등)
├── boot/            ← 부팅 관련 파일
├── dev/             ← 장치 파일 (디스크, USB 등)
├── etc/             ← 시스템 설정 파일
├── home/            ← 사용자 홈 디렉토리
│   ├── user1/
│   └── user2/
├── opt/             ← 추가 소프트웨어
├── root/            ← root 사용자 홈
├── tmp/             ← 임시 파일
├── usr/             ← 사용자 프로그램
│   ├── bin/
│   └── local/
└── var/             ← 가변 데이터 (로그 등)
    └── log/
```

**경로 표현:**

| 표현 | 의미 | 예시 |
|------|------|------|
| `/` | 루트 디렉토리 | `/home/user` |
| `~` | 홈 디렉토리 | `~/projects` = `/home/user/projects` |
| `.` | 현재 디렉토리 | `./script.sh` |
| `..` | 상위 디렉토리 | `cd ..` |
| `-` | 이전 디렉토리 | `cd -` |

---

### 리눅스 파일 시스템 권한

리눅스는 파일마다 접근 권한을 세밀하게 관리합니다.

**권한 확인:**

```bash
$ ls -l
-rw-r--r-- 1 user group 1024 Jan 15 10:00 file.txt
│├──┤├──┤├──┤
│ │   │   │
│ │   │   └── 기타 사용자 권한 (r--)
│ │   └── 그룹 권한 (r--)
│ └── 소유자 권한 (rw-)
└── 파일 타입 (- = 파일, d = 디렉토리)
```

**권한 종류:**

| 문자 | 숫자 | 파일 | 디렉토리 |
|------|------|------|----------|
| `r` | 4 | 읽기 | 목록 보기 |
| `w` | 2 | 쓰기/수정 | 파일 생성/삭제 |
| `x` | 1 | 실행 | 디렉토리 진입 |
| `-` | 0 | 권한 없음 | 권한 없음 |

**권한 숫자 계산:**

```
rwx = 4 + 2 + 1 = 7
rw- = 4 + 2 + 0 = 6
r-x = 4 + 0 + 1 = 5
r-- = 4 + 0 + 0 = 4

예: 755 = rwxr-xr-x (소유자: 모든 권한, 그룹/기타: 읽기+실행)
예: 644 = rw-r--r-- (소유자: 읽기+쓰기, 그룹/기타: 읽기만)
```

**권한 변경:**

```bash
# 숫자 방식
chmod 755 script.sh    # rwxr-xr-x
chmod 644 file.txt     # rw-r--r--

# 문자 방식
chmod +x script.sh     # 실행 권한 추가
chmod u+w file.txt     # 소유자에게 쓰기 권한 추가
chmod go-w file.txt    # 그룹/기타의 쓰기 권한 제거

# 소유자 변경
chown user:group file.txt
```

---

### 터미널용 Git 명령어

GUI 없이 터미널에서 Git을 사용하는 방법입니다.

**Git 워크플로우:**

```
[Working Directory] --add--> [Staging Area] --commit--> [Local Repo] --push--> [Remote]
      작업 공간                 스테이징              로컬 저장소            원격 저장소
```

**필수 Git 명령어:**

| 명령어 | 설명 |
|--------|------|
| `git init` | 새 저장소 초기화 |
| `git clone <url>` | 원격 저장소 복제 |
| `git status` | 현재 상태 확인 |
| `git add <file>` | 스테이징에 추가 |
| `git add .` | 모든 변경사항 추가 |
| `git commit -m "msg"` | 커밋 생성 |
| `git push` | 원격에 업로드 |
| `git pull` | 원격에서 가져오기 |
| `git log` | 커밋 이력 확인 |
| `git diff` | 변경사항 비교 |

**Git 상태 확인:**

```bash
$ git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
        modified:   README.md      # 빨간색: 수정됨, 스테이징 안 됨

Untracked files:
        newfile.txt               # 빨간색: 추적 안 되는 새 파일

Changes to be committed:
        new file:   staged.txt    # 초록색: 스테이징됨, 커밋 대기
```

---

## 실습활동

### 실습 1: 리눅스 필수 명령어 연습

**목표:** 가상머신 터미널에서 기본 명령어를 익힙니다.

#### 터미널 열기

- Ubuntu: `Ctrl + Alt + T`
- 또는 Activities → Terminal 검색

#### 디렉토리 탐색 명령어

```bash
# 현재 위치 확인
pwd
# 출력: /home/username

# 디렉토리 내용 보기
ls              # 기본 목록
ls -l           # 상세 정보 (권한, 크기, 날짜)
ls -la          # 숨김 파일 포함
ls -lh          # 파일 크기를 읽기 쉽게 (KB, MB)

# 디렉토리 이동
cd /home        # 절대 경로로 이동
cd ~            # 홈 디렉토리로
cd ..           # 상위 디렉토리로
cd -            # 이전 디렉토리로
cd projects     # 하위 디렉토리로

# 디렉토리 생성
mkdir my-project              # 단일 디렉토리
mkdir -p parent/child/grand   # 중첩 디렉토리 한번에 생성

# 디렉토리 삭제
rmdir empty-folder            # 빈 디렉토리만 삭제
rm -r folder-with-files       # 내용물 포함 삭제 (주의!)
```

#### 파일 조작 명령어

```bash
# 파일 생성
touch newfile.txt             # 빈 파일 생성
echo "Hello" > hello.txt      # 내용 포함 파일 생성
echo "World" >> hello.txt     # 기존 파일에 추가

# 파일 내용 보기
cat file.txt                  # 전체 내용
head -n 10 file.txt           # 처음 10줄
tail -n 10 file.txt           # 마지막 10줄
less file.txt                 # 페이지 단위 (q로 종료)

# 파일 복사/이동/삭제
cp source.txt dest.txt        # 복사
cp -r folder1 folder2         # 디렉토리 복사
mv old.txt new.txt            # 이름 변경
mv file.txt /path/to/         # 이동
rm file.txt                   # 삭제
rm -rf folder                 # 디렉토리 강제 삭제 (주의!)

# 파일 검색
find . -name "*.txt"          # 현재 폴더에서 txt 파일 찾기
find /home -name "config"     # /home에서 config 찾기
grep "keyword" file.txt       # 파일 내 키워드 검색
grep -r "keyword" .           # 현재 폴더 전체에서 검색
```

#### 시스템 정보 명령어

```bash
# 시스템 정보
uname -a                      # 커널 정보
hostname                      # 호스트 이름
whoami                        # 현재 사용자
date                          # 현재 날짜/시간

# 디스크 사용량
df -h                         # 디스크 용량
du -sh *                      # 현재 폴더 내 크기

# 프로세스
ps aux                        # 실행 중인 프로세스
top                           # 실시간 모니터링 (q로 종료)
htop                          # 향상된 top (설치 필요)

# 네트워크
ip addr                       # IP 주소 확인
ping google.com               # 네트워크 연결 테스트 (Ctrl+C로 중단)
curl https://api.github.com   # HTTP 요청
```

#### 유용한 단축키

| 단축키 | 기능 |
|--------|------|
| `Tab` | 자동 완성 |
| `Ctrl + C` | 실행 중인 명령 중단 |
| `Ctrl + L` | 화면 지우기 (clear) |
| `Ctrl + A` | 줄 맨 앞으로 |
| `Ctrl + E` | 줄 맨 뒤로 |
| `Ctrl + R` | 명령어 검색 |
| `↑` / `↓` | 이전/다음 명령어 |
| `!!` | 마지막 명령 재실행 |
| `sudo !!` | 마지막 명령을 sudo로 재실행 |

---

### 실습 2: 터미널에서 Git 작업 완료하기

**목표:** 마우스 없이 터미널 Git 명령어만으로 전체 워크플로우를 수행합니다.

#### Step 1: Git 설정 확인

```bash
# Git 설치 확인
git --version

# 사용자 정보 설정 (최초 1회)
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# 설정 확인
git config --list
```

#### Step 2: 프로젝트 생성 및 초기화

```bash
# 프로젝트 폴더 생성
mkdir ~/git-practice
cd ~/git-practice

# Git 저장소 초기화
git init

# 확인 (.git 폴더 생성됨)
ls -la
```

#### Step 3: 파일 생성 및 첫 커밋

```bash
# README 파일 생성
echo "# Git Practice" > README.md
echo "터미널에서 Git 연습하기" >> README.md

# 상태 확인
git status
# Untracked files: README.md (빨간색)

# 스테이징
git add README.md

# 상태 다시 확인
git status
# Changes to be committed: README.md (초록색)

# 커밋
git commit -m "docs: README 파일 생성"

# 커밋 로그 확인
git log --oneline
```

#### Step 4: 파일 수정 및 추가 커밋

```bash
# 새 파일 생성
echo '#!/bin/bash' > hello.sh
echo 'echo "Hello, Git!"' >> hello.sh

# 실행 권한 추가
chmod +x hello.sh

# 스크립트 실행 테스트
./hello.sh

# 변경사항 확인
git status

# 모든 변경사항 스테이징
git add .

# 또는 특정 파일만
git add hello.sh

# 커밋
git commit -m "feat: hello.sh 스크립트 추가"

# 로그 확인
git log --oneline
```

#### Step 5: GitHub 원격 저장소 연결

```bash
# GitHub에서 새 저장소 생성 후 URL 복사

# 원격 저장소 추가
git remote add origin https://github.com/username/git-practice.git

# 원격 저장소 확인
git remote -v

# 푸시 (최초)
git push -u origin main
# 또는 master인 경우
git push -u origin master

# Username과 Password(Token) 입력
```

#### Step 6: 추가 수정 및 푸시

```bash
# 파일 수정
echo "" >> README.md
echo "## 학습 내용" >> README.md
echo "- 리눅스 명령어" >> README.md
echo "- Git 터미널 사용법" >> README.md

# 변경사항 확인
git diff

# 스테이징 + 커밋 한번에 (추적 중인 파일만)
git commit -am "docs: README 학습 내용 추가"

# 푸시
git push
```

---

### 실습 3: Git 이력 관리 실습

**목표:** 커밋 이력 확인, 변경사항 비교, 되돌리기를 연습합니다.

#### 커밋 이력 확인

```bash
# 기본 로그
git log

# 한 줄씩 보기
git log --oneline

# 그래프로 보기
git log --oneline --graph

# 최근 5개만
git log --oneline -5

# 특정 파일의 이력
git log --oneline README.md

# 상세 변경 내용 포함
git log -p

# 통계 정보
git log --stat
```

#### 변경사항 비교

```bash
# 작업 디렉토리 vs 스테이징
git diff

# 스테이징 vs 최신 커밋
git diff --staged

# 특정 커밋 간 비교
git diff abc123 def456

# 특정 파일만
git diff README.md
```

#### 변경 되돌리기

```bash
# 작업 디렉토리 변경 취소 (스테이징 전)
git checkout -- file.txt
# 또는 (Git 2.23+)
git restore file.txt

# 스테이징 취소 (변경 내용은 유지)
git reset HEAD file.txt
# 또는
git restore --staged file.txt

# 마지막 커밋 메시지 수정
git commit --amend -m "새로운 메시지"

# 특정 커밋으로 되돌리기 (새 커밋 생성)
git revert abc123
```

---

### 실습 4: 종합 실습 - 프로젝트 완성하기

**목표:** 처음부터 끝까지 터미널만으로 프로젝트를 만들고 GitHub에 업로드합니다.

````bash
# 1. 프로젝트 생성
mkdir ~/my-cli-project
cd ~/my-cli-project
git init

# 2. .gitignore 생성
cat << 'EOF' > .gitignore
# Python
__pycache__/
*.pyc
.env

# OS
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/
EOF

# 3. README 작성
cat << 'EOF' > README.md
# My CLI Project

터미널 Git 실습 프로젝트입니다.

## 설치 방법

```bash
git clone https://github.com/username/my-cli-project.git
cd my-cli-project
```

## 사용 방법

```bash
./run.sh
```
EOF

# 4. 스크립트 작성
cat << 'EOF' > run.sh
#!/bin/bash
echo "============================"
echo "  My CLI Project v1.0"
echo "============================"
echo ""
echo "현재 시간: $(date)"
echo "사용자: $(whoami)"
echo "디렉토리: $(pwd)"
EOF

chmod +x run.sh

# 5. 첫 커밋
git add .
git status
git commit -m "Initial commit: 프로젝트 초기 설정"

# 6. GitHub 연결 및 푸시
git remote add origin https://github.com/username/my-cli-project.git
git branch -M main
git push -u origin main

# 7. 확인
git log --oneline
echo "완료! GitHub에서 확인하세요."
````

---

## 명령어 치트시트

### 리눅스 필수 명령어

| 명령어 | 설명 | 예시 |
|--------|------|------|
| `pwd` | 현재 경로 | `pwd` |
| `ls` | 목록 보기 | `ls -la` |
| `cd` | 디렉토리 이동 | `cd ~/projects` |
| `mkdir` | 디렉토리 생성 | `mkdir -p a/b/c` |
| `touch` | 파일 생성 | `touch file.txt` |
| `cat` | 파일 내용 출력 | `cat file.txt` |
| `cp` | 복사 | `cp -r src dest` |
| `mv` | 이동/이름변경 | `mv old new` |
| `rm` | 삭제 | `rm -rf folder` |
| `chmod` | 권한 변경 | `chmod +x script.sh` |
| `grep` | 검색 | `grep "text" file` |
| `find` | 파일 찾기 | `find . -name "*.txt"` |

### Git 필수 명령어

| 명령어 | 설명 |
|--------|------|
| `git init` | 저장소 초기화 |
| `git clone <url>` | 저장소 복제 |
| `git status` | 상태 확인 |
| `git add <file>` | 스테이징 |
| `git commit -m "msg"` | 커밋 |
| `git push` | 원격에 업로드 |
| `git pull` | 원격에서 가져오기 |
| `git log --oneline` | 이력 확인 |
| `git diff` | 변경사항 비교 |
| `git checkout -b <branch>` | 브랜치 생성 및 이동 |

---

## 완료 체크리스트

- [ ] 터미널에서 `pwd`, `ls`, `cd` 명령어를 사용할 수 있는가?
- [ ] `mkdir`, `touch`, `rm` 등 파일 조작 명령어를 사용할 수 있는가?
- [ ] 파일 권한(`chmod`)의 개념을 이해했는가?
- [ ] `git init`으로 저장소를 초기화할 수 있는가?
- [ ] `git add`, `git commit`, `git push` 워크플로우를 수행할 수 있는가?
- [ ] `git log`, `git diff`로 이력과 변경사항을 확인할 수 있는가?
- [ ] 마우스 없이 전체 Git 작업을 완료했는가?

---

## 참고 자료

- [Linux Command 치트시트](https://www.guru99.com/linux-commands-cheat-sheet.html)
- [Git 공식 문서](https://git-scm.com/doc)
- [Pro Git Book (한글)](https://git-scm.com/book/ko/v2)
- [ExplainShell](https://explainshell.com/) - 명령어 설명 사이트
- [Linux Journey](https://linuxjourney.com/) - 리눅스 학습 사이트
