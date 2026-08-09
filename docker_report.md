
# 🐳 Docker & Git 실습 프로젝트

> OrbStack 기반 Docker 컨테이너 실습 및 Git/GitHub 연동 과제

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![OrbStack](https://img.shields.io/badge/OrbStack-000000?style=flat&logo=apple&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)
![VSCode](https://img.shields.io/badge/VSCode-007ACC?style=flat&logo=visualstudiocode&logoColor=white)

---

## 📖 목차
- [실습 환경](#-실습-환경)
- [1. Docker 기초](#1-docker-기초)
- [2. 컨테이너 상호작용](#2-컨테이너-상호작용)
- [3. 이미지 빌드](#3-이미지-빌드)
- [4. 데이터 관리](#4-데이터-관리)
- [5. Git & GitHub](#5-git--github)
- [실습 요약](#-실습-요약)

---

## 🖥 실습 환경

| 항목 | 내용 |
|------|------|
| OS | macOS |
| 컨테이너 런타임 | OrbStack (Docker 호환) |
| 에디터 | VSCode |
| 버전 관리 | Git / GitHub |

---

## 1. Docker 기초

<details>
<summary><b>#8 이미지 다운로드 (docker pull)</b></summary>

```bash
docker pull nginx
```
- nginx 공식 이미지를 정상적으로 다운로드하였다.
</details>

<details>
<summary><b>#9 컨테이너 실행 (docker run)</b></summary>

```bash
docker run --name my-nginx -d -p 8080:80 nginx
```
이름 중복 에러 발생 → 기존 컨테이너 삭제 후 재실행:
```bash
docker rm -f my-nginx
docker run --name my-nginx -d -p 8080:80 nginx
```
- nginx 컨테이너가 정상 실행됨을 확인하였다.
</details>

<details>
<summary><b>#10 컨테이너 모니터링 (logs / stats)</b></summary>

```bash
docker logs my-nginx
docker stats
```
- 로그 확인 및 실시간 리소스 모니터링 후 `Ctrl+C`로 종료하였다.
</details>

<details>
<summary><b>#11 hello-world 실행</b></summary>

```bash
docker run hello-world
```
- 이미지 자동 다운로드 후 정상 동작 메시지를 확인하였다.
</details>

---

## 2. 컨테이너 상호작용

<details>
<summary><b>#12 Ubuntu 컨테이너 진입</b></summary>

```bash
docker run -it ubuntu bash
```
내부 명령어 수행:
```bash
ls
echo "hello"
pwd
```
- 컨테이너 내부 셸에서 기본 명령어를 정상 수행하였다.
</details>

<details>
<summary><b>#13 attach / exec / detach 차이</b></summary>

```bash
docker run -dit --name ubuntu-keep ubuntu bash
```

**attach** (detach 키 변경: VSCode 단축키 충돌 회피)
```bash
docker attach --detach-keys="ctrl-]" ubuntu-keep
```

**exec** (새 셸 실행)
```bash
docker exec -it ubuntu-keep bash
exit
```

| 명령어 | 설명 |
|--------|------|
| `attach` | 실행 중인 컨테이너의 메인 프로세스에 연결 |
| `exec` | 컨테이너 내부에 새 셸 실행 |
| `detach` | 컨테이너 종료 없이 빠져나옴 (`Ctrl + ]`) |

- `exit` 후에도 컨테이너가 유지됨을 `docker ps`로 확인하였다.
</details>

---

## 3. 이미지 빌드

<details>
<summary><b>#14 베이스 이미지 선정 & 정적 콘텐츠 준비</b></summary>

- 베이스 이미지: `nginx:alpine`
- 배포용 `index.html` 정적 파일 준비
</details>

<details>
<summary><b>#15 Dockerfile 작성</b></summary>

```dockerfile
FROM nginx:alpine

# 환경변수 설정
ENV APP_ENV=production

# curl 설치
RUN apk add --no-cache curl

# 정적 콘텐츠 교체
COPY app/index.html /usr/share/nginx/html/index.html

# 헬스체크 추가
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1
```
- curl 설치, ENV, COPY, HEALTHCHECK를 포함하여 작성하였다.
</details>

<details>
<summary><b>#16 이미지 빌드 및 실행</b></summary>

```bash
docker build -t my-web:1.0 .
docker run -d --name my-web -p 8080:80 my-web:1.0
```
- 빌드 성공 및 포트 `8080:80` 매핑으로 실행하였다.
</details>

<details>
<summary><b>#17 접속 검증</b></summary>

```bash
curl http://localhost:8080
```
- 브라우저와 `curl`로 정적 페이지 정상 응답을 확인하였다.
</details>

---

## 4. 데이터 관리

<details>
<summary><b>#18 바인드 마운트로 변경 반영 검증</b></summary>

```bash
docker run -d --name my-nginx-bind -p 8081:80 \
  -v "$(pwd)/app:/usr/share/nginx/html" nginx:alpine
```

호스트 파일 수정 후 확인:
```bash
curl http://localhost:8081
```
- 호스트에서 수정한 내용이 컨테이너에 **즉시 반영**됨을 확인하였다.
</details>

<details>
<summary><b>#19 Docker 볼륨 생성 및 연결</b></summary>

```bash
docker volume create mydata
docker run -dit --name vol-test -v mydata:/data ubuntu bash
docker exec vol-test bash -c 'echo "hello docker volume" > /data/message.txt && cat /data/message.txt'
```
- 볼륨을 생성하고 `/data`에 연결하여 데이터를 저장하였다.
</details>

<details>
<summary><b>#20 볼륨 영속성 검증</b></summary>

```bash
# 컨테이너 삭제 (볼륨 유지)
docker rm -f vol-test

# 동일 볼륨으로 새 컨테이너 실행
docker run -dit --name vol-test2 -v mydata:/data ubuntu bash
docker exec vol-test2 cat /data/message.txt
```
- 컨테이너 삭제 후에도 데이터가 유지됨을 확인하여 **영속성**을 검증하였다.
</details>

---

## 5. Git & GitHub

<details>
<summary><b>#21 Git 사용자 정보 및 기본 브랜치 설정</b></summary>

```bash
git config --global user.name "본인이름"
git config --global user.email "본인이메일@example.com"
git config --global init.defaultBranch main
git config --global --list
```

결과 예시:
```
user.name=홍길동
user.email=hong@example.com
init.defaultbranch=main
```
- Git 사용자 정보와 기본 브랜치(`main`)를 설정하였다.
</details>

<details>
<summary><b>#22 VSCode - GitHub 로그인 및 저장소 연동</b></summary>

```bash
git init
git branch -M main
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/nahyun2/codr_01.git
git push -u origin main
```

연동 확인:
```bash
git remote -v
```
```
origin  https://github.com/nahun2/codr_01.git (fetch)
origin  https://github.com/nahyun2/codr_01.git (push)
```

</details>

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

---
