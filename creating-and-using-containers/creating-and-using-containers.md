# Creating and Using Containers

## Check our Docker Install and Config

`docker version` - Check your versions and that docker is working.

```powershell
Aditya :: System32 » docker version
Client: Docker Engine - Community
 Cloud integration: 1.0.4
 Version:           20.10.0
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        7287ab3
 Built:             Tue Dec  8 18:55:31 2020
 OS/Arch:           windows/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.0
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       eeddea2
  Built:            Tue Dec  8 18:58:04 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v1.4.3
  GitCommit:        269548fa27e0089a8b8278fc4fc781d7f65a939b
 runc:
  Version:          1.0.0-rc92
  GitCommit:        ff819c7e9184c13b7c2607fe6c30ae19403a7aff
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

`docker info` - Shows most configuration values for the engine.

```powershell
Aditya :: System32 » docker info
Client:
 Context:    default
 Debug Mode: false
 Plugins:
  app: Docker App (Docker Inc., v0.9.1-beta3)
  buildx: Build with BuildKit (Docker Inc., v0.4.2-docker)
  scan: Docker Scan (Docker Inc., v0.5.0)

Server:
 Containers: 36
  Running: 36
  Paused: 0
  Stopped: 0
 Images: 11
 Server Version: 20.10.0
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 269548fa27e0089a8b8278fc4fc781d7f65a939b
 runc version: ff819c7e9184c13b7c2607fe6c30ae19403a7aff
 init version: de40ad0
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 4.19.128-microsoft-standard
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 12
 Total Memory: 12.39GiB
 Name: docker-desktop
 ID: HHTP:JIH2:BC6E:DDT7:RAFS:XFNT:7L4P:WFZY:LLJC:BSVD:JSRW:RNAN
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine

WARNING: No blkio weight support
WARNING: No blkio weight_device support
WARNING: No blkio throttle.read_bps_device support
WARNING: No blkio throttle.write_bps_device support
WARNING: No blkio throttle.read_iops_device support
WARNING: No blkio throttle.write_iops_device support
WARNING: bridge-nf-call-iptables is disabled
WARNING: bridge-nf-call-ip6tables is disabled
```

`docker` command formats:

1.  new `management commands` format:

```powershell
docker <command> <sub-command> (OPTIONS)
```

2. old way (still works):

```powershell
docker <command> (OPTIONS)
```

# Starting a Nginx Web Server

To create a simple container of `nginx`, we run:

```powershell
docker container run --publish 8080:80 --name webhost --detach nginx:1.11 nginx -T
```

What actually happens in `docker container run` ?

1. Looks for that image locally in image cache, doesn't find anything.
2. Then looks in remote image repository (defaults to Docker Hub)
3. Downloads the latest version (`nginx:latest` by default)
4. Create new container based on that image and prepares to start
5. Gives it a virtual IP on a private network inside docker engine
6. Opens up port 80 on host and forwards to port 80 in container
7. Starts container by using the CMD in the image Dockerfile

## Containers v/s VMs

`docker run --name mongo -d mongo` creates a `mongodb` container.

```powershell
Aditya :: System32 » docker container run --name mongo -d mongo
Unable to find image 'mongo:latest' locally
latest: Pulling from library/mongo
f22ccc0b8772: Pull complete
3cf8fb62ba5f: Pull complete
e80c964ece6a: Pull complete
329e632c35b3: Pull complete
3e1bd1325a3d: Pull complete
4aa6e3d64a4a: Pull complete
035bca87b778: Pull complete
874e4e43cb00: Pull complete
08cb97662b8b: Pull complete
f623ce2ba1e1: Pull complete
f100ac278196: Pull complete
6f5539f9b3ee: Pull complete
Digest: sha256:02e9941ddcb949424fa4eb01f9d235da91a5b7b64feb5887eab77e1ef84a3bad
Status: Downloaded newer image for mongo:latest
da0fb8ecc5cf57014ec25dc606fdb85da259596302127bc4b4c940ec4114add0
```

`docker top da0fb8ecc5cf57014ec25dc606fdb85da259596302127bc4b4c940ec4114add0 ` gives a list of running processes in specific container.

```powershell
Aditya :: System32 » docker top da0fb8ecc5cf57014ec25dc606fdb85da259596302127bc4b4c940ec4114add0
PID                 USER                TIME                COMMAND
15592               999                 0:01                mongod --bind_ip_all
```

`docker ps`  show all containers (default shows just running)

```powershell
Aditya :: System32 » docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS       NAMES
da0fb8ecc5cf   mongo     "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   27017/tcp   mongo
```

For Linux, use `ps aux|grep "mongod"`. For Windows, `tasklist | findstr "mongod"`. We can see here that that `mongo` container is running as a process.

```powershell
Aditya :: System32 » tasklist | findstr "mongo"
mongod.exe                    4296 Services                   0     20,044 K
```

If we stop the above container.....

```powershell
docker container stop mongo
```



```powershell

```



```powershell

```



```powershell

```



```powershell

```



```powershell

```



```powershell

```



```powershell

```



```powershell

```



```powershell

```



```powershell

```



```powershell

```



```powershell

```

