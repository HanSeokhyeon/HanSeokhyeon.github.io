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

![2019-11-08-50-33](/assets/images/2019-11-08 13_50_33.png)

# 2. CRNN, layer = [2, 3, 3], filters = [64, 128, 256]
epoch 10 loss 0.8609 acc 0.7297  
epoch 7 loss 0.6213 acc 0.7838  
epoch 41 loss 0.8283 acc 0.8378  
epoch 35 loss 0.6847 acc 0.8468  

![2019-11-08-16:58:47](/assets/images/2019-11-08 16:58:47.png)

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

# 5. resnetRNN, layer = [5, 4, 4], filters = [64, 128, 256]
epoch 4 loss 0.7461 acc 0.7568  
epoch 9 loss 0.9223 acc 0.6937  

# 6. resnetRNN, layer = [3, 2, 2], filters = [64, 128, 256]
epoch 4 loss 0.9112 acc 0.7027  
epoch 7 loss 0.7967 acc 0.7117  

# 7. resnetRNN, layer = [7, 6, 6], filters = [64, 128, 256]
epoch 13 loss 1.5899 acc 0.6757  
epoch 9 loss 0.9989 acc 0.7027  

# 8. resnetRNN, layer = [9, 8, 8], filters = [64, 128, 256]
epoch 18 loss 1.2800 acc 0.6667  
epoch 4 loss 1.0925 acc 0.5856  

# 9. resnetRNN, layer = [3, 4, 4], filters = [64, 128, 256]
epoch 14 loss 0.9747 acc 0.6757  
epoch 14 loss 0.8728 acc 0.7748  
epoch 10 loss 0.8545 acc 0.7297  
epoch 5 loss 0.8566 acc 0.6847

# 10. resnetRNN, layer = [3, 4, 4], filters = [64, 128, 256], non_initialization
epoch 14 loss 0.8709 acc 0.7658  
epoch 9 loss 0.9039 acc 0.7568

# 11. resnetRNN, layer = [5, 4, 4], filters = [64, 128, 256], non_initialization
epoch 14 loss 0.9539 acc 0.7658  
epoch 13 loss 0.9006 acc 0.7748  

# 12. resnetRNN, layer = [7, 6, 6], filters = [64, 128, 256], non_initialization
epoch 11 loss 1.2741 acc 0.5946  
epoch 19 loss 1.5301 acc 0.6577

# 13. resnetRNN, layer = [3, 2, 2], filters = [64, 128, 256], non_initialization
epoch 27 loss 1.6902 acc 0.6306  
epoch 12 loss 1.6095 acc 0.6667  

# 14. CRNN, layer = [3, 2, 2], filters = [64, 128, 256]
epoch 20 loss 1.1918 acc 0.7568
epoch 18 loss 1.2211 acc 0.7568  

# 15. CRNN, layer = [3, 4, 4], filters = [64, 128, 256]
Epoch 16 (Test) Loss 0.9358 Acc 0.7477  
Epoch 16 (Test) Loss 1.0985 Acc 0.7838
Epoch 28 (Test) Loss 1.1612 Acc 0.7477
Epoch 20 (Test) Loss 1.0648 Acc 0.7568
