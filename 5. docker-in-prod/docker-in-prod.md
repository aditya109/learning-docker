# Docker in Production

## Limit your Simultaneous Innovation

- Many initial container project are too big in scope
- Solutions you maybe don't need on Day 1:
  - Fully automatic CI/CD
  - Dynamic performance scaling 
  - Containerizing all or nothing
  - Starting with persistent data

## Legacy apps work in Containers too

- Microservice conversion isn't required.
- 12 Factor is a horizon we're always chasing.
- Don't let these ideals delay containerization.

## Focus: Dockerfiles

- More important than fancy orchestration; it's your new build and environment documentation.
- Study Dockerfile/ENTRYPOINT of Hub Officials

## Docker Maturity Model

- Make it start
- Make it log all thing to stdout/stderr
- Make it documented in file
- Make it work for others
- Make it lean 
- Make it scale

### Dockerfile Anti-Pattern: Trapping Data

- Problem: Storing unique data in container

- Solution: Define `VOLUME` for each location

  ```dockerfile
  VOLUME /var/lib/mysql
  
  ENTRYPOINT ["docker-entrypoint.sh"]
  
  CMD ["mysqld"]
  ```

### Dockerfile Anti-Pattern: Using Latest

- Latest = Image builds will be o((⊙﹏⊙))o.

- Problem: Image builds pull FROM latest

- Solution: Using specific FROM tags

- Problem: Image builds install latest packages

- Solution: Specify version for critical `apt`/`yum`/`apk` packages.

  ```dockerfile
  FROM php:7.0.24-fpm
  
  ENV NGINX_VERSION 1.12.1-1~jessie\
  	NODE_VERSION 6.11.4
  ```

  ```dockerfile
  FROM ubuntu:xenial-20170915
  
  RUN apt-get update && apt-get install g++
  ```

### Dockerfile Anti-Pattern: Leaving Default Config

- Problem: Not changing app defaults, or blindly copying VM conf.
  - E.g., php.ini, mysql.conf.d, java memory.
- Solution: Update default configs via `ENV`, `RUN` and `ENTRYPOINT`.

### Dockerfile Anti-Pattern: Environment Specific

- Problem: Copy in environment config at image build

- Solution: Single Dockerfile with default `ENV`'s, and overwrite per-environment with `ENTRYPOINT` script.

  ```dockerfile
  FROM node:6.10
  
  COPY test-environment.json test-environment.json
  ```

## Big 3 Infrastructure Decisions

- **Containers-on-VM or Container-on-Bare-Metal ?**
  - Do either, or both. Lots of pros/cons to either
  - Stick with what you know at first
  - Do some basic performance testing.
- **OS Linux Distribution/Kernel Matters ?**
  - Docker is very kernel and storage driver dependent
  - Innovations/fixes are still happening here.
  - "Minimum" version  != "best" version
  - No pre-existing opinion ? Ubuntu 16.04 LTS
    - Popular, well-tested with Docker
    - 4.x Kernel and wide storage driver support
  - Or Infrakit and LinuxKit
- **Container base Distribution: Which One ?**
  - Which FROM image should you use ?
  - Don't make a decision based on image size (remember it's Single Instance Storage)
  - At first, match your existing deployment process
  - Consider changing to `Alpine` later, maybe much later.

### 

