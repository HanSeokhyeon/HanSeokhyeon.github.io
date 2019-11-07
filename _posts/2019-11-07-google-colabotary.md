---
layout: post
title: 구글 colaboratory 시작하기!
date: 2019-11-07 15:22:00 +0900
category: blog
author: HanSeokhyeon
---

다시 딥러닝 공부를 시작했고 데스크탑 하나로 실험하기 아쉬워서 구글 콜랩에 대해 공부를 시작하였다.

# 1. 데이터 올리기

구글 콜랩은 파일을 서버에 올릴 수 있지만 본인 아이디의 구글 드라이브와 연동하는 것이 가장 편리하다.

구글 콜랩 첫 페이지는 이러하다.

![colab_start](/assets/images/colab_start.png)

오른쪽 위에 파일/새 Python3 노트를 눌러 새 노트를 열어보자.

![colab_note](/assets/images/colab_note.png)

구글 드라이브와 연동하기 위한 코드는 아래와 같다.

```
from google.colab import drive
drive.mount('/gdrive', force_remount=True)
```

코드를 실행하면 아래와 같이 머 링크 들어가서 로그인하고 인증코드를 입력하라고 한다. 방법은 어렵지 않으니 스크린샷은 생략한다.

![colab_googledrive](/assets/images/colab_googledrive.png)

구글드라이브에 프로젝트를 올리기 전에 파일 형식이 구글 문서형식으로 자동으로 바뀌는 것을 방지하기 위해 구글드라이브에 가서 설정을 확인해본다.

![colab_googledrive2](/assets/images/colab_googledrive2.png)

사진에 Convert uploads에 체크가 해제되어있으면 된다. 이후 자신이 올릴 프로그램을 구글 드라이브에 올린다. 방법은 어렵지 않으니 설명 생략...

```
!ls "/gdrive/My Drive"
```
콜랩에서 명령어를 사용하고 싶으면 !를 앞에 쓴 후 사용할 수 있다. 위와 같이 구글 드라이브 안에 내용이 궁금하면 볼 수 있다.

![colab_ls](/assets/images/colab_ls.png)

목록들이 잘 출력된다.

```
!python3 "/gdrive/My Drive/speech_emotion_recognition/main.py"
```
를 입력해 python 파일을 run해봤다.

![colab_run](/assets/images/colab_run.png)

돌아는 간다. 근데 매우 느리다. 생각해보니 GPU 설정을 안해줬다.

/런타임/런타임 유형 변경에 들어가면 아래와 같이 GPU로 설정할 수 있다.

![colab_gpu](/assets/images/colab_gpu.png)

바꾸고 나서 다시 돌려보았다. 뭐 빨라지긴했는데 그래도 로컬에서 돌릴때에 비해 좀 느리다... GTX 1070 > Tesla K80...? 여튼 공짜가 어디냐.

---

출처:   
<https://tykimos.github.io/2019/01/22/colab_getting_started/>