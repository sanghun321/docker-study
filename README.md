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
  ```

  출력 결과 :
```text
user.name=sanghun321
user.email=sanghun321@example.com
init.defaultbranch=main
```


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

  ## 4. Docker 설치 및 기본 점검

- **Docker 버전 및 시스템/데몬 상태 확인**
  ```bash
  docker --version
  docker info
  docker ps
  ```

  출력 결과:

- **Docker 버전 확인**
  ```text
  @sanghun321 ~/workspaces/docker-study (main) $ docker --version
  Docker version 29.9.2-2, build a82e528901260c8df6073311379eac65f92db204
  ```

  **Docker 시스템 정보 확인**
  ```text
  @sanghun321 ~/workspaces/docker-study (main) $ docker info
  Client:
  Version:    29.9.2
  Context:    default
  Debug Mode: false

  Server:
  Containers: 0
  Images:     0
  Storage Driver: overlayfs
  Kernel Version: 6.8.0-1064-azure
  Operating System: Ubuntu 24.04.5 LTS (containerized)
  Total Memory: 7.756GiB
  ```

  **컨테이너 목록 확인 (docker ps)**
  ```text
  @sanghun321 ~/workspaces/docker-study (main) $ docker ps
  CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
  ```

## 5. Docker 기본 운영 명령어 수행

- **이미지 및 컨테이너 상태 확인**
  ```bash
  docker images
  docker ps
  docker ps -a
  ```
  출력 결과 :
  ```text
  @sanghun321 ~/workspaces/docker-study (main) $ docker images
  REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
  hello-world   latest    c567b231f656   2 weeks ago     9.49kB

  @sanghun321 ~/workspaces/docker-study (main) $ docker ps
  CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

  @sanghun321 ~/workspaces/docker-study (main) $ docker ps -a
  CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
  9cee53326202   hello-world   "/hello"   9 minutes ago    Exited (0) 9 minutes ago              wizardly_mclaren
  d2635bee269b   hello-world   "/hello"   9 minutes ago    Exited (0) 9 minutes ago              suspicious_fermat
  ```

- **컨테이너 운영 정보 확인 (로그 및 리소스)**

출력 결과:
```text
@sanghun321 ~/workspaces/docker-study (main) $ docker logs 9cee
1. The Docker client contacted the Docker daemon.
2. The Docker daemon pulled the "hello-world" image from the Docker Hub. (amd64)
3. The Docker daemon created a new container from that image which runs the executable that produces the output you are currently reading.
4. The Docker daemon streamed that output to the Docker client, which sent it to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
$ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
[https://hub.docker.com/](https://hub.docker.com/)

For more examples and ideas, visit:
[https://docs.docker.com/get-started/](https://docs.docker.com/get-started/)

@sanghun321 ~/workspaces/docker-study (main) $ docker stats --no-stream
CONTAINER ID   NAME              CPU %     MEM USAGE / LIMIT     MEM %     NET I/O     BLOCK I/O   PIDS
9cee53326202   wizardly_mclaren  0.00%     0B / 7.756GiB         0.00%     0B / 0B     0B / 0B     0
```

## 6 컨테이너 실행 실습
**hello-world 컨테이너 실행 및 상태 검증**
```text
docker run hello-world
docker images
docker ps -a
```

출력 결과 :
```text
@sanghun321 ~/workspaces/docker-study (main) $ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c567b231f656: Pull complete 
Digest: sha256:49c54a500b1716fa549a0d81fa2207908c6ff78627b003a27c9f69744c017d9a
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the "hello-world" container (containerized)
 3. The Docker daemon created a new container from that image which runs the executable that
    produces the output you are currently reading.
 4. The Docker daemon streamed output to the output to the client, which exited cleanly.

To try something more ambitious, you can run:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
https://hub.docker.com/

For more examples and ideas, visit:
https://docs.docker.com/get-started/

@sanghun321 ~/workspaces/docker-study (main) $ docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    c567b231f656   2 weeks ago     9.49kB

@sanghun321 ~/workspaces/docker-study (main) $ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
9cee53326202   hello-world   "/hello"   1 second ago     Exited (0) 1 second ago               wizardly_mclaren
d2635bee269b   hello-world   "/hello"   22 seconds ago   Exited (0) 21 seconds ago             suspicious_fermat
```

**ubuntu 컨테이너 내부 진입 및 명령어 수행**
  ```bash
  docker run -it ubuntu bash
  ls
  echo "Hello Codyssey"
  cat /etc/issue
  exit
  ```

  출력 결과:
  ```text
  @sanghun321 ~/workspaces/docker-study (main) $ docker run -it ubuntu bash
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
f3f25b84a7d7: Pull complete 
edafa32f5d9e: Pull complete 
21594acc2d8b: Download complete 
Digest: sha256:6a06111c3b0a8600feadb270aec771dc239bf407b25625f93f44ab00e11f4d3c
Status: Downloaded newer image for ubuntu:latest

root@ed3c985641d3:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

root@ed3c985641d3:/# echo "Hello Codyssey"
Hello Codyssey

root@ed3c985641d3:/# cat /etc/issue
Ubuntu 26.04.1 LTS \n \l

root@ed3c985641d3:/# exit
exit

@sanghun321 ~/workspaces/docker-study (main) $
```

- **컨테이너 종료 및 유지 방식 (`attach` vs `exec`) 비교 정리**
  - **`docker run -it` / `docker attach`**: 
    - 컨테이너 내부의 주 프로세스(PID 1, 예: `bash`)와 직접 연결(Attach)됩니다.
    - 터미널에서 `exit` 명령어를 입력하거나 내부 프로세스가 종료되면, 컨테이너의 주 프로세스가 끝나면서 컨테이너 상태가 정지(`Exited`) 상태로 전환됩니다.
  - **`docker exec`**: 
    - 이미 실행 중인 컨테이너 내부에 새로운 독립적인 프로세스를 생성하여 접속합니다.
    - 접속을 종료(`exit`)하더라도 주 프로세스는 유지되므로, 컨테이너는 중지되지 않고 계속해서 실행 상태(`Running`)로 유지됩니다.

    