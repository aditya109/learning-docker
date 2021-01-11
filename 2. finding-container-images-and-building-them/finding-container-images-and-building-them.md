# Table of Contents

- [Docker Hub Registry](#docker-hub-registry)
- [Images and Their Layers: Image Cache](#images-and-their-layers--image-cache)
- [Image Tagging and Pushing to Docker Hub](#image-tagging-and-pushing-to-docker-hub)
- [Dockerfile](#dockerfile)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

# What is an Image?

An Image is a compilation of an application binaries and dependencies and metadata about the image data and how to run the image.

> An Image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime.

> Not a complete OS. No kernel, kernel module (e.g., drivers)

# Images and Their Layers: Image Cache

`docker image history` shows the history of an image.

```powershell
Aditya :: System32 » docker image history alpine
IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
389fef711851   3 weeks ago   /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B
<missing>      3 weeks ago   /bin/sh -c #(nop) ADD file:ec475c2abb2d46435…   5.58MB
Aditya :: System32 » docker image history centos
IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
300e315adb2f   4 weeks ago   /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B
<missing>      4 weeks ago   /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B
<missing>      4 weeks ago   /bin/sh -c #(nop) ADD file:bd7a2aed6ede423b7…   209MB
```

`docker image inspect` displays detailed information on one or more images.

```powershell
Aditya :: System32 » docker image inspect alpine
[
    {
        "Id": "sha256:389fef7118515c70fd6c0e0d50bb75669942ea722ccb976507d7b087e54d5a23",
        "RepoTags": [
            "alpine:latest"
        ],
        "RepoDigests": [
            "alpine@sha256:3c7497bf0c7af93428242d6176e8f7905f2201d8fc5861f45be7a346b5f23436"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2020-12-17T00:19:42.11518025Z",
        "Container": "6cb7658787e77979d777b9725147ac992230ccc7426e678c918c3144e5ccb78e",
        "ContainerConfig": {
            "Hostname": "6cb7658787e7",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"/bin/sh\"]"
            ],
            "Image": "sha256:174c398109132a9b2fc879f752eeef1b25528a04b514a1e3ff1b5085bcf28f51",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "19.03.12",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh"
            ],
            "Image": "sha256:174c398109132a9b2fc879f752eeef1b25528a04b514a1e3ff1b5085bcf28f51",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 5577287,
        "VirtualSize": 5577287,
        "GraphDriver": {
            "Data": {
                "MergedDir": "/var/lib/docker/overlay2/cc1468eb925342627363ace0c3f6513751214cdf63c852d40bd1d9d70a47f8c4/merged",
                "UpperDir": "/var/lib/docker/overlay2/cc1468eb925342627363ace0c3f6513751214cdf63c852d40bd1d9d70a47f8c4/diff",
                "WorkDir": "/var/lib/docker/overlay2/cc1468eb925342627363ace0c3f6513751214cdf63c852d40bd1d9d70a47f8c4/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:777b2c648970480f50f5b4d0af8f9a8ea798eea43dbcf40ce4a8c7118736bdcf"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
```

- Images are made up of file system changes and metadata.
- Each layer is uniquely identified and only stored once on a host.
- This saves storage space on host and transfer time on push/pull.
- A container is just a single read/write layer on top of image.

# Image Tagging and Pushing to Docker Hub

`docker image tag` assigns one or more tags to an image.
The images have three information pieces:

```
[org_name]/[img_name]:[tag_name]
```

```powershell
docker image tag nginx daitya96/nginx:v1.0
```

```powershell
docker login
docker image push daitya96/nginx:v1.0
docker logout
```

# Dockerfile

`FROM` sets base distribution for the image.

`ENV` sets environment-variables.

`RUN` executes shell commands and shell scripts.

`EXPOSE` exposes specified ports the virtual docker network.

`CMD` runs this command when the container is launched.

```powershell
docker image build -t [img_name] .
```

**Assignment: Build your own image**

`docker image prune` to clean up just "dangling" images

`docker system prune` will clean up everything

`docker image prune -a` will remove all images you're not using

`docker system df` to see space usage

 







































