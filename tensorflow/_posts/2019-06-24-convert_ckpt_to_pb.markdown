---
layout: post
title:  "convert ckpt to pb"
date:   2019-06-24 08:43:59
author: kim-jaehun
categories: [tensorflow]
tags: [tensorflow]
sitemap :
  changefreq : daily
  priority : 1.0
---

## Checkpoint(ckpt) 파일

- .ckpt.meta
- .ckpt.index
- .ckpt.data


`.ckpt.meta`는 메타그래프를 포함한다. 우리가 tensorboard에서 볼 수 있는 연산 그래프(computation graph)의 구조만을 가진다.

`.ckpt-data`는 변수의 값(the values of the variables)만을 포함한다.

`.ckpt-index` - ?

```python
saver = tf.train.import_meta_graph(path_to_ckpt_meta)
saver.restore(sess, path_to_ckpt_data)
```
<br>
## Protocol buffer(pb) 파일

pb파일은 `메타그래프 + 변수 `를 함께 저장 할 수 있다.

ckpt 파일을 [freeze_graph.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/tools/freeze_graph.py) 스크립트를 이용해 pb파일로 만들 수 있다.(binary 가능)


<br>
## Convert ckpt to pb

ckpt파일을 [freeze_graph.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/tools/freeze_graph.py) 파일을 이용해 변환해 보자.



**Exporting the Inference Graph**
모델의 아키텍쳐를 포함한 GraphDef(serialized version of Graph)를 inception_v3_inf_graph.pb에 저장한다.

[export_inference_graph.py](https://github.com/tensorflow/models/tree/master/research/slim)
```
$ python export_inference_graph.py \
  --alsologtostderr \
  --model_name=inception_v3 \
  --output_file=/tmp/inception_v3_inf_graph.pb
```
[freeze_graph.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/tools/freeze_graph.py) 를 실행하여 다음과 같이 변수를 인라인 된 변수와 함께 그래프 정의를 얻을 수 있습니다.
```
$ python freeze_graph.py
  --input_graph=/tmp/inception_v3_inf_graph.pb \
  --input_checkpoint=/tmp/checkpoints/inception_v3.ckpt \
  --input_binary=true \
  --output_graph=/tmp/frozen_inception_v3.pb \
  --output_node_names=InceptionV3/Predictions/Reshape_1
```
[label_image.py](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/examples/label_image)
```
$ python label_image.py \
  --image=${HOME}/Pictures/flowers.jpg \
  --input_layer=input \
  --output_layer=InceptionV3/Predictions/Reshape_1 \
  --graph=/tmp/frozen_inception_v3.pb \
  --labels=/tmp/imagenet_slim_labels.txt \
  --input_mean=0 \
  --input_std=255
```



<br><br>
#### 참고문헌
* https://github.com/tensorflow/models/tree/master/research/slim
* https://stackoverflow.com/questions/44516609/tensorflow-what-is-the-relationship-between-ckpt-file-and-ckpt-meta-and-ckp
