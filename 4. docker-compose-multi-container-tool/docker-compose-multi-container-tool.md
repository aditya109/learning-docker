# Table of Contents

- [Docker Compose: Multi Container tool](#docker-compose--multi-container-tool)
  * [Why use `docker-compose` ?](#why-use--docker-compose---)
  * [`docker-compose.yml`](#-docker-composeyml-)
  * [Building Images using `docker-compose.yml`](#building-images-using--docker-composeyml-)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

# Docker Compose: Multi Container tool

## Why use `docker-compose` ?

- configure relationships between containers
- save our docker container run settings in easy-to-read file
- create one-liner developer environment startups

## `docker-compose.yml`

- Compose YAML format has its' own versions: 1, 2, 2.1, 3, 3.1
- YAML file can be used with `docker-compose` command for local docker automation, or;
- with `docker` directly in production with Swarm.
  - `docker-compose.yml` is default filename, but any can be used with `docker-compose -f`.

`template.yml`

```yaml
version: '3.1'  # if no version is specified then v1 is assumed. Recommend v2 minimum

services:  # containers. same as docker run
  servicename: # a friendly name. this is also DNS name inside network
    image: # Optional if you use build:
    command: # Optional, replace the default CMD specified by the image
    environment: # Optional, same as -e in docker run
    volumes: # Optional, same as -v in docker run
  servicename2:

volumes: # Optional, same as docker volume create

networks: # Optional, same as docker network create

```

## Building Images using `docker-compose.yml`

```yaml
# docker-compose.yml
version: '3.9'

services:
  proxy:
    build:
      context: .
      dockerfile: nginx.Dockerfile
    image: nginx-custom
    ports:
      - '81:80'
  web:
    image: httpd
    volumes:
      - ./html:/usr/local/apache2/htdocs
```

```powershell
Aditya :: example » docker-compose up
Creating network "example_default" with the default driver
Building proxy
f2aa67a397c4: Extracting [==================================================>f2aa67a397c4: Pull complete
3c091c23e29d: Extracting [==================================================>3c091c23e29d: Pull complete
4a99993b8636: Pull complete
Digest: sha256:b1d09e9718890e6ebbbd2bc319ef1611559e30ce1b6f56b2e3b479d9da51dc35
Status: Downloaded newer image for nginx:1.13
 ---> ae513a47849c
Step 2/2 : COPY nginx.conf /etc/nginx/conf.d/default.conf
 ---> 8ed867c8e86a

Successfully built 8ed867c8e86a
Successfully tagged nginx-custom:latest
WARNING: Image for service proxy was built because it did not already exist. 
a076a628af6f: Downloading [====================>
a076a628af6f: Extracting [==================================================>a076a628af6f: Pull complete
e444656f7792: Extracting [==================================================>e444656f7792: Extracting [==================================================>0ec35e191b09: Pull complete
4aad5d8db1a6: Extracting [==================================================>4aad5d8db1a6: Pull complete
eb1da3ea630f: Pull complete
Digest: sha256:2fab99fb3b1c7ddfa99d7dc55de8dad0a62dbe3e7c605d78ecbdf2c6c49fd636
Status: Downloaded newer image for httpd:latest
Creating example_proxy_1 ... done
Creating example_web_1   ... done
Attaching to example_proxy_1, example_web_1
web_1    | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.18.0.3. Set the 'ServerName' directive globally to suppress this message
web_1    | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.18.0.3. Set the 'ServerName' directive globally to suppress this message
web_1    | [Fri Jan 15 03:05:28.304615 2021] [mpm_event:notice] [pid 1:tid 140203842864256] AH00489: Apache/2.4.46 (Unix) configured -- resuming normal operations
web_1    | [Fri Jan 15 03:05:28.304749 2021] [core:notice] [pid 1:tid 140203842864256] AH00094: Command line: 'httpd -D FOREGROUND'
```

```powershell
Aditya :: example » docker-compose up -d
Starting example_web_1   ... done
Starting example_proxy_1 ... done
```

```powershell
Aditya :: example » docker-compose down --rmi local
Stopping example_web_1   ... done
Stopping example_proxy_1 ... done
Removing example_web_1   ... done
Removing example_proxy_1 ... done
Removing network example_default
```

