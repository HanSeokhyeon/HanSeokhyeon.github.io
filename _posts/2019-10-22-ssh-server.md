---
layout: post
title: Ubuntu ssh 서버 구축하기
---

연구실 서버 컴퓨터에서 docker를 이용해야 할 상황이 생겼다. 그래서 ssh를 이용해 로컬에서 서버로 접속해 docker를 사용하기 위해 ssh 서버 사용하는 법을 공부했고 정리한다.

# 1. ssh 설치

ubuntu에 로컬로 사용하기 위한 클라이언트 프로그램은 존재하지만 openssh-server는 없다. 그래서 설치가 필요하다. 업데이트 후 설치한다.

```
sudo apt-get update
sudo apt-get install openssh-server
```

# 2. ssh 서버 서비스 시작

```
sudo service ssh start
```
서버를 실행한 후 
```
service ssh status
```
로 상태확인을 해보자.  
output:
```
● ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enab
   Active: active (running) since Tue 2019-10-22 14:39:25 KST; 27min ago
 Main PID: 4635 (sshd)
    Tasks: 1 (limit: 4915)
   CGroup: /system.slice/ssh.service
           └─4635 /usr/sbin/sshd -D

10월 22 14:39:25 han-desktop systemd[1]: Starting OpenBSD Secure Shell server...
10월 22 14:39:25 han-desktop sshd[4635]: Server listening on 0.0.0.0 port 22.
10월 22 14:39:25 han-desktop sshd[4635]: Server listening on :: port 22.
10월 22 14:39:25 han-desktop systemd[1]: Started OpenBSD Secure Shell server.
10월 22 14:39:57 han-desktop sshd[5419]: Did not receive identification string f
10월 22 14:59:13 han-desktop sshd[5472]: Did not receive identification string f
10월 22 14:59:14 han-desktop sshd[5473]: pam_unix(sshd:auth): authentication fai
10월 22 14:59:16 han-desktop sshd[5473]: Failed password for root from 185.197.7
10월 22 14:59:18 han-desktop sshd[5473]: Connection closed by authenticating use
lines 1-17/17 (END)
```
```
ps -ef | grep sshd
```
로 실행중인 ssh 서버의 프로세스를 확인할 수 있다.  
output:
```
root      4635     1  0 14:39 ?        00:00:00 /usr/sbin/sshd -D
han       5631  3459  0 15:08 pts/0    00:00:00 grep --color=auto sshd
```

```
sudo netstat -ntlp | grep sshd
```
로 ip주소와 port 번호를 확인할 수 없다.  
output:
```
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      4635/sshd           
tcp6       0      0 :::22                   :::*                    LISTEN      4635/sshd
```
netstat가 없는 경우는
```
sudo apt install net-tools
```
로 설치 후 다시 확인.

# 3. 내부 IP 주소 확인

```
ifconfig
```
를 통해 현재 서버 컴퓨터에 할당된 IP 주소를 알 수 있다.  
output:
```
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 123.456.7.890  netmask 123.456.7.890  broadcast 123.456.7.890
        ether 02:42:82:b5:22:84  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp0s31f6: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.268.1.123  netmask 123.456.7.890  broadcast 123.456.7.890
        inet6 fe80::4f6c:76d6:b51f:aed9  prefixlen 64  scopeid 0x20<link>
        ether 70:85:c2:69:98:df  txqueuelen 1000  (Ethernet)
        RX packets 264847  bytes 25393804 (25.3 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2474  bytes 257398 (257.3 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 16  memory 0xdf100000-df120000  

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 530  bytes 48618 (48.6 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 530  bytes 48618 (48.6 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

docker0는 무시하고 enp0s31f6:에 inet 다음에 있는 것이 내부 IP 주소다.

# 4. ssh 서버 접속하기

준비가 끝났다. 이제 접속하자. 로컬에서 
```
ssh [username]@[Hostname]
```
와 같이 접속하면 된다
```
ssh han@192.268.1.123
```
output:
```
The authenticity of host '192.268.1.123 (192.268.1.123)' can't be established.
ECDSA key fingerprint is --------------------------.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.268.1.123' (ECDSA) to the list of known hosts.
han@192.268.1.123's password: 
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 5.0.0-32-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 * Kata Containers are now fully integrated in Charmed Kubernetes 1.16!
   Yes, charms take the Krazy out of K8s Kata Kluster Konstruction.

     https://ubuntu.com/kubernetes/docs/release-notes

 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

패키지 0개를  업데이트할 수 있습니다.
0 업데이트는 보안 업데이트입니다.

Your Hardware Enablement Stack (HWE) is supported until April 2023.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

han@han-desktop:~$ 
```
접속에 성공했다.

# 5. 끝내기
```
exit
```
output:
```
로그아웃
Connection to 192.268.1.123 closed.
```

# 6. 추가

기본 포트 22번을 이용하면 보안에 취약하다고들 한다.
그래서 22를 자신만의 특별한 숫자로 바꾸는 것도 추천한다.
또한 openssh의 기본 설정으로 비밀번호와 함께 root 권한으로 접속할 수 있다.
하지만 단순하게 프로그램을 돌릴 계획인데 root 권한은 필요 없을 뿐더러 보안에 악영향을 끼칠 수 있으므로 금지시키는 것도 좋다.
public key?라 해서 파일형태로 key를 가지고 접속하는 방법도 있다.

---
출처:  
<https://jimnong.tistory.com/713>  
<https://cupjoo.tistory.com/98>  
<http://blog.naver.com/PostView.nhn?blogId=ares157&logNo=220984809651>