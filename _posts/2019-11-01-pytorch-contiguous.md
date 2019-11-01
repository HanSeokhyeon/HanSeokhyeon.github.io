---
layout: post
title: PyTorch contiguous() 함수
date: 2019-11-01 18:40:00 +0900
category: blog
author: HanSeokhyeon
---

torch.contiguous()에 대해 알아보자.

# 1. PyTorch documentation

>Returns a contiguous tensor containing the same data as self tensor. If self tensor is contiguous, this function returns the self tensor.

라는데... 그러면 contiguous tensor가 먼데...?

구글링을 시작했다.

# 2. Stack overflow 글

>There are few operations on Tensor in PyTorch that do not really change the content of the tensor, but only how to convert indices in to tensor to byte location. These operations include:  
>PyTorch의 텐서에는 텐서의 내용을 실제로 변경하지 않고, 인덱스만 바꿔 동작을 처리하는 함수들이 있습니다. 이러한 작업에는 다음이 포함됩니다.
```
    narrow(), view(), expand() and transpose()
```
>For example: when you call `transpose()`, PyTorch doesn't generate new tensor with new layout, it just modifies meta information in Tensor object so offset and stride are for new shape. The transposed tensor and original tensor are indeed sharing the memory!  
>예를 들어, `transpose()`를 호출하면 PyTorch는 새로운 레이아웃으로 새로운 텐서를 생성하지 않고, Tensor 객체에서 메타 정보를 수정하기 때문에 offset과 stride가 새로운 모양을 갖습니다. 바뀐 텐서와 원래 텐서는 실제로 메모리를 공유하고 있습니다!

```
x = torch.randn(3,2)
y = torch.transpose(x, 0, 1)
x[0, 0] = 42
print(y[0,0])
# prints 42
```
>This is where the concept of contiguous comes in. Above `x` is contiguous but `y` is not because its memory layout is different than a tensor of same shape made from scratch. Note that the word "contiguous" is bit misleading because its not that the content of tensor is spread out around disconnected blocks of memory. Here bytes are still allocated in one block of memory but the order of the elements is different!  
>`x`는 연속적이지만 `y`는 메모리 레이아웃이 처음부터 같은 모양의 텐서와 다르기 때문에 아닙니다. "연속적인"이라는 단어는 텐서의 내용이 연결이 끊어진 메모리 블록 주위에 퍼져 있지 않기 때문에 약간 오해의 소지가 있습니다. 여기서 바이트는 여전히 하나의 메모리 블록에 할당되지만 요소의 순서는 다릅니다!

>When you call `contiguous()`, it actually makes a copy of tensor so the order of elements would be same as if tensor of same shape created from scratch.  
>`contiguous()`를 호출하면 실제로 텐서의 복사본이 만들어 지므로 요소의 순서는 같은 모양의 텐서가 처음부터 만들어진 것처럼 같습니다.

>Normally you don't need to worry about this. If PyTorch expects contiguous tensor but if its not then you will get `RuntimeError: input is not contiguous` and then you just add a call to `contiguous()`.  
>일반적으로 당신은 이것에 대해 걱정할 필요가 없습니다. PyTorch가 연속적인 텐서를 기대하지만 그렇지 않은 경우 `RuntimeError : input is not contiguous`가 아니라면 `contiguous()`에 대한 호출을 추가합니다.  

친절한 개발자의 답변을 번역해보았다. 아마 메모리 아끼려고 pytorch가 이렇게 만들어지지 않았나싶다.

---

출처:  
<https://stackoverflow.com/questions/48915810/pytorch-contiguous>