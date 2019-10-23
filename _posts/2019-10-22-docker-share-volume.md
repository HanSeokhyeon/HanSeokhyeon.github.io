---
layout: post
title: docker 볼륨 공유
date:   2019-10-22 01:30:17 +0900
category: blog
author: HanSeokhyeon
---

docker를 사용할 때 필요한 데이터는 호스트와 공유해서 사용하는 것이 편리하다. 그러므로 docker의 볼륨 공유 기능을 사용해서 폴더나 파일을 호스트와 컨테이너 간에 공유한다.

# 1. 공유할 폴더와 파일 생성

```
mkdir docker_share
cd docker_share
vi test.py
```
![testpy](/assets/images/testpy.png)

# 2. docker run
```
