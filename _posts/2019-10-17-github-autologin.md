---
layout: post
title: 터미널에서 git 사용시 자동으로 로그인 하는 방법
---

코드를 수정후 github에 push할 때마다 로그인하는 것은 매우 귀찮아서 찾아보았다.

```
$ git config credential.helper store
$ git push https://github.com/HanSeokhyeon/hanseokhyeon.github.io.git
Username for 'https://github.com': HanSeokhyeon
Password for 'https://HanSeokhyeon@github.com': 
Everything up-to-date
```

출처 : <https://franzpark.github.io/git-permanent-authentication/>