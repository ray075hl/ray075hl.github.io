---
layout: post
title: Put scipy sparse matrix to pytorch
date: 2018-08-12 23:48:00
img: sparse_matrix.jpg
---

```python
import numpy as np
import torch


def convert(M):
    """
    input: M is Scipy sparse matrix
    output: pytorch sparse tensor in GPU 
    """
    M = M.tocoo().astype(np.float32)
    indices = torch.from_numpy(np.vstack((M.row, M.col))).long().cuda()
    values = torch.from_numpy(M.data).cuda()
    shape = torch.Size(M.shape)
    Ms = torch.sparse_coo_tensor(indices, values, shape, device=torch.device('cuda'))
    return Ms
```

