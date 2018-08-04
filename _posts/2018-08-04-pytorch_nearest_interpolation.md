# Pytorch nearest interpolation

## numpy nearest interpolation 



```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def nearest_inter(array, height, width):
  '''
  array:  channel x height x width
  '''
  channel, ori_h, ori_w = array.shape
  ratio_h = ori_h / height
  ratio_w = ori_w / width
  target_array = np.zeros((channel, height, width))
  for i in range(height):
    for j in range(width):
      th = int(i * ratio_h)
      tw = int(j * ratio_w)
      target_array[:, i, j] = array[:, th, tw]
  return target_array

if __name__ == '__main__':
  image = cv2.irmead('note3.jpg')
  array = image.transpose(2, 0, 1)
  t_array = nearest_inter(array, 1200, 500)
  t_array = t_array.transpose(1, 2, 0)
  
  plt.imshow(np.flip(t_array, 2).astype('uint8'))
  plt.show()
```





