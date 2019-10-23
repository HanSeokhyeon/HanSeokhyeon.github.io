---
layout: post
title: 파이썬 pow 함수
date:   2019-10-17 05:30:17 +0900
category: blog
author: HanSeokhyeon
---

알고리즘 문제를 풀던중 pow(x, y, z)와 같이 3번째 인자를 사용한 경우가 나와서 찾아보게 되었다.


```
>>> pow(2, 4, 3)
1
>>> pow(2, 4) % 3
1
```  
파이썬 pow 함수의 3번째 인자는 모듈러연산이다. 아래 예제를 보면 알 수 있듯이 2의 4승인 16을 3으로 나눈 나머지 1이 출력된다.  

<br>

```
>>> pow(2, 4)
16
>>> 2 ** 4
16
```  
사실 내가 보통 파이썬 코드를 작성중엔 pow 함수보단  ** 연산자를 더 많이 이용한다.  

<br>

출처: <https://m.blog.naver.com/PostView.nhn?blogId=wideeyed&logNo=221137999832&proxyReferer=https%3A%2F%2Fwww.google.com%2F>
