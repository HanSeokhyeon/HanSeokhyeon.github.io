---
layout: post
title: Apache license 2.0에 대해 알아보자.
date: 2019-11-01 16:28:00 +0900
category: blog
author: HanSeokhyeon
---

CRNN을 이용한 speech emotion recognition 코드를 작성하고 있는데 github에 업로드할 예정이다. Naver의 Speech AI hackathon 코드를 참고 중인데 코드에 적용된 license가 Apache license 2.0이어서 공부하였다.

# 1. 요약

* Apache license는 원 저작물을 어떠한 방식으로 사용하든지 상관이 없다.
* 2차 저작물은 소스 코드를 공개할 의무가 없으며, Apache license를 사용하지 않아도 된다.
* 사용 결과에 대해서 원 저작자가 책임을 지지 않는다.

# 2. 사용법

## 1. code에 저작권 명시

아래에 원 저작자를 코드 맨 처음에 명시하면 된다.

```
"""

Copyright 2017- IBM Corporation

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

"""
```

## 2. project에 license 포함

[여기](https://github.com/moby/moby/blob/master/LICENSE)에 가보면 Apache license 2.0 원본이 있다. 이 원본을 개인 프로젝트에 포함하면 된다.


---
출처:  
<https://linuxism.ustd.ip.or.kr/1143>  
<https://jm4488.tistory.com/4>  
<https://www.oss.kr/oss_license_qna/show/8c7cf208-7c24-499f-b143-9c0f7b22ab6a>