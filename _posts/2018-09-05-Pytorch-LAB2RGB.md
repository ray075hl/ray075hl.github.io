---
layout: post
title: Pytorch Image LAB To RGB
data: 2018-09-06 18:00:00
img: pytorch_lab2rgb.jpg
---
## <center>Pytorch Image LAB To RGB</center>
在使用Pytorch时, 将LAB格式的图片转化为RGB图片,采用mask的方式能够充分  
利用GPU的单指令多数据式的并行计算模式.
```python
import torch
import numpy as np
import cv2


def torch_lab2rgb(lab_image, batch_size):
    '''
    :param lab_image: [0., 1.]  format: Bx3XHxW
    :param batch_size:
    :return: rgb_image [0., 1.]
    '''
    
    #rgb_image = torch.zeros(lab_image.size()).to(config.device)
    rgb_image = torch.zeros(lab_image.size()).cuda()
    
    l_s = lab_image[:, 0, :, :] * 100
    a_s = (lab_image[:, 1, :, :] * 255) - 128
    b_s = (lab_image[:, 2, :, :] * 255) - 128

    var_Y = (l_s + 16.0) / 116.
    var_X = a_s / 500. + var_Y
    var_Z = var_Y - b_s / 200.

    mask_Y = var_Y.pow(3.0) > 0.008856
    mask_X = var_X.pow(3.0) > 0.008856
    mask_Z = var_Z.pow(3.0) > 0.008856

    Y_1 = var_Y.pow(3.0) * mask_Y.float()
    Y_2 = (var_Y - 16. / 116.) / 7.787 * (~mask_Y).float()
    var_Y = Y_1 + Y_2 

    X_1 = var_X.pow(3.0) * mask_X .float()
    X_2 = (var_X - 16. / 116.) / 7.787 * (~mask_X).float()
    var_X = X_1 + X_2 

    Z_1 = var_Z.pow(3.0) * mask_Z.float()
    Z_2 = (var_Z - 16. / 116.) / 7.787 * (~mask_Z).float()
    var_Z = Z_1 + Z_2 


    X = 0.95047 * var_X
    Y = 1.00000 * var_Y
    Z = 1.08883 * var_Z

    var_R = X *  3.2406 + Y * -1.5372 + Z * -0.4986
    var_G = X * -0.9689 + Y *  1.8758 + Z *  0.0415
    var_B = X *  0.0557 + Y * -0.2040 + Z *  1.0570

    mask_R = var_R > 0.0031308
    R_1 = (1.055 * var_R.pow(1/2.4) - 0.055) * mask_R.float()
    R_2 = (12.92 * var_R) * (~mask_R).float()
    var_R = R_1 + R_2

    mask_G = var_G> 0.0031308
    G_1 = (1.055 * var_G.pow(1/2.4) - 0.055) * mask_G.float()
    G_2 = (12.92 * var_G) * (~mask_G).float()
    var_G = G_1 + G_2
    
    mask_B = var_B > 0.0031308
    B_1 = (1.055 * var_B.pow(1/2.4) - 0.055) * mask_B.float()
    B_2 = (12.92 * temp_B) * (~mask_B).float()
    var_B = B_1 + B_2

    return torch.stack((var_R.unsqueeze(1), 
                        var_G.unsqueeze(1), 
                        var_B.unsqueeze(1)), dim=1).clamp(0., 1.)


# Unit test
if __name__ == '__main__':
    image_bgr = cv2.imread('test.png')
    image_lab = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2Lab)
    image_lab = 1.0 * image_lab /255.0
    image_lab = np.transpose(image_lab, (2, 1, 0))  # channel first

    tensor_labimg = torch.from_numpy(image_lab).\
        unsqueeze(0).float()  # add batch dim
    tensor_rgb = torch_lab2rgb(tensor_labimg, 1)[0]
    numpy_rgb = tensor_rgb.cpu().numpy()
    rgb_img = (np.transpose(numpy_rgb, (2, 1, 0)) * 255.0).astype('uint8')
    cv2.imwrite('result.png', np.flip(rgb_img, 2))
```

### Reference
[https://docs.opencv.org/3.3.0/de/d25/imgproc_color_conversions.html#color_convert_rgb_lab](https://docs.opencv.org/3.3.0/de/d25/imgproc_color_conversions.html#color_convert_rgb_lab)