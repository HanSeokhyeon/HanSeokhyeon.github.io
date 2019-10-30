---
layout: post
title: 리눅스 명령어 cp, rm, mv
date: 2019-10-30 16:12:00 +0900
category: blog
author: HanSeokhyeon
---

C를 vim으로 개발하다보니 파일과 디렉토리의 복사, 제거, 이름 변경 명령어를 사용해야할 일이 자주 생겼다.

# 1. 파일, 디렉토리 복사 cp
단순하게
```
cp [원래 위치의 파일] [옮길 위치]
cp ~/ANC/v2.3.0_16k/main.c ~/CProjects/filtered-X
```
로 하면 원래 위치의 파일이 옮길 위치로 복사된다.

```
cp [원래 위치/*] [옮길 위치]
cp ~/ANC/v2.3.0_16k/* ~/CProjects/filtered-X
```
원래 위치의 모든 파일이 옮길 위치로 복사된다.

```
cp -r [원래 위치] [옮길 위치]
cp -r ~/ANC/v2.3.0_16k ~/CProjects/filtered-X
```
원래 위치 디렉토리가 옮길 위치로 이동된다.

# 2. 파일, 디렉토리 제거
```
rm [파일 이름]
rm main.c
```
파일 삭제

```
rm -f main.c
```
파일 강제 삭제

```
rm -r filtered-X
```
디렉토리 삭제

# 3. 파일, 디렉토리 이름 변경
```
mv [변경전 이름] [변경할 이름]
mv filtered-X filtered-X_no_secondary_path
```
이렇게 하면 디렉토리의 이름이 변경된다. 사실 mv는 파일이나 디렉토리를 이동시키는 명령어다. 여기서 나는 파일, 디렉토리 이름 변경이 이동으로 이뤄진다는 걸 깨달았다.

---
출처:  
<https://webisfree.com/2018-02-05/%EB%A6%AC%EB%88%85%EC%8A%A4%EC%97%90%EC%84%9C-%ED%8C%8C%EC%9D%BC-%ED%8F%B4%EB%8D%94-%EC%9D%B4%EB%A6%84-%EB%B0%94%EA%BE%B8%EB%8A%94-%EB%B0%A9%EB%B2%95>  
<https://www.manualfactory.net/10805>  
<https://webdir.tistory.com/140>