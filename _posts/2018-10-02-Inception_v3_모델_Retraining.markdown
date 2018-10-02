---
layout: post
title:  "Inception v3 모델 Retraining"
date:   2018-10-02 08:43:59
author: Kim-JaeHun
categories: [Inception v3, Retraining]
tags: Inception v3
---

### How to <a>retrain</a> a Image Classifier for <a>New Categories</a>


>Transfer learning is a technique that shortcuts much of this by taking a piece of a model that has already been trained on a
related task and reusing it in a new model.

  Transfer learning은 이미 관련 작업에 대해 훈련된 모델의 일부를 가져와서 새로운 모델에 다시 사용함으로써 대부분의 과정을 단축시키는 기술입니다.
<br>

>In this tutorial, we will reuse the feature extraction capabilities from powerful image classifiers trained on ImageNet and simply train a new classification layer on top.

이 튜토리얼에서는 ImageNet에서 학습한 강력한 이미지 분류기의 피쳐 추출 기능을 재사용하고 새로운 분류 레이어를 간단하게 훈련시킵니다.
<br>


### 사용법

- 설치
```
pip3 install tensorflow-hub
```

- 파일 다운로드 및 준비
```
cd ~
curl -LO http://download.tensorflow.org/example_images/flower_photos.tgz
tar xzf flower_photos.tgz
```
```
mkdir ~/example_code
cd ~/example_code
curl -LO https://github.com/tensorflow/hub/raw/master/examples/image_retraining/retrain.py
```

- 재학습
```
python retrain.py --image_dir ~/flower_photos
```

- 새로운 카테고리 추가

여기서 Cherry Blossom 추가 (한글은 안됨)

![add_new_category](https://drive.google.com/uc?id=1nbwlch6TC5MN-YlsZHZVpCmsgEniMFT_)



- 예측
```
curl -LO https://github.com/tensorflow/tensorflow/raw/master/tensorflow/examples/label_image/label_image.py
python label_image.py \
--graph=/tmp/output_graph.pb --labels=/tmp/output_labels.txt \
--input_layer=Placeholder \
--output_layer=final_result \
--image=$HOME/flower_photos/daisy/21652746_cc379e0eea_m.jpg
```

- 옵션 확인
```
retrain.py -h
```
