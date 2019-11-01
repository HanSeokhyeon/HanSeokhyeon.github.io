---
layout: post
title: PyTorch nn.Sequential 알아보기
date: 2019-11-01 17:21:00 +0900
category: blog
author: HanSeokhyeon
---

PyTorch 코드를 구경하다보면 종종 nn.Sequential 함수가 보인다. 그래서 알아보았다.

# 1. 그냥 구현

```
class CNN(nn.Module):
    def __init__():
        super(CNN, self).__init__():

        self.conv1 = nn.Conv2d(1, 32, kernel_size=(41, 11), stride=(2, 2), padding=(20, 5))
        self.conv2 = nn.Conv2d(32, 32, kernel_size=(21, 11), stride=(2, 1), padding=(10, 5))
        self.bn = nn.BatchNorm2d(32)
        self.act = nn.Hardtanh(0, 20, inplace=True),
        
    def forward(self, x):
        x = self.conv1(x)
        x = self.bn(x)
        x = self.act(x)
        x = self.conv2(x)
        x = self.bn(x)
        x = self.act(x)
        return x
```

# 2. nn.Sequential 이용

```
class CNN(nn.Module):
    def __init__():
        super(CNN, self).__init__():

        self.conv = nn.Sequential(
            nn.Conv2d(1, 32, kernel_size=(41, 11), stride=(2, 2), padding=(20, 5)),
            nn.BatchNorm2d(32),
            nn.Hardtanh(0, 20, inplace=True),
            nn.Conv2d(32, 32, kernel_size=(21, 11), stride=(2, 1), padding=(10, 5)),
            nn.BatchNorm2d(32),
            nn.Hardtanh(0, 20, inplace=True)
        )

    def forward(self, x):
        x = self.conv(x)
        return x
```

확실히 간결하게 사용할 수 있다.

---
출처:  
<https://hichoe95.tistory.com/18>
