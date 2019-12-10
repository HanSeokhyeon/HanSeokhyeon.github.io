---
layout: post
title: 우분투에서 하드디스크 자동 마운트하기
date: 2019-12-10 18:00:00 +0900
author: Han Seokhyeon
category: blog
---

파일 백업을 위하여 하드디스크로 복사를 하는데 제대로 마운트를 안해놔서 잘 이동이 안된다. 읽기전용시스템이라며 복사되기를 거부한다. 그래서 찾아보았다.

# 1. 하드디스크 확인

```
sudo fdisk -l
```
output:
```
Disk /dev/sda: 238.5 GiB, 256060514304 bytes, 500118192 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 1AFA2164-B071-4FB1-9F97-831DE5F97024

Device         Start       End   Sectors  Size Type
/dev/sda1       2048   1023999   1021952  499M Windows recovery environment
/dev/sda2    1024000   1228799    204800  100M EFI System
/dev/sda3    1228800   1261567     32768   16M Microsoft reserved
/dev/sda4    1261568 250689535 249427968  119G Microsoft basic data
/dev/sda5  250689536 252690431   2000896  977M EFI System
/dev/sda6  252690432 500117503 247427072  118G Linux filesystem


Disk /dev/sdb: 298.1 GiB, 320072933376 bytes, 625142448 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: EC920510-9C58-4BE5-9157-DA1DD22C45D8

Device       Start       End   Sectors   Size Type
/dev/sdb1  1261568 625141759 623880192 297.5G Microsoft basic data
```

위에 /dev/sda가 SSD 아래에 /dev/sdb가 HDD이다.

# 2. UUID 확인
```
sudo blkid
```
output:
```
/dev/sda1: UUID="6000F9A000F97CFA" TYPE="ntfs" PARTLABEL="Basic data partition" PARTUUID="61268627-7833-4582-bc8f-148a800a13bb"
/dev/sda2: UUID="3AC6-A4D0" TYPE="vfat" PARTLABEL="EFI system partition" PARTUUID="504e2bb5-5bac-4053-9660-3ab789cb3394"
/dev/sda3: PARTLABEL="Microsoft reserved partition" PARTUUID="91025f36-b1a4-44bf-997d-d269ab72d8af"
/dev/sda4: UUID="5842CA7242CA5502" TYPE="ntfs" PARTLABEL="Basic data partition" PARTUUID="680c492c-863e-4249-b39f-b0905ff6b65e"
/dev/sda5: UUID="BEF9-E7A5" TYPE="vfat" PARTUUID="e3129401-59f5-44f0-bdf0-a627f34dc705"
/dev/sda6: UUID="ab8797b2-5e1f-460a-b029-b8203e60bab5" TYPE="ext4" PARTUUID="dee3d51b-8d25-4631-8d7b-e02acd8afaa8"
/dev/sdb1: UUID="56A22AFFA22AE36B" TYPE="ntfs" PARTLABEL="Basic data partition" PARTUUID="ce3d4871-adda-44e1-9b22-ef2421c354b4"
```

이런식으로 뜨는데 `/dev/sdb1: UUID="56A22AFFA22AE36B"`를 확인할 수 있었다.

# 3. 자동 마운트 목록 추가
```
sudo mkdir /HDD

sudo vim /etc/fstab
```
마운트할 폴더를 생성하고 마운트 목록에 추가해보자.

```
# /etc/fstab: static file system information.
# 
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda6 during installation
UUID=ab8797b2-5e1f-460a-b029-b8203e60bab5 /               ext4    errors=remount-ro 0       1
# /boot/efi was on /dev/sda2 during installation
UUID=3AC6-A4D0  /boot/efi       vfat    umask=0077      0       1
/swapfile                                 none            swap    sw              0       0
UUID=56A22AFFA22AE36B   /HDD         ntfs    defaults        0       0
```

위와 같이 맨 마지막 줄을 추가하면 된다.

# 4. 마운트 test

```
sudo mount -a
```
를 쳤을때 아무런 log가 안뜨면 성공적이다. 

## 4-1. 에러 1
```
Mount is denied because the NTFS volume is already exclusively opened.
The volume may be already mounted, or another software may use it which
could be identified for example by the help of the 'fuser' command.
```
하지만 이미 마운트된 volume이라고 머라한다. 우분투에서 파일/+ 다른 위치에 마운트된 기존 하드디스크가 있다면 마운트 해제를 한다. 그러고 나서 위에 명령어를 치면 아무런 로그가 없고 하드디스크가 정상적으로 마운트되었을 것이다.

## 4-2. 에러 2
```
The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
Falling back to read-only mount because the NTFS partition is in an
unsafe state. Please resume and shutdown Windows fully (no hibernation
or fast restarting.)
```
구형 HDD면 이러한 에러가 발생할 수 있다. 머 하드상태가 메롱하단다.

```
sudo ntfsfix /dev/sdb1
```
하드를 고치는 명령어다. 내부에 파일은 건드리지 않으니 걱정안해도 된다.  

output:
```
Mounting volume... The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
FAILED
Attempting to correct errors... 
Processing $MFT and $MFTMirr...
Reading $MFT... OK
Reading $MFTMirr... OK
Comparing $MFTMirr to $MFT... OK
Processing of $MFT and $MFTMirr completed successfully.
Setting required flags on partition... OK
Going to empty the journal ($LogFile)... OK
Checking the alternate boot sector... OK
NTFS volume version is 3.1.
NTFS partition /dev/sdb1 was processed successfully.
```
이후 다시 마운트를 하면 정상적으로 된다.

# 5. 심볼릭 링크 만들기

마운트는 정상적으로 성공했지만 마운트된 폴더가 root에 있어 접근하기가 불편하다. 그래서 심볼릭 링크를 만들어주었다. 심볼릭 링크는 윈도우의 바로가기 정도로 생각하면 된다.

```
sudo ln -s /HDD /home/han
```
위의 명령어를 치면 심볼릭 링크가 생성된다. 하지만 읽기 쓰기 권한이 없어 파일을 하드에 쓰는 것이 불가능할 것이다.
```
cd ~
sudo chmod 777 HDD
```
위의 명령어를 치면 이제 HDD 디렉토리에 읽기 쓰기 권한이 생긴다.

## 5-1. 에러 1
```
chmod: 'HDD'의 권한 설정 중: 읽기전용 파일 시스템
```
와 같은 에러를 만날 수 있다. 해결법은 간단하다. 

```
reboot
```

이것이 해결해준다.

---
출처:  
<https://seongkyun.github.io/others/2019/03/05/hdd_mnt/>  
<https://m.blog.naver.com/watney0813/221017927194>  
<https://m.blog.naver.com/PostView.nhn?blogId=rbar&logNo=220569935459&proxyReferer=https%3A%2F%2Fwww.google.com%2F>  
<https://kldp.org/node/92447>