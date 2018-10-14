---
layout: post
title: Kungfu effect for video
img: leimu.jpg
date: 2018-10-14 24:00:00
tag: [CV]

---



思路： 保留历史画面给予一定的权重并与当前画面进行叠加，形成一种残影的功夫效果。

```python
# -*- coding: utf-8 -*-
"""
    Created on Thu Nov 3 23:42:57 2016
    @author: hl
"""

import cv2

image_list = []
cameraCapture = cv2.VideoCapture(0)
cv2.namedWindow('MyWindow')

success, frame = cameraCapture.read()

frameid = 0
while success and cv2.waitKey(1) == -1:
    success, frame = cameraCapture.read()
    frame = cv2.cvtColor(frame, cv2.COLOR_BGR2BGRA)
    frame_temp = frame.copy()
    if image_list.__len__() == 30:
        for i in range(30):
            if i % 3 == 0:
                frame_temp = cv2.addWeighted(frame_temp, 0.8, image_list[29 - i], 0.2, 0.0)
    if image_list.__len__() == 30:
        image_list.pop(0)
    image_list.append(frame)

    cv2.imshow('MyWindow', frame_temp)

    frameid += 1

cv2.destroyWindow('MyWindow')

cameraCapture.release()
```

