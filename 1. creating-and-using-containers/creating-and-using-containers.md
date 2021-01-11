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

## Starting a Nginx Web Server

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

For Linux, use `ps aux|grep "mongod"`. 

For Windows, `tasklist | findstr "mongod"`. 

We can see here that that `mongo` container is running as a process.

```powershell
Aditya :: System32 » tasklist | findstr "mongo"
mongod.exe                    4296 Services                   0     20,044 K
```

If we stop the above container.....

```powershell
docker container stop mongo
```

and we will get the same process but running on a different port.

If we want to remove all containers at once, we type `docker containers 

**Assignment: Manage multiple containers**

## CLI Process Monitoring

Let's spin up two containers - one from `nginx` and other from `mysql`.

```powershell
Aditya :: learning-docker » docker container run --publish 81:80 --detach --name nginx_app nginx
2bb4c649c17b63433241dcfec77179fd60d0ecb1e3456d40e8dd5741bb240047
```

```powershell
Aditya :: learning-docker » docker run --name mysql_app --publish 3306:3306 --env MYSQL_RANDOM_ROOT_PASSWORD=yes --detach mysql
fbff2b981ad16bafc5105c484e109be2229e27d1c7853e962da9e863acdf2f04
```

Running `docker container ls -aq`, we should see two running containers. 

```powershell
Aditya :: learning-docker » docker container ls -aq
fbff2b981ad1
2bb4c649c17b
```

`docker container top mysql_app` lists the processes running in that specific container. 

```powershell
Aditya :: learning-docker » docker container top mysql_app
PID                 USER                TIME                COMMAND
14910               999                 0:02                mysqld
```

 Similarly, `docker container top nginx_app` lists the processes running in that specific container.

```powershell
Aditya :: learning-docker » docker container top nginx_app
PID                 USER                TIME                COMMAND
14618               root                0:00                nginx: master process nginx -g daemon off;
14689               101                 0:00                nginx: worker process
```

`docker container inspect nginx_app` shows metadata about the container.

```powershell
Aditya :: System32 » docker container inspect nginx_app
[
    {
        "Id": "2bb4c649c17b63433241dcfec77179fd60d0ecb1e3456d40e8dd5741bb240047",
        "Created": "2021-01-04T02:43:38.52542Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 14618,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-01-04T02:43:39.1400873Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:ae2feff98a0cc5095d97c6c283dcd33090770c76d63877caa99aefbbe4343bdd",
        "ResolvConfPath": "/var/lib/docker/containers/2bb4c649c17b63433241dcfec77179fd60d0ecb1e3456d40e8dd5741bb240047/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/2bb4c649c17b63433241dcfec77179fd60d0ecb1e3456d40e8dd5741bb240047/hostname",
        "HostsPath": "/var/lib/docker/containers/2bb4c649c17b63433241dcfec77179fd60d0ecb1e3456d40e8dd5741bb240047/hosts",
        "LogPath": "/var/lib/docker/containers/2bb4c649c17b63433241dcfec77179fd60d0ecb1e3456d40e8dd5741bb240047/2bb4c649c17b63433241dcfec77179fd60d0ecb1e3456d40e8dd5741bb240047-json.log",
        "Name": "/nginx_app",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {
                "80/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "81"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                52,
                102
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/48a173e4e2293d5d7b57589771f9c00a06e68766b9e0b885968d49ea6160b8eb-init/diff:/var/lib/docker/overlay2/19a72843084a455cff1ed0e52b208c53854528818ccddf8a252fe6d054ac00d8/diff:/var/lib/docker/overlay2/af57f5fe5d0ff52731939a282bec22e9bbd45c78d280be114d1cf58994da88b5/diff:/var/lib/docker/overlay2/186e8478e1cc2447a09e1c7cb2c3a35b39b05bc7dd62e5fbd123784224fbe623/diff:/var/lib/docker/overlay2/19b26dfa4018c8aa6dfb50b4952c23d2593ad84e67ed5580db9fd4e604f92c22/diff:/var/lib/docker/overlay2/49a98dde916cafa3956d366bf884db8d9638e38ed7616ec7af98770d6386f031/diff",
                "MergedDir": "/var/lib/docker/overlay2/48a173e4e2293d5d7b57589771f9c00a06e68766b9e0b885968d49ea6160b8eb/merged",
                "UpperDir": "/var/lib/docker/overlay2/48a173e4e2293d5d7b57589771f9c00a06e68766b9e0b885968d49ea6160b8eb/diff",
                "WorkDir": "/var/lib/docker/overlay2/48a173e4e2293d5d7b57589771f9c00a06e68766b9e0b885968d49ea6160b8eb/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "2bb4c649c17b",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.19.6",
                "NJS_VERSION=0.5.0",
                "PKG_RELEASE=1~buster"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "nginx",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers \u003cdocker-maint@nginx.com\u003e"
            },
            "StopSignal": "SIGQUIT"
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "22d0b5d5b1552a4e7318675584e0fd668d0e12413b58b190a0c93bd06a56b7b8",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "80/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "81"
                    }
                ]
            },
            "SandboxKey": "/var/run/docker/netns/22d0b5d5b155",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "1aa574223d640c714ae83a0aec7e2c183ace23e40827d5bb8de7b83aedf9e5ab",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "5f10213090984174d45c417b17efd155f20d7cdd03667fd30d6cfbcfc39c9c34",
                    "EndpointID": "1aa574223d640c714ae83a0aec7e2c183ace23e40827d5bb8de7b83aedf9e5ab",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

`docker container stats nginx_app`  shows live performance data for all containers. 

```powershell
CONTAINER ID   NAME        CPU %     MEM USAGE / LIMIT     MEM %     NET I/O      BLOCK I/O   PIDS
2bb4c649c17b   nginx_app   0.00%     4.324MiB / 12.39GiB   0.03%     1.8kB / 0B   0B / 0B     2
```

## Getting into a Shell inside Containers 

`docker container run -it` starts a new container interactively. 

```powershell
Aditya :: System32 » docker container run -it --name proxy nginx bash
root@18b8618b88cb:/#_
```

Now, if we did `docker container inspect` on both `nginx_app` and `proxy`,  we'd get:

```powershell
Aditya :: System32 » docker container inspect nginx_app
[
    {
        "Id": "2bb4c649c17b63433241dcfec77179fd60d0ecb1e3456d40e8dd5741bb240047",
        "Created": "2021-01-04T02:43:38.52542Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",						👈
            "-g",							👈
            "daemon off;"					👈
        ], ....
Aditya :: System32 » docker container inspect proxy
[
    {
        "Id": "18b8618b88cbbaa8f417ef164c079b05dbe94281a841d11132b1e55e4d5d25c7",
        "Created": "2021-01-04T13:04:26.9538719Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "bash"							👈
        ],        
```

The way it does this is `docker container run -it --name proxy nginx bash` , by replacing the default command `nginx -g daemon off;` by `bash` and hence, that is how we can exec into the container.

To start a stopped container, we say `docker container start -ai nginx_app`.

`docker container exec -it` runs additional command in existing container. 

```powershell
Aditya :: System32 » docker container exec -it proxy bash
root@700fd7a62e0e:/#
```

> **Tip:**
>
> To get into `alpine` container, we do `docker container run -it alpine sh` not `bash`.
>

## Docker Networking

- Each container connected to a private virtual network `bridge`.
- Each virtual network routes through a NAT firewall on host IP.
- All containers on a virtual network can talk to each other without `-p`.

***Batteries Included, But Removable.*** Defaults work well in many cases, but easy to swap out parts to customize it.

- Make new virtual networks.
- Attach containers to more than one virtual network (or none).
- Skip virtual networks and use host IP(--net=host).
- Use different Docker network drivers to gain new abilities.

> Best practice is to create a new virtual network for each app:
>
> - network `my_web_app` for `mysql` and `php/apache` containers.
> - network `my_api` for `mongo` and `nodejs` containers.

For exposing ports on containers , we use `--publish`. The syntax remains as `docker container run --publish HOST_PORT:CONTAINER_PORT IMAGE_NAME`.

To check the port mapping, we use `port` management command.

```powershell
Aditya :: System32 » docker container port af5a5abab1de02fed5d1bee3c19ed49054587ec2efa1c42f1fae5e1adfa30de4
80/tcp -> 0.0.0.0:8080
```

To get the container IP, we can `inspet` the running container and `--format` out the `IPAddress`. 

```powershell
Aditya :: System32 » docker container inspect --format "{{ .NetworkSettings.IPAddress }}" proxy
172.17.0.2
```

## CLI Management of Virtual Networks

- `docker network ls` shows networks.

  ```powershell
  Aditya :: learning-docker » docker network ls
  NETWORK ID     NAME      DRIVER    SCOPE
  de2e69bf271d   bridge    bridge    local
  6c467bd0fda4   host      host      local
  202ce568f0b7   none      null      local
  ```

  > `--network bridge` Default Docker virtual network, which is NAT'd behind the Host IP.

- `docker network inspect NETWORK_NAME` inspects a network.

  ```powershell
  Aditya :: learning-docker » docker network inspect bridge
  [
      {
          "Name": "bridge",
          "Id": "de2e69bf271d31e1a39f0f21d1a973f438d0cb1c76d04d3775338fbc82023ba3",
          "Created": "2021-01-06T02:42:08.6789436Z",
          "Scope": "local",
          "Driver": "bridge",
          "EnableIPv6": false,
          "IPAM": {
              "Driver": "default",
              "Options": null,
              "Config": [
                  {
                      "Subnet": "172.17.0.0/16",			👈
                      "Gateway": "172.17.0.1"				👈
                  }
              ]
          },
          "Internal": false,
          "Attachable": false,
          "Ingress": false,
          "ConfigFrom": {
              "Network": ""
          },
          "ConfigOnly": false,
          "Containers": {
              "af5a5abab1de02fed5d1bee3c19ed49054587ec2efa1c42f1fae5e1adfa30de4": {
                  "Name": "proxy",
                  "EndpointID": "463928ba98a0fb740e32de3cc6c2ed0f9c2ba4c73dcda1cc306037a42093d072",
                  "MacAddress": "02:42:ac:11:00:02",
                  "IPv4Address": "172.17.0.2/16",
                  "IPv6Address": ""
              }
          },
          "Options": {
              "com.docker.network.bridge.default_bridge": "true",
              "com.docker.network.bridge.enable_icc": "true",
              "com.docker.network.bridge.enable_ip_masquerade": "true",
              "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
              "com.docker.network.bridge.name": "docker0",
              "com.docker.network.driver.mtu": "1500"
          },
          "Labels": {}
      }
  ]
  ```

  > `--network host` gains performance by skipping virtual networks but sacrifices security of container model.
  >
  > `--network none` removes `eth0` and only leaves you with localhost interface in container.

- `docker network create --driver DRIVER_NAME` create a network, spawns a new virtual network for you to attach containers.

  ```powershell
  Aditya :: learning-docker » docker network create my_app_net
  973176dc6a251d003a8b649b2c495c3d800ad4878b867a25e509fe858b9cf2f1
  ```

  ```powershell
  Aditya :: learning-docker » docker network ls
  NETWORK ID     NAME         DRIVER    SCOPE
  de2e69bf271d   bridge       bridge    local
  6c467bd0fda4   host         host      local
  973176dc6a25   my_app_net   bridge    local
  202ce568f0b7   none         null      local
  ```

  > `network driver` They are built-in or third-party extensions that give you virtual network features.

- `docker network connect` attaches a network to container.

  ```powershell
  Aditya :: System32 » docker network connect my_app_net proxy
  Aditya :: System32 » docker container inspect proxy
  [
      {
          ....,
          "NetworkSettings": {
              ....,
              "Networks": {
                  "bridge": {
                      "IPAMConfig": null,
                      "Links": null,
                      "Aliases": null,
                      "NetworkID": "4b999276351265f09f9698b36eb2b73257279ca97b461d201dabc949c196f52c",                    "EndpointID": "71c5679c7a8cdb376644ca9569a15e22a38a0d21dac01e069cc438e22fd63029",
                      "Gateway": "172.17.0.1",
                      "IPAddress": "172.17.0.2",
                      "IPPrefixLen": 16,
                      "IPv6Gateway": "",
                      "GlobalIPv6Address": "",
                      "GlobalIPv6PrefixLen": 0,
                      "MacAddress": "02:42:ac:11:00:02",
                      "DriverOpts": null
                  },
                  "my_app_net": {
                      "IPAMConfig": {},
                      "Links": null,
                      "Aliases": [
                          "af5a5abab1de"
                      ],
                      "NetworkID": "bd382ea27e51ee7563c708fbfb94183ac626eb652d3430e9db433f23dc4f0b78",                    "EndpointID": "45ad21a59a842c7b3f3a20d3e45224cd76a9203766bb4da91547e8e002d1ba61",
                      "Gateway": "172.18.0.1",
                      "IPAddress": "172.18.0.2",
                      "IPPrefixLen": 16,
                      "IPv6Gateway": "",
                      "GlobalIPv6Address": "",
                      "GlobalIPv6PrefixLen": 0,
                      "MacAddress": "02:42:ac:12:00:02",
                      "DriverOpts": {}
                 }
              }
          }
      }
  ]
  ```

- `docker network disconnect` detach a network from container.

```powershell
Aditya :: System32 » docker network disconnect my_app_net proxy
Aditya :: System32 » docker container inspect proxy
[
    {
        .....,
        "NetworkSettings": {
            ....,
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "4b999276351265f09f9698b36eb2b73257279ca97b461d201dabc949c196f52c",
                    "EndpointID": "71c5679c7a8cdb376644ca9569a15e22a38a0d21dac01e069cc438e22fd63029",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

## Docker Networks: DNS

<img src="https://raw.githubusercontent.com/aditya109/learning-docker/main/assets/diag0.svg"/>

> Static IP's and using IP's for talking to containers is an anti-pattern. Do your best to avoid it.

## Docker DNS

Docker daemon has a built-in DNS server that containers use by-default.

```powershell
Aditya :: System32 » docker network connect my_app_net proxy
Aditya :: System32 » docker network inspect my_app_net
[
    {
        "Name": "my_app_net",
        "Id": "bd382ea27e51ee7563c708fbfb94183ac626eb652d3430e9db433f23dc4f0b78",
        "Created": "2021-01-06T09:19:25.4270614Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "af5a5abab1de02fed5d1bee3c19ed49054587ec2efa1c42f1fae5e1adfa30de4": {
                "Name": "proxy",
                "EndpointID": "b8dc65eaa0d7bf351d7d3e7a61b5ab9e5a7e4baaa3391279cf2daa0118e1bdba",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

Docker defaults the hostname to the container's name, but you can also set aliases.

Let's create another container under `my_app_net` network.

```powershell
Aditya :: System32 » docker container run --publish 5001:80 --name app1 --detach --network my_app_net nginx:alpine
8e71ef45d862e3a15e10317e5568b125add2cb959644688aadf9244688495fdd
Aditya :: System32 » docker container run --publish 5002:80 --name app2 --detach --network my_app_net nginx:alpine
a6c80a468966c83f1686da5db59ae50d479afd251f67bb932f1c9ff0e1149ab2
Aditya :: System32 » docker network inspect my_app_net
[
    {
        "Name": "my_app_net",
        "Id": "bd382ea27e51ee7563c708fbfb94183ac626eb652d3430e9db433f23dc4f0b78",
        "Created": "2021-01-06T09:19:25.4270614Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "8e71ef45d862e3a15e10317e5568b125add2cb959644688aadf9244688495fdd": {
                "Name": "app1",
                "EndpointID": "9eb326edbc0954bc9cdaeffa45a94eeb9fdb001cfebba38e88c16c153a0be0ca",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            },
            "a6c80a468966c83f1686da5db59ae50d479afd251f67bb932f1c9ff0e1149ab2": {
                "Name": "app2",
                "EndpointID": "1f401641d87f82671758ae6f922a2a53b37e8b798fa2d5d1d1a4f30cd92faf66",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            },
            "c92399081ac7dd63ef330d4255e896fda423d29b9c882ed915b47aa120d621db": {
                "Name": "app3",
                "EndpointID": "128326b21568c7de832289e8ef29b9aafac8d9fe3b3cc7bd1bb0d72d8004bf44",
                "MacAddress": "02:42:ac:12:00:04",
                "IPv4Address": "172.18.0.4/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

```powershell
Aditya :: System32 » docker container run --publish 5003:80 --name app3 -it --network my_app_net nginx:alpine ping app1
PING app1 (172.18.0.2): 56 data bytes
64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.130 ms
64 bytes from 172.18.0.2: seq=1 ttl=64 time=0.082 ms
64 bytes from 172.18.0.2: seq=2 ttl=64 time=0.187 ms
64 bytes from 172.18.0.2: seq=3 ttl=64 time=0.187 ms
64 bytes from 172.18.0.2: seq=4 ttl=64 time=0.242 ms
64 bytes from 172.18.0.2: seq=5 ttl=64 time=0.184 ms
64 bytes from 172.18.0.2: seq=6 ttl=64 time=0.162 ms
64 bytes from 172.18.0.2: seq=7 ttl=64 time=0.182 ms
64 bytes from 172.18.0.2: seq=8 ttl=64 time=0.140 ms
64 bytes from 172.18.0.2: seq=9 ttl=64 time=0.183 ms
^C
--- app1 ping statistics ---
10 packets transmitted, 10 packets received, 0% packet loss
round-trip min/avg/max = 0.082/0.167/0.242 ms
```

The default network **bridge** does not have DNS built into it, however a work-around has been provided. We could `--link` for our containers; but it s better to create a new network.

**Assignment: CLI App Testing**

**Assignment: DNS Round Robin Test**

>  `docker container run --rm alpine nslookup search `automatically remove the container when it exits.



