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
chlskgus7895030@c6r5s5 ~ % 
