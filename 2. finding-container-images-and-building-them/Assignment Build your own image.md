# Assignment: Build your own image

```
# use this empty Dockerfile to build your assignment

# This dir contains a Node.js app, you need to get it running in a container
# No modifications to the app should be necessary, only edit this Dockerfile

# Overview of this assignment
# use the instructions from developer below to create a working Dockerfile
# feel free to add command inline below or use a new file, up to you (but must be named Dockerfile)
# once Dockerfile builds correctly, start container locally to make sure it works on http://localhost
# then ensure image is named properly for your Docker Hub account with a new repo name
# push to Docker Hub, then go to https://hub.docker.com and verify
# then remove local image from cache
# then start a new container from your Hub image, and watch how it auto downloads and runs
# test again that it works at http://localhost


# Instructions from the app developer
# - you should use the 'node' official image, with the alpine 6.x branch (node:6-alpine)
  #  yes this is a 2-year old image of node, but all official images are always
  #  available on Docker Hub forever, to ensure even old apps still work.
  #  It is common to still need to deploy old app versions, even years later.
# - this app listens on port 3000, but the container should launch on port 80
  #  so it will respond to http://localhost:80 on your computer
# - then it should use alpine package manager to install tini: 'apk add --update tini'
# - then it should create directory /usr/src/app for app files with 'mkdir -p /usr/src/app'
# - Node uses a "package manager", so it needs to copy in package.json file
# - then it needs to run 'npm install' to install dependencies from that file
# - to keep it clean and small, run 'npm cache clean --force' after above
# - then it needs to copy in all files from current directory
# - then it needs to start container with command '/sbin/tini -- node ./bin/www'
# - in the end you should be using FROM, RUN, WORKDIR, COPY, EXPOSE, and CMD commands

# Bonus Extra Credit
# this will not have you setting up a complete image useful for local development, test, and prod
# it's just meant to get you started with basic Dockerfile concepts and not focus too much on
# proper Node.js use in a container. **If you happen to be a Node.js Developer**, then 
# after you get through more of this course, you should come back and use my 
# Node Docker Good Defaults sample project on GitHub to change this Dockerfile for 
# better local development with more advanced topics
# https://github.com/BretFisher/node-docker-good-defaults

```

```dockerfile
FROM node:10.23.1-alpine3.9

EXPOSE 3000

RUN apk add --update tini 

WORKDIR /usr/src/app

COPY package.json .

RUN npm install \
    && npm cache clean --force

COPY . .

CMD ["/sbin/tini", "--", "node", "./bin/www"]
```

```powershell
Aditya :: dockerfile-assignment-1 » docker build -t nodeapp .
[+] Building 18.1s (12/12) FINISHED
 => [internal] load build definition from Dockerfile                                             0.0s
 => => transferring dockerfile: 274B                                                             0.0s
 => [internal] load .dockerignore                                                                0.1s
 => => transferring context: 455B                                                                0.0s
 => [internal] load metadata for docker.io/library/node:10.23.1-alpine3.9                        5.1s
 => [auth] library/node:pull token for registry-1.docker.io                                      0.0s
 => [internal] load build context                                                                0.4s
 => => transferring context: 445.15kB                                                            0.3s
 => [1/6] FROM docker.io/library/node:10.23.1-alpine3.9@sha256:88218791bfbc260e6208398af20d358c  6.1s
 => => resolve docker.io/library/node:10.23.1-alpine3.9@sha256:88218791bfbc260e6208398af20d358c  0.0s
 => => sha256:88218791bfbc260e6208398af20d358c013d0e7ad5dfca6df3667fe69b69cbab 1.65kB / 1.65kB   0.0s
 => => sha256:8da316ef5956ba6c0bb8897602c8f4dc370fe5463ccb16d16a763de567dc49e1 1.16kB / 1.16kB   0.0s
 => => sha256:ba44e68678c7d4236730cfe4d83be7f47e033cc2ab4706c325127188313e4941 6.73kB / 6.73kB   0.0s
 => => sha256:31603596830fc7e56753139f9c2c6bd3759e48a850659506ebfb885d1cf3aef5 2.77MB / 2.77MB   0.8s
 => => sha256:77468f7fefaf90aa586bd6a3dc49c12954d97a655ba8489b2287d6d61f2790e 22.10MB / 22.10MB  4.4s
 => => sha256:925152b1612474343baee4cc2f1382bf9f4b318df93979a6c4f516f607fdbcd4 2.34MB / 2.34MB   1.6s
 => => extracting sha256:31603596830fc7e56753139f9c2c6bd3759e48a850659506ebfb885d1cf3aef5        0.1s
 => => sha256:2a9a1b59946b6d578e52e1a995c1610292497c131aedf6e8110952a6c080e554 282B / 282B       1.6s
 => => extracting sha256:77468f7fefaf90aa586bd6a3dc49c12954d97a655ba8489b2287d6d61f2790ea        1.2s
 => => extracting sha256:925152b1612474343baee4cc2f1382bf9f4b318df93979a6c4f516f607fdbcd4        0.1s
 => => extracting sha256:2a9a1b59946b6d578e52e1a995c1610292497c131aedf6e8110952a6c080e554        0.0s
 => [2/6] RUN apk add --update tini                                                              2.1s
 => [3/6] WORKDIR /usr/src/app                                                                   0.1s
 => [4/6] COPY package.json .                                                                    0.1s
 => [5/6] RUN npm install     && npm cache clean --force                                         4.2s
 => [6/6] COPY . .                                                                               0.1s
 => exporting to image                                                                           0.2s
 => => exporting layers                                                                          0.2s
 => => writing image sha256:c4b927abe2518307950cc26c89bf8e4012f57c566fa22d029c1b211aa02fc828     0.0s
 => => naming to docker.io/library/nodeapp                                                       0.0s
```

```powershell
Aditya :: dockerfile-assignment-1 » docker container run -d --publish 81:3000 nodeapp
47861b49fe00cc1c851c2d93c9c4fd0944a33aae1da8ffb4318dce450de1daa7
```

```powershell
Aditya :: dockerfile-assignment-1 » curl http://localhost:81


StatusCode        : 200
StatusDescription : OK
Content           : <!DOCTYPE html>
                    <html>
                      <head>
                        <title>Node.js Express App</title>
                        <link rel='stylesheet' href='/stylesheets/style.css' />
                      </head>
                      <body>
                        <h1>Node.js Express App</h1>
                    <p>It Wor...
RawContent        : HTTP/1.1 200 OK
                    Connection: keep-alive
                    Content-Length: 304
                    Content-Type: text/html; charset=utf-8
                    Date: Mon, 11 Jan 2021 13:42:46 GMT
                    ETag: W/"130-YVfXhlVkpFZ3PcW5EOh+G9mPJ8g"
                    X-Powered-By: Expr...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 304], [Content-Type, text/html;
                    charset=utf-8], [Date, Mon, 11 Jan 2021 13:42:46 GMT]...}
Images            : {@{innerHTML=; innerText=; outerHTML=<IMG src="/images/picard.gif">; outerText=;
                    tagName=IMG; src=/images/picard.gif}}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 304

```

