chlskgus7895030@c6r5s5 ~ % docker --version            
Docker version 28.5.2, build ecc6942
chlskgus7895030@c6r5s5 ~ % docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/chlskgus7895030/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/chlskgus7895030/.docker/cli-plugins/docker-compose

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 28.5.2
 Storage Driver: overlay2
  Backing Filesystem: btrfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 1c4457e00facac03ce1d75f7b6777a7a851e5c41
 runc version: d842d7719497cc3b774fd71620278ac9e17710e0
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.17.8-orbstack-00308-g8f9c941121b1
 Operating System: OrbStack
 OSType: linux
 Architecture: x86_64
 CPUs: 6
 Total Memory: 15.67GiB
 Name: orbstack
 ID: dfa49c4a-0793-478e-8d3a-0be8e7e43f6c
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine
 Default Address Pools:
   Base: 192.168.97.0/24, Size: 24
   Base: 192.168.107.0/24, Size: 24
   Base: 192.168.117.0/24, Size: 24
   Base: 192.168.147.0/24, Size: 24
   Base: 192.168.148.0/24, Size: 24
   Base: 192.168.155.0/24, Size: 24
   Base: 192.168.156.0/24, Size: 24
   Base: 192.168.158.0/24, Size: 24
   Base: 192.168.163.0/24, Size: 24
   Base: 192.168.164.0/24, Size: 24
   Base: 192.168.165.0/24, Size: 24
   Base: 192.168.166.0/24, Size: 24
   Base: 192.168.167.0/24, Size: 24
   Base: 192.168.171.0/24, Size: 24
   Base: 192.168.172.0/24, Size: 24
   Base: 192.168.181.0/24, Size: 24
   Base: 192.168.183.0/24, Size: 24
   Base: 192.168.186.0/24, Size: 24
   Base: 192.168.207.0/24, Size: 24
   Base: 192.168.214.0/24, Size: 24
   Base: 192.168.215.0/24, Size: 24
   Base: 192.168.216.0/24, Size: 24
   Base: 192.168.223.0/24, Size: 24
   Base: 192.168.227.0/24, Size: 24
   Base: 192.168.228.0/24, Size: 24
   Base: 192.168.229.0/24, Size: 24
   Base: 192.168.237.0/24, Size: 24
   Base: 192.168.239.0/24, Size: 24
   Base: 192.168.242.0/24, Size: 24
   Base: 192.168.247.0/24, Size: 24
   Base: fd07:b51a:cc66:d000::/56, Size: 64

WARNING: DOCKER_INSECURE_NO_IPTABLES_RAW is set

chlskgus7895030@c6r5s5 ~ % docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
26c307b5e35a: Pull complete 
3c55dc422a81: Pull complete 
d84ae7b21412: Pull complete 
c0df8d325117: Pull complete 
b8b80b9bc028: Pull complete 
f5de6e85ac74: Pull complete 
5a4222b844e8: Pull complete 
Digest: sha256:640dee81b9ada2bf929ae17c2c7e88930f244216aa6418306226ce9bdc3271e6
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest

chlskgus7895030@c6r5s5 ~ % docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    5253dc86cc93   11 hours ago   161MB


chlskgus7895030@c6r5s5 ~ % docker run -d --name nginx-lab -p 8080:80 nginx
docker: Error response from daemon: Conflict. The container name "/nginx-lab" is already in use by container "f3798749f13093da344646d5bb44285cce178070aa8c6625f38b68100c10ab19". You have to remove (or rename) that container to be able to reuse that name.

Run 'docker run --help' for more information
chlskgus7895030@c6r5s5 ~ % docker ps                                      
CONTAINER ID   IMAGE     COMMAND                   CREATED         STATUS         PORTS                                     NAMES
f3798749f130   nginx     "/docker-entrypoint.…"   6 minutes ago   Up 6 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   nginx-lab
chlskgus7895030@c6r5s5 ~ % docker logs nginx-lab                         
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/08/05 11:13:03 [notice] 1#1: using the "epoll" event method
2026/08/05 11:13:03 [notice] 1#1: nginx/1.31.3
2026/08/05 11:13:03 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19) 
2026/08/05 11:13:03 [notice] 1#1: OS: Linux 6.17.8-orbstack-00308-g8f9c941121b1
2026/08/05 11:13:03 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 20480:1048576
2026/08/05 11:13:03 [notice] 1#1: start worker processes
2026/08/05 11:13:03 [notice] 1#1: start worker process 29
2026/08/05 11:13:03 [notice] 1#1: start worker process 30
2026/08/05 11:13:03 [notice] 1#1: start worker process 31
2026/08/05 11:13:03 [notice] 1#1: start worker process 32
2026/08/05 11:13:03 [notice] 1#1: start worker process 33
2026/08/05 11:13:03 [notice] 1#1: start worker process 34
chlskgus7895030@c6r5s5 ~ % docker stats nginx-lab    
CONTAINER ID   NAME        CPU %     MEM USAGE / LIMIT   MEM %     NET I/O   BLOCK I/O   PIDS 
f3798749f130   nginx-lab   --        -- / --             --        --        --          -- 
 
^C
got 3 SIGTERM/SIGINTs, forcefully exiting

chlskgus7895030@c6r5s5 ~ % docker stop nginx-lab                         
nginx-lab
chlskgus7895030@c6r5s5 ~ % docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
chlskgus7895030@c6r5s5 ~ % docker ps -a
CONTAINER ID   IMAGE     COMMAND                   CREATED         STATUS                      PORTS     NAMES
f3798749f130   nginx     "/docker-entrypoint.…"   9 minutes ago   Exited (0) 25 seconds ago             nginx-lab

chlskgus7895030@c6r5s5 ~ % docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
Digest: sha256:7f4da0fc94bcece205a8c0b6f4d11c8196924654ffe5c4d1aa439b7f632048b2
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

chlskgus7895030@c6r5s5 ~ % docker run -it --name ubuntu-lab ubuntu bash
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
617772c7d19b: Pull complete 
a7fb98a8eddd: Pull complete 
Digest: sha256:678c6550cc43645e08669028bc177f50be4e7c5b8cca677067b1914d4afc7a03
Status: Downloaded newer image for ubuntu:latest
root@39c52e95b712:/# #
root@39c52e95b712:/# #   
root@39c52e95b712:/# ls   
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@39c52e95b712:/# echo "hello docker"
hello docker
root@39c52e95b712:/# pwd
/
root@39c52e95b712:/# exit
exit
chlskgus7895030@c6r5s5 ~ % docker run -dit --name ubuntu-keep ubuntu bash
9db3aecee4996ac7a1947784cc44b96933e220c376467fa239288dc4ca1a18e6
chlskgus7895030@c6r5s5 ~ % docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS     NAMES
9db3aecee499   ubuntu    "bash"    7 seconds ago   Up 7 seconds             ubuntu-keep
chlskgus7895030@c6r5s5 ~ % docker attach ubuntu-keep
root@9db3aecee499:/# echo "attach test"
attach test
root@9db3aecee499:/# 

chlskgus7895030@c6r5s5 ~ % docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS          PORTS     NAMES
9db3aecee499   ubuntu    "bash"    10 minutes ago   Up 10 minutes             ubuntu-keep
chlskgus7895030@c6r5s5 ~ % docker attach ubuntu-keep
root@9db3aecee499:/# exit
exit
chlskgus7895030@c6r5s5 ~ % docker ps -a             
CONTAINER ID   IMAGE         COMMAND                   CREATED          STATUS                      PORTS     NAMES
9db3aecee499   ubuntu        "bash"                    13 minutes ago   Exited (0) 13 seconds ago             ubuntu-keep
39c52e95b712   ubuntu        "bash"                    15 minutes ago   Exited (0) 13 minutes ago             ubuntu-lab
041d3f3a5729   hello-world   "/hello"                  17 minutes ago   Exited (0) 17 minutes ago             admiring_jemison
f3798749f130   nginx         "/docker-entrypoint.…"   38 minutes ago   Exited (0) 29 minutes ago             nginx-lab
chlskgus7895030@c6r5s5 ~ % 

chlskgus7895030@c6r5s5 ~ % mkdir -p ~/docker-web-lab/app
cd ~/docker-web-lab
chlskgus7895030@c6r5s5 docker-web-lab % cat > app/index.html <<'EOF'
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Docker Web Lab</title>
</head>
<body>
  <h1>Docker 커스텀 이미지 실습 성공</h1>
  <p>베이스 이미지: nginx:alpine</p>
  <p>작성자: 학생 실습 페이지</p>
</body>
</html>
EOF
chlskgus7895030@c6r5s5 docker-web-lab % cat > Dockerfile <<'EOF'
FROM nginx:alpine

RUN apk add --no-cache curl

ENV APP_NAME="docker-web-lab"

COPY app/index.html /usr/share/nginx/html/index.html

HEALTHCHECK --interval=30s --timeout=3s --retries=3 CMD curl -f http://localhost/ || exit 1
EOF

chlskgus7895030@c6r5s5 docker-web-lab % docker build -t my-nginx-lab:v1 .
[+] Building 7.5s (8/8) FINISHED                                   docker:orbstack
 => [internal] load build definition from Dockerfile                          0.2s
 => => transferring dockerfile: 264B                                          0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine               2.5s
 => [internal] load .dockerignore                                             0.1s
 => => transferring context: 2B                                               0.0s
 => [1/3] FROM docker.io/library/nginx:alpine@sha256:4a73073bd557c65b759505d  3.0s
 => => resolve docker.io/library/nginx:alpine@sha256:4a73073bd557c65b759505d  0.2s
 => => sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3c 10.33kB / 10.33kB  0.0s
 => => sha256:1d40e3eb3bf4f138de1d67193f2aa5309fcaf343eb5ffa 2.50kB / 2.50kB  0.0s
 => => sha256:f0ba77f796e57c6fa89ae7f4fdad1665d6fcbd8e3f21 12.32kB / 12.32kB  0.0s
 => => sha256:55afa1ecc21d2bb5e5045f32dafee56272ffd89860bac2 3.85MB / 3.85MB  0.4s
 => => sha256:1223f016b4e4a2c21f7c49d4837fbfd47a9da6436b511690ca 627B / 627B  0.6s
 => => sha256:3cd534fe98c64d68a1f4f1c83abb8d5cba7ecfd7be88e5 1.89MB / 1.89MB  0.5s
 => => extracting sha256:55afa1ecc21d2bb5e5045f32dafee56272ffd89860bac26f6c3  0.1s
 => => sha256:62bec68d7c31c4c8a19d812d84da5f7748e54690c037979945 957B / 957B  0.7s
 => => extracting sha256:3cd534fe98c64d68a1f4f1c83abb8d5cba7ecfd7be88e592389  0.1s
 => => sha256:46f977ee452f4399c208714afa034868d6056864f8a0cf3c64 404B / 404B  0.8s
 => => sha256:390dc935348d8070e695fbaae2a4bb114fb9e69c59f628 1.40kB / 1.40kB  1.0s
 => => sha256:d0008c891db48b5f526d914bce9e8d889fe1a9d1f08291 1.21kB / 1.21kB  0.9s
 => => extracting sha256:1223f016b4e4a2c21f7c49d4837fbfd47a9da6436b511690ca1  0.0s
 => => extracting sha256:62bec68d7c31c4c8a19d812d84da5f7748e54690c037979945b  0.0s
 => => sha256:46519e7231d2eb5604df229beb44d59719a489eaa7ac 20.31MB / 20.31MB  1.3s
 => => extracting sha256:46f977ee452f4399c208714afa034868d6056864f8a0cf3c643  0.0s
 => => extracting sha256:d0008c891db48b5f526d914bce9e8d889fe1a9d1f08291ae03f  0.0s
 => => extracting sha256:390dc935348d8070e695fbaae2a4bb114fb9e69c59f628e7576  0.0s
 => => extracting sha256:46519e7231d2eb5604df229beb44d59719a489eaa7aca529825  0.4s
 => [internal] load build context                                             0.2s
 => => transferring context: 337B                                             0.0s
 => [2/3] RUN apk add --no-cache curl                                         0.8s
 => [3/3] COPY app/index.html /usr/share/nginx/html/index.html                0.2s 
 => exporting to image                                                        0.2s
 => => exporting layers                                                       0.2s
 => => writing image sha256:96160b250527206d0f5a5ec87dd08632546ab99234dd6538  0.0s
 => => naming to docker.io/library/my-nginx-lab:v1                            0.0s
chlskgus7895030@c6r5s5 docker-web-lab % docker run -d --name my-nginx-web -p 8080:80 my-nginx-lab:v1
31f48e61031da2d68586519254281c7ee96ac9d606c902eb71a277c55f4b3b2f
chlskgus7895030@c6r5s5 docker-web-lab % docker ps
CONTAINER ID   IMAGE             COMMAND                   CREATED          STATUS                             PORTS                                     NAMES
31f48e61031d   my-nginx-lab:v1   "/docker-entrypoint.…"   11 seconds ago   Up 10 seconds (health: starting)   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-nginx-web
chlskgus7895030@c6r5s5 docker-web-lab % docker logs my-nginx-web
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/08/05 12:03:24 [notice] 1#1: using the "epoll" event method
2026/08/05 12:03:24 [notice] 1#1: nginx/1.31.3
2026/08/05 12:03:24 [notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0) 
2026/08/05 12:03:24 [notice] 1#1: OS: Linux 6.17.8-orbstack-00308-g8f9c941121b1
2026/08/05 12:03:24 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 20480:1048576
2026/08/05 12:03:24 [notice] 1#1: start worker processes
2026/08/05 12:03:24 [notice] 1#1: start worker process 31
2026/08/05 12:03:24 [notice] 1#1: start worker process 32
2026/08/05 12:03:24 [notice] 1#1: start worker process 33
2026/08/05 12:03:24 [notice] 1#1: start worker process 34
2026/08/05 12:03:24 [notice] 1#1: start worker process 35
2026/08/05 12:03:24 [notice] 1#1: start worker process 36
::1 - - [05/Aug/2026:12:03:54 +0000] "GET / HTTP/1.1" 200 267 "-" "curl/8.21.0" "-"
chlskgus7895030@c6r5s5 docker-web-lab % docker build -t my-nginx-lab:v1 .
[+] Building 1.3s (8/8) FINISHED                                                                                                                       docker:orbstack
 => [internal] load build definition from Dockerfile                                                                                                              0.1s
 => => transferring dockerfile: 264B                                                                                                                              0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                                                                                   0.8s
 => [internal] load .dockerignore                                                                                                                                 0.1s
 => => transferring context: 2B                                                                                                                                   0.0s
 => [1/3] FROM docker.io/library/nginx:alpine@sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3c2c3aeac22d2a470c1752                                             0.0s
 => [internal] load build context                                                                                                                                 0.1s
 => => transferring context: 59B                                                                                                                                  0.0s
 => CACHED [2/3] RUN apk add --no-cache curl                                                                                                                      0.0s
 => CACHED [3/3] COPY app/index.html /usr/share/nginx/html/index.html                                                                                             0.0s
 => exporting to image                                                                                                                                            0.0s
 => => exporting layers                                                                                                                                           0.0s
 => => writing image sha256:96160b250527206d0f5a5ec87dd08632546ab99234dd65386cafd7103f7badc9                                                                      0.0s
 => => naming to docker.io/library/my-nginx-lab:v1                                                                                                                0.0s
chlskgus7895030@c6r5s5 docker-web-lab % docker run -d --name my-nginx-web -p 8080:80 my-nginx-lab:v1
docker: Error response from daemon: Conflict. The container name "/my-nginx-web" is already in use by container "31f48e61031da2d68586519254281c7ee96ac9d606c902eb71a277c55f4b3b2f". You have to remove (or rename) that container to be able to reuse that name.


chlskgus7895030@c6r5s5 docker-web-lab % cd ~/docker-web-lab
chlskgus7895030@c6r5s5 docker-web-lab % docker run -d --name my-nginx-bind -p 8081:80 -v "$(pwd)/app:/usr/share/nginx/html" nginx:alpine
Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
55afa1ecc21d: Already exists 
3cd534fe98c6: Already exists 
1223f016b4e4: Already exists 
62bec68d7c31: Already exists 
46f977ee452f: Already exists 
d0008c891db4: Already exists 
390dc935348d: Already exists 
46519e7231d2: Already exists 
Digest: sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3c2c3aeac22d2a470c1752
Status: Downloaded newer image for nginx:alpine
94176e3287bdac2a79d10761e73d00f5884f274e3fb7a77b703a8c058b7c0fd6
chlskgus7895030@c6r5s5 docker-web-lab % curl http://localhost:8081
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Docker Web Lab</title>
</head>
<body>
  <h1>Docker 커스텀 이미지 실습 성공</h1>
  <p>베이스 이미지: nginx:alpine</p>
  <p>작성자: 학생 실습 페이지</p>
</body>
</html>
chlskgus7895030@c6r5s5 docker-web-lab % cat > app/index.html <<'EOF'
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Bind Mount Test</title>
</head>
<body>
  <h1>바인드 마운트 변경 반영 성공</h1>
  <p>호스트에서 수정한 내용입니다.</p>
</body>
</html>
EOF
chlskgus7895030@c6r5s5 docker-web-lab % curl http://localhost:8081
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Bind Mount Test</title>
</head>
<body>
  <h1>바인드 마운트 변경 반영 성공</h1>
  <p>호스트에서 수정한 내용입니다.</p>
</body>
</html>
chlskgus7895030@c6r5s5 docker-web-lab % docker volume create mydata
mydata
chlskgus7895030@c6r5s5 docker-web-lab % docker volume ls
DRIVER    VOLUME NAME
local     mydata
chlskgus7895030@c6r5s5 docker-web-lab % docker run -dit --name vol-test -v mydata:/data ubuntu bash
4b04d050d8124f6f39b40188e4435323c6e5945d8e206d071de42f178d83ed2f
chlskgus7895030@c6r5s5 docker-web-lab % docker exec vol-test bash -c 'echo "hello docker volume" > /data/message.txt && cat /data/message.txt'
hello docker volume

Run 'docker run --help' for more information

chlskgus7895030@c6r5s5 docker-web-lab % docker inspect vol-test

chlskgus7895030@c6r5s5 docker-web-lab % docker exec vol-test cat /data/message.txt
hello docker volume
chlskgus7895030@c6r5s5 docker-web-lab % docker rm -f vol-test
vol-test
chlskgus7895030@c6r5s5 docker-web-lab % docker run -dit --name vol-test2 -v mydata:/data ubuntu bash
85c3637c0a92930bed6a66948bc3d7cf198cfdc2819b8de85c1a72b4b34ef063
chlskgus7895030@c6r5s5 docker-web-lab % docker exec vol-test2 cat /data/message.txt
hello docker volume

