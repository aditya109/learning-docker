# Assignment: DNS Round Robin Test

- [x] Create a new virtual network (default **bridge** driver).
- [x] Create two containers from `elasticsearch:2` image.
- [x] Research and use `--network-alias search` when creating them to give them an additional DNS name to respond to.
- [x] Run `alpine nslookup search` with `--net` to see the two containers list for the same DNS name.
- [x] Run `centos curl -s search:9200` with `--net` multiple times until you see both "name" fields show.

## Solution

First, let's create a network `dnsrr`:

```powershell
Aditya :: System32 » docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
3388308e5a37   bridge    bridge    local
6c467bd0fda4   host      host      local
202ce568f0b7   none      null      local
Aditya :: System32 » docker network create dnsrr
ab0bf342a3d65f07918fb7a131390c308ede3dad2c90092a5e71ff257c0fc9c7
Aditya :: System32 » docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
3388308e5a37   bridge    bridge    local
ab0bf342a3d6   dnsrr     bridge    local
6c467bd0fda4   host      host      local
202ce568f0b7   none      null      local
```
Next creating two containers `elasticsearch:2` with their `network alias` as `search`.

```powershell
Aditya :: System32 » docker container run -d --net dnsrr --net-alias search elasticsearch:2
Unable to find image 'elasticsearch:2' locally
2: Pulling from library/elasticsearch
05d1a5232b46: Pull complete
5cee356eda6b: Pull complete
89d3385f0fd3: Pull complete
65dd87f6620b: Pull complete
78a183a01190: Pull complete
1a4499c85f97: Pull complete
2c9d39b4bfc1: Pull complete
1b1cec2222c9: Pull complete
59ff4ce9df68: Pull complete
1976bc3ee432: Pull complete
a27899b7a5b5: Pull complete
b0fc7d2c927a: Pull complete
6d94b96bbcd0: Pull complete
6f5bf40725fd: Pull complete
2bf2a528ae9a: Pull complete
Digest: sha256:41ed3a1a16b63de740767944d5405843db00e55058626c22838f23b413aa4a39
Status: Downloaded newer image for elasticsearch:2
6f7484ce5670534ca3ce38397ce0845be5892292cfa1ba6f554732ccbf6f086e
Aditya :: System32 » docker container run -d --net dnsrr --net-alias search elasticsearch:2
893129bfef5b543219a2c1b97ea2157b49f42a3830b7b3bab5c95896fd9a4d99
Aditya :: System32 » docker container ls -a
CONTAINER ID   IMAGE             COMMAND                  CREATED              STATUS              PORTS                NAMES
893129bfef5b   elasticsearch:2   "/docker-entrypoint.…"   About a minute ago   Up About a minute   9200/tcp, 9300/tcp   boring_varahamihira
6f7484ce5670   elasticsearch:2   "/docker-entrypoint.…"   2 minutes ago        Up 2 minutes        9200/tcp, 9300/tcp   gallant_jackson
```
Now performing `nslookup` using `alpine` containers; followed by multiple `curl`s in `centos` container, we can see that the round robin set up re-routes us to different containers on each hit. 

```powershell
Aditya :: System32 » docker container run --rm --net dnsrr alpine nslookup search
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
801bfaa63ef2: Already exists
Digest: sha256:3c7497bf0c7af93428242d6176e8f7905f2201d8fc5861f45be7a346b5f23436
Status: Downloaded newer image for alpine:latest
Address:        127.0.0.11:53

Non-authoritative answer:
Non-authoritative answer:
Name:   search
Address: 172.18.0.3
Name:   search
Address: 172.18.0.2
```

```powershell

Aditya :: System32 » docker container run --rm --net dnsrr centos curl -s search:9200
Unable to find image 'centos:latest' locally
latest: Pulling from library/centos
7a0437f04f83: Pull complete
Status: Downloaded newer image for centos:latest
{
  "name" : "Man-Brute",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "9aKzzX1FT_e2xq_D1iEmNQ",
  "version" : {
    "number" : "2.4.6",
    "build_hash" : "5376dca9f70f3abef96a77f4bb22720ace8240fd",
    "build_timestamp" : "2017-07-18T12:17:44Z",
    "build_snapshot" : false,
    "lucene_version" : "5.5.4"
  },
  "tagline" : "You Know, for Search"
}
Aditya :: System32 » docker container run --rm --net dnsrr centos curl -s search:9200
{
  "name" : "Vindicator",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "DIl5QDrFRcmXMbdmqB8HWw",
  "version" : {
    "number" : "2.4.6",
    "build_hash" : "5376dca9f70f3abef96a77f4bb22720ace8240fd",
    "build_timestamp" : "2017-07-18T12:17:44Z",
    "build_snapshot" : false,
    "lucene_version" : "5.5.4"
  },
  "tagline" : "You Know, for Search"
}
Aditya :: System32 » docker container run --rm --net dnsrr centos curl -s search:9200
{
  "name" : "Man-Brute",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "9aKzzX1FT_e2xq_D1iEmNQ",
  "version" : {
    "number" : "2.4.6",
    "build_hash" : "5376dca9f70f3abef96a77f4bb22720ace8240fd",
    "build_timestamp" : "2017-07-18T12:17:44Z",
    "build_snapshot" : false,
    "lucene_version" : "5.5.4"
  },
  "tagline" : "You Know, for Search"
}
Aditya :: System32 » docker container run --rm --net dnsrr centos curl -s search:9200
{
  "name" : "Vindicator",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "DIl5QDrFRcmXMbdmqB8HWw",
  "version" : {
    "number" : "2.4.6",
    "build_hash" : "5376dca9f70f3abef96a77f4bb22720ace8240fd",
    "build_timestamp" : "2017-07-18T12:17:44Z",
    "build_snapshot" : false,
    "lucene_version" : "5.5.4"
  },
  "tagline" : "You Know, for Search"
}
Aditya :: System32 » docker container run --rm --net dnsrr centos curl -s search:9200
{
  "name" : "Man-Brute",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "9aKzzX1FT_e2xq_D1iEmNQ",
  "version" : {
    "number" : "2.4.6",
    "build_hash" : "5376dca9f70f3abef96a77f4bb22720ace8240fd",
    "build_timestamp" : "2017-07-18T12:17:44Z",
    "build_snapshot" : false,
    "lucene_version" : "5.5.4"
  },
  "tagline" : "You Know, for Search"
}
```

