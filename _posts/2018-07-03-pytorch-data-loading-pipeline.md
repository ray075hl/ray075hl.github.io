#Pytorch data loading pipeline
There are three classes we will use to customize our own dataset pipeline on usual.


## Dataset class
```python
from torch.utils.data import Dataset
```
**Dataset** is an ablstract class representing a dataset. 
Custom dataset should inherit **Dataset** and override the following methods:

*    **\__len\__** so that **len(dataset)** returns the size of the dataset.
*    **\__getitem\__** to support the indexing such that **dataset[i]** can be used to get
**i th** sample 

We using **minist** to explain this class

```python
from skimage import io
import numpy as np

class MnistDataset(Dataset):
    def __init__(self, root_dir_image, root_dir_label, transform=None):
        """
        Args: 
            root_dir (string): Directory with all the images and labels.
            transform (callable, optional): Optional transform to be applied
                on a sample.
        """
        """
        Suppose data storage as follow: 1.jpg 2.jpg 3.jpg ...
                                        1.txt 2.txt 3.txt ...
        """
        self.root_dir_image = root_dir_image
        self.root_dir_label = root_dir_label
        self.transform = transform

    def __len__(self):
        return len(os.listdir(self.root_dir_image))

    def __getitem__(self, idx)
        img_name = os.path.join(self.root_dir_image, idx+'.jpg')
        label_name = os.path.join(self.root_dir_label, idx+'.txt')
        
        image = io.imread(img_name)
        label = np.loadtxt(label_name)  # assume already one hot encoded
        
        sample = {'image' : image, 'label' : label}
    
        if self.tranform:
            sample = tranform(sample)
    
        return sample
        
``` 

## DataLoader class
```python
from torch.utils.data import DataLoader
```
Using **MnistDataset** to obtain just one sample each time. As training a neural network(NN)
, batching the input data can accelerate convergence of NN. **DataLoader** can deal with that.

*    Batching the data
*    Shuffling the data
*    Load the data in parallel using **multiprocessing** workers.

```python
minist_data = MnistDataset('./images', './label', None)
dataloader = DataLoader(minist_data, batch_size=4, 
                        shuffle=True, num_workers=4)


if __name__ == '__main__':
    for i_batch, sample_batched in enumerate(dataloader):
        print(i_batch, sample_batched['image'].size().
             sample_batched['label'].size())

```


## Transforms class
Data argumentation will using this class in general.
```python
import cv2
from skimage import transform
from torchvision import transforms, utils

"""
Using opencv and skimage to transform a image
for example.
"""
class Recale(object):
    """
    Recalse the image in a sample to a given size.
    """
    def __init__(self, output_size):
        pass
     
    def __call__(self, sample):
        pass
        return a_recale_sample
        
class Crop(object):  
    """
    Random crop a image
    """
    def __init__(self, output_size):
        pass
        
    def __call__(self, sample):
        pass
        return a_cropped_sample


class ToTensor(object):
    pass


"""
Intergate with Dataset class
"""
scale = Recale(224)
crop = Crop(200)
composed = tranform.Compose([Recale(224), Crop(200), ToTensor()])

transformed_minist_dataset = MnistDataset('./images', './label', transform=composed)
transformed_dataloader = DataLoader(transformed_minist_dataset, batch_size=4, 
                        shuffle=True, num_workers=4)
```



## Reference
[Data Loading and Processing Tutorial](https://pytorch.org/tutorials/beginner/data_loading_tutorial.html)
