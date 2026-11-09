# 🚀 Codyssey Mission 1: Developer Workstation Setup

> 개발자 워크스테이션 환경 구축 미션 수행 결과 보고서입니다. 본문서는 Linux CLI 제어, 권한 관리, Docker 컨테이너 및 볼륨/마운트 실습, 그리고 Git 버전 관리 과정을 기록한 기술 문서입니다.

---

## 📌 1. 프로젝트 개요 (Overview)
- **미션 목표:** 
  - 로컬 및 클라우드(Codespaces) 환경에서 재현 가능한 개발 워크스테이션 구축
  - Linux CLI 및 파일 권한 제어 원리 체득
  - Docker를 활용한 컨테이너 격리, 커스텀 이미지 빌드, 포트 매핑, 볼륨 영속성 검증
  - Git/GitHub을 이용한 형상 관리 및 원격 협업 기반 다지기
- **실습 환경 (Execution Environment):**
  - **OS / Shell / Terminal:** Ubuntu Linux (GitHub Codespaces / Bash Shell)
  - **Docker Version:** `Docker version 29.9.3` (또는 환경 기본 제공 버전)
  - **Git Version:** `git version 2.x` (계정: `sanghun321`)

---

## ✅ 2. 수행 항목 체크리스트 (Checklist)
- [x] 터미널 기본 조작 (현재 위치 확인, 숨김 파일 포함 목록, 이동, 생성, 복사, 삭제 등)
- [x] 권한 변경 실습 (파일 `644`, 디렉토리 `755` 권한 변경 전/후 비교)
- [x] Docker 설치 및 데몬 상태 점검 (`docker --version`, `docker info`)
- [x] Docker 기본 운영 명령 수행 (`images`, `ps`, `logs`, `stats`)
- [x] 컨테이너 실행 실습 (`hello-world`, `ubuntu` 대화형 진입 및 종료 방식 차이 관찰)
- [x] 커스텀 Dockerfile 제작 및 Nginx 기반 웹서버 이미지 빌드
- [x] 포트 매핑 (`8080:80`) 및 웹서버 접속 검증 (`curl`)
- [x] 바인드 마운트 및 Docker 볼륨 영속성 검증
- [x] Git 설정 및 GitHub 저장소 연동 (`git config --list`, 원격 푸시)
- [x] 보안 및 개인정보 마스킹 (토큰 및 민감 정보 제거)

---

## 🛠️ 3. 상세 수행 내용 및 검증 로그

### ① Git 설정 및 GitHub 연동
- **Git 사용자 설정 및 확인 (`git config --list`)**
  ```bash
  git config --global user.name "sanghun321"
  git config --global user.email "sanghun321@example.com"
  git config --global init.defaultBranch main

  git config --list

  출력 결과 :
user.name=sanghun321
user.email=sanghun321@example.com
init.defaultbranch=main

### ① 실행 환경 및 터미널 기본 조작
- **현재 위치 및 시스템/셸 정보 확인**
  ```bash
  pwd
  uname -a
  echo $SHELL
  ```

  출력 결과:
```text
@sanghun321 ~/workspaces/docker-study (main) $ pwd
/workspaces/docker-study
@sanghun321 ~/workspaces/docker-study (main) $ uname -a
Linux codespaces-82305b 6.8.0-1064-azure ...
@sanghun321 ~/workspaces/docker-study (main) $echo$SHELL
/bin/bash
```

# 1. 빈 파일 및 디렉토리 생성
```text
touch practice.txt
mkdir sample_dir
```

# 2. 파일 내용 작성 및 확인
```text
echo "Codyssey Mission 1 CLI Test" > practice.txt
cat practice.txt
```

# 3. 파일 복사 및 이동/이름 변경, 삭제 실습
```text
cp practice.txt practice_copy.txt
mv practice_copy.txt sample_dir/renamed_file.txt
ls -la
ls -la sample_dir
rm sample_dir/renamed_file.txt
ls -la sample_dir
```

출력 결과 : 
```text
@sanghun321 ~/workspaces/docker-study (main) $ touch practice.txt
@sanghun321 ~/workspaces/docker-study (main) $ mkdir sample_dir
@sanghun321 ~/workspaces/docker-study (main) $ echo "Codyssey Mission 1 CLI Test" > practice.txt
@sanghun321 ~/workspaces/docker-study (main) $ cat practice.txt
Codyssey Mission 1 CLI Test
@sanghun321 ~/workspaces/docker-study (main) $ cp practice.txt practice_copy.txt
@sanghun321 ~/workspaces/docker-study (main) $ mv practice_copy.txt sample_dir/renamed_file.txt
@sanghun321 ~/workspaces/docker-study (main) $ ls -la
total 20
drwxrwxr-x+ 4 sanghun321 sanghun321 4096 Nov  9 18:30 .
drwxrwxr-x+ 3 sanghun321 sanghun321 4096 Nov  9 18:00 ..
drwxrwxr-x+ 8 sanghun321 sanghun321 4096 Nov  9 18:30 .git
-rw-rw-r--  1 sanghun321 sanghun321   13 Nov  9 17:50 README.md
-rw-rw-r--  1 sanghun321 sanghun321   28 Nov  9 18:30 practice.txt
drwxrwxr-x+ 2 sanghun321 sanghun321 4096 Nov  9 18:30 sample_dir
@sanghun321 ~/workspaces/docker-study (main) $ ls -la sample_dir
total 12
drwxrwxr-x+ 2 sanghun321 sanghun321 4096 Nov  9 18:30 .
drwxrwxr-x+ 4 sanghun321 sanghun321 4096 Nov  9 18:30 ..
-rw-rw-r--  1 sanghun321 sanghun321   28 Nov  9 18:30 renamed_file.txt
@sanghun321 ~/workspaces/docker-study (main) $ rm sample_dir/renamed_file.txt
@sanghun321 ~/workspaces/docker-study (main) $ ls -la sample_dir
total 8
drwxrwxr-x+ 2 sanghun321 sanghun321 4096 Nov  9 18:30 .
drwxrwxr-x+ 4 sanghun321 sanghun321 4096 Nov  9 18:30 ..
```

# 3. 권한 실습 및 증거 기록

### 1. 현재 생성되어 있는 파일 및 디렉토리의 기본 권한 확인
```text
ls -l practice.txt
ls -ld sample_dir
```

### 2. 권한 변경 및 변경 결과 확인
#### 2-1 파일 권한을 644(-rw-r--r--)로 변경:
```text
chmod 644 practice.txt
ls -l practice.txt
```

#### 2-2 디렉토리 권한을 755(drwxr-xr-x)로 변경:
```text
chmod 755 sample_dir
ls -ld sample_dir
```

### ③ 권한 실습 및 증거 기록 (`chmod`)
- **파일 및 디렉토리 권한 변경 실험**
  ```text
  @sanghun321 ~/workspaces/docker-study (main) $ ls -l practice.txt
  -rw-rw-rw- 1 codespace codespace 28 Nov  9 08:17 practice.txt
  @sanghun321 ~/workspaces/docker-study (main) $ ls -ld sample_dir
  drwxrwxrwx+ 2 codespace codespace 4096 Nov  9 08:18 sample_dir
  @sanghun321 ~/workspaces/docker-study (main) $ chmod 644 practice.txt
  @sanghun321 ~/workspaces/docker-study (main) $ ls -l practice.txt
  -rw-r--r-- 1 codespace codespace 28 Nov  9 08:17 practice.txt
  @sanghun321 ~/workspaces/docker-study (main) $ chmod 755 sample_dir
  @sanghun321 ~/workspaces/docker-study (main) $ ls -ld sample_dir
  drwxr-xr-x+ 2 codespace codespace 4096 Nov  9 08:18 sample_dir
  ```

  