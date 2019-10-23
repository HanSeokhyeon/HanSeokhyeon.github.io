---
layout: post
title: tmux 사용법
date:   2019-10-22 04:30:17 +0900
category: blog
author: HanSeokhyeon
---

ssh server를 이용할 때 로컬에서 터미널을 끄면 서버와의 연결이 끊기며 서버에 프로그램이 종료되는 것이 불편하다. 
하지만 tmux를 이용하면 터미널을 꺼도 연결이 끊어지지 않으며 프로그램이 서버에서 계속 작동한다. 
그래서 tmux를 사용하고 사용법을 알아보고자 한다.

# 1. 설치

```
sudo apt install tmux
```
매우 간단.

# 2. 실행
```
tmux
```
매우 간단.

![tmux](/assets/images/tmux.png)

# 3. ssh server 접속
```
ssh han@123.123.12.12
```
output:  
![ssh_server](/assets/images/ssh_server.png)

# 4. Run

![run_ssh](/assets/images/run_ssh.png)

터졌지만 중요한게 아니기에...

# 5. Log-off

Ctrl + b 누른 후 d를 누르면 log-off 된다. 터미널이 꺼진 것처럼 보이지만 사실 꺼진 것이 아니다.

output:
```
[detached (from session 0)]
```
![log-off](/assets/images/log-off.png)

# 6. Attach

```
tmux attach
```
를 입력하면 다시 터미널로 돌아가게 된다.  

![attach](/assets/images/attach.png)

---
출처:  
<https://dgkim5360.tistory.com/entry/the-first-steps-for-tmux-terminal-multiplexer>  
<https://m.blog.naver.com/kimmingul/221339305735>
