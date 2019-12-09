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

# 3. Jansson 설치

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

# 4. Github에서 kaldi 가져오기

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

# 5. Kaldi tools 컴파일

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

# 6. Kaldi src 컴파일

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
sudo make depend
sudo make
```
무리없이 된 거 같다. 30분은 걸린 거 같다..

```
cd /opt/kaldi/src/online
sudo make depend
sudo make
```

이것도 마지막으로 완료.

# 7. gst-plugin 컴파일

```
cd /opt/kaldi/src/gst-plugin
sudo make depend
sudo make
```

# 8. gst-kaldi-nnet2-online 컴파일

```
cd /opt
sudo git clone https://github.com/alumae/gst-kaldi-nnet2-online.git

cd /opt/gst-kaldi-nnet2-online/src
sudo sed -i '/KALDI_ROOT?=\/home\/tanel\/tools\/kaldi-trunk/c\KALDI_ROOT?=\/opt\/kaldi' Makefile
sudo make depend
sudo make
```

# 9. kaldi-gstreamer-server clone

```
cd /opt
sudo git clone https://github.com/alumae/kaldi-gstreamer-server.git
sudo git clone https://github.com/jcsilva/docker-kaldi-gstreamer-server.git

cd docker-kaldi-gstreamer-server

sudo chmod +x start.sh
sudo chmod +x stop.sh
```
후 설치는 다 끝났다.


# 10. DB 다운로드

```
sudo mkdir /opt/models
cd /opt/models

sudo wget https://phon.ioc.ee/~tanela/tedlium_nnet_ms_sp_online.tgz
sudo tar -zxvf tedlium_nnet_ms_sp_online.tgz
```
DB가 1.4GB라 다운받는데 시간이 꽤 걸린다.

```
sudo wget https://raw.githubusercontent.com/alumae/kaldi-gstreamer-server/master/sample_english_nnet2.yaml
sudo find /opt/models/ -type f | sudo xargs sed -i 's:test:/opt:g'
sudo xargs sed -i 's:full-post-processor:#full-post-processor:g' /opt/models/sample_english_nnet2.yaml
```
sed 명령어는 어떠한 문자열을 원하는 문자열로 바꾸는데 주로 쓰인다. `sudo find ...` 명령어는 파일 안에서 모든 test를 /opt로 바꾼다. 우리의 models 디렉토리의 경로가 /opt이기 때문이다.

```
sudo cat ./sample_english_nnet2.yaml
```
로 확인해보면

```
# You have to download TEDLIUM "online nnet2" models in order to use this sample
# Run download-tedlium-nnet2.sh in '/opt/models' to download them.
use-nnet2: True
decoder:
    # All the properties nested here correspond to the kaldinnet2onlinedecoder GStreamer plugin properties.
    # Use gst-inspect-1.0 ./libgstkaldionline2.so kaldinnet2onlinedecoder to discover the available properties
    use-threaded-decoder:  true
    model : /opt/models/english/tedlium_nnet_ms_sp_online/final.mdl
    word-syms : /opt/models/english/tedlium_nnet_ms_sp_online/words.txt
    fst : /opt/models/english/tedlium_nnet_ms_sp_online/HCLG.fst
    mfcc-config : /opt/models/english/tedlium_nnet_ms_sp_online/conf/mfcc.conf
    ivector-extraction-config : /opt/models/english/tedlium_nnet_ms_sp_online/conf/ivector_extractor.conf
    max-active: 10000
    beam: 10.0
    lattice-beam: 6.0
    acoustic-scale: 0.083
    do-endpointing : true
    endpoint-silence-phones : "1:2:3:4:5:6:7:8:9:10"
    traceback-period-in-secs: 0.25
    chunk-length-in-secs: 0.25
    num-nbest: 1
    #Additional functionality that you can play with:
    #lm-fst:  /opt/models/english/tedlium_nnet_ms_sp_online/G.fst
    #big-lm-const-arpa: /opt/models/english/tedlium_nnet_ms_sp_online/G.carpa
    #phone-syms: /opt/models/english/tedlium_nnet_ms_sp_online/phones.txt
    #word-boundary-file: /opt/models/english/tedlium_nnet_ms_sp_online/word_boundary.int
    #do-phone-alignment: true
out-dir: tmp

use-vad: False
silence-timeout: 10

# Just a sample post-processor that appends "." to the hypothesis
post-processor: perl -npe 'BEGIN {use IO::Handle; STDOUT->autoflush(1);} sleep(1); s/(.*)/\1./;'

#post-processor: (while read LINE; do echo $LINE; done)

# A sample full post processor that add a confidence score to 1-best hyp and deletes other n-best hyps
##full-post-processor: ./sample_full_post_processor.py

logging:
    version : 1
    disable_existing_loggers: False
    formatters:
        simpleFormater:
            format: '%(asctime)s - %(levelname)7s: %(name)10s: %(message)s'
            datefmt: '%Y-%m-%d %H:%M:%S'
    handlers:
        console:
            class: logging.StreamHandler
            formatter: simpleFormater
            level: DEBUG
    root:
        level: DEBUG
        handlers: [console]
```

경로가 /opt/models로 바껴있는 것을 확인할 수 있다.

# 11. Test

```
cd /opt/docker-kaldi-gstreamer-server

sh start.sh -y /opt/models/sample_english_nnet2.yaml
```
Start!!

```
wget https://raw.githubusercontent.com/alumae/kaldi-gstreamer-server/master/kaldigstserver/client.py -P /tmp
wget https://raw.githubusercontent.com/jcsilva/docker-kaldi-gstreamer-server/master/audio/1272-128104-000.wav -P /tmp
wget https://raw.githubusercontent.com/alumae/kaldi-gstreamer-server/master/test/data/bill_gates-TED.mp3 -P /tmp
python /tmp/client.py -u ws://localhost:8080/client/ws/speech -r 32000 /tmp/1272-128104-0000.wav
python /tmp/client.py -u ws://locallhost:8080/client/ws/speech -r 8192 /tmp/bill_gates-TED.mp3
```
