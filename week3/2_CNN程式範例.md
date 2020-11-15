# 體驗主題
```
KERAS
1.圖片分類==>使用MLP
2.圖片分類==>使用CNN
3.使用別人的Model[Google InceptionV3]進行圖片辨識
4.transfer Learning 遷移學習
```
### 史丹佛大學2017的CNN教學
```
Lecture Collection | Convolutional Neural Networks for Visual Recognition (Spring 2017)
16 部影片觀看次數：2,387,867次上次更新日期：2017年8月11日
https://www.youtube.com/playlist?list=PL3FW7Lu3i5JvHM8ljYj-zLfQRF3EO8sYv


Lecture 1 | Introduction to Convolutional Neural Networks for Visual Recognition
觀看次數：1,346,277次•2017年8月11日
https://www.youtube.com/watch?v=vT1JzLTH4G4
```

## 1.圖片分類==>使用MLP
```
https://www.tensorflow.org/tutorials/quickstart/beginner?hl=zh-tw
```
```
import tensorflow as tf

mnist = tf.keras.datasets.mnist

(x_train, y_train), (x_test, y_test) = mnist.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0
```
```
Training set訓練集
(x_train, y_train)==(訓練集資料,訓練集答案)

Test Data測試集
(x_test, y_test) ==(測試集資料,測試集答案)
```
### 定義模型
```
model = tf.keras.models.Sequential([
  tf.keras.layers.Flatten(input_shape=(28, 28)),
  tf.keras.layers.Dense(128, activation='relu'),
# tf.keras.layers.Dropout(0.2),
  tf.keras.layers.Dense(10)
])
```

### 查看模型架構
```
model.summary()
```
```
model.summary()
Model: "sequential"
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
flatten (Flatten)            (None, 784)               0         
_________________________________________________________________
dense (Dense)                (None, 128)               100480    
_________________________________________________________________
dropout (Dropout)            (None, 128)               0         
_________________________________________________________________
dense_1 (Dense)              (None, 10)                1290      
=================================================================
Total params: 101,770
Trainable params: 101,770
Non-trainable params: 0
```
```
(784+1)*128=100480    
(128+1)*10=1290
```
### 定義損失函數
```
#The losses.SparseCategoricalCrossentropy loss takes a vector of logits and a True index and returns a scalar loss for each example

loss_fn = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)
```
### 設定其他參數
```
model.compile(optimizer='adam',
              loss=loss_fn,
              metrics=['accuracy'])
```
```
optimizer=最佳化演算法
loss=損失函數
metrics=評估指標
    accuracy==答對的比率
```
### 使用fit()進行訓練
```
model.fit(x_train, y_train, epochs=5)
```
### 使用evaluate()進行準確度計算
```
model.evaluate(x_test,  y_test, verbose=2)
```
### 使用predict()進行預測

# 2.圖片分類==>使用CNN
```
https://www.tensorflow.org/tutorials/images/cnn?hl=zh-tw
```
```

```
# 3.使用別人的Model[Google InceptionV3]進行圖片辨識
```
#下載網路上的一張芒果圖
#!wget https://images.freeimages.com/images/large-previews/0cd/mango-1327290.jpg

#改自己上傳一張圖片
from google.colab import files
print('上傳一張要辨識的圖片')
uploaded = files.upload()
#檔案及大小
for fn in uploaded.keys():
  print('User uploaded file "{name}" with length {length} bytes'.format(
      name=fn, length=len(uploaded[fn])))
#上傳的檔案名      
fn  
```
```
# -*- coding: utf-8 -*-

import numpy as np
import matplotlib.pyplot as plt
import tensorflow as tf

## 使用Google InceptionV3模型
model = tf.keras.applications.InceptionV3(include_top=True, weights='imagenet')

## 看看Google InceptionV3模型的結構
model.summary()

from tensorflow.keras.applications.inception_v3 import preprocess_input
from tensorflow.keras.applications.inception_v3 import decode_predictions


# 撰寫讀取圖片的函數
def read_img(img_path, resize=(299,299)):
    img_string = tf.io.read_file(img_path)  # 讀取檔案
    img_decode = tf.image.decode_image(img_string)  # 將檔案以影像格式來解碼
    img_decode = tf.image.resize(img_decode, resize)  # 將影像resize到網路輸入大小
    # 將影像格式增加到4維(batch, height, width, channels)，模型預測要求格式
    img_decode = tf.expand_dims(img_decode, axis=0)
    return img_decode

# 
#img_path = 'mango-1327290.jpg' # 要辨識的圖片
img_path = fn  # 要辨識的上傳圖片

img = read_img(img_path) #讀取圖片

img = preprocess_input(img)  # 圖片前處理

preds = model.predict(img)  # 預測圖片

print("Predicted:", decode_predictions(preds, top=3)[0])  # 輸出預測最高的三個類別
```
## 4.transfer Learning 遷移學習
```
https://www.tensorflow.org/tutorials/images/transfer_learning_with_hub?hl=zh-tw
```
```

```
