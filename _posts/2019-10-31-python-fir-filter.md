---
layout: post
title: Python과 Scipy 이용해서 FIR filter 만들기
date: 2019-10-31 00:16:00 +0900
category: blog
author: HanSeokhyeon
---

Filtered-X LMS 알고리즘을 검증하는 과정에서 추정한 secondary path 대신 내가 임의로 만든 FIR filter를 넣으려고 한다. 그래서 FIR filter를 구현해야할 필요가 있었고 python과 scipy를 이용해 구현하였다. 사실 코드는 전부 참고하고 스펙만 변경하였다.

# 1. 함수 선언
```
from pylab import *
import scipy.signal as signal

#Plot frequency and phase response
def mfreqz(b,a=1):
    w,h = signal.freqz(b,a)
    h_dB = 20 * log10 (abs(h))
    subplot(211)
    plot(w/max(w),h_dB)
    ylim(-150, 5)
    ylabel('Magnitude (db)')
    xlabel(r'Normalized Frequency (x$\pi$rad/sample)')
    title(r'Frequency response')
    subplot(212)
    h_Phase = unwrap(arctan2(imag(h),real(h)))
    plot(w/max(w),h_Phase)
    ylabel('Phase (radians)')
    xlabel(r'Normalized Frequency (x$\pi$rad/sample)')
    title(r'Phase response')
    subplots_adjust(hspace=0.5)

#Plot step and impulse response
def impz(b,a=1):
    l = len(b)
    impulse = repeat(0.,l); impulse[0] =1.
    x = arange(0,l)
    response = signal.lfilter(b,a,impulse)
    subplot(211)
    stem(x, response)
    ylabel('Amplitude')
    xlabel(r'n (samples)')
    title(r'Impulse response')
    subplot(212)
    step = cumsum(response)
    stem(x, step)
    ylabel('Amplitude')
    xlabel(r'n (samples)')
    title(r'Step response')
    subplots_adjust(hspace=0.5)
```

생성된 FIR filter의 frequency, phase, impulse, step response를 plot하는 함수이다.

```
n = 64
a = signal.firwin(n, cutoff = 0.3, window = "hamming")
```
scipy.signal.firwin 함수를 통해 FIR filter를 생성하였다.

```
#Frequency and phase response
mfreqz(a)
show()
```
output:  
![lpf_spec2](/assets/images/lpf_spec2.png)

```
impz(a)
show()
```
output:  
![lpf_spec](/assets/images/lpf_spec.png)

그림과 같이 LPF가 잘 생성되었다.

---
출처:  
<http://mpastell.com/pweave/_downloads/FIR_design_rst.html>