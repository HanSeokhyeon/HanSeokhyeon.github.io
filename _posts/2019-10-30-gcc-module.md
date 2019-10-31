---
layout: post
title: gcc 모듈 c 파일 이용해 컴파일하기
date: 2019-10-30 12:47:00 +0900
category: blog
author: HanSeokhyeon
---

당연히 c 개발을 하면 c파일과 header파일로 파일을 분할해서 관리한다. 그래서 main.c와 모듈이 되는 c파일을 같이 컴파일하는 방법에 대해 알아보았다.

# 1. file 준비
현 나의 상황으로는  
* main.c
* anc.c, anc.h
* functional.c, functional.h
* secondary_path.c, secondary_path.h
* synchronizer.c, synchronizer.h
* common.h

로 프로젝트가 이루어져 있다.  
모두 같은 디렉토리에 존재한다. header file들은 현재 디렉토리에 꼭 존재해야 한다.

Visual studio에서는 common.h에서 stdlib.h 등 필수 헤더파일들을 include했지만 리눅스에서 gcc를 이용해 컴파일할 때는 common.h의 #include를 인식하지 못해 각 c 파일마다 수정해주었다.

가장 먼저 무식하게 main object를 만들어보았다.
```
gcc -o main main.c
```
output:
```
main.c: In function ‘main’:
main.c:44:2: warning: implicit declaration of function ‘fcloseall’; did you mean ‘fclose’? [-Wimplicit-function-declaration]
  fcloseall();
  ^~~~~~~~~
  fclose
/tmp/ccORf9Gr.o: In function `main':
main.c:(.text+0x7d): undefined reference to `Synchronizer'
main.c:(.text+0x91): undefined reference to `estimate_secondary_path'
main.c:(.text+0xbd): undefined reference to `save_filter'
main.c:(.text+0x361): undefined reference to `check_file_length'
main.c:(.text+0x3da): undefined reference to `cancel_noise'
collect2: error: ld returned 1 exit status
```

당연히 모듈에 존재하는 함수들이 define 되지 않았다고 에러를 출력하였다. 또한 fcloseall();에 대해 warning을 출력하기에 구글링해보니 `#define _GNU_SOURCE`를 삽입해주면 된다해서 수정하였더니 정말 사라졌다.

이제 제대로 된 명령어를 입력하였다.
```
gcc -o main -g main.c synchronizer.c anc.c functional.c secondary_path.c
```
-o는 object 파일의 이름을 넣어주는 옵션이고 -g는 컴파일 후 링크까지 해 실행파일을 만드는 옵션이다.

output:
```
/tmp/ccKfl5g7.o: In function `cancel_noise':
/home/hanseokhyeon/ANC/v2.3.0_16k/anc.c:49: undefined reference to `floor'
/home/hanseokhyeon/ANC/v2.3.0_16k/anc.c:86: undefined reference to `pow'
/home/hanseokhyeon/ANC/v2.3.0_16k/anc.c:109: undefined reference to `floor'
/home/hanseokhyeon/ANC/v2.3.0_16k/anc.c:110: undefined reference to `floor'
/tmp/cckOEKHk.o: In function `gaussianRandom':
/home/hanseokhyeon/ANC/v2.3.0_16k/functional.c:52: undefined reference to `log'
/home/hanseokhyeon/ANC/v2.3.0_16k/functional.c:52: undefined reference to `sqrt'
collect2: error: ld returned 1 exit status
```
그러나 floor, pow, log, sqrt 함수가 define 되지 않았다고 에러를 출력하였다. 그렇다. 모두 math.h의 함수들이다. 그래서 구글링해보니 -lm이라는 옵션에 대한 정보를 찾아볼 수 있었다. -lm은 수학 라이브러리 옵션으로 수학 라이브러리를 include해준다.

다시 제대로
```
gcc -o main -g main.c synchronizer.c anc.c functional.c secondary_path.c -lm

or

gcc -o main -g *.c -lm
```
하니 머 warning이 이것저것 뜨나 별거아니므로 무시했다. 여튼 실행파일이 만들어졌다.

이제 실행해보자
```
./main
```
output:
```
Secondary path processing : 32000
processing : 32000ite   white
processing : 32000ite   white
processing : 32000ite   white
```
정상적으로 작동하였다. (숫자 뒤에 ite는 쓴적 없는거 같은데...) 역시나 다른 코드들과 마찬가지로 같은 코드를 돌렸을 때 속도가 눈에 띌 정도로 리눅스가 빨랐다. 이래서 쓰는건가. 아직 vim으로 편집하는 것이 어색하나 그래도 실제 많은 개발자들이 간단하게 vim을 이용해서 프로그래밍을 하는 경우가 종종 있기때문에 나도 연습해야겠다. 또한 gdb?를 이용해 디버깅하는 방법에 대해서도 알아봐야겠다.

---
출처:  
<https://thrillfighter.tistory.com/101>  
<https://askubuntu.com/questions/332884/how-to-compile-a-c-program-that-uses-math-h>
