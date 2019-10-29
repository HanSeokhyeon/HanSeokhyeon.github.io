---
layout: post
title: gcc 설치 및 사용법
date:   2019-10-29 16:37:17 +0900
category: blog
author: HanSeokhyeon
---

리눅스에서 C언어 개발 환경을 구축하기 위해 gcc를 사용해본다.

# 1. 설치

보통 우분투를 설치하면 gcc는 설치되어있다.
아래의 명령어를 통해 설치여부를 확인해보자.

```
gcc
```
output:
```
gcc: fatal error: no input files
compilation terminated.
```

이렇게 나오면 설치되어 있는 것이다.
버젼을 확인해보자.

```
gcc --version
```
output:
```
gcc (Ubuntu 7.4.0-1ubuntu1~18.04.1) 7.4.0
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
이러하다.

만약 설치가 안되어 있다면
```
sudo apt-get install gcc
```
를 통해 설치하자.

# 2. test.c
```
vim test.c
```
를 터미널에 쳐서 test.c 파일을 만들 준비를 하자.

![testc](/assets/images/testc.png)

vim 단축키를 잠시 설명하자면 a는 입력모드, ESC는 모드 나가기, x는 지우기, :wq는 저장후 종료다.

:wq를 했으면 아마 home에 test.c가 생겼을 것이다. 
ls 명령어로 각자 확인하자.
이제 gcc 컴파일러를 통해 object 파일을 생성해보자.

```
gcc -o test test.c
```

생성한 후 ls 명령어로 test라는 object 파일이 생성된 것을 확인해보자.
생성이 되었다면 파일을 실행해보자.

```
./test
```
output:
```
hello world
```

잘 실행된다.  
Visual studio라는 강력한 IDE를 사용하다보니 vim로 C code를 개발하기는 매우 불편할 수 있다. 하지만 많은 IT회사가 C를 리눅스 기반으로 개발하니 피해갈 수 없는 도전이라고 생각한다.

---
출처:
<https://byd0105.tistory.com/9>