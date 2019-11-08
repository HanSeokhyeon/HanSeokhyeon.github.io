---
layout: post
title: speech emotion recognition 연구 기록
date: 2019-11-08 13:53:00 +0900
author: HanSeokhyeon
category: blog
---

맨날 공책에 연구기록 정리해놔도 다 없어진다. 그래서 이제 웹에 저장해볼까 한다.

# 1. CRNN, layer = [2, 2, 3], filters = [64, 128, 256]  
epoch 19 loss 0.6085 acc 0.8649  
epoch 41 loss 0.6767 acc 0.8198  
epoch 15 loss 0.5758 acc 0.8378  
epoch 37 loss 0.6570 acc 0.8108  

![2019-11-08_50_33](/assets/images/2019-11-08_50_33.png)

# 2. CRNN, layer = [2, 3, 3], filters = [64, 128, 256]
epoch 10 loss 0.8609 acc 0.7297  
epoch 7 loss 0.6213 acc 0.7838  
epoch 41 loss 0.8283 acc 0.8378  
epoch 35 loss 0.6847 acc 0.8468  

![2019-11-08_16:58:47](/assets/images/2019-11-08_16:58:47.png)

# 3. CRNN, layer = [3, 3, 3], filters = [64, 128, 256]
epoch 28 loss 0.6859 acc 0.8198  
epoch 33 loss 0.7510 acc 0.8198  
epoch 17 loss 0.7368 acc 0.8288   
epoch 17 loss 1.2993 acc 0.6757  

# 4. CRNN, layer = [2, 2, 2], filters = [64, 128, 256]
epoch 16 loss 0.6999 acc 0.8108
epoch 18 loss 0.5953 acc 0.8198
epoch 8 loss 0.7030 acc 0.7568
epoch 11 loss 0.6641 acc 0.8288