# Table of Contents

- [Container Lifetime and Persistent Data](#container-lifetime-and-persistent-data)
  * [Introduction](#introduction)
  * [Persistent Data: Data Volumes](#persistent-data--data-volumes)
    + [Learning `—format` flag for Docker Commands](#learning---format--flag-for-docker-commands)
  * [Persistent Data: Data Mounting](#persistent-data--data-mounting)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

# Container Lifetime and Persistent Data

## Introduction 

- [ ] Containers are usually immutable and ephemeral.
- [ ] `immutable infrastructure`: only re-deploy containers, never change. (*ideal scenario*)

Databases being used by containers is called **persistent data**.

There are two ways to do that:

1. Volumes - make special location outside of container UFS
2. Bind Mounts - link container path to host path

## Persistent Data: Data Volumes

- `VOLUME` command in Dockerfile tells the containers where to look persistent data. That means, any file stored there will outlive the container until we manually delete the container.

  > `docker volume prune` cleans up unused volumes.

```powershell
Aditya :: System32 » docker pull mysql
Using default tag: latest
latest: Pulling from library/mysql
a076a628af6f: Pull complete
f6c208f3f991: Pull complete
88a9455a9165: Pull complete
406c9b8427c6: Pull complete
7c88599c0b25: Pull complete
25b5c6debdaf: Pull complete
43a5816f1617: Pull complete
69dd1fbf9190: Pull complete
5346a60dcee8: Pull complete
ef28da371fc9: Pull complete
fd04d935b852: Pull complete
050c49742ea2: Pull complete
Digest: sha256:0fd2898dc1c946b34dceaccc3b80d38b1049285c1dab70df7480de62265d6213
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest
```

```powershell
Aditya :: System32 » docker image inspect --format "{{.Config.Volumes}}" mysql
map[/var/lib/mysql:{}]
```

```powershell
Aditya :: System32 » docker container run -d -e MYSQL_ALLOW_EMPTY_PASSWORD=True --name mysql mysql
b60c6dc41d31d19d83c76bc84db520b73d080c55e8b92f037b8822b5dfd3df42
```

```powershell
Aditya :: System32 » docker container ls -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                 NAMES
b60c6dc41d31   mysql     "docker-entrypoint.s…"   26 seconds ago   Up 24 seconds   3306/tcp, 33060/tcp   mysql
```

### Learning `—format` flag for Docker Commands

```powershell
Aditya :: System32 » docker container inspect mysql
[
    {
        "Id": "b60c6dc41d31d19d83c76bc84db520b73d080c55e8b92f037b8822b5dfd3df42",
        "Created": "2021-01-13T03:00:56.7149389Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "mysqld"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 1232,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-01-13T03:00:57.1072675Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:d4c3cafb11d573699728f9e7de10d1b976089b01298c0360e03f0afd9a1a8b36",
        "ResolvConfPath": "/var/lib/docker/containers/b60c6dc41d31d19d83c76bc84db520b73d080c55e8b92f037b8822b5dfd3df42/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/b60c6dc41d31d19d83c76bc84db520b73d080c55e8b92f037b8822b5dfd3df42/hostname",
        "HostsPath": "/var/lib/docker/containers/b60c6dc41d31d19d83c76bc84db520b73d080c55e8b92f037b8822b5dfd3df42/hosts",
        "LogPath": "/var/lib/docker/containers/b60c6dc41d31d19d83c76bc84db520b73d080c55e8b92f037b8822b5dfd3df42/b60c6dc41d31d19d83c76bc84db520b73d080c55e8b92f037b8822b5dfd3df42-json.log",
        "Name": "/mysql",
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
            "PortBindings": {},
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
                80
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
                "LowerDir": "/var/lib/docker/overlay2/7739a17b4a3abcf36c19a4c1f8d8d8e6b6075140e07adc37d65c005097ff8d50-init/diff:/var/lib/docker/overlay2/65b5371984012e44613c78d315dba6683e5a55fac12a2ed8c58647b34b516ff2/diff:/var/lib/docker/overlay2/e8f3d9308b44fec6988b07305243899f9d40c75ed0bb1aed1721c2bf53c21a88/diff:/var/lib/docker/overlay2/c03f9e315d7c7f88b8faf33c6aee3a870c6591851d5b366a537bf95c7539db1e/diff:/var/lib/docker/overlay2/500d0efba9c30197217c4f0bd8726cc60f19c5e8dc989efbf962c022ef73107c/diff:/var/lib/docker/overlay2/cf9fe42c871df8d3dbaeceb2016ed194e95fd6d75ca5b8754ff8ccb0cdd91414/diff:/var/lib/docker/overlay2/8be49204530494b42fb10e64a300a40131bc886b03c711612cf2db0724abe4f0/diff:/var/lib/docker/overlay2/1dfe7d4215924c87020a780e280972d4600b9391d66bdd1426154b3a7f82df10/diff:/var/lib/docker/overlay2/b670dd989039ef747ad1f20d5ea885e236a850ff388faff1071482d5e622159e/diff:/var/lib/docker/overlay2/cc5bbb94b4f376d9dc0dbc87019e7ff07feabe1d4404479766770f541cb6fc3c/diff:/var/lib/docker/overlay2/a613f053b522c8127af48282842f2344b6c29a36bf3266c793c62d561bd4a310/diff:/var/lib/docker/overlay2/07ca0c4398e0dcc9cd89a00e793a27d82ab287bf6f5565116a039d17582a61ac/diff:/var/lib/docker/overlay2/f929a8103cfeb902de909d670f368b4470cbfed31c66d39327dd2c977e37b2b5/diff",
                "MergedDir": "/var/lib/docker/overlay2/7739a17b4a3abcf36c19a4c1f8d8d8e6b6075140e07adc37d65c005097ff8d50/merged",
                "UpperDir": "/var/lib/docker/overlay2/7739a17b4a3abcf36c19a4c1f8d8d8e6b6075140e07adc37d65c005097ff8d50/diff",
                "WorkDir": "/var/lib/docker/overlay2/7739a17b4a3abcf36c19a4c1f8d8d8e6b6075140e07adc37d65c005097ff8d50/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [
            {
                "Type": "volume",
                "Name": "a5e73fe93fcdff1f0a48bf4323c868ceea464d4d09d950848a3c0348c9b946f3",
                "Source": "/var/lib/docker/volumes/a5e73fe93fcdff1f0a48bf4323c868ceea464d4d09d950848a3c0348c9b946f3/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
        "Config": {
            "Hostname": "b60c6dc41d31",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "3306/tcp": {},
                "33060/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "MYSQL_ALLOW_EMPTY_PASSWORD=True",
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "MYSQL_MAJOR=8.0",
                "MYSQL_VERSION=8.0.22-1debian10"
            ],
            "Cmd": [
                "mysqld"
            ],
            "Image": "mysql",
            "Volumes": {
                "/var/lib/mysql": {}
            },
            "WorkingDir": "",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "d1f01443b7f899efd8c74dc88b1908fb654161340bdde0b9812343146d2aad8f",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "3306/tcp": null,
                "33060/tcp": null
            },
            "SandboxKey": "/var/run/docker/netns/d1f01443b7f8",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "7735456ed676b0c36d6ffb1da61407ee1296158b270bd1af84b2617db4515ffe",
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
                    "NetworkID": "ce4d17b58facf86bcd7249db2f5d3c207798d6e3764abaf94ea946430869c592",
                    "EndpointID": "7735456ed676b0c36d6ffb1da61407ee1296158b270bd1af84b2617db4515ffe",
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

```powershell
Aditya :: System32 » docker container inspect --format "{{json .Config.Volumes}}" mysql
{"/var/lib/mysql":{}}
```

```powershell
Aditya :: System32 » docker container inspect --format "{{json .Mounts}}" mysql
[{"Type":"volume","Name":"a5e73fe93fcdff1f0a48bf4323c868ceea464d4d09d950848a3c0348c9b946f3","Source":"/var/lib/docker/volumes/a5e73fe93fcdff1f0a48bf4323c868ceea464d4d09d950848a3c0348c9b946f3/_data","Destination":"/var/lib/mysql","Driver":"local","Mode":"","RW":true,"Propagation":""}]
```

```powershell
Aditya :: System32 » docker volume inspect a5e73fe93fcdff1f0a48bf4323c868ceea464d4d09d950848a3c0348c9b946f3
[
    {
        "CreatedAt": "2021-01-13T03:01:11Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/a5e73fe93fcdff1f0a48bf4323c868ceea464d4d09d950848a3c0348c9b946f3/_data",
        "Name": "a5e73fe93fcdff1f0a48bf4323c868ceea464d4d09d950848a3c0348c9b946f3",
        "Options": null,
        "Scope": "local"
    }
]
```

Also there is no way to identify the data volumes in docker.

```powershell
Aditya :: System32 » docker volume ls
DRIVER    VOLUME NAME
local     a5e73fe93fcdff1f0a48bf4323c868ceea464d4d09d950848a3c0348c9b946f3
```

Also on removing the containers the data volumes persist. Data outlives the container.

```powershell
Aditya :: System32 » docker container rm -f mysql
mysql
Aditya :: System32 » docker volume ls
DRIVER    VOLUME NAME
local     a5e73fe93fcdff1f0a48bf4323c868ceea464d4d09d950848a3c0348c9b946f3
Aditya :: System32 » docker container ls -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAME
```

To identity the data volumes, we can use`-v` to create **named volumes**.

```
Aditya :: System32 » docker container run -d -e MYSQL_ALLOW_EMPTY_PASSWORD=True --name mysql -v mysql-db:/var/lib/mysql mysql
32ad25ee6a81eed86b33016a164e5c3e36341799c3d4cc89aadc8a38fbe2f750
Aditya :: System32 » docker volume ls
DRIVER    VOLUME NAME
local     mysql-db
```

```powershell
Aditya :: System32 » docker container inspect mysql --format "{{json .Mounts}}"
[{"Type":"volume","Name":"mysql-db","Source":"/var/lib/docker/volumes/mysql-db/_data","Destination":"/var/lib/mysql","Driver":"local","Mode":"z","RW":true,"Propagation":""}]
```

`docker volume create` creates a volume. Advantage here is we can provide options  like:`–driver string` to specify a volume driver and `–opt map`  to set driver specific options.

> **Here's the important part. Each shell may do this differently,** so here's a cheat sheet for which OS and Shell your using. I'll be using `$(pwd)`on a Mac, **but yours may be different!**
>
> This isn't a Docker thing, it's a Shell thing.
>
> For PowerShell use: `${pwd}` 
>
> For cmd.exe "Command Prompt use: `%cd%`
>
> Linux/macOS bash, sh, zsh, and Windows Docker Toolbox Quickstart Terminal use: `$(pwd)` 
>
> Note, if you have spaces in your path, you'll usually need to quote the whole path in the docker command.

## Persistent Data: Data Mounting

- Maps a host file or directory to a container file or directory.
- Basically just two locations pointing to the same file(s).
- This also skips UFS, and hosts files overwrite any in container.
- We cannot use in Dockerfile, must be at `container run`.
  - `docker container run -v /Users/bret/stuff:/path/container` (Mac/Linux)
  - `docker container run -v //c/Users/bret/stuff:/path/container` (Windows)

```powershell
Aditya :: System32 » docker container run -d --name nginx -p 81:80 -v ${pwd}:/usr/share/nginx/html nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
a076a628af6f: Already exists
0732ab25fa22: Pull complete
d7f36f6fe38f: Pull complete
f72584a26f32: Pull complete
7125e4df9063: Pull complete
Digest: sha256:10b8cc432d56da8b61b070f4c7d2543a9ed17c2b23010b43af434fd40e2ca4aa
Status: Downloaded newer image for nginx:latest
25bba25e86d2997f6716599ceefe7867e8787be14d4b7a7d7d96cb9ba65398d6
```

**Assignment: Named Volumes**

**Assignment: Bind Mounts**

















































