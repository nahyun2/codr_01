docker pull nginx : nginx 이미지를 다운로드했다.
docker images : 다운로드된 이미지 목록을 확인했다.
docker run -d --name nginx-lab -p 8080:80 nginx : nginx 컨테이너를 백그라운드에서 실행했다.
docker ps : 실행 중인 컨테이너 목록을 확인했다.
docker logs nginx-lab : 컨테이너 로그를 확인했다.
docker stats nginx-lab : CPU/메모리 등 리소스 사용량을 확인했다.
docker stop nginx-lab : 실행 중인 컨테이너를 중지했다.
docker ps -a : 중지된 컨테이너까지 포함한 전체 목록을 확인했다.

