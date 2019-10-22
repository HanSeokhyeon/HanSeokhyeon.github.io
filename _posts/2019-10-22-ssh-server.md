---
layout: post
title: [Ubuntu] ssh 서버 구축하기
---

연구실 서버 컴퓨터에서 docker를 이용해야 할 상황이 생겼다. 그래서 ssh를 이용해 로컬에서 서버로 접속해 docker를 사용하기 위해 ssh 서버 사용하는 법을 공부했고 정리한다.

# 1. ssh 설치

ubuntu에 로컬로 사용하기 위한 클라이언트 프로그램은 존재하지만 openssh-server는 없다. 그래서 설치가 필요하다. 업데이트 후 설치한다.

```
sudo apt-get update
sudo apt-get install openssh-server
```

# 2. ssh 서버 서비스 시작

```
sudo service ssh start
```
