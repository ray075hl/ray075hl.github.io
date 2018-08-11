# Pytorch bilinear interpolation

```python
import cv2
import numpy as np

import torch
import torch.nn.functional as F

import matplotlib.pyplot as plt


def bilinear_interpolation(tensor, target_h, target_w):
    xc = torch.linspace(-1, 1, target_w).repeat(target_h, 1)
    yc = torch.linspace(-1, 1, target_h).view(-1,1).repeat(1, target_w)
    
    grid = torch.cat( (xc.unsqueeze(2), yc.unsqueeze(2)), 2)
    grid = grid.unsqueeze(0)
    
    t_array = F.grid_sample(tensor, grid).squeeze(0).data.numpy()
    
    return t_array

if __name__ == '__main__':
    image = cv2.imread('note3.jpg')
    array = image.transpose(2, 0, 1)  # C x H x W
    tensor_array = torch.from_numpy(array).unsqueeze(0).type(torch.FloatTensor)
    
    target_h = 300
    target_w = 600
    
    resized_array = bilinear_interpolation(tensor_array, target_h, target_w)
    resized_array = resize_array.transpose(1, 2, 0)
    
    plt.figure(1)
    plt.imshow(np.flip(resized_array, 2).astype('uint8'))
    plt.show()
    
    
```






