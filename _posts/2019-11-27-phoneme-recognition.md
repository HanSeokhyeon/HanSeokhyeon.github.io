---
layout: post
title: Phoneme recognition (Spikegram, MFCC, Spectrogram)
date: 2019-11-27 15:30:00 +0900
category: blog
author: HanSeokhyeon
---

Phoneme recognition을 위해 다양한 feature를 사용하여 실험해보았다.

# 1. Spikegram
```
Obstruent - Stops : 0.5738  
Obstruent - Affricate : 0.4219  
Obstruent - Fricative : 0.7066  
Sonorant - Glides : 0.5514  
Sonorant - Nasals : 0.5915  
Sonorant - Vowels : 0.5305  
Others : 0.9224  

Obstruent : 0.6576  
Sonorant : 0.5412  
Others : 0.9224  

Non-mute : 0.5749  
Mute : 0.9224  

Total : 0.6526  
```

![126_spikegram](/assets/images/126_spikegram.png)

# 2. MFCC

```
Obstruent - Stops : 0.5097
Obstruent - Affricate : 0.3061
Obstruent - Fricative : 0.6693
Sonorant - Glides : 0.5659
Sonorant - Nasals : 0.6236
Sonorant - Vowels : 0.5338
Others : 0.9196

Obstruent : 0.6106
Sonorant : 0.5499
Others : 0.9196

Non-mute : 0.5677
Mute : 0.9196

Total : 0.6550
```

![120_mfcc](/assets/images/120_mfcc.png)

# 3. Spectrogram

```
Obstruent - Stops : 0.5045
Obstruent - Affricate : 0.3606
Obstruent - Fricative : 0.6698
Sonorant - Glides : 0.5598
Sonorant - Nasals : 0.6039
Sonorant - Vowels : 0.5244
Others : 0.9179

Obstruent : 0.6123
Sonorant : 0.5399
Others : 0.9179

Non-mute : 0.5611
Mute : 0.9179

Total : 0.6496
```

![120_spectrogram](/assets/images/120_spectrogram.png)
