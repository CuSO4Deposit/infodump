---
date: 2023-03-27
title: Machine Learning Quickref
tags:
categories:
lastMod: 2025-10-18
--- 
# Matplotlib

Recommend using OO-style:

```python
from matplotlib import pyplot as plt

def draw_image(data: dict):
  """
  data = {"data1": [x0, y00, y01], "data2": [x1, y10, y11],
          "param": {"structure", "lr", "activation"}
          }
  """
  fig, axs = plt.subplots(nrows=1, ncols=3, layout="tight", figsize=[9.6, 4.8])
  axs[0].plot(data["data1"][0], data["data1"][1], label="target")
  axs[0].plot(data["data1"][0], data["data1"][2], label="model")
  axs[0].set_xlabel("x")
  axs[0].set_ylabel("y = f(x)")
  axs[0].set_title("Fitting")
  axs[0].legend()
  ...
  fig.suptitle(f'structure: {data["param"]["structure"]}\
          lr: {data["param"]["lr"]}\
          activation: {data["param"]["activation"]}')

  fig.savefig('./figure/figure.png')
  plt.show()
```

When operating on images, image input and ndarray input are both supported:

```python
from matplotlib.images import imread
#...
axs[0].imshow(imread('a.png'))

import numpy as np
from PIL import Image
#...
axs[1].imshow(np.asarray(Image.open('a.png')))

plt.show()
```
  
# PyTorch

[Learn the Basics — PyTorch Tutorials 2.0.0+cu117 documentation](https://pytorch.org/tutorials/beginner/basics/)
  
## Device

```python
device = "cuda" if torch.cude.is_available() else "cpu"
print(f"Device: {device}")

tensor = tensor.to(device)
model = model.to(device)
```
  
## Tensors

[torch.Tensor - PyTorch 2.0 documentation](https://pytorch.org/docs/stable/tensors.html)
  
### Create tensors

```python
shape = (2, 3)
tensorr = torch.rand(shape)
tensor0 = torch.zeros(shape)
tensor1 = torch.ones(shape)
tensorn = torch.from_numpy(np.array([...]))
```
  
### Attributes

`tensor.shape`

`tensor.dtype`

`tensor.device`

`tensor.require_grad`
  
### Methods

[torch.Tensor.detach()](https://pytorch.org/docs/stable/generated/torch.Tensor.detach.html?highlight=detach#torch.Tensor.detach): returns a tensor sharing same storage with original tensor but never requiring grad (so that can be convert to normal datatypes if on cpu)

[torch.Tensor.item()](https://pytorch.org/docs/stable/generated/torch.Tensor.item.html?highlight=item#torch.Tensor.item): works when only one element in tensor

```python
tensor = Tensor([12.0])
print(tensort.item(), type(tensor.item())

# 12.0 <class 'float'>
```

[torch.Tensor.tolist()](https://pytorch.org/docs/stable/generated/torch.Tensor.tolist.html#torch.Tensor.tolist)

Add "\_" suffix to operations means inplace operations. Example: `tensor.add_()`.
  
## Datasets & DataLoaders

primitives: 

`torch.utils.data.DataLoader`: wraps an iterable around `Dataset`

`torch.utils.data.Dataset`: samples and labels
  
### Load FashionMNIST

```python
import torch
from torch.utils.data import Dataset
from torchvision import datasets
from torchvision.transforms import ToTensor
import matplotlib.pyplot as plt


training_data = datasets.FashionMNIST(
  root="data",    # path to data
  train=True,    # specify training/test dataset 
  download=True,    # if not available at root, whether download from internet
  transform=ToTensor()
)

test_data = datasets.FashionMNIST(
  root="data",
  train=False,
  download=True,
  transform=ToTensor()
)
```

We can index `Datasets` manually like a list: `training_data[index]`.
We use `matplotlib` to visualize some samples in our training data.

```python
labels_map = {
  0: "T-Shirt",
  1: "Trouser",
  2: "Pullover",
  3: "Dress",
  4: "Coat",
  5: "Sandal",
  6: "Shirt",
  7: "Sneaker",
  8: "Bag",
  9: "Ankle Boot",
}
sample_idx = torch.randint(len(training_data), size=(1,)).item()
img, label = training_data[sample_idx]
plt.title(labels_map[label])
```
  
### Wrap my data to dataset

PyTorch has 2 kinds of dataset: `map` and `iterate`. We focus on the first kind.

All datasets should subclass `torch.utils.data.Dataset`.

All datasets should overwrite `__getitem()__`, and optionally overwrite `__len__()`.

My data should have: a file containing the map each data's filename to its label; a data directory.

A sample dataset:

```python
from torch.utils.data import Dataset
import pandas as pd
from pathlib import Path

class MyDataset(Dataset):
  def __init__(self, annotation_file="./dataset/label.csv",\
          data_dir="./dataset/data/", transform=None, target_transform=None):
      self.labels = pd.read_csv(annotation_file)
      self.data_dir = data_dir

  def __getitem__(self, index):
      path = Path(self.dir) + Path(self.labels.iloc[index, 0])
      item = read_item(path)  # read_item() TODO
      label = self.labels.iloc[index, 1]
      if self.transform:
          item = self.transform(item)
      if self.target_transform:
          label = self.target_transform(label)
      return item, label

  def __len__(self):
      return len(self.labels)
```
  
### Useful APIs

[torch.utils.data.ImageFolder](https://pytorch.org/vision/stable/generated/torchvision.datasets.ImageFolder.html?highlight=imagefolder#torchvision.datasets.ImageFolder)

[torch.utils.data.random_split()](https://pytorch.org/docs/stable/data.html?highlight=random_split#torch.utils.data.random_split)
 