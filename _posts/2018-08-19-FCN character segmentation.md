---
layout: post
title: FCN character segmentation
date: 2018-08-19 23:50:00
img: moon.jpg
---
使用FCN来做字符分割，参考这篇文章的算法
Chinese/English mixed Character Segmentation as Semantic Segmentation
算法很简单，只有两个关键步骤
1.搭建FCN的网络框架。
2.解决正负样本不均衡的问题。
```python
import tensorflow as tf
import keras
from keras.layers import Conv2D, MaxPooling2D, Input, LSTM, GRU, Masking, UpSampling2D
from keras.layers import Activation, Dropout, Flatten, Dense, merge, Reshape, Lambda
from keras.preprocessing.image import ImageDataGenerator
from keras.preprocessing.sequence import pad_sequences
from keras.models import Model
from keras import backend as K
from keras.models import *
from keras.layers import *
import numpy as np
import cv2

#----------------load training data--------------------------
training_img = np.load('fcn_training_data.npy')
training_label = np.load('fcn_training_label.npy')
#----------------load training data--------------------------


sess = tf.Session()
K.set_session(sess)
img_rows = 48   #  height
img_cols = 512  #  width
# tensorflow inputs
# inputs = tf.placeholder(tf.float32, shape=(None, img_rows, img_cols, 3))
labels = tf.placeholder(tf.float32, shape=(None, img_cols))
alpha = tf.placeholder(tf.float32)
beta = tf.placeholder(tf.float32)
learning_rate = tf.placeholder(tf.float32)
inputs = Input(shape=(img_rows, img_cols, 3))  # keras

conv1 = Conv2D(filters=32, kernel_size=(2, 2), activation='relu', padding='same')(inputs)
pool1 = MaxPooling2D(pool_size=(2, 2))(conv1)
conv2 = Conv2D(filters=64, kernel_size=(2, 2), activation='relu', padding='same')(pool1)
pool2 = MaxPooling2D(pool_size=(2, 2))(conv2)
conv3 = Conv2D(filters=128, kernel_size=(2, 2), activation='relu', padding='same')(pool2)
pool3 = MaxPooling2D(pool_size=(2, 2))(conv3)
conv4 = Conv2D(filters=256, kernel_size=(2, 2), activation='relu', padding='same')(pool3)
pool4 = MaxPooling2D(pool_size=(2, 2))(conv4)
conv5 = Conv2D(filters=512, kernel_size=(3, 2), activation='relu', padding='same')(pool4)
pool5 = MaxPooling2D(pool_size=(3, 2))(conv5)

up1 = UpSampling2D(size=(1, 2))(pool5)
up_conv1 = Conv2D(filters=512, kernel_size=(1, 2), activation='relu', padding='same')(up1)
up2 = UpSampling2D(size=(1, 2))(up_conv1)
up_conv2 = Conv2D(filters=256, kernel_size=(1, 2), activation='relu', padding='same')(up2)
up3 = UpSampling2D(size=(1, 2))(up_conv2)
up_conv3 = Conv2D(filters=128, kernel_size=(1, 2), activation='relu', padding='same')(up3)
up4 = UpSampling2D(size=(1, 2))(up_conv3)
up_conv4 = Conv2D(filters=64, kernel_size=(1, 2), activation='relu', padding='same')(up4)
up5 = UpSampling2D(size=(1, 2))(up_conv4)
up_conv5 = Conv2D(filters=1, kernel_size=(1, 2), activation='sigmoid', padding='same')(up5)

out = Flatten()(up_conv5)
model = Model(inputs=[inputs], outputs=[out])
loss = -tf.reduce_mean(alpha*(labels)*tf.log(out) + beta*(1.0-labels)*tf.log(1.0-out))*1000
train_setp = tf.train.MomentumOptimizer(learning_rate, 0.9).minimize(loss)


init_op = tf.global_variables_initializer()
sess.run(init_op)
TRAINING_EPOCHS = 10
with sess.as_default():
    batch_num = 0
    acc_pos = 0.0
    acc_neg = 0.0
    al = 0.9
    be = 0.1
    lr = 0.0001
    for epoch in range(TRAINING_EPOCHS):
        for i in range(5000):

            a = 0.0
            b = 0.0
            c = 0.0
            d = 0.0

            #x_batch, y_batch = next(name_training_data_generator(8))
            x_batch = training_img  [i*10:i*10+10]
            y_batch = training_label[i*10:i*10+10]
            train_setp.run(feed_dict={inputs: x_batch, labels: y_batch, 
                                      alpha: al, beta: be, learning_rate: lr})
            out_result = sess.run([out], feed_dict={inputs: x_batch, labels: y_batch,                                           alpha: al, beta: be, learning_rate: lr})
            loss_result = sess.run([loss], feed_dict={inputs: x_batch, labels: y_batch,                                         alpha: al, beta: be, learning_rate: lr})
            y_pred = out_result[0] # shape [10, 512]
            y_pred[y_pred > 0.5] = 1
            y_pred[y_pred < 0.5] = 0
            h, w = y_pred.shape[0], y_pred.shape[1]

            # ---------------------------------------------
            a = np.sum(y_pred*y_batch)
            b = np.sum(y_batch)
            for i in range(h):
                for j in range(w):
                    if y_pred[i,j] == y_batch[i,j] == 0:
                        c += 1.0
            d = h*w - b
            acc_pos = a/b
            acc_neg = c/d
            if acc_pos<acc_neg:
                delta = min(be, 0.001)
                al = al + delta
                be = be - delta
            else:
                delta = min(al, 0.001)
                al = al - delta
                be = be + delta
            # ----------------------------------------------

            batch_num += 1
            print('batch_num: ',batch_num,' loss: ',loss_result, ' al: ', al, ' be: ', be)
            if batch_num % 500 == 0:
                """
                save model
                """
            if batch_num > 20000:
                lr = lr/10
            if batch_num > 40000:
                lr = lr/10
```
## reference
[Chinese/English mixed Character Segmentation as Semantic Segmentation](https://arxiv.org/abs/1611.01982)