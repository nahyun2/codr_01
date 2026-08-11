# 🐳 Docker & Git 실습 프로젝트

> OrbStack 기반 Docker 컨테이너 실습 및 Git/GitHub 연동 과제
> 사용자명은 `user`로 마스킹했습니다.

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![OrbStack](https://img.shields.io/badge/OrbStack-000000?style=flat&logo=apple&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)

---

## 📖 목차
- [0. 환경 확인](#0-환경-확인)
- [1. Docker 기초](#1-docker-기초)
- [2. 컨테이너 상호작용](#2-컨테이너-상호작용)
- [3. 이미지 빌드](#3-이미지-빌드)
- [4. 데이터 관리](#4-데이터-관리)
- [5. Git & GitHub](#5-git--github)
- [실습 요약](#-실습-요약)


---

## 0. 환경 확인

```bash
docker --version
```
```
Docker version 28.5.2, build ecc6942
```

```bash
docker info
```
```
Client:
 Version:    28.5.2
 Context:    orbstack

Server:
 Server Version: 28.5.2
 Storage Driver: overlay2
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Operating System: OrbStack
 Architecture: x86_64
 CPUs: 6
 Total Memory: 15.67GiB
 Default Runtime: runc
```
- OrbStack이 Docker CLI와 완전히 호환되며, `docker` 명령어를 그대로 사용할 수 있음을 확인하였다.

---

## 1. Docker 기초

### #8 이미지 다운로드 (docker pull)

```bash
docker pull nginx
```
```
Using default tag: latest
latest: Pulling from library/nginx
26c307b5e35a: Pull complete
3c55dc422a81: Pull complete
...
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```

```bash
docker images
```
```
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    5253dc86cc93   11 hours ago   161MB
```
- nginx 공식 이미지를 정상적으로 다운로드하였다.

### #9 컨테이너 실행 (docker run)

```bash
docker run -d --name nginx-lab -p 8080:80 nginx
```
```
docker: Error response from daemon: Conflict. The container name "/nginx-lab" is already in use...
```
이름 중복 에러 발생 → 기존 컨테이너 확인:
```bash
docker ps
```
```
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                     NAMES
f3798749f130   nginx     "/docker-entrypoint.…"   6 minutes ago   Up 6 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   nginx-lab
```
- 동일한 이름의 컨테이너가 이미 실행 중이어서 충돌이 발생함을 확인하였다. (삭제 후 재실행하거나 이름을 바꿔 실행하면 해결)

### #10 컨테이너 모니터링 (logs / stats)

```bash
docker logs nginx-lab
```
```
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/08/05 11:13:03 [notice] 1#1: using the "epoll" event method
2026/08/05 11:13:03 [notice] 1#1: nginx/1.31.3
2026/08/05 11:13:03 [notice] 1#1: start worker processes
```

```bash
docker stats nginx-lab
```
```
CONTAINER ID   NAME        CPU %     MEM USAGE / LIMIT   MEM %     NET I/O   BLOCK I/O   PIDS
f3798749f130   nginx-lab   --        -- / --             --        --        --          --
```
`Ctrl+C`로 종료 후 컨테이너 정리:
```bash
docker stop nginx-lab
docker ps
docker ps -a
```
```
CONTAINER ID   IMAGE     COMMAND                   CREATED         STATUS                      PORTS     NAMES
f3798749f130   nginx     "/docker-entrypoint.…"   9 minutes ago   Exited (0) 25 seconds ago             nginx-lab
```
- 로그 확인 및 실시간 리소스 모니터링을 수행하고, `docker stop` 후 `docker ps -a`로 종료된 컨테이너 상태를 확인하였다.

### #11 hello-world 실행

```bash
docker run hello-world
```
```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
```
- 이미지 자동 다운로드 후 정상 동작 메시지를 확인하였다.

---

## 2. 컨테이너 상호작용

### #12 Ubuntu 컨테이너 진입

```bash
docker run -it --name ubuntu-lab ubuntu bash
```
```
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
Status: Downloaded newer image for ubuntu:latest
```
컨테이너 내부에서:
```bash
ls
echo "hello docker"
pwd
exit
```
```
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
hello docker
/
```
- 컨테이너 내부 셸에서 기본 명령어를 정상 수행하였다.

### #13 attach / exec / detach 차이

```bash
docker run -dit --name ubuntu-keep ubuntu bash
docker ps
```
```
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS     NAMES
9db3aecee499   ubuntu    "bash"    7 seconds ago   Up 7 seconds             ubuntu-keep
```

**attach** (메인 프로세스에 연결)
```bash
docker attach ubuntu-keep
```
```
root@9db3aecee499:/# echo "attach test"
attach test
```

이후 다시 attach하여 `exit` 실행:
```bash
docker attach ubuntu-keep
```
```
root@9db3aecee499:/# exit
exit
```

```bash
docker ps -a
```
```
CONTAINER ID   IMAGE         COMMAND                   CREATED          STATUS                      PORTS     NAMES
9db3aecee499   ubuntu        "bash"                    13 minutes ago   Exited (0) 13 seconds ago             ubuntu-keep
39c52e95b712   ubuntu        "bash"                    15 minutes ago   Exited (0) 13 minutes ago             ubuntu-lab
041d3f3a5729   hello-world   "/hello"                  17 minutes ago   Exited (0) 17 minutes ago             admiring_jemison
f3798749f130   nginx         "/docker-entrypoint.…"   38 minutes ago   Exited (0) 29 minutes ago             nginx-lab
```

| 명령어 | 설명 |
|--------|------|
| `attach` | 실행 중인 컨테이너의 메인 프로세스에 연결. **여기서 `exit`을 입력하면 메인 프로세스가 종료되어 컨테이너 자체가 정지됨** |
| `exec` | 컨테이너 내부에 새 셸을 실행. `exit`해도 메인 프로세스는 유지되어 컨테이너가 계속 실행됨 |
| `detach` | 컨테이너를 종료하지 않고 빠져나옴 (`Ctrl + P, Ctrl + Q`, 또는 `--detach-keys`로 키 조합 변경 가능) |

- `attach` 상태에서 `exit`을 입력하면 컨테이너 메인 프로세스가 종료되어 컨테이너가 `Exited` 상태가 되는 것을 확인하였다. (`exec`로 접속했다면 `exit` 후에도 컨테이너가 유지됨)

---

## 3. 이미지 빌드

### #14 베이스 이미지 선정 & 정적 콘텐츠 준비

- 베이스 이미지: `nginx:alpine`
- 배포용 `index.html` 정적 파일 준비

```bash
mkdir -p ~/docker-web-lab/app
cd ~/docker-web-lab
cat > app/index.html <<'EOF'
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Docker Web Lab</title>
</head>
<body>
  <h1>Docker 커스텀 이미지 실습 성공</h1>
  <p>베이스 이미지: nginx:alpine</p>
</body>
</html>
EOF
```

### #15 Dockerfile 작성

```bash
cat > Dockerfile <<'EOF'
FROM nginx:alpine

RUN apk add --no-cache curl

ENV APP_NAME="docker-web-lab"

COPY app/index.html /usr/share/nginx/html/index.html

HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD curl -f http://localhost/ || exit 1
EOF
```
- curl 설치(`RUN`), 환경변수(`ENV`), 정적 파일 교체(`COPY`), 헬스체크(`HEALTHCHECK`)를 포함하여 작성하였다.

### #16 이미지 빌드 및 실행

```bash
docker build -t my-nginx-lab:v1 .
```
```
[+] Building 7.5s (8/8) FINISHED
 => [internal] load build definition from Dockerfile         0.2s
 => [1/3] FROM docker.io/library/nginx:alpine                3.0s
 => [2/3] RUN apk add --no-cache curl                        0.8s
 => [3/3] COPY app/index.html /usr/share/nginx/html/index.html  0.2s
 => exporting to image                                       0.2s
 => => naming to docker.io/library/my-nginx-lab:v1
```

```bash
docker run -d --name my-nginx-web -p 8080:80 my-nginx-lab:v1
docker ps
```
```
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS                            PORTS                                     NAMES
31f48e61031d   my-nginx-lab:v1   "/docker-entrypoint.…"   11 seconds ago   Up 10 seconds (health: starting)  0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-nginx-web
```
- 빌드 성공 및 포트 `8080:80` 매핑으로 실행하였다. `STATUS`에 `(health: starting)`이 표시되어 `HEALTHCHECK`가 정상 동작 중임을 확인할 수 있다.

### #17 접속 검증

```bash
docker logs my-nginx-web
```
```
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/08/05 12:03:24 [notice] 1#1: nginx/1.31.3
::1 - - [05/Aug/2026:12:03:54 +0000] "GET / HTTP/1.1" 200 267 "-" "curl/8.21.0" "-"
```
- 브라우저와 `curl`로 정적 페이지에 정상 응답(200)이 오는 것을 확인하였다.

> 참고: Dockerfile을 수정하지 않은 상태에서 `docker build`를 다시 실행하면 아래처럼 각 단계가 `CACHED`로 표시되며 빠르게 완료된다.
> ```
> => CACHED [2/3] RUN apk add --no-cache curl        0.0s
> => CACHED [3/3] COPY app/index.html ...             0.0s
> ```
> 또한 이미 사용 중인 컨테이너 이름으로 다시 `docker run`을 실행하면 `Conflict` 에러가 발생하므로, 기존 컨테이너를 삭제(`docker rm -f`)하거나 다른 이름을 사용해야 한다.

---

## 4. 데이터 관리

### #18 바인드 마운트로 변경 반영 검증

```bash
cd ~/docker-web-lab
docker run -d --name my-nginx-bind -p 8081:80 \
  -v "$(pwd)/app:/usr/share/nginx/html" nginx:alpine
```
```
94176e3287bdac2a79d10761e73d00f5884f274e3fb7a77b703a8c058b7c0fd6
```

```bash
curl http://localhost:8081
```
```
<h1>Docker 커스텀 이미지 실습 성공</h1>
<p>베이스 이미지: nginx:alpine</p>
```

호스트에서 `app/index.html` 내용을 직접 수정한 뒤 다시 요청:
```bash
curl http://localhost:8081
```
```
<h1>바인드 마운트 변경 반영 성공</h1>
<p>호스트에서 수정한 내용입니다.</p>
```
- 컨테이너를 재시작하지 않아도 호스트에서 수정한 파일 내용이 **즉시 반영**됨을 확인하였다. (바인드 마운트는 호스트 디렉토리를 컨테이너 경로에 실시간으로 연결)
- docker inspect 명령을 사용해 json 형식으로 바운트된 경로를 확인하였다.

### #19 Docker 볼륨 생성 및 연결

```bash
docker volume create mydata
docker volume ls
```
```
mydata
DRIVER    VOLUME NAME
local     mydata
```

```bash
docker run -dit --name vol-test -v mydata:/data ubuntu bash
docker exec vol-test bash -c 'echo "hello docker volume" > /data/message.txt && cat /data/message.txt'
```
```
hello docker volume
```
- 볼륨을 생성하고 `/data` 경로에 연결하여 데이터를 저장하였다.

### #20 볼륨 영속성 검증

```bash
docker rm -f vol-test
docker run -dit --name vol-test2 -v mydata:/data ubuntu bash
docker exec vol-test2 cat /data/message.txt
```
```
hello docker volume
```
- `vol-test` 컨테이너를 삭제한 뒤 동일한 볼륨(`mydata`)을 새 컨테이너(`vol-test2`)에 연결했을 때도 이전에 저장한 데이터가 그대로 남아있음을 확인하여 **볼륨의 영속성**을 검증하였다.

---

## 5. Git & GitHub

### #21 Git 사용자 정보 및 기본 브랜치 설정

```bash
git config --global user.name "본인이름"
git config --global user.email "본인이메일@example.com"
git config --global init.defaultBranch main
git config --global --list
```
```
user.name=홍길동
user.email=hong@example.com
init.defaultbranch=main
```
- Git 사용자 정보와 기본 브랜치(`main`)를 설정하였다.

### #22 VSCode - GitHub 로그인 및 저장소 연동

```bash
git init
git branch -M main
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/<username>/<repo>.git
git push -u origin main
```

연동 확인:
```bash
git remote -v
```
```
origin  https://github.com/<username>/<repo>.git (fetch)
origin  https://github.com/<username>/<repo>.git (push)
```
- 로컬 저장소를 초기화하고 GitHub 원격 저장소와 연동하여 최초 커밋을 push하였다.

---

## ✅ 실습 요약

| 번호 | 실습 내용 | 결과 |
|:----:|-----------|:----:|
| #8 | 이미지 pull | ✅ |
| #9 | 컨테이너 run | ✅ |
| #10 | logs / stats | ✅ |
| #11 | hello-world | ✅ |
| #12 | ubuntu 진입 | ✅ |
| #13 | attach/exec/detach | ✅ |
| #14 | 베이스 이미지 선정 | ✅ |
| #15 | Dockerfile 작성 | ✅ |
| #16 | build & run | ✅ |
| #17 | 접속 검증 | ✅ |
| #18 | 바인드 마운트 | ✅ |
| #19 | 볼륨 생성/연결 | ✅ |
| #20 | 볼륨 영속성 | ✅ |
| #21 | Git 설정 | ✅ |
| #22 | GitHub 연동 | ✅ |
