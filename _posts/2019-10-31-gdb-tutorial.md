---
layout: post
title: gdb 사용해서 C 디버깅하기
date: 2019-10-31 00:32:00 +0900
category: blog
author: HanSeokhyeon
---

C를 개발하는데 디버깅 안할 수는 없다. 
vim을 이용해 gcc로 C 개발을 시작했고, 드디어 디버깅이 필요한 시점이 왔다.

# 1. 디버깅 시작
```
gdb ./main
```
실행파일을 gdb로 연다.

output:
```
GNU gdb (Ubuntu 8.1-0ubuntu3.1) 8.1.0.20180409-git
Copyright (C) 2018 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from ./main...done.
(gdb)
```

# 2. list 명령어
list 명령어는 현재 위치로부터 10줄의 코드를 출력한다.
```
(gdb) list
```

output:
```
2	#include <stdio.h>
3	#include <stdlib.h>
4	#include <math.h>
5	#include <string.h>
6	
7	#include "common.h"
8	#include "synchronizer.h"
9	#include "secondary_path.h"
10	#include "anc.h"
11	#include "functional.h"
```

참고로 list 명령어를 입력할 때마다 현재 위치가 이동한다.
```
(gdb) list
12	
13	//function
14	
15	int main()
16	{
17	
18		float *secondary_path, *anc_filter;
19		
20		if (SECONDARY_PATH) {
21			////////////////////Estimate secondary path////////////////////
(gdb) list
22	
23			struct FILE_SP file_sp;
24			file_sp.err_sp = fopen("out/han/SP/error.csv", "wt"); // save error of estimating secondary path 
25			file_sp.f_white = fopen("data/han/SP/white_noise_11second_16k.raw", "rb"); // original
26			file_sp.f_reals = fopen("data/han/SP/white_16k_100.raw", "rb"); // recorded by FB mic
27	
28			if (USE_S_HAN){
29				char sp_name[100] = "data/han/filter_tab/lpf64.csv";
30				filter_init_file(secondary_path, FILTER_TAB_SP, sp_name);
31			}
(gdb)
```

# 3. break 명령어
```
(gdb) b 28
```

output:  
```
Breakpoint 1 at 0xd92: file main.c, line 28.
```

위와 같이 입력하면 break가 설정된다.

# 4. run 명령어
```
(gdb) r
```

output:  
```
Starting program: /home/hanseokhyeon/CProjects/filtered-X/main 

Breakpoint 1, main () at main.c:29
29				char sp_name[100] = "data/han/filter_tab/lpf64.csv";
```

r을 입력하면 break point까지 프로그램이 실행된다.

# 5. step 명령어
```
(gdb) s
```

output:  
```
30				filter_init_file(secondary_path, FILTER_TAB_SP, sp_name);
```

s를 입력하면 한 줄을 실행한다.

# 6. print 명령어
```
(gdb) p sp_name
```

output:  
```
$1 = "data/han/filter_tab/lpf64.csv", '\000' <repeats 70 times>
```

p와 변수명을 입력하면 변수를 출력한다.

# 7. next 명령어
```
(gdb) n
```

output:  
```
Program received signal SIGSEGV, Segmentation fault.
0x00007ffff76b577d in _IO_vfscanf_internal (s=s@entry=0x5555557598f0, format=<optimized out>, 
    argptr=argptr@entry=0x7fffffffd450, errp=errp@entry=0x0) at vfscanf.c:2447
2447	vfscanf.c: 그런 파일이나 디렉터리가 없습니다.
```

n을 입력하면 다음 행을 실행한다. 그러면서 에러 발생 지점을 발견하였다. 파일이 없다한다.

# 8. step 명령어
```
(gdb) s
30				filter_init_file(secondary_path, FILTER_TAB_SP, sp_name);
(gdb) s
filter_init_file (filter=0x200000, size=64, filename=0x7fffffffdc30 "data/han/filter_tab/lpf64.csv") at functional.c:38
38		FILE* filter_file = fopen(filename, "rt");
```

s를 입력하면 다음 행을 실행하며, 함수가 있다면 함수 내부까지 접근한다.


# 9. quit 명령어
```
(gdb) quit
```

종료다.

# 10. display 명령어
차차 공부해보자.

---
출처:  
<https://jangpd007.tistory.com/54>