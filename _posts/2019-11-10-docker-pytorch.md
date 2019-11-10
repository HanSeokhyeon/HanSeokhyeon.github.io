---
layout: post
title: docker pytorch image 이용해서 pytorch 사용하기
date: 2019-11-10 16:06:00 +0900
category: blog
author: HanSeokhyeon
---

이젠 더이상 피할 수 없다. 도커를 이용해 딥러닝해보자.

# 1. Nvidia pytorch image pull

```
docker pull nvcr.io/nvidia/pytorch:19.09-py3
```

와이파이로 해서 그런가 매우 오래걸렸다.

```
docker images
```
output:
```
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
nvcr.io/nvidia/pytorch   19.09-py3           9d6f9ccfbe31        2 months ago        9.15GB
```
9.15GB... 오래 걸린 이유가 있었다.

# 2. Run

돌려보자.

```
docker run -i -t --name pytorch nvcr.io/nvidia/pytorch:19.09-py3 /bin/bash
```

터미널을 켜봤다.

```

=============
== PyTorch ==
=============

NVIDIA Release 19.09 (build 7911588)
PyTorch Version 1.2.0a0+afb7a16

Container image Copyright (c) 2019, NVIDIA CORPORATION.  All rights reserved.

Copyright (c) 2014-2019 Facebook Inc.
Copyright (c) 2011-2014 Idiap Research Institute (Ronan Collobert)
Copyright (c) 2012-2014 Deepmind Technologies    (Koray Kavukcuoglu)
Copyright (c) 2011-2012 NEC Laboratories America (Koray Kavukcuoglu)
Copyright (c) 2011-2013 NYU                      (Clement Farabet)
Copyright (c) 2006-2010 NEC Laboratories America (Ronan Collobert, Leon Bottou, Iain Melvin, Jason Weston)
Copyright (c) 2006      Idiap Research Institute (Samy Bengio)
Copyright (c) 2001-2004 Idiap Research Institute (Ronan Collobert, Samy Bengio, Johnny Mariethoz)
Copyright (c) 2015      Google Inc.
Copyright (c) 2015      Yangqing Jia
Copyright (c) 2013-2016 The Caffe contributors
All rights reserved.

Various files include modifications (c) NVIDIA CORPORATION.  All rights reserved.
NVIDIA modifications are covered by the license terms that apply to the underlying project or file.

WARNING: The NVIDIA Driver was not detected.  GPU functionality will not be available.
   Use 'nvidia-docker run' to start this container; see
   https://github.com/NVIDIA/nvidia-docker/wiki/nvidia-docker .

NOTE: MOFED driver for multi-node communication was not detected.
      Multi-node communication performance may be reduced.

NOTE: The SHMEM allocation limit is set to the default of 64MB.  This may be
   insufficient for PyTorch.  NVIDIA recommends the use of the following flags:
   nvidia-docker run --ipc=host ...

root@14b57ccf6500:/workspace#
```
화려하다.

```
root@14b57ccf6500:/workspace# ls
```
output:
```
README.md  docker-examples  examples  tutorials
```

정상적으로 실행하였다.

# 3. Host의 파이썬 프로젝트를 마운트

```
docker run -i -t --name pytorch -v /home/hanseokhyeon/PycharmProjects/speech_emotion_recognition:/workspace nvcr.io/nvidia/pytorch:19.09-py3 /bin/bash
```

따로 만들어둔 디렉토리가 없어서 그냥 workspace에 마운트하였다.

```
ls
```
output:
```
LICENSE    data_downloader.py  loader.py  models  result.py  save_model
README.md  dataset             main.py    result  run.sh     wavio.py
```

원래 있던 디렉토리와 파일들이 다 사라지고 내 파이썬 프로젝트로 대체되었다.

돌려보자.

```
python3 main.py
```
output:
```
[2019-11-10 07:01:46,762 pyplot.py:225 - switch_backend()] Loaded backend qt5agg version unknown.
[2019-11-10 07:01:46,796 pyplot.py:225 - switch_backend()] Loaded backend tkagg version unknown.
[2019-11-10 07:01:46,797 pyplot.py:225 - switch_backend()] Loaded backend agg version unknown.
[2019-11-10 07:01:46,797 pyplot.py:225 - switch_backend()] Loaded backend agg version unknown.
[2019-11-10 07:01:47,227 data_downloader.py:27 - data_download()] emo-db downloading
[2019-11-10 07:01:47,232 connectionpool.py:205 - _new_conn()] Starting new HTTP connection (1): emodb.bilderbar.info:80
[2019-11-10 07:01:49,233 connectionpool.py:393 - _make_request()] http://emodb.bilderbar.info:80 "GET /download/download.zip HTTP/1.1" 200 40567066
[2019-11-10 07:02:40,629 main.py:246 - main()] start
[2019-11-10 07:02:40,631 main.py:44 - train()] train() start
[2019-11-10 07:02:49,562 main.py:98 - train()] batch:    0/  46, loss: 1.9348, elapsed: 8.93s 0.15m 0.00h
Killed
```

돌아는 간다. 그런데 gpu 세팅을 안해줘서 그런지 pytorch가 cpu로 돌아간다. 무지막지한 네트워크를 그냥 노트북에서 돌리다보니 결국 8GB의 램은 견뎌주지 못했다.

gpu로 도전해야겠다. 물론 서버에서.

---
출처:  
<https://www.joinc.co.kr/w/man/12/docker/Guide/DataWithContainer>  
<https://brunch.co.kr/@hopeless/10>  
<https://hanseokhyeon.github.io/docker-tutorial/>