---
layout: post
title: Kaldi 설치 및 간단한 예제 실행
data: 2019-12-05 17:03:00 +0900
author: Han Seokhyeon
category: blog
---

많은 기업들이 음성인식을 개발할 때 Kaldi를 이용한다고 한다. Kaldi에 대해 공부하기 위해 먼저 깔아보자!

# 1. Kaldi를 위해 필요한 패키지 설치

```
sudo apt-get update && apt-get install -y \
autoconf \
automake \
bzip2 \
g++ \
git \
gstreamer1.0-plugins-good \
gstreamer1.0-tools \
gstreamer1.0-pulseaudio \ 
gstreamer1.0-plugins-bad \ 
gstreamer1.0-plugins-base \ 
gstreamer1.0-plugins-ugly \
libatlas3-base \
libgstreamer1.0-dev \
libtool-bin \ 
make \ 
python2.7 \ 
python3 \ 
python-pip \ 
python-yaml \
python-simplejson \
python-gi \
subversion \
wget \
build-essential \
python-dev \
sox \
zlib1g-dev && \
apt-get clean autoclean && \
apt-get autoremove -y && \
pip install ws4py==0.3.2 && \
pip install tornado==4.5.3 && \
ln -s /usr/bin/python2.7 /usr/bin/python ; ln -s -f bash /bin/sh
```

복사해서 한번에 터미널에 치려했지만 짤려서 결국 일일이 쳤다...

마지막줄에
```
ln -s /usr/bin/python2.7 /usr/bin/python
ln -s -f bash /bin/sh
```
예상 output:
```
ln: failed to create symbolic link '/usr/bin/python': 파일이 있습니다
ln: failed to create symbolic link '/bin/sh': 허가 거부
```
이 명령어 두줄은 심볼릭 링크파일을 생성해주는 명령어이다. 쉽게 생각하면 윈도우의 바로가기와 같다. 기존에 있든 새로 설치했든 파이썬 2.7을 파이썬에 링크함으로써 리눅스의 기본 파이썬을 파이썬 2.7로 설정하는 과정이다. 근데 대부분 이미 파이썬 3.6이 기본 파이썬으로 설정되어있을 것이다. 그래서 파일이 있다는 에러가 출력될 것이다. 또한 /bin/sh는 관리자 권한 없이는 접근 불가능해서 허가 거부 에러가 출력될 것이다.

이렇게 개선된 명령어를 입력하면 심볼릭 파일이 잘 생성된다.

```
sudo ln -s -f /usr/bin/python2.7 /usr/bin/python
sudo ln -s -f bash /bin/sh
```

-f는 파일이 존재하든지 안하든지 강제로 심볼릭 파일을 생성한다.

# 2. Cuda 설치 & gcc 버젼 설정

Kaldi는 GPU를 주로 사용하기 때문에 cuda를 설치해줘야 한다. cuda 설치 방법은 다른 블로그에 많이 존재하니 이 글에서 언급하지는 않겠다.

Cuda는 gcc-7과 g++-7 버전으로 사용해야한다.

```
sudo apt install gcc-7 g++-7

sudo ln -s /usr/bin/gcc-7 /usr/local/cuda/bin/gcc
sudo ln -s /usr/bin/g++-7 /usr/local/cuda/bin/g++
```

위와 같이 설치후 심볼릭 링크를 걸어준다.



