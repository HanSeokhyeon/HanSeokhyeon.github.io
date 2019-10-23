---
layout: post
title: ANC 논문 정리
date:   2019-10-17 01:30:17 +0900
category: blog
author: HanSeokhyeon
---

프로젝트하면서 유명한 ANC 논문들의 실험 스펙을 정리할 필요가 있어 포스팅하게 되었다.

# A Robust Hybrid Feedback Active Noise Cancellation Headset

[논문 링크](https://core.ac.uk/download/pdf/83936972.pdf)  
Y Song 저술 - 2005 - 99회 인용

* Active noise cancellation (ANC), feedback control, filtered-x least-mean-square (FXLMS), headset
* Sampling rate of sound card : 8 kHz
* Sampling rate : 2 kHz
* Filter tab : 64
* Test data : a 230 Hz tone embedded in wideband noise

---

### 구조
![fig6](/assets/images/article1/fig6.png)

---

### 특징

* Analog와 digital ANC를 동시에 사용해 hybrid ANC를 구현했다.
* H(s)는 second-order highpass filter

---

### 실험결과
![fig10](/assets/images/article1/fig10.png)

100~600 Hz까지 ANC를 해서 그래프를 600 Hz까지만 그렸다.
> The sampling rate is 2 kHz, and the cutoff
frequency of the anti-aliasing and reconstruction lowpass filters
is 600 Hz. The working frequency of this prototype is from
100 Hz to 600 Hz, and the headset shells that cover the ears can
effectively attenuate noise higher than 600 Hz.

<br>

---
---

<br> 

# Active Noise Control System for Headphone Applications

[논문 링크](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=1597204)  
SM Kuo 저술 - 2006 - 150회 인용

* Active noise control (ANC), adaptive feedback ANC systems, ANC headphones, error sensor optimization, secondary path modeling
* Sampling rate : 8 kHz
* Filter tab
  * secondary path filter : 65
  * ANC filter : 110
* Step size
  * secondary path filter : 0.05
  * ANC filter : 0.3
* Test data
  * 0 ~ 2 kHz까지 100 Hz씩 상승하는 sinusoidal noises
  * engine noise
    * 2200 rpm
    * 3700 rpm
     
---

### 구조
![fig1](/assets/images/article2/fig1.png)

---

### 특징

* 헤드폰 착용 구조에 대한 실험을 하였다.

---

### 실험 결과
![fig7](/assets/images/article2/fig7.png)

당연히 저주파가 잘된다.

![fig9](/assets/images/article2/fig9.png)

2200 rpm에 대해 적당히 잘된다.

![fig8](/assets/images/article2/fig8.png)
![fig10](/assets/images/article2/fig10.png)

Bose꺼랑 비교해놨는데 사실 누가 더 좋은지 모르겠다. y축 스케일이 안맞는데 불편하다. 그리고 800 Hz까지밖에 안그렸는데 그 이상은 무의미한 것 같다.

<br>

---
---

<br>

# 결론
현재 나의 실험 상태와 크게 다르지 않은 것 같다. 나는 현재 10가지의 노이즈 데이터를 가지고 실험중인데 위 논문들은 데이터가 너무 부족하다. 그래도 filter tab 길이나 등등 정보를 얻을 수 있었다.
