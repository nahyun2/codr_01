# codyssey_01
코디세이 루키마리너2기 1주차 미션 수행 결과

# 개발 워크스테이션 구축 (Terminal / Docker / Git)

## 목차
- [프로젝트 개요](#프로젝트-개요)
- [실습 환경](#실습-환경)
- [수행 항목 체크리스트](#수행-항목-체크리스트)
- [터미널 명령어 실습](terminal_practice.md)
- [도커 및 깃허브 연동 실습](docker_report.md)
- [프로젝트 구조](#프로젝트-구조)
- [검증 방법](#검증-방법)
- [트러블슈팅](#트러블슈팅)

---

## 프로젝트 개요
로컬 macOS 환경에서 셸(터미널) 기본기를 익힌 뒤, 이를 바탕으로 OrbStack 기반 Docker로 컨테이너를 다루고, 최종적으로 Git/GitHub를 통해 작업물을 원격 저장소에 연동하는 것까지 한 흐름으로 진행한 실습 과제입니다. "터미널 명령어 → 컨테이너 환경 조작 → 버전 관리 및 배포 준비"로 이어지는 하나의 개발 워크플로우를 구성합니다.

| 단계 | 실습 주제 | 핵심 도구 |
|:---:|---|---|
| 1 | 터미널 기초 조작 | `zsh`, 기본 유닉스 명령어 |
| 2 | Docker 컨테이너 실습 | Docker, OrbStack |
| 3 | Git/GitHub 연동 | Git, GitHub |
 
---
 
## 실습 환경
 
| 항목 | 내용 |
|------|------|
| OS | macOS |
| 셸 | zsh |
| 컨테이너 런타임 | OrbStack (Docker 호환, v28.5.2) |
| 에디터 | VSCode |
| 버전 관리 | Git / GitHub |

## 수행 항목 체크리스트
- [v] 터미널 기본 조작 (위치확인/목록/이동/생성/복사/이동·이름변경/삭제)
- [v] 파일 내용 확인 및 빈 파일 생성
- [v] 파일 권한 변경 실습 (파일 1개)
- [v] 디렉토리 권한 변경 실습 (디렉토리 1개)
- [v] Docker 설치 및 버전/데몬 점검
- [v] Docker 이미지 다운로드/목록 확인
- [v] Docker 컨테이너 실행/중지/목록 확인
- [v] Docker 로그/리소스 확인
- [v] hello-world 컨테이너 실행
- [v] ubuntu 컨테이너 진입 및 내부 명령 수행
- [v] 컨테이너 종료 vs 유지 방식 비교 정리
- [v] 커스텀 Dockerfile 작성 및 이미지 빌드
- [v] 포트 매핑 접속 증거 확보
- [v] 바인드 마운트 변경 반영 검증
- [v] Docker 볼륨 생성/연결 및 영속성 검증
- [v] Git 사용자 정보 및 기본 브랜치 설정
- [v] VSCode-GitHub 로그인 및 저장소 연동

## 프로젝트 구조
 
```
docker-web-lab/
├── app/
│   ├── index.html
│   └── Dockerfile
├── screenshots/
├── README.md
├── docker_practice.md
├── docker_report.md
└── terminal_practice.md
```
 
- `app/` — Docker 이미지에 담을 정적 웹 콘텐츠(`index.html`)와 빌드 정의(`Dockerfile`)
- `screenshots/` — 실습 과정 캡처 이미지 모음
- `README.md` — 프로젝트 개요 (본 문서)
- `docker_report.md` — Docker 실습 리포트
- `terminal_practice.md` — 터미널 기초 실습 정리
---

## 검증 방법
<!-- 형식: 무엇을 확인했는지 → 사용한 명령 → 결과 위치(스크린샷/로그 링크) -->

| 검증 대상 | 사용 명령 |
|---|---|
| Docker 설치 확인 | `docker --version` |
| Docker 데몬 확인 | `docker info` |
| 이미지 목록 | `docker images` |
| 컨테이너 목록 | `docker ps -a` |
| 컨테이너 로그 | `docker logs <container>` |
| 리소스 사용량 | `docker stats` |
| 포트 매핑 접속 | 브라우저 접속 / `curl` |
| 바인드 마운트 반영 | 호스트 파일 수정 → 컨테이너 내부 확인 |
| 볼륨 영속성 | 컨테이너 삭제 후 재생성 → 데이터 확인 |
| Git 설정 | `git config --list` |

## 트러블슈팅

### 1. 컨테이너 이름 중복으로 run 실패
- **문제**: `docker run --name my-nginx ...` 실행 시 `Conflict. The container name "/my-nginx" is already in use` 에러가 발생하며 컨테이너가 실행되지 않음.
- **원인 가설**: 이전에 동일한 이름(`my-nginx`)으로 생성된 컨테이너가 남아 있어 이름이 중복된 것으로 추정.
- **확인**: 
  ```bash
  docker ps -a
  ```
  → 중지 상태(Exited)의 `my-nginx` 컨테이너가 목록에 존재함을 확인.
- **해결/대안**: 
  ```bash
  docker rm -f my-nginx
  docker run --name my-nginx -d -p 8080:80 nginx
  ```
  기존 컨테이너를 삭제한 뒤 재실행하여 정상 동작 확인.  
  → 도커는 컨테이너 이름이 유일해야 하므로, 삭제 후 재사용하거나 다른 이름을 지정해야 함.


