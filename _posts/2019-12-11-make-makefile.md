---
layout: post
title: make와 Makefile 사용하기
date: 2019-12-11 12:52:00 +0900
author: Han Seokhyeon
category: blog
---

그동안 gcc -o main -g *.c -lm 명령어를 이용해 직접 컴파일을 하였다. 하지만 GNU에서는 주로 make를 이용해 컴파일을 한다.(kaldi 설치하면서 많이 느낌.) Makefile은 make로 어떤 프로그램 구조를 컴파일하기 위해 명령어들을 작성해 놓은 파일로 보면 된다. 그럼 시작해보자.

# 1. Makefile 만들기

```
vim Make file
```
vim을 이용해 파일을 생성하고 그안에 아래의 코드를 작성하였다.

```
  1 run_anc : main.o anc.o functional.o secondary_path.o synchronizer.o
  2     gcc -o run_anc main.o anc.o functional.o secondary_path.o synchronizer.o
  3 
  4 main.o : main.c
  5     gcc -c main.c
  6 
  7 anc.o : anc.c
  8     gcc -c anc.c
  9 
 10 functional.o : functional.c
 11     gcc -c functional.c
 12 
 13 secondary_path.o : secondary_path.c
 14     gcc -c secondary_path.c
 15 
 16 synchronizer.o : synchronizer.c
 17     gcc -c synchronizer.c
 18 
 19 clean:
 20     rm *.o
 21     rm run_anc 
```

1번줄의 코드는 run_anc라는 실행 파일을 만드는 코드이다. Makefile은 기본적으로
```
<대상> : <의존관계>
    <명령>
<Target> : <Dependencies>
    <Recipe>
```
와 같은 구조다. 즉 `main.o anc.o functional.o secondary_path.o synchronizer.o`는 `run_anc`를 만들기 위해 필요한 파일들이다. 아래 줄에 `gcc -o run_anc main.o anc.o functional.o secondary_path.o synchronizer.o`는 명령이다.

아래에 코드들은 object 파일들을 생성하기 위한 코드들이다.

마지막에 `clean`은 다시 아무것도 없는 환경을 위해 지워주는 역할을 한다.

# 2. make

Makefile을 만든 후 컴파일하기 위해서는 make라는 명령어를 실행하면 된다.
```
make
```
output:
```
gcc -c main.c
gcc -c anc.c
gcc -c functional.c
gcc -c secondary_path.c
gcc -c synchronizer.c
gcc -o run_anc main.o anc.o functional.o secondary_path.o synchronizer.o
```
`run_anc`을 만들기 위해서는 object 파일들이 필요했고 명령어를 통해 자동으로 만들었다.

```
ls
```
output:
```
Makefile		anc.o			functional.h		main.o			secondary_path.h	synchronizer.h
anc.c			common.h		functional.o		run_anc			secondary_path.o	synchronizer.o
anc.h			functional.c		main.c			secondary_path.c	synchronizer.c
```

# 3. Run

```
./run_anc
```
를 하면 만들어놓은 실행 파일이 작동한다.

# 4. clean

clean 명령어를 통해 지워보자.

현상태:
```
Makefile		anc.o			functional.h		main.o			secondary_path.h	synchronizer.h
anc.c			common.h		functional.o		run_anc			secondary_path.o	synchronizer.o
anc.h			functional.c		main.c			secondary_path.c	synchronizer.c
```
명령어를 입력하면
```
make clean
```
output:
```
rm *.o
rm run_anc 
```
와 같이 명령어가 실행되고

이후:
```
Makefile		anc.h			functional.c		main.c			secondary_path.h	synchronizer.h
anc.c			common.h		functional.h		secondary_path.c	synchronizer.c
```

object 파일과 실행 파일이 사라져 깨끗한 상태가 되었다.

# 5. 부록 - 에러들

## 1. fcloseall()
```
Undefined symbols for architecture x86_64:
  "_fcloseall", referenced from:
      _main in main.o
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
make: *** [run_anc] Error 1
```
위 에러는 `fcloseall()`을 사용해서 나는 에러다. gcc가 낮은 C 버젼을 사용하나보다. `fcloseall()` 대신 `fclose(FILE* f)`로 일일이 꺼주니깐 에러가 사라졌다.


## 2. Tab

vim으로 python code를 작성할 일이 종종 있었기 때문에 tab을 누르면 tab이 입력되는 대신 space 4개가 입력되도록 vim을 설정해놓은 상태였다. 이상태로 별 생각없이 Makefile을 작성할 때 tab을 눌러 space 4개를 입력했더니 아래와 같은 에러가 발생하였다.
```
Makefile:6: *** missing separator. Stop
```
[갓택 오버플로우](https://stackoverflow.com/questions/24145650/makefile6-missing-separator-stop)는 친절하게 알려주었다.

여튼 결론적으로는 space 4개 대신 tab을 입력해야하는데 검색결과 방법을 알게 되었다.

### 1. vim의 명령모드(:)에서 아래 명령어 입력.
```
:inoremap <S-Tab> <C-V><Tab>
```
### 2. 입력모드에서 shift + tab을 누르면 tab이 입력된다.

사용해본 결과 vim을 끄고 다시 키면 다시 입력해줘야하는 것 같다.

---
출처:  
<https://bowbowbow.tistory.com/12>  
<https://www.tuwlab.com/ece/27193>
<https://zetawiki.com/wiki/Makefile_만들기>  
<https://stackoverflow.com/questions/24145650/makefile6-missing-separator-stop>  
<https://wnsgp.tistory.com/39>  
<https://www.lesstif.com/pages/viewpage.action?pageId=33489088>  
