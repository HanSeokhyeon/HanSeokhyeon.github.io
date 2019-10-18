---
layout: post
title : 파이썬에서 librosa 패키지로 스펙트럼 그리기
---

프로젝트에서 파형의 스펙트럼을 그려서 분석할 일이 있어 코드를 작성해보았다.

<br>

### data load, normalization

내가 사용한 파일이 raw 파일이라 아래와 같은 방식으로 데이터를 읽었다. 
librosa 패키지는 함수의 입력으로 -1 ~ 1로 노말라이즈된 데이터를 사용하기 때문에 32767로 나누어주었다.


{% raw %}

```angular2
data = np.fromfile("ANC_X_8k/boeing_FF.raw", dtype=np.int16)
data = data.astype(np.float) / 32767
```

<br>

{% endraw %}

### spectrogram, amplitude, dB

librosa.stft()는 data의 스펙트로그램을 리턴한다. 
여기서 n_fft로 FFT 사이즈를 설정할 수 있다.
스펙트로그램은 복소수로 리턴되므로 np.abs를 이용해서 amplitude로 바꿔준다. 
librosa.amplitude_to_db()를 이용해 스펙트로그램을 dB 스케일로 바꿔준다.
원칙상으론 데이터의 길이만큼 fft를 하면 됐으나 편의상 스펙트로그램의 평균으로 대체하였다.

{% raw %}

```angular2
data = librosa.stft(data[sample_rate // 4:sample_rate // 4 * 2], n_fft=n_fft)
data = np.mean(librosa.amplitude_to_db(np.abs(data)), axis=1)
```

{% endraw %}

<br>

다음은 pyplot을 이용해 plot하는 과정이므로 전체 코드를 참고하면 된다.

<br>

### 전체 코드

{% raw %}

```angular2
import numpy as np
import librosa
import matplotlib.pyplot as plt


sample_rate = 8000
n_fft = 512

data = np.fromfile("ANC_X_8k/boeing_FF.raw", dtype=np.int16)
data = data.astype(np.float) / 32767
data = librosa.stft(data[sample_rate // 4:sample_rate // 4 * 2], n_fft=n_fft)
data = np.mean(librosa.amplitude_to_db(np.abs(data)), axis=1)

plt.title('boeing')

plt.xlabel("Frequency[Hz]")
plt.xticks(range(0, n_fft//2+1, 32), np.arange(0, 4001, 500))
plt.xlim(0, n_fft//2)

plt.ylabel("Amplitude[dB]")

plt.plot(data, label='data', alpha=0.7)

plt.grid()
plt.legend()

fig = plt.gcf()

plt.show()
fig.savefig('boeing.png')
```

{% endraw %}

<br>

### 결과물

![boeing](/assets/images/boeing.png)