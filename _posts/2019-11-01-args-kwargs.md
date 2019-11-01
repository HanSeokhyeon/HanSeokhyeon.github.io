---
layout: post
title: 파이썬의 *args와 **kwargs
date: 2019-11-01 16:47:00 +0900
category: blog
author: HanSeokhyeon
---

파이썬 코드를 보다보니 함수 인자에 *args와 **kwargs가 있는데 잘 몰라서 찾아보았다.

# 1. *args

*args는 함수의 인자로 들어온 내용이 list에 담긴다고 생각하면 된다.

```
def myFun(*args):
    for arg in args:
        print(arg)

myFun('Hello', 'Welcome', 'to', 'HanSeokhyeon')
```
output:
```
Hello
Welcome
to
HanSeokhyeon
```

남은 인자를 싹 담는다.

```
def myFun(arg1, *args):
    print("First: {}".format(arg1))
    for arg in args:
        print("Next: {}".format(arg))

myFun('Hello', 'Welcome', 'to', 'HanSeokhyeon')
```
output:
```
First: Hello
Next: Welcome
Next: to
Next: HanSeokhyeon
```

# 2. **kwargs

**kwargs는 함수의 인자로 들어온 내용이 dictionary에 담긴다고 생각하면 된다. Dictionary에 담기 위해 key를 지정해줘야한다.

```
def myFun(**kwargs):  
    for key, value in kwargs.items(): 
        print ("%s == %s" %(key, value)) 
  
myFun(first ='Han', mid ='seok', last='hyeon')
```

output:
```
first == Han
mid == seok
last == hyeon
```

마찬가지로 가능하다.

```
def myFun(arg1, **kwargs):
    print("First: {}".format(arg1))
    for key, value in kwargs.items(): 
        print ("%s == %s" %(key, value)) 
  
myFun('Hi', first ='Han', mid ='seok', last='hyeon')
```

output:
```
First: Hi
first == Han
mid == seok
last == hyeon
```

# 3. *args와 **kwargs 둘 다 사용하기

같이 쓰면 key가 없으면 args로, key가 있으면 kwargs로 들어간다.

```
def myFun(*args, **kwargs):
    for arg in args:
        print("args: {}".format(arg))
    for key, value in kwargs.items(): 
        print ("%s == %s" %(key, value)) 
  
myFun('Hi', 'everyone', first ='Han', mid ='seok', last='hyeon')
```

output:
```
argv: Hi
argv: everyone
first == Han
mid == seok
last == hyeon
```

---
출처:  
<https://www.geeksforgeeks.org/args-kwargs-python/>