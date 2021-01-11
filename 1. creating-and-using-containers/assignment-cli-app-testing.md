# Assignment: CLI App Testing

- [x] Use different Linux distro containers to check `curl` cli tool version
- [x] Use two different terminal windows to start bash in both `centos:7` and `ubuntu:14:04`, using `-it`.
- [x] Learn the `docker container rm` option so you can save cleanup.
- [x] Ensure curl is installed and on latest version for that distro
  - `ubuntu`: `apt-get update && apt-get install curl`.
  - `centos`: `yum update curl`.
- [x] Check `curl --version`.

### Solution:

---

For `centos:7`:

```powershell
Aditya :: System32 » docker container run -it centos:7 bash
Unable to find image 'centos:7' locally
7: Pulling from library/centos
2d473b07cdd5: Pull complete
Digest: sha256:0f4ec88e21daf75124b8a9e5ca03c37a5e937e0e108a255d890492430789b60e
Status: Downloaded newer image for centos:7
[root@b0e8861ce99f /]# curl https://reqres.in/api/users
{"page":1,"per_page":6,"total":12,"total_pages":2,"data":[{"id":1,"email":"george.bluth@reqres.in","first_name":"George","last_name":"Bluth","avatar":"https://reqres.in/img/faces/1-image.jpg"},{"id":2,"email":"janet.weaver@reqres.in","first_name":"Janet","last_name":"Weaver","avatar":"https://reqres.in/img/faces/2-image.jpg"},{"id":3,"email":"emma.wong@reqres.in","first_name":"Emma","last_name":"Wong","avatar":"https://reqres.in/img/faces/3-image.jpg"},{"id":4,"email":"eve.holt@reqres.in","first_name":"Eve","last_name":"Holt","avatar":"https://reqres.in/img/faces/4-image.jpg"},{"id":5,"email":"charles.morris@reqres.in","first_name":"Charles","last_name":"Morris","avatar":"https://reqres.in/img/faces/5-image.jpg"},{"id":6,"email":"tracey.ramos@reqres.in","first_name":"Tracey","last_name":"Ramos","avatar":"https://reqres.in/img/faces/6-image.jpg"}],"support":{"url":"https://reqres.in/#support-heading","text":"To keep ReqRes free, contributions towards server costs are appreciated!"}}
[root@b0e8861ce99f /]# curl --version
curl 7.29.0 (x86_64-redhat-linux-gnu) libcurl/7.29.0 NSS/3.44 zlib/1.2.7 libidn/1.28 libssh2/1.8.0
Protocols: dict file ftp ftps gopher http https imap imaps ldap ldaps pop3 pop3s rtsp scp sftp smtp smtps telnet tftp
Features: AsynchDNS GSS-Negotiate IDN IPv6 Largefile NTLM NTLM_WB SSL libz unix-sockets
```

For `ubuntu:14.04`:

```powershell
Aditya :: System32 » docker container run -it ubuntu:14.04 bash
Unable to find image 'ubuntu:14.04' locally
14.04: Pulling from library/ubuntu
2e6e20c8e2e6: Pull complete
95201152d9ff: Pull complete
5f63a3b65493: Pull complete
Digest: sha256:63fce984528cec8714c365919882f8fb64c8a3edf23fdfa0b218a2756125456f
Status: Downloaded newer image for ubuntu:14.04
root@b55e70c90b6e:/# curl 8.8.8.8
bash: curl: command not found
root@b55e70c90b6e:/# apt-get update && apt-get install curl
Get:1 http://security.ubuntu.com trusty-security InRelease [65.9 kB]
Ign http://archive.ubuntu.com trusty InRelease
Get:2 http://archive.ubuntu.com trusty-updates InRelease [65.9 kB]
Get:3 https://esm.ubuntu.com trusty-infra-security InRelease
Get:4 http://security.ubuntu.com trusty-security/main amd64 Packages [1032 kB]
Get:5 http://archive.ubuntu.com trusty-backports InRelease [65.9 kB]
Get:6 https://esm.ubuntu.com trusty-infra-updates InRelease
Get:7 https://esm.ubuntu.com trusty-infra-security/main amd64 Packages
Hit http://archive.ubuntu.com trusty Release.gpg
Get:8 http://archive.ubuntu.com trusty-updates/main amd64 Packages [1460 kB]
Get:9 https://esm.ubuntu.com trusty-infra-updates/main amd64 Packages
Get:10 http://security.ubuntu.com trusty-security/restricted amd64 Packages [18.1 kB]
Get:11 http://security.ubuntu.com trusty-security/universe amd64 Packages [378 kB]
Get:12 http://security.ubuntu.com trusty-security/multiverse amd64 Packages [4730 B]
Get:13 http://archive.ubuntu.com trusty-updates/restricted amd64 Packages [21.4 kB]
Get:14 http://archive.ubuntu.com trusty-updates/universe amd64 Packages [671 kB]
Get:15 http://archive.ubuntu.com trusty-updates/multiverse amd64 Packages [16.1 kB]
Get:16 http://archive.ubuntu.com trusty-backports/main amd64 Packages [14.7 kB]
Get:17 http://archive.ubuntu.com trusty-backports/restricted amd64 Packages [40 B]
Get:18 http://archive.ubuntu.com trusty-backports/universe amd64 Packages [52.5 kB]
Get:19 http://archive.ubuntu.com trusty-backports/multiverse amd64 Packages [1392 B]
Hit http://archive.ubuntu.com trusty Release
Get:20 http://archive.ubuntu.com trusty/main amd64 Packages [1743 kB]
Get:21 http://archive.ubuntu.com trusty/restricted amd64 Packages [16.0 kB]
Get:22 http://archive.ubuntu.com trusty/universe amd64 Packages [7589 kB]
Get:23 http://archive.ubuntu.com trusty/multiverse amd64 Packages [169 kB]
Fetched 13.8 MB in 36s (384 kB/s)
Reading package lists... Done
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following extra packages will be installed:
  libcurl3
The following NEW packages will be installed:
  curl libcurl3
0 upgraded, 2 newly installed, 0 to remove and 1 not upgraded.
Need to get 297 kB of archives.
After this operation, 878 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://archive.ubuntu.com/ubuntu/ trusty-updates/main libcurl3 amd64 7.35.0-1ubuntu2.20 [173 kB]
Get:2 http://archive.ubuntu.com/ubuntu/ trusty-updates/main curl amd64 7.35.0-1ubuntu2.20 [123 kB]
Fetched 297 kB in 1s (159 kB/s)
Selecting previously unselected package libcurl3:amd64.
(Reading database ... 12097 files and directories currently installed.)
Preparing to unpack .../libcurl3_7.35.0-1ubuntu2.20_amd64.deb ...
Unpacking libcurl3:amd64 (7.35.0-1ubuntu2.20) ...
Selecting previously unselected package curl.
Preparing to unpack .../curl_7.35.0-1ubuntu2.20_amd64.deb ...
Unpacking curl (7.35.0-1ubuntu2.20) ...
Setting up libcurl3:amd64 (7.35.0-1ubuntu2.20) ...
Setting up curl (7.35.0-1ubuntu2.20) ...
Processing triggers for libc-bin (2.19-0ubuntu6.15) ...
root@b55e70c90b6e:/# curl https://reqres.in/api/users
{"page":1,"per_page":6,"total":12,"total_pages":2,"data":[{"id":1,"email":"george.bluth@reqres.in","first_name":"George","last_name":"Bluth","avatar":"https://reqres.in/img/faces/1-image.jpg"},{"id":2,"email":"janet.weaver@reqres.in","first_name":"Janet","last_name":"Weaver","avatar":"https://reqres.in/img/faces/2-image.jpg"},{"id":3,"email":"emma.wong@reqres.in","first_name":"Emma","last_name":"Wong","avatar":"https://reqres.in/img/faces/3-image.jpg"},{"id":4,"email":"eve.holt@reqres.in","first_name":"Eve","last_name":"Holt","avatar":"https://reqres.in/img/faces/4-image.jpg"},{"id":5,"email":"charles.morris@reqres.in","first_name":"Charles","last_name":"Morris","avatar":"https://reqres.in/img/faces/5-image.jpg"},{"id":6,"email":"tracey.ramos@reqres.in","first_name":"Tracey","last_name":"Ramos","avatar":"https://reqres.in/img/faces/6-image.jpg"}],"support":{"url":"https://reqres.in/#support-heading","text":"To keep ReqRes free, contributions towards server costs are appreciated!"}}
root@b55e70c90b6e:/# curl --version
curl 7.35.0 (x86_64-pc-linux-gnu) libcurl/7.35.0 OpenSSL/1.0.1f zlib/1.2.8 libidn/1.28 librtmp/2.3
Protocols: dict file ftp ftps gopher http https imap imaps ldap ldaps pop3 pop3s rtmp rtsp smtp smtps telnet tftp
Features: AsynchDNS GSS-Negotiate IDN IPv6 Largefile NTLM NTLM_WB SSL libz TLS-SRP

```