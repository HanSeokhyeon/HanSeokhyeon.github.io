---
layout: post
title: Pycharm SSH Terminal 연결하기
date: 2019-11-01 11:31:00 +0900
category: blog
author: HanSeokhyeon
---

연구실의 데스크탑을 서버로 사용하고 개인 노트북을 로컬로 사용하려고 시도하고 있다. SSH server로 데스크탑 우분투로 접속하는 것을 성공했지만, 좀 더 편한 이용을 위해 PyCharm Professional에서 지원하는 SSH plugin을 사용해보고자 한다. 참고로 PyCharm Professional은 유료버전이나 학생인증을 하면 무료로 사용할 수 있다.

# 1. SSH server 접속 환경 구축

[여기](https://hanseokhyeon.github.io/ssh-server/)를 참고하길 바란다.

# 2. Pycharm SSH Remote Run plugin 설치

SSH Remote Run은 bundle plugin이므로 기본적으로 설치 되어 있을 것이다.

Settings/Plugins에 installed를 눌러서 설치가 되어 있는지 확인하자.

![ssh_remote_run](/assets/images/ssh_remote_run.png)

# 3. SSH terminal 연결

메인메뉴에 Tools/Start SSH Session을 누르면 정보를 입력하는 창이 뜬다. 평소에 터미널로 SSH server에 접속하듯이 호스트, 사용자, 포트번호, 비밀번호를 입력하면 터미널이 연결된다.

![ssh_session](/assets/images/ssh_session.png)

이와 같이 입력하면

![terminal](/assets/images/terminal.png)

연결된다. 다음에는 run을 서버에서 하는 방법을 연구할 계획이다.

---
출처:  
<https://www.jetbrains.com/help/pycharm/running-ssh-terminal.html>

