---
layout: post
title: gdb에서 fopen 함수 디버깅시 step을 사용했을 때 에러
date: 2019-10-31 12:50:00 +0900
category: blog
author: HanSeokhyeon
---

gdb를 이용해 step 명령어를 사용해 디버깅시 fopen에서 파일을 찾을 수 없다며 에러가 나는 것을 확인하였다.

```
Breakpoint 1, main () at main.c:24
24			file_sp.err_sp = fopen("out/han/SP/error.csv", "wt"); // save error of estimating secondary path 
(gdb) s
_IO_new_fopen (filename=0x555555556383 "out/han/SP/error.csv", mode=0x555555556380 "wt") at iofopen.c:88
88	iofopen.c: 그런 파일이나 디렉터리가 없습니다.
```

찾아보니 step과 next 명령어의 차이점에서 나타나는 문제였는데 영어로는 설명이 이렇다.
> "The step command follows the code into a function call. When stepping, gdb prints each source line before executing it. If you step into a library function, gdb wants to display the source, but those file are not available on the system. The complaint is expected and harmless. If you use the next command instead, it will step over and treat the entire call as a single step."

>"step 명령어는 코드를 따라 함수 호출로 이어집니다. step 할 때 gdb는 각 소스 라인을 실행하기 전에 인쇄합니다. 라이브러리 함수로 들어가면 gdb가 소스를 표시하려고하지만 해당 파일을 시스템에서 사용할 수 없습니다. 다음 명령을 대신 사용하면 전체 호출이 한 단계 씩 처리됩니다."

머 이렇단다.

```
Breakpoint 1, main () at main.c:24
24			file_sp.err_sp = fopen("out/han/SP/error.csv", "wt"); // save error of estimating secondary path 
(gdb) n
25			file_sp.f_white = fopen("data/han/SP/white_noise_11second_16k.raw", "rb"); // original
(gdb) 
```
s 대신 n을 사용하니 정상적으로 작동한다.

---
출처:  
<https://stackoverflow.com/questions/25564864/trouble-with-opening-file-for-read-with-fopen/25564973>