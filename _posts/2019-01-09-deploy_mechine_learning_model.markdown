---
layout: post
title:  "deploy machine learning model"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---

### 목표

`scikit-learn`으로 생성한 머신러닝 모델을 `Flask`를 이용해서 배포

<br>
### 모델 생성

`scikit-learn`에서 기본적으로 제공하는 iris 데이터 셋을 `SVM`로 학습 한 후 `pickle`을 이용해서 저장한다.

현재 경로에 `SVMModel.pckl` 파일이 생성된다.

```python
# Import libraries and packages
from sklearn import svm, datasets
import pickle
import numpy as np

# Load Sample data
iris = datasets.load_iris()

# Split loaded data into independent and target features
X = iris.data
y = iris.target

# Train Support Vector Machine (SVM) model with all data
svmModel = svm.SVC(kernel='poly', degree=3, C=1.0).fit(X, y)

# Persist model so that it can be used by different consumers
svmFile = open('SVMModel.pckl', 'wb')
pickle.dump(svmModel, svmFile)
svmFile.close()
```

<br>
######iris 데이터 셋
```python
#for i,j in zip(X,y):
#    print(i,"--",j)
[5.  3.3 1.4 0.2] -- 0
[7.  3.2 4.7 1.4] -- 1
```


<br>
### Flask를 이용해 모델 배포

```python
# import Flask class from the flask module
from flask import Flask, request

import numpy as np
import pickle

# Create Flask object to run
app = Flask(__name__)


@app.route('/')
def home():
    return "Hi, Welcome to Flask!!"


@app.route('/predict')
def predict():
    # Get values from browser
    sepLen = request.args['sepal_length']
    sepWid = request.args['sepal_width']
    petLen = request.args['petal_length']
    petWid = request.args['petal_width']

    testData = np.array([sepLen, sepWid, petLen, petWid]).reshape(1, 4)
    class_prediced = int(svmIrisModel.predict(testData)[0])
    output = "Predicted Iris Class: " + str(class_prediced)

    return (output)

# Load the pre-trained and persisted SVM model
# Note: The model will be loaded only once at the start of the server
def load_model():
    global svmIrisModel

    svmIrisFile = open('SVMModel.pckl', 'rb')
    svmIrisModel = pickle.load(svmIrisFile)
    svmIrisFile.close()


if __name__ == "__main__":
    print("**Starting Server...")
    # Call function that loads Model
    load_model()
    # Run Server
    app.run()
```

이미 학습된 모델은 `server.py`가 최초에 실행 될때 한번 로드된다.

* `127.0.0.1:5000/predict?sepal_length=2&sepal_width=4&petal_length=5&petal_width=2`로 접속하면 된다.
* `Predicted Iris Class: 1`를 결과를 받게 된다.


<br><br>
#### 참고문헌
* https://blog.socratesk.com/blog/2018/01/29/expose-ML-model-as-REST-API
