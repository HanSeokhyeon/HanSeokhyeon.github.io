---
layout: post
title: Pycharm SSH로 서버에서 run하기
date: 2019-11-01 13:44:00 +0900
category: blog
author: HanSeokhyeon
---

지난 포스트에서 파이참에서 서버의 터미널을 여는 것 까지 성공했다. 하지만 진짜 파이참으로 이용해 딥러닝 개발을 하기 위해서는 run까지 해야한다. 그래서 공부해봤다.

# 1. Interpreter 설정

Files/Settings에 들어간다. 다시 Project: ***/Project Interpreter를 누르면 아래와 같은 화면을 볼 수 있다. 

![project_interpreter](/assets/images/project_interpreter.png)

오른쪽 위에 톱니바퀴모양 설정 버튼을 누르고 추가적으로 add까지 누른다. 다시 SSH Interpreter를 누르면 아래와 같은 화면을 볼 수 있다.

![ssh_interpreter](/assets/images/ssh_interpreter.png)

Host에는 서버의 IP주소, Username은 서버의 name을 입력한다. Port 번호를 변경했다면 바꾼 번호로 입력한다.

![ssh_interpreter2](/assets/images/ssh_interpreter2.png)

Password를 사용한다면 password를, 공개키를 사용한다면 공개키 정보를 입력한다.

![ssh_interpreter3](/assets/images/ssh_interpreter3.png)

흠 머 finish.

![ssh_interpreter4](/assets/images/ssh_interpreter4.png)

이렇게 이제 새로운 ssh interpreter가 생겼다. 서버에 깔려있는 파이썬 패키지들이 눈에 들어온다.

OK를 누르면 로컬에 있는 파이썬 프로젝트가 서버에 /tmp/pycharm_project로 전송된다. 전송이 완료되고 나서 평소 파이썬 코드를 돌리듯이 run하면 서버에서 코드가 돌아간다. 신난다!

---

출처:  
<https://simonjisu.github.io/datascience/2018/06/24/pycharmssh.html>