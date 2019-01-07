---
layout: post
title:  "Tensorflow Serving"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---


### Tensorflow Serving

 텐서플로우 서빙은 이 모델을 운영환경에서 사용할 수 있도록 도와줍니다. 모델은 텐서플로우를 이용해 훈련시키고 사용자로부터 들어오는 입력 데이터를 반영하기 위해 텐서플로우 서빙 API를 이용할 수 있습니다.

```
# 1.2 APT 통한 설치
sudo apt-get update && sudo apt-get install tensorflow-model-server

# 2. 샘플 코드 실행하기.

# 2.1 소스코드 다운로드
git clone  https://github.com/tensorflow/serving

# 2.2 tensorflow-serving-api 설치
pip install tensorflow-serving-api

# 2.3 샘플 Model 다운로드.
python tensorflow_serving/example/mnist_saved_model.py /tmp/mnist_model

# 2.4 인식서버 실행
tensorflow_model_server --port=9000 --model_name=mnist --model_base_path=/tmp/mnist_model/


# 2.5 Client.py 실행
python tensorflow_serving/example/mnist_client.py --num_tests=1000 --server=localhost:9000

# 위의 스텝들 아무런 이상이 없이 설치 되었다면 설치 끝.

# 개인적으로 Python2.7환경에서만 사용하기 때문에 쉽게 설치하여 사용 가능.
# 다른 환경 | 언어로 활용이 필요하신 분은 TF_Serving에서 제공하는 문서대로 bazel 컴파일 하는 방식으로 설치가 필요해 보임.
# Tensorflow Serving 문서
# https://www.tensorflow.org/serving/
```










<br><br>
#### 참고문헌
* https://www.tensorflow.org/serving/serving_basic
* https://github.com/tensorflow/serving
