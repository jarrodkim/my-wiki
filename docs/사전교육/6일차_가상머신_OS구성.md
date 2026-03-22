# 06일차: 가상머신(VM) 설치 및 OS 구성

---

## 핵심개념

### 가상화 (Virtualization)의 원리

가상화는 하나의 물리적 컴퓨터에서 여러 개의 가상 컴퓨터를 실행하는 기술입니다. 각 가상 컴퓨터는 독립적인 운영체제와 애플리케이션을 실행할 수 있습니다.

**가상화의 구조:**

```
┌─────────────────────────────────────────────────┐
│              가상 머신 (Guest OS)                │
│  ┌───────────┐  ┌───────────┐  ┌───────────┐   │
│  │  Ubuntu   │  │  Windows  │  │   Rocky   │   │
│  │   Linux   │  │    11     │  │   Linux   │   │
│  └───────────┘  └───────────┘  └───────────┘   │
├─────────────────────────────────────────────────┤
│              하이퍼바이저 (Hypervisor)           │
│           (VMware, VirtualBox 등)               │
├─────────────────────────────────────────────────┤
│              호스트 OS (Host OS)                 │
│              (Windows, macOS 등)                │
├─────────────────────────────────────────────────┤
│              물리적 하드웨어                     │
│         (CPU, RAM, 디스크, 네트워크)             │
└─────────────────────────────────────────────────┘
```

**가상화의 장점:**

| 장점 | 설명 |
|------|------|
| 비용 절감 | 하나의 서버에서 여러 OS 실행 |
| 격리성 | VM 간 독립적으로 운영, 보안 강화 |
| 유연성 | 필요에 따라 VM 생성/삭제 용이 |
| 테스트 환경 | 안전하게 새로운 소프트웨어 테스트 |
| 스냅샷 | 특정 시점으로 즉시 복원 가능 |
| 이식성 | VM 파일을 다른 컴퓨터로 이동 가능 |

**가상화 vs 실제 설치:**

| 구분 | 가상화 | 실제 설치 (듀얼부팅) |
|------|--------|---------------------|
| 리소스 | 호스트와 공유 | 전체 리소스 사용 |
| 성능 | 약간의 오버헤드 | 최대 성능 |
| 편의성 | 동시에 여러 OS 실행 | 재부팅 필요 |
| 안전성 | 호스트에 영향 없음 | 파티션 관리 필요 |
| 학습용 | 매우 적합 | 복잡함 |

---

### 하이퍼바이저 (Hypervisor)

하이퍼바이저는 가상 머신을 생성하고 관리하는 소프트웨어입니다. 물리적 하드웨어와 가상 머신 사이에서 리소스를 분배합니다.

**하이퍼바이저 종류:**

| 유형 | 설명 | 예시 |
|------|------|------|
| **Type 1 (베어메탈)** | 하드웨어 위에 직접 설치 | VMware ESXi, Hyper-V Server, Xen |
| **Type 2 (호스트형)** | 호스트 OS 위에 설치 | VMware Workstation, VirtualBox, Parallels |

**Type 1 vs Type 2:**

```
Type 1 (베어메탈)              Type 2 (호스트형)
┌─────────────────┐           ┌─────────────────┐
│    Guest OS     │           │    Guest OS     │
├─────────────────┤           ├─────────────────┤
│   Hypervisor    │           │   Hypervisor    │
├─────────────────┤           ├─────────────────┤
│    Hardware     │           │    Host OS      │
└─────────────────┘           ├─────────────────┤
                              │    Hardware     │
                              └─────────────────┘

- 서버/데이터센터용              - 개인 PC/개발용
- 높은 성능                     - 설치 간편
- 전문 관리 필요                - 학습용으로 적합
```

**주요 가상화 소프트웨어:**

| 소프트웨어 | 유형 | 특징 | 가격 |
|-----------|------|------|------|
| VMware Workstation Pro | Type 2 | 강력한 기능, 기업용 | 유료 |
| VMware Workstation Player | Type 2 | 개인용 무료 버전 | 무료/유료 |
| Oracle VirtualBox | Type 2 | 오픈소스, 크로스플랫폼 | 무료 |
| Hyper-V | Type 1/2 | Windows 내장 | Windows Pro 이상 |
| Parallels | Type 2 | Mac 전용, 최적화 | 유료 |

---

### 운영체제 이미지 (ISO)

ISO 파일은 CD/DVD 전체 내용을 하나의 파일로 만든 디스크 이미지입니다. 운영체제 설치에 사용됩니다.

**ISO 파일이란:**
- 국제 표준화 기구(ISO 9660)에서 정한 파일 형식
- 확장자: `.iso`
- 부팅 가능한 설치 미디어를 파일로 저장
- USB나 가상 드라이브로 마운트하여 사용

**주요 리눅스 배포판:**

| 배포판 | 특징 | 용도 |
|--------|------|------|
| **Ubuntu** | 사용자 친화적, 큰 커뮤니티 | 데스크탑, 서버, 입문용 |
| **Rocky Linux** | RHEL 호환, 엔터프라이즈급 | 서버, 기업 환경 |
| **CentOS Stream** | RHEL 업스트림 | 개발/테스트 |
| **Debian** | 안정성, 자유 소프트웨어 | 서버 |
| **Fedora** | 최신 기술, Red Hat 지원 | 개발자 |

**Ubuntu vs Rocky Linux:**

| 구분 | Ubuntu | Rocky Linux |
|------|--------|-------------|
| 기반 | Debian | RHEL (Red Hat) |
| 패키지 관리 | APT (apt, dpkg) | DNF/YUM (rpm) |
| 릴리즈 주기 | 6개월 (LTS: 2년) | ~2년 |
| 지원 기간 | LTS: 5년 | 10년 |
| 데스크탑 | GNOME (기본) | GNOME (선택) |
| 기업 사용 | 클라우드, IoT | 서버, 엔터프라이즈 |

**ISO 다운로드 위치:**
- Ubuntu: [https://ubuntu.com/download](https://ubuntu.com/download)
- Rocky Linux: [https://rockylinux.org/download](https://rockylinux.org/download)

---

## 실습활동

### 실습 1: VMware Workstation Player 설치

**목표:** VMware Workstation Player를 설치하여 가상화 환경을 준비합니다.

#### Step 1: 시스템 요구사항 확인

| 항목 | 최소 요구사항 | 권장 사양 |
|------|--------------|----------|
| CPU | 64비트, VT-x/AMD-V 지원 | 멀티코어 |
| RAM | 4GB | 8GB 이상 |
| 디스크 | 20GB 여유 공간 | SSD 권장 |
| OS | Windows 10/11 64비트 | 최신 버전 |

#### Step 2: BIOS에서 가상화 기능 활성화

1. 컴퓨터 재시작 → BIOS 진입 (F2, Del, F10 등)
2. Advanced 또는 CPU 설정 메뉴 찾기
3. **Intel VT-x** 또는 **AMD-V** 활성화 (Enable)
4. 저장 후 재부팅

**확인 방법 (Windows):**
```
작업 관리자 → 성능 탭 → CPU → "가상화: 사용"
```

#### Step 3: VMware Workstation Player 다운로드

1. [VMware 다운로드 페이지](https://www.vmware.com/products/workstation-player.html) 접속
2. "Download for Free" 클릭
3. Windows용 설치 파일 다운로드 (약 500MB)

#### Step 4: 설치 진행

1. 다운로드한 설치 파일 실행
2. **사용자 계정 컨트롤** → "예" 클릭
3. 설치 마법사 진행:
   - Next 클릭
   - 라이선스 동의 체크 → Next
   - 설치 경로 확인 → Next
   - "Enhanced Keyboard Driver" 체크 권장
   - "Check for product updates" 체크
   - Next → Install
4. 설치 완료 → Finish

#### Step 5: 첫 실행 및 설정

1. VMware Workstation Player 실행
2. "Use VMware Workstation Player for free for non-commercial use" 선택
3. Continue → Finish

---

### 실습 2: Ubuntu Linux 설치

**목표:** 가상 머신에 Ubuntu를 설치하고 기본 데스크탑 환경을 확인합니다.

#### Step 1: Ubuntu ISO 다운로드

1. [Ubuntu 다운로드 페이지](https://ubuntu.com/download/desktop) 접속
2. **Ubuntu 22.04 LTS** 또는 최신 LTS 버전 선택
3. Download 클릭 (약 4.5GB)

#### Step 2: 새 가상 머신 생성

1. VMware Player 실행
2. **"Create a New Virtual Machine"** 클릭
3. 설치 방법 선택:
   - **"Installer disc image file (iso)"** 선택
   - Browse → 다운로드한 Ubuntu ISO 파일 선택
   - Next

4. 사용자 정보 입력 (Easy Install):
   - Full name: 본인 이름
   - User name: 로그인 ID (영문, 소문자)
   - Password: 비밀번호 설정
   - Next

5. 가상 머신 이름 및 위치:
   - Virtual machine name: `Ubuntu-22.04`
   - Location: 기본값 또는 원하는 폴더
   - Next

6. 디스크 용량 설정:
   - Maximum disk size: **40GB** 권장 (최소 20GB)
   - **"Store virtual disk as a single file"** 선택
   - Next

7. 하드웨어 커스터마이즈 (Customize Hardware):
   - **Memory**: 4096MB (4GB) 이상 권장
   - **Processors**: 2개 이상
   - **Network Adapter**: NAT (기본값)
   - Close → Finish

#### Step 3: Ubuntu 설치 진행

1. **"Play virtual machine"** 클릭하여 VM 시작
2. Easy Install 사용 시 자동 설치 진행 (약 15-30분)

**수동 설치 시:**
1. "Install Ubuntu" 선택
2. 언어: 한국어 선택
3. 키보드 레이아웃: Korean
4. 설치 유형: "디스크를 지우고 Ubuntu 설치" (VM이므로 안전)
5. 지역: Seoul
6. 사용자 계정 설정
7. 설치 완료 후 "지금 다시 시작" 클릭

#### Step 4: 기본 환경 확인

**데스크탑 둘러보기:**
- 왼쪽: Dock (앱 런처)
- 상단: 시스템 트레이 (시간, 네트워크, 전원)
- Activities: 앱 검색 및 작업 공간

**터미널 열기:**
- `Ctrl + Alt + T` 또는 Activities에서 "Terminal" 검색

**기본 명령어 테스트:**

```bash
# 시스템 정보 확인
uname -a

# Ubuntu 버전 확인
lsb_release -a

# 디스크 용량 확인
df -h

# 메모리 확인
free -h

# 네트워크 확인
ip addr

# 패키지 업데이트
sudo apt update
sudo apt upgrade -y
```

#### Step 5: VMware Tools 설치

VMware Tools는 성능 향상과 호스트-게스트 간 통합 기능을 제공합니다.

```bash
# Ubuntu에서 open-vm-tools 설치
sudo apt install open-vm-tools open-vm-tools-desktop -y

# 재부팅
sudo reboot
```

**VMware Tools 기능:**
- 화면 해상도 자동 조절
- 호스트-게스트 간 복사/붙여넣기
- 공유 폴더 기능
- 시간 동기화
- 향상된 마우스 통합

---

### 실습 3: Rocky Linux 설치 (선택)

**목표:** RHEL 호환 리눅스인 Rocky Linux를 설치해봅니다.

#### Step 1: Rocky Linux ISO 다운로드

1. [Rocky Linux 다운로드](https://rockylinux.org/download) 접속
2. **Rocky Linux 9** 선택
3. **DVD** ISO 다운로드 (약 8GB)
   - 또는 Minimal ISO (약 1.5GB)

#### Step 2: 가상 머신 생성

1. VMware Player → "Create a New Virtual Machine"
2. "I will install the operating system later" 선택 → Next
3. Guest operating system:
   - Linux 선택
   - Version: **Red Hat Enterprise Linux 9 64-bit**
4. 이름: `Rocky-Linux-9`
5. 디스크: 40GB, Single file
6. Customize Hardware:
   - Memory: 4GB
   - Processors: 2
   - CD/DVD: Use ISO image file → Rocky ISO 선택

#### Step 3: 설치 진행

1. VM 시작 → "Install Rocky Linux 9" 선택
2. 언어: 한국어
3. 설치 요약 화면:
   - **설치 대상**: 자동 파티셔닝
   - **소프트웨어 선택**: "Server with GUI" 또는 "Workstation"
   - **Root 암호**: 설정
   - **사용자 생성**: 관리자 권한 부여
4. "설치 시작" 클릭
5. 완료 후 재부팅

#### Step 4: 기본 명령어 (Rocky Linux)

```bash
# 시스템 정보
cat /etc/rocky-release

# 패키지 업데이트 (dnf 사용)
sudo dnf update -y

# 패키지 검색
dnf search httpd

# 패키지 설치
sudo dnf install vim -y

# 서비스 상태 확인
systemctl status sshd
```

---

## 가상 머신 관리 팁

**스냅샷 사용:**
- VM 상태를 저장하여 언제든 복원 가능
- 실험 전 스냅샷 생성 권장
- Player: VM → Manage → Clone (전체 복사)
- Pro: VM → Snapshot → Take Snapshot

**리소스 관리:**
```
호스트 RAM 8GB인 경우:
- VM에 4GB 할당
- 호스트용 4GB 남겨두기

호스트 RAM 16GB인 경우:
- VM에 6-8GB 할당 가능
```

**VM 파일 백업:**
- VM 폴더 전체를 복사하면 백업 완료
- `.vmx`: 설정 파일
- `.vmdk`: 가상 디스크

---

## 완료 체크리스트

- [ ] BIOS에서 가상화(VT-x/AMD-V)가 활성화되어 있는가?
- [ ] VMware Workstation Player가 설치되었는가?
- [ ] Ubuntu ISO 파일을 다운로드했는가?
- [ ] 가상 머신을 생성하고 Ubuntu를 설치했는가?
- [ ] Ubuntu 데스크탑에 로그인할 수 있는가?
- [ ] 터미널에서 기본 명령어를 실행할 수 있는가?
- [ ] VMware Tools(open-vm-tools)가 설치되었는가?

---

## 참고 자료

- [VMware Workstation Player 문서](https://docs.vmware.com/en/VMware-Workstation-Player/)
- [Ubuntu 공식 튜토리얼](https://ubuntu.com/tutorials)
- [Rocky Linux 문서](https://docs.rockylinux.org/)
- [가상화 기초 개념 (Red Hat)](https://www.redhat.com/ko/topics/virtualization)
