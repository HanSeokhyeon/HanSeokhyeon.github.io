---
layout: post
title: Kaldi 예제 Voxforge 데이터
date: 2019-12-16 14:00:00 +0900
author: Han Seokhyeon
category: blog
---
 
지난번에 했던 예제는 나에게 불필요한 기능이 너무 많아서 다른 포스트를 보고 다시 시도한다.

# 1. kaldi projects 다운로드
```
git clone https://github.com/kaldi-asr/kaldi.git
```

본인이 설치하고 싶은 곳에 kaldi 프로젝트를 clone하자.

```
cd kaldi
ls
```
output:
```
CMakeLists.txt  COPYING  INSTALL  README.md  cmake  docker  egs  misc  scripts  src  tools  windows
```

프로젝트 안에는 이러한 디렉토리와 파일들이 있다.

# 2. path 설정

나는 voxforge 예제를 실행할 것이기 때문에 voxforge의 디렉토리로 이동해야한다.

```
cd egs/voxforge/s5
ls
```
output:
```
cmd.sh  conf  getdata.sh  local  path.sh  run.sh  steps  utils
```

`s5` 디렉토리에 `path.sh`를 열면 voxforge의 `DATA_ROOT`가 주석처리 되어있다. 이를 없애보자.
```
vim path.sh
```
output:
```
...

# VoxForge data will be stored in:
# export DATA_ROOT="$KALDI_ROOT/egs/voxforge/s5/voxforge"    # e.g. something like /media/secondary/voxforge

...
```

`export` 앞에 #을 지워 주석처리를 제거하자. 제거했으면 저장후 `path.sh`를 실행해준다.

```
./path.sh
```

# 3. Voxforge DATASET 다운로드

kaldi는 친절하게 데이터셋 다운로드하는 스크립트를 다 작성해두었다. 용량이 20GB가 넘으니 공간 확보 후 실행해보자.

```
./getdata.sh
```

output:
```
FINISHED --2019-12-16 15:32:47--
Total wall clock time: 1h 4m 59s
Downloaded: 6252 files, 10G in 44m 34s (3.99 MB/s)
--- Starting VoxForge archives extraction ...
```

# 4. kaldi 환경 구축

## 4-1. package
```
sudo apt install gawk
sudo apt install coreutils
sudo apt install swig
sudo apt install flac
```

한번에 설치할 수도 있지만 설치에 실패하고 출력되는 로그를 지나칠 수 있으므로 하나하나씩 해주자.

## 4-2. SRILM

다음은 `kaldi/tools`에 있는 `install_srilm.sh`을 실행시켜주자.

```
cd kaldi/tools

./install_srilm.sh
```
output:
```
...
This script cannot install SRILM in a completely automatic
way because you need to put your address in a download form.
Please download SRILM from http://www.speech.sri.com/projects/srilm/download.html
put it in ./srilm.tgz, then run this script.
```

다운로드 양식을 입력해야해서 SRILM을 완벽하게 자동으로 설치할 수 없다고 한다. 위에 주소로 들어가서 다운 받아주자. 1.7.2로 받자.

다운로드하였다면 다운로드 폴더에 있을테고 그것을 ./srilm.tgz에 넣어주자.

```
mv ~/Downloads/srilm-1.7.2.tar.gz ./srilm.tgz
```

옮겼으면 다시 설치파일을 실행하자.

```
./install_srilm.sh
```

output:
```
...
Installation of SRILM finished successfully
Please source the tools/env.sh in your path.sh to enable it
```

## 4-3. SEQUITUR

```
./extras/install_sequitur.sh
```
output:
```
python setup.py build
Traceback (most recent call last):
  File "setup.py", line 33, in <module>
    import numpy
ImportError: No module named numpy
Makefile:6: recipe for target 'build' failed
make: *** [build] Error 1
USER SITE: /home/han/.local/lib/python2.7/site-packages
SEQUITUR_PACKAGE: lib/python2.7/site-packages
SEQUITUR: /home/han/kaldi/tools/sequitur-g2p/lib/python2.7/site-packages
PYTHONPATH: 
Traceback (most recent call last):
  File "setup.py", line 33, in <module>
    import numpy
ImportError: No module named numpy
Problem installing sequitur!
```

numpy가 없다고 한다. 파이썬 2.7은 평소에 쓰질 않아서 그렇다.

```
pip install numpy
```

다시 `install_sequitur.sh`를 실행하였다.

output:
```
Using /home/han/.local/lib/python2.7/site-packages
Finished processing dependencies for sequitur==1.0a1
find: ‘./lib64’: 그런 파일이나 디렉터리가 없습니다
Installation of SEQUITUR finished successfully
Please source tools/env.sh in your path.sh to enable it
```

그런 파일이나 디렉토리가 없다고 하지만 설치가 성공적으로 됐다니깐 넘어가자.

## 4-4. cmd.sh 수정

```
vim cmd.sh
```
`cmd.sh` 안에 내용을 바꿔주자

```
export train_cmd="queue.pl --mem 2G"
export decode_cmd="queue.pl --mem 4G"
export mkgraph_cmd="queue.pl --mem 8G"
```
를
```
export train_cmd="run.pl --mem 2G"
export decode_cmd="run.pl --mem 4G"
export mkgraph_cmd="run.pl --mem 8G"
```
로 바꿔주자.

# 5. path.sh와 run.sh

path.sh로 환경변수를 다시 설정하고 run.sh를 실행시키자.

```
./path.sh
./run./sh
```

output:
```
...
--- Preparing pronunciations for OOV words ...
failed to convert "-pau-": translation failed
failed to convert "<unk>": translation failed
...
utils/prepare_lang.sh: line 466: fstcompile: command not found
utils/prepare_lang.sh: line 468: fstarcsort: command not found
Traceback (most recent call last):
  File "utils/lang/make_lexicon_fst.py", line 411, in <module>
    main()
  File "utils/lang/make_lexicon_fst.py", line 401, in main
    left_context_phones=left_context_phones)
  File "utils/lang/make_lexicon_fst.py", line 278, in write_fst_with_silence
    cost=(pron_cost if i == 0 else 0.0)))
BrokenPipeError: [Errno 32] Broken pipe
...
```

왜 이런 에러가 나냐면 git에서 kaldi 프로젝트를 clone만 하고 컴파일을 하지 않아서 그렇다.
[5번, 6번](https://hanseokhyeon.github.io/kardi-tutorial/)을 참고하자.

정상적으로 kaldi를 컴파일하고 다시 실행하니 동작한다. 그러나 엄청 오래 걸리는것 같다...

---
출처:  
<https://jybaek.tistory.com/772>

