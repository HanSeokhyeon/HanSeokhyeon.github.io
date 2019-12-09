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

# 3. Setting

/opt로 이동 (관리자 권한이 계속 필요하다..)
```
cd /opt
```

jansson 설치 후 압축풀기
```
sudo wget http://www.digip.org/jansson/releases/jansson-2.7.tar.bz2
sudo bunzip2 -c jansson-2.7.tar.bz2 | sudo tar xf -
```

압축 푼 jansson-2.7로 가서 make, check, install
```
cd jansson-2.7

sudo ./configure
sudo make
sudo make check
sudo make install
```
머 이리저리 엄청 뜨는데 make check에서 PASS: 1로 뜨긴했다.

output:
```
...
============================================================================
Testsuite summary for jansson 2.7
============================================================================
# TOTAL: 1
# PASS:  1
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================
...
```

주소를 파일에 입력
```
sudo echo "/usr/local/lib" >> sudo /etc/ld.so.conf.d/jansson.conf
sudo ldconfig
sudo rm /opt/jansson-2.7.tar.bz2
sudo rm -rf /opt/jansson-2.7
```

github에서 kaldi 가져오기

```
cd /opt

sudo git clone https://github.com/kaldi-asr/kaldi.git
```

output:
```
'kaldi'에 복제합니다...
remote: Enumerating objects: 8, done.
remote: Counting objects: 100% (8/8), done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 104130 (delta 0), reused 1 (delta 0), pack-reused 104122
오브젝트를 받는 중: 100% (104130/104130), 114.17 MiB | 10.18 MiB/s, 완료.
델타를 알아내는 중: 100% (80381/80381), 완료.
```

kaldi tools를 컴파일하러 가자.
```
cd /opt/kaldi/tools
sudo make
```
output:
```
extras/check_dependencies.sh
extras/check_dependencies.sh: gfortran is not installed.
extras/check_dependencies.sh: Intel MKL is not installed. Run extras/install_mkl.sh to install it.
 ... You can also use other matrix algebra libraries. For information, see:
 ...   http://kaldi-asr.org/doc/matrixwrap.html
extras/check_dependencies.sh: Some prerequisites are missing; install them using the command:
  sudo apt-get install gfortran
Makefile:38: recipe for target 'check_required_programs' failed
make: *** [check_required_programs] Error 1
```
한번에 되면 섭하다. 에러를 읽어보니 gfortran이랑 Intel MKL이 설치가 안되있다고 한다.

```
sudo apt-get install gfortran
```
으로 gfortran을 설치해줬다. 다시 `sudo make`를 실행하니 여전히 Intel MKL이 설치되어 있지 않다고 뜬다. 그런데 컴파일은 시작되었고 보기 무섭게 빠르게 log가 빨리 올라간다... Intel MKL을 설치하고 다시 컴파일 해봐야겠다...

```
sh extras/install_mkl.sh
```
output:
```
E: The repository 'http://ppa.launchpad.net/george-edison55/cmake-3.x/ubuntu bionic Release' does not have a Release file.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.
N: See apt-secure(8) manpage for repository creation and user configuration details.
extras/install_mkl.sh: MKL package intel-mkl-64bit-2019.2-057 installation FAILED.
```
설치 실패란다... 뭐가 문제일까... gfortran이랑 mkl을 둘다 설치하고 컴파일 했어야했는데... 우선 컴파일이 엄청 되긴 했으니 그냥 넘어가본다...(error1)


```
sudo ./install_portaudio.sh
```
위 명령어를 실행하니 설치가 엄청되었다.


```
cd /opt/kaldi/src

sudo ./configure --shared
```
output:
```
Configuring KALDI to use MKL.
Checking compiler g++ ...
Checking OpenFst library in /opt/kaldi/tools/openfst-1.6.7 ...
Checking cub library in /opt/kaldi/tools/cub-1.8.0 ...
Doing OS specific configurations ...
On Linux: Checking for linear algebra header files ...
Configuring MKL library directory: ***configure failed: MKL libraries could not be found. Please use the switch --mkl-libdir or try another math library, e.g. --mathlib=ATLAS (would be slower) ***
```
MKL libraries를 찾을 수 없다... 

하지만 갓글, [갓허브](https://github.com/kaldi-asr/kaldi/issues/3628#issuecomment-538070238)는 해결해주었다.  
<https://github.com/kaldi-asr/kaldi/issues/3628#issuecomment-538070238>

이 글대로 하고나서 다시 `sudo sh install_mkl.sh` 하니깐
output:
```
extras/install_mkl.sh: MKL version 2019.0.1 is already installed.
...
```
이미 설치되어 있단다!!

다시 make...
```
sudo make
```
output:
```
extras/check_dependencies.sh
extras/check_dependencies.sh: all OK.
...
Warning: IRSTLM is not installed by default anymore. If you need IRSTLM
Warning: use the script extras/install_irstlm.sh
All done OK.
```
후... all OK.는 훼이크고 IRSTLM?설치하러 가자.

```
sudo sh extras/install_irstlm.sh
```
output:
```
...
***() Installation of IRSTLM finished successfully
***() Please source the tools/extras/env.sh in your path.sh to enable it
```

tools/extras/env.sh를 source해서 너의 path.sh에 IRSTLM을 가능하게 하라.라고 해서 실행했는데 env.sh는 extras가 아니라 tools안에 있다... 다시 실행하니 된다. ㅎㅎ

머 이제 진짜 make를 해보자...ㅎㅎ 

```
sudo make
```
그러나 저 Warning은 안사라졌다... 넘어가보자...

```
cd /opt/kaldi/src

sudo ./configure --shared
```
output:
```
Kaldi has been successfully configured. To compile:

  make -j clean depend; make -j <NCPU>
```
성공적으로 configured하였다.

```
sudo sed -i '/-g # -O0 -DKALDI_PARANOID/c\-03 -DNDEBUG' kaldi.mk
sudo make depend
sudo make
```
무리없이 된 거 같다.

cd /opt/kaldi/src/online
sudo make depend
sudo make
```

