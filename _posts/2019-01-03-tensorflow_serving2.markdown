---
layout: post
title:  "Tensorflow Serving2"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---


### Tensorflow Serving 예제

```
import tensorflow as tf
import os

tf.app.flags.DEFINE_integer('model_version', 1, 'version number of the model.')
FLAGS = tf.app.flags.FLAGS

# define the tensorflow network and do some trains
x = tf.placeholder("float", name="x")
w = tf.Variable(2.0, name="w")
b = tf.Variable(0.0, name="bias")

h = tf.multiply(x, w)
y = tf.add(h, b, name="y")
sess = tf.Session()
sess.run(tf.global_variables_initializer())


# save the model
export_path_base = '/tmp/savedmodel'
export_path = os.path.join(
    tf.compat.as_bytes(export_path_base),
    tf.compat.as_bytes(str(FLAGS.model_version)))
print('Exporting trained model to', export_path)

# Saving
builder = tf.saved_model.builder.SavedModelBuilder(export_path)

tensor_info_x = tf.saved_model.utils.build_tensor_info(x)
tensor_info_y = tf.saved_model.utils.build_tensor_info(y)

prediction_signature = (
  tf.saved_model.signature_def_utils.build_signature_def(
      inputs={'x_input': tensor_info_x},
      outputs={'y_output': tensor_info_y},
      method_name=tf.saved_model.signature_constants.PREDICT_METHOD_NAME))

builder.add_meta_graph_and_variables(
  sess, [tf.saved_model.tag_constants.SERVING],
  signature_def_map={
      tf.saved_model.signature_constants.DEFAULT_SERVING_SIGNATURE_DEF_KEY:
          prediction_signature,
      # "predict_x": prediction_signature
  },
  )
builder.save()
```

위의 코드를 실행하게 되면 `/tmp/savedmodel`에 `.pb`와 `variables`가 생성된다.
위의 예제는 간단하게 2배를 출력하는 예제이다.

<br>

* `/tmp/savedmodel`의 파일 구조이다.
```
kimjaehun@kimjaehun-HP-Pro-3330-MT:/tmp/savedmodel$ tree
.
└── 1
    ├── saved_model.pb
    └── variables
        ├── variables.data-00000-of-00001
        └── variables.index
```


<br>
### tensorflow_model_server 실행
<br>

옵션은 `--model_name=`은 `testmodel`은 요청 시에 사용된다. `--model_base`는 model이 저장된 경로이다. `rest_api_port`는 REST api 호출시 포트 번호이다.
```
tensorflow_model_server --model_name=testmodel --model_base_path=/tmp/savedmodel --rest_api_port=60001
```

<br>
### URL 요청
<br>

* Model status API

```
#GET
http://127.0.0.1:60001/v1/models/testmodel
```
```
{
    "model_version_status": [
        {
            "version": "1",
            "state": "AVAILABLE",
            "status": {
                "error_code": "OK",
                "error_message": ""
            }
        }
    ]
}
```

* REST API 호출

```
#POST
127.0.0.1:60001/v1/models/testmodel:predict

#body
{"instances": [1.0]}

curl -d '{"inputs": [1.0]}' -X POST 127.0.0.1:60001/v1/models/testmodel:predict
```

```
{
    "predictions": [
        2
    ]
}
```


<br><br>
#### 참고문헌
* https://guillaumegenthial.github.io/serving.html
