---
layout: post
title: docker 설치하고 jupyter notebook으로 tensorflow하기
---


로컬이 아닌 공동으로 사용하는 서버에서 딥러닝을 해야할 경우 버젼 충돌 문제가 발생하게 된다. 
내가 사용하는 TF나 PyTorch 버젼이 다른 사람과 다르다면 docker를 사용하여 환경을 구축하는 것을 추천한다.


# 1. 설치 (Ubuntu)

자동 설치 스크립트를 이용해서 설치!

```
curl -fsSL https://get.docker.com/ | sudo sh
```

# 2. root 권한 없이 docker 사용하기

docker는 root권한을 필요로해서 sudo를 추가해주고 비밀번호를 매번 요구한다. 귀찮으므로 없앤다.

```
sudo usermod -aG docker $USER
```

재부팅하면 적용완료.

# 3. 설치 확인

```
docker version
```

output:

```
Client: Docker Engine - Community
 Version:           19.03.4
 API version:       1.40
 Go version:        go1.12.10
 Git commit:        9013bf583a
 Built:             Fri Oct 18 15:54:09 2019
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.4
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.10
  Git commit:       9013bf583a
  Built:            Fri Oct 18 15:52:40 2019
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.10
  GitCommit:        b34a5c8af56e510852c35414db4c1f4fa6172339
 runc:
  Version:          1.0.0-rc8+dev
  GitCommit:        3e425f80a8c931f88e6d94a8c831b9d5aa481657
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

만약 2번을 실행하고 재부팅을 하지 않았다면 Server 정보가 나오지 않는다.

# 4. Run TensorFlow 공식 이미지

```
docker run -it -p 8888:8888 tensorflow/tensorflow
```

아마 tensorflow/tensorflow docker이미지가 없을것이기 때문에 설치를 장시간 진행할 것이다. tensorflow/tensorflow 이미지는 tensorflow cpu 버전이며 원래는 jupyter notebook이 자동으로 실행된다고 했는데 나는 이상하게 안됐다...

```
$ docker run -it -p 8888:8888 tensorflow/tensorflow

________                               _______________                
___  __/__________________________________  ____/__  /________      __
__  /  _  _ \_  __ \_  ___/  __ \_  ___/_  /_   __  /_  __ \_ | /| / /
_  /   /  __/  / / /(__  )/ /_/ /  /   _  __/   _  / / /_/ /_ |/ |/ / 
/_/    \___//_/ /_//____/ \____//_/    /_/      /_/  \____/____/|__/


WARNING: You are running this container as root, which can cause new files in
mounted volumes to be created as the root user on your host machine.

To avoid this, run the container by specifying your user's userid:

$ docker run -u $(id -u):$(id -g) args...

root@ae73f71e625d:/#
```

이런식으로 실행이 되고 터미널이 떴다.

이후 jupyter notebook을 실행해보았다.

```
root@ae73f71e625d:/# jupyter notebook
bash: jupyter: command not found
```

하지만 없단다. 그래서 설치를 해줬다.

```
pip install jupyter notebook
```

참고로 python 버전은 2.7이다. jupyter notebook 설치 이후 실행해보았다.

```
socket.error: [Errno 99] Cannot assign requested address
```

머라머라 에러가 난다. 위에서 pip를 업그레이드 하라는 경고를 줬기때문에 업그레이드해줬다. 하지만 그래도 에러가 난다. 아마 업데이트를 해줘야하는듯하다.

```
apt-get update
```

docker에서 터미널에 sudo를 치면 없는 명령어라고 하더라. 그래도 안되서 찾아보니깐 local에서 실행 안해서 IP가 안맞는다뭐라나... 결론은 이거다

```
jupyter notebook --ip=0.0.0.0 --allow-root
```

output:
```
[I 05:01:58.014 NotebookApp] Serving notebooks from local directory: /
[I 05:01:58.014 NotebookApp] The Jupyter Notebook is running at:
[I 05:01:58.014 NotebookApp] http://(ae73f71e625d or 127.0.0.1):8888/?token=54bc8c34b32e8fa4d3227284afe146c779f9327be4107b52
[I 05:01:58.014 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[W 05:01:58.020 NotebookApp] No web browser found: could not locate runnable browser.
[C 05:01:58.020 NotebookApp] 
    
    To access the notebook, open this file in a browser:
        file:///root/.local/share/jupyter/runtime/nbserver-411-open.html
    Or copy and paste one of these URLs:
        http://(ae73f71e625d or 127.0.0.1):8888/?token=54bc8c34b32e8fa4d3227284afe146c779f9327be4107b52
```

이렇게 마지막에 URLs를 알려주는데 주소에 or이 있다. http://127.0.0.1:8888/?token=54bc8c34b32e8fa4d3227284afe146c779f9327be4107b52 와 같이 앞부분 없애고 실행하니 정상작동한다.

휴...

# 5. TensorFlow hello world 예제

```
import tensorflow as tf

hello = tf.constant('Hello, TensorFlow!')

sess = tf.Session()

print(sess.run(hello))
```

별 생각없이 실행했더니 Session()이 없는 거란다. 그렇다 tensorflow 2.0이다.(안써봄 ㅎ)

공홈에서 예제를 가져왔다.

```
import tensorflow as tf

mnist = tf.keras.datasets.mnist

(x_train, y_train), (x_test, y_test) = mnist.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0

model = tf.keras.models.Sequential([
  tf.keras.layers.Flatten(input_shape=(28, 28)),
  tf.keras.layers.Dense(128, activation='relu'),
  tf.keras.layers.Dropout(0.2),
  tf.keras.layers.Dense(10, activation='softmax')
])

model.compile(optimizer='adam',
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])

model.fit(x_train, y_train, epochs=5)

model.evaluate(x_test,  y_test, verbose=2)
```

output:
```
Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/mnist.npz
11493376/11490434 [==============================] - 1s 0us/step
11501568/11490434 [==============================] - 1s 0us/step
Train on 60000 samples
Epoch 1/5
60000/60000 [==============================] - 13s 216us/sample - loss: 0.2952 - accuracy: 0.9141
Epoch 2/5
60000/60000 [==============================] - 11s 181us/sample - loss: 0.1423 - accuracy: 0.9574
Epoch 3/5
60000/60000 [==============================] - 11s 188us/sample - loss: 0.1058 - accuracy: 0.9686
Epoch 4/5
60000/60000 [==============================] - 11s 191us/sample - loss: 0.0855 - accuracy: 0.9735
Epoch 5/5
60000/60000 [==============================] - 13s 217us/sample - loss: 0.0734 - accuracy: 0.9766
10000/1 - 1s - loss: 0.0362 - accuracy: 0.9782

[0.07176612680561374, 0.9782]
```

잘된다.

# 6. Docker container commit하기
기껏 jupyter notebook 다 설치했는데 날릴 수 없다. 그러므로 docker 종료 전에 이미지에 commit해서 변경사항을 등록해줘야한다. 

```
docker images
```
를 입력해 이미지를 확인하면  
output:
```
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
tensorflow/tensorflow   latest              d64a95598d6c        2 weeks ago         1.03GB
```
와 같은 식으로 이미지의 내용이 출력된다. commit을 하기 위해서는 container의 ID가 필요하다. 아래의 명령어를 사용하면 알 수 있다.

```
docker ps -a
```
output:
```
CONTAINER ID        IMAGE                   COMMAND             CREATED             STATUS                    PORTS                    NAMES
ae73f71e625d        tensorflow/tensorflow   "/bin/bash"         About an hour ago   Up About an hour          0.0.0.0:8888->8888/tcp   pensive_buck
f585a03cd7ae        tensorflow/tensorflow   "/bin/bash"         19 hours ago        Exited (0) 19 hours ago                            wizardly_haslett
```
복잡해 보이지만 첫번재 있는게 container ID다. 복사한후

```
docker commit ae73f71e625d tensorflow/tensorflow
```
output:
```
sha256:b47a8c553c49e9f8db060811feba1ad69725390580f1112a37d68927d7b2938c
```
라는 결과를 볼 수 있다.

다시
```
docker images
```
를 입력하면  
output:
```
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
tensorflow/tensorflow   latest              b47a8c553c49        8 seconds ago       1.15GB
tensorflow/tensorflow   <none>              d64a95598d6c        2 weeks ago         1.03GB
```
를 얻을 수 있고, 8초전에 하나 추가되었다. SIZE가 증가한거 보니 제대로 된거 같다. 귀찮아서 테스트 안해본건 아니다.

---
출처:  
<http://moducon.kr/2018/wp-content/uploads/sites/2/2018/12/leesangsoo_slide.pdf>  
<https://www.tensorflow.org/install/docker?hl=ko>  
<https://www.tensorflow.org/tutorials/quickstart/beginner>  
<https://pbj0812.tistory.com/134>  
<https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html>
<https://nicewoong.github.io/development/2018/03/06/docker-commit-container/>