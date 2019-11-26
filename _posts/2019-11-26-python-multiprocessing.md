---
layout: post
title: Python multiprocessing 속도 비교
date: 2019-11-26 13:48:00 +0900
category: blog
author: HanSeokhyeon
---

연구가 끝난 코드들이 많은데 정리가 안되어있어서 요새 정리를 하고 있다. 그러던 와중 전에 작동하였던 python multiprocessing 코드가 작동하지 않아 버그를 수정하였고 속도도 비교하였다.

# 1. 에러

```
IOError: [Errno 24] Too many open files: 'xxxxxxxx.raw'
```

위와 같이 너무 많은 파일을 열었다고 에러가 난다. 구글링을 해보니 리눅스에서 열 수 있는 파일을 제한하고 있어서 그 수를 늘려주면 된다고 나온다.

```
ulimit -a
```
output(mac):
```
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
file size               (blocks, -f) unlimited
max locked memory       (kbytes, -l) unlimited
max memory size         (kbytes, -m) unlimited
open files                      (-n) 10240
pipe size            (512 bytes, -p) 1
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 1392
virtual memory          (kbytes, -v) unlimited

```

output(ubuntu):
```
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 63411
max locked memory       (kbytes, -l) 16384
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1024
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 63411
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```

mac에서는 잘 돌아가는 코드가 우분투에서만 에러가 나서 둘을 비교해봤다. 중간에 open files를 보면 mac이 우분투의 10배로 설정되어 있다. 그래서 우분투에서는 1024번째 파일쯤 가면 에러가 났던 것이다.

# 2. 해결

```
sudo vim /etc/security/limits.conf
```

output:
```
*               soft    nofile          65535
*               hard    nofile          65535
root            soft    nofile          65535
root            hard    nofile          65535
# End of file
```

위와 같이 nofile을 전부 65535로 혹은 큰 숫자로 변경해준다. 그러면 잘 돌아간다.

# 3. 속도 비교

train 3696개, valid 400개, test 192개의 파일에 대해서 feature를 뽑는 코드이다.

* no multiprocessing

```
time = 681.01
```

* multiprocessing

```
time = 186.69
```

3.65배의 속도 차이가 난다. 멀티프로세싱이 프로세스를 생성하고 다시 join하는 과정이 추가로 시간이 들지만 프로세서 1개와 프로세서 12개의 차이는 어마어마하다. 사실 큰 차이가 아니라고 느낄 수도 있지만 파일을 처리하는 log를 보고 있으면 괜히 기분이 좋다 시원시원해서.

---
출처:   
<https://medium.com/hbsmith/too-many-open-files-에러-대응법-9b388aea4d4e>
