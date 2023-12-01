---
title: Pytorch的学习笔记
date: 2023.7.14
tags: [Pytorch, 计算机视觉]
categories: 科研
---



# Pytorch的学习笔记

## Pytorch的安装

### 下载anconda

### 在anconda建立一个环境

```
conda create -n pytorch python=3.9
```

### 进入pytorch环境中

```
conda activate pytorch
```

### 登入官网

 [Pytorch官网](https://pytorch.org/)

下拉选择选项，并复制代码，在conda命令框中下载。

![选项页面](https://s2.loli.net/2023/07/07/eDrtNZT1uzonVdb.png)



### 检查是否安装成功

先要进入python环境，在命令行中输入`Python` 

```py
import torch
torch.cuda.is_available()
```

如果显示`True`或者`False`即为成功

### Pycharm安装和JupterNotebook安装

## Pytorch基础使用

### 基础函数

###### 用于打开内容
```
dir()
```



###### 帮助文档
```
help()
# 注意如果是函数要去除函数的括号
```

## Dataset数据加载

```py
from torch.utils.data import Dataset # 用于加载数据
from PIL import Image # 用于加载图片数据
import os # 可以操作系统文件

# 自定义myData继承Dataset类
class myData(Dataset):
    # 构造函数
    def __init__(self, root_dir, label_dir):
        self.root_dir = root_dir
        self.label_dir = label_dir
        self.path = os.path.join(self.root_dir, self.label_dir) # 用于将两个地址拼接起来
        self.img_path = os.listdir(self.path) # 将当前路径的中的所有文件名返回为一个列表

	# 重载索引获取列表对象的函数
    def __getitem__(self, idx):
        img_name = self.img_path[idx]
        img_item_path = os.path.join(self.path, img_name)
        img = Image.open(img_item_path)
        label = self.label_dir
        return img, label
    
	# 重载获取数据长度的函数
    def __len__(self):
        return len(self.img_path)

root_dir = "练手数据集/train"
ants_dir = "ants_image"
ants_dataset = myData(root_dir, ants_dir)
print(len(ants_dataset))
img, label = ants_dataset[1]
img.show()
```

**注意：**

如果是绝对路径要在字符串前面加上`r`，避免出现转义

`self`相当于类的全局变量，便于类中其他的函数访问到另一个函数的中的变量，类似于C++和java中的`this`





## tensorboard的使用

### 创建logs文件

```py
from torch.utils.tensorboard import SummaryWriter

# 创建名为logs的文件读写器
writer =  SummaryWriter("logs")

for i in range(100):
    writer.add_scalar("y=x", i, i)

writer.close()
```



### 打开logs文件

```
tensorboard --logdir=logs
```

注意：

可以通过添加选项`--post=6001`指定启动端口

要在终端运行

### OpenCV的使用

```

```

## Transforms的使用

### 概述

是一个工具箱，主要用于提供进行图像的处理的工具，主要有`ToTensor`，`resize`。

### ToTensor

将图片转换为`Tensor`的数据类型



```py
from PIL import Image
from torchvision import transforms

img_path = "练手数据集/train/ants_image/0013035.jpg"
img = Image.open(img_path)
print(img)

tensor_trans = transforms.ToTensor()
tensor_img = tensor_trans(img)
```

## torchvision的简单使用

```py
import torchvision
from torch.utils.tensorboard import SummaryWriter

dataset_transform = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor()
])

train_set = torchvision.datasets.CIFAR10(root='./dataset', train=True, download=True,
                                         transform=dataset_transform)

test_set = torchvision.datasets.CIFAR10(root='./dataset', train=False, download=True,
                                        transform=dataset_transform)
# print(test_set[0])
# print(test_set.classes)
#
# img, target = test_set[0]
# print(img)
# print(target)
# print(test_set.classes[target])
# img.show()

writer = SummaryWriter('logs')
for i in range(10):
    img, target = test_set[i]
    writer.add_image("CIFAR10", img, i)
writer.close()

```

## DataLoader的使用

```
import torchvision
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

dataset_transform = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor()
])

test_set = torchvision.datasets.CIFAR10(root='./dataset', train=False, download=True,
                                        transform=dataset_transform)

test_loader = DataLoader(dataset=test_set, batch_size=4, shuffle=True, num_workers=0, drop_last=False)

# print(test_set[0])

writer = SummaryWriter('logs')
step = 0
for data in test_loader:
    imgs, targets = data
    writer.add_images("data_loader", imgs, step)
    step += 1
writer.close()

```

##  神经网络框架

```py
import torch
from torch import nn


class Module(nn.Module):
    def __init__(self):
        super().__init__()

    def forward(self, input):
        output = input + 1.0
        return output


module = Module()
x = torch.tensor(1.0)
output = module(x)
print(output)

```

## 卷积神经网络(1D)

```py
import torch
import torch.nn.functional as F

# 输入层
input = torch.tensor([[1, 2, 0, 3, 1],
                      [0, 1, 2, 3, 1],
                      [1, 2, 1, 0, 0],
                      [5, 2, 3, 1, 1],
                      [2, 1, 0, 1, 1]])
# 卷积核
kernel = torch.tensor([[1, 2, 1],
                       [0, 1, 0],
                       [2, 1, 0]])
# 转换数据类型
input = torch.reshape(input, (1, 1, 5, 5))
kernel = torch.reshape(kernel, (1, 1, 3, 3))

print(input.shape)
print(kernel.shape)

output = F.conv2d(input, kernel, stride=1)
print(output)

```

## 卷积神经网络(2D)

```py
import torchvision
from torch import nn
from torch.nn import Conv2d
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter
import torch

# 定义数据集的转换操作
dataset_transform = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor()
])

# 加载测试数据集
test_set = torchvision.datasets.CIFAR10(root='./dataset', train=False, download=True,
                                        transform=dataset_transform)

# 创建测试数据加载器
test_loader = DataLoader(dataset=test_set, batch_size=64, shuffle=True, num_workers=0, drop_last=False)

# 定义自定义模型
class Module(nn.Module):
    def __init__(self):
        super(Module, self).__init__()
        self.conv1 = Conv2d(in_channels=3, out_channels=6, kernel_size=3, stride=1, padding=0)

    def forward(self, x):
        x = self.conv1(x)
        return x

module = Module()

# 创建用于写入TensorBoard日志的SummaryWriter对象
writer = SummaryWriter("logs")

step = 0
# 对测试数据进行迭代
for data in test_loader:
    imgs, targets = data

    # 前向传播计算输出
    output = module(imgs)

    print(imgs.shape)
    print(output.shape)

    # 将输入图像写入TensorBoard
    writer.add_images("input", imgs, step)

    # 重塑输出张量的形状
    output = torch.reshape(output, (-1, 3, 30, 30))

    # 将输出图像写入TensorBoard
    writer.add_images("output", output, step)

    step += 1
```

## 线性模型

```py
import torchvision
from torch import nn
from torch.nn import MaxPool2d, ReLU, Sigmoid, Linear
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter
import torch

# 定义数据集的转换操作
dataset_transform = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor()
])

# 加载测试数据集
test_set = torchvision.datasets.CIFAR10(root='./dataset', train=False, download=True,
                                        transform=dataset_transform)

# 创建测试数据加载器
test_loader = DataLoader(dataset=test_set, batch_size=64, shuffle=False, num_workers=0, drop_last=True)

# 定义自定义模型
class Module(nn.Module):
    def __init__(self):
        super(Module, self).__init__()
        self.linear = Linear(196608, 10)

    def forward(self, x):
        x = self.linear(x)
        return x

module = Module()

# 对测试数据进行迭代
for data in test_loader:
    imgs, targets = data

    print(imgs.shape)

    # 展平输入张量
    output = torch.flatten(imgs)
    print(output.shape)

    # 运行模型的前向传播
    output = module(output)
    print(output)

```

## 损失函数

```py
from torch.nn import MSELoss, L1Loss
import torch

# 创建输入和目标张量
inputs = torch.tensor([1, 2, 3], dtype=torch.float32)
targets = torch.tensor([1, 2, 5], dtype=torch.float32)

# 重新调整张量的形状
inputs = torch.reshape(inputs, (1, 1, 1, 3))
targets = torch.reshape(targets, (1, 1, 1, 3))

# 创建L1损失函数对象
loss = L1Loss(reduction='sum')

# 计算L1损失
result = loss(inputs, targets)

# 创建MSE损失函数对象
loss_mse = MSELoss()

# 计算MSE损失
result_mse = loss_mse(inputs, targets)

# 打印结果
print(result)
print(result_mse)

```

## 神经网络的损失函数

```py
import torch
import torchvision
from torch import nn
from torch.nn import Linear, Conv2d, MaxPool2d, Flatten, Sequential, CrossEntropyLoss
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

# 定义数据集的转换操作
dataset_transform = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor()
])

# 加载测试数据集
test_set = torchvision.datasets.CIFAR10(root='./dataset', train=False, download=True,
                                        transform=dataset_transform)

# 创建测试数据加载器
test_loader = DataLoader(dataset=test_set, batch_size=64, shuffle=False, num_workers=0, drop_last=True)

# 定义自定义模型
class Module(nn.Module):
    def __init__(self):
        super(Module, self).__init__()
        self.model1 = Sequential(
            Conv2d(3, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 64, 5, padding=2),
            MaxPool2d(2),
            Flatten(),
            Linear(1024, 64),
            Linear(64, 10)
        )

    def forward(self, x):
        return self.model1(x)

# 创建交叉熵损失函数对象
loss = CrossEntropyLoss()

# 创建模型对象
module = Module()

# 对测试数据进行迭代
for data in test_loader:
    imgs, targets = data

    # 前向传播计算输出
    output = module(data)

    # 计算损失
    result_loss = loss(output, targets)

    # 反向传播
    result_loss.backward()

```

## 优化器

```py
import torchvision
from torch import nn
from torch.nn import Linear, Conv2d, MaxPool2d, Flatten, Sequential, CrossEntropyLoss
from torch.optim import SGD
from torch.utils.data import DataLoader

# 定义数据集的转换操作
dataset_transform = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor()
])

# 加载测试数据集
test_set = torchvision.datasets.CIFAR10(root='./dataset', train=False, download=True,
                                        transform=dataset_transform)

# 创建测试数据加载器
test_loader = DataLoader(dataset=test_set, batch_size=64, shuffle=False, num_workers=0, drop_last=True)

# 定义自定义模型
class Module(nn.Module):
    def __init__(self):
        super(Module, self).__init__()
        self.model1 = Sequential(
            Conv2d(3, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 64, 5, padding=2),
            MaxPool2d(2),
            Flatten(),
            Linear(1024, 64),
            Linear(64, 10)
        )

    def forward(self, x):
        return self.model1(x)

# 创建交叉熵损失函数对象
loss = CrossEntropyLoss()

# 创建模型对象
module = Module()

# 创建优化器对象
optim = SGD(module.parameters(), lr=0.1)

# 进行训练循环
for epoch in range(20):
    running_loss = 0.0
    for data in test_loader:
        imgs, targets = data

        # 前向传播计算输出
        output = module(imgs)

        # 计算损失
        result_loss = loss(output, targets)

        # 梯度清零
        optim.zero_grad()

        # 反向传播
        result_loss.backward()

        # 更新模型参数
        optim.step()

        running_loss += result_loss

    print("第{}次循环, loss = {}".format(epoch, running_loss))

```

## 预训练

```py
import torchvision
from torch.nn import Linear

# 使用预训练的VGG16模型（pretrained=True）
vgg16_true = torchvision.models.vgg16(pretrained=True)
print(vgg16_true)

# 使用随机初始化的VGG16模型（pretrained=False）
vgg16_false = torchvision.models.vgg16(pretrained=False)
print(vgg16_false)

# 修改pretrained=True的VGG16模型的分类器层，并添加一个新的线性层
vgg16_true.classifier.add_module('add_linear', Linear(1000, 10))
print(vgg16_true)

# 修改pretrained=False的VGG16模型的分类器层的最后一个线性层
vgg16_false.classifier[6] = Linear(4096, 10)
print(vgg16_false)

```

## 模型保存

```py
import torch
import torchvision

# 从torchvision中获取预训练的VGG16模型（pretrained=False）
vgg16 = torchvision.models.vgg16(pretrained=False)

# 方法一：保存整个模型
torch.save(vgg16, "vgg16_method1.pth")

# 方法二：保存模型的状态字典
torch.save(vgg16.state_dict(), "vgg16_method2.pth")

# 陷阱
# 方法一中保存模型时，在加载模型时，如果是自定义模型还是，给出模型的结构

```

## 模型加载

```py
import torch
import torchvision

# 方法一：加载整个模型
vgg16_1 = torch.load("vgg16_method1.pth")
print(vgg16_1)

# 方法二：加载模型的状态字典并应用到新创建的模型对象上
vgg16_2 = torchvision.models.vgg16(pretrained=False)
vgg16_2.load_state_dict(torch.load("vgg16_method2.pth"))
print(vgg16_2)
```



## 用gpu训练模型

```py
# -*- coding: utf-8 -*-
# 作者：小土堆
# 公众号：土堆碎念
import torch
import torchvision
from torch.utils.tensorboard import SummaryWriter

# from model import *
# 准备数据集
from torch import nn
from torch.utils.data import DataLoader

# 定义训练的设备
device = torch.device("cuda")

train_data = torchvision.datasets.CIFAR10(root="../data", train=True, transform=torchvision.transforms.ToTensor(),
                                          download=True)
test_data = torchvision.datasets.CIFAR10(root="../data", train=False, transform=torchvision.transforms.ToTensor(),
                                         download=True)

# length 长度
train_data_size = len(train_data)
test_data_size = len(test_data)
# 如果train_data_size=10, 训练数据集的长度为：10
print("训练数据集的长度为：{}".format(train_data_size))
print("测试数据集的长度为：{}".format(test_data_size))


# 利用 DataLoader 来加载数据集
train_dataloader = DataLoader(train_data, batch_size=64)
test_dataloader = DataLoader(test_data, batch_size=64)

# 创建网络模型
class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        self.model = nn.Sequential(
            nn.Conv2d(3, 32, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 32, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 64, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Flatten(),
            nn.Linear(64*4*4, 64),
            nn.Linear(64, 10)
        )

    def forward(self, x):
        x = self.model(x)
        return x
tudui = Tudui()
tudui = tudui.to(device)

# 损失函数
loss_fn = nn.CrossEntropyLoss()
loss_fn = loss_fn.to(device)
# 优化器
# learning_rate = 0.01
# 1e-2=1 x (10)^(-2) = 1 /100 = 0.01
learning_rate = 1e-2
optimizer = torch.optim.SGD(tudui.parameters(), lr=learning_rate)

# 设置训练网络的一些参数
# 记录训练的次数
total_train_step = 0
# 记录测试的次数
total_test_step = 0
# 训练的轮数
epoch = 10

# 添加tensorboard
writer = SummaryWriter("../logs_train")

for i in range(epoch):
    print("-------第 {} 轮训练开始-------".format(i+1))

    # 训练步骤开始
    tudui.train()
    for data in train_dataloader:
        imgs, targets = data
        imgs = imgs.to(device)
        targets = targets.to(device)
        outputs = tudui(imgs)
        loss = loss_fn(outputs, targets)

        # 优化器优化模型
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

        total_train_step = total_train_step + 1
        if total_train_step % 100 == 0:
            print("训练次数：{}, Loss: {}".format(total_train_step, loss.item()))
            writer.add_scalar("train_loss", loss.item(), total_train_step)

    # 测试步骤开始
    tudui.eval()
    total_test_loss = 0
    total_accuracy = 0
    with torch.no_grad():
        for data in test_dataloader:
            imgs, targets = data
            imgs = imgs.to(device)
            targets = targets.to(device)
            outputs = tudui(imgs)
            loss = loss_fn(outputs, targets)
            total_test_loss = total_test_loss + loss.item()
            accuracy = (outputs.argmax(1) == targets).sum()
            total_accuracy = total_accuracy + accuracy

    print("整体测试集上的Loss: {}".format(total_test_loss))
    print("整体测试集上的正确率: {}".format(total_accuracy/test_data_size))
    writer.add_scalar("test_loss", total_test_loss, total_test_step)
    writer.add_scalar("test_accuracy", total_accuracy/test_data_size, total_test_step)
    total_test_step = total_test_step + 1

    torch.save(tudui, "tudui_{}.pth".format(i))
    print("模型已保存")

writer.close()

```

也可以采用这种方式来提高代码的复用性：

```py
# 如果有可用的 GPU，将损失函数移动到 GPU 上
if torch.cuda.is_available():
    loss_fn = loss_fn.cuda()
```



## 测试训练的模型

```py
# -*- coding: utf-8 -*-
# 作者：小土堆
# 公众号：土堆碎念
import torch
import torchvision
from PIL import Image
from torch import nn

# 定义图像路径
image_path = "imgs/dog_1.png"
# 使用PIL库打开图像
image = Image.open(image_path)
print(image)
# 将图像转换为RGB模式
image = image.convert('RGB')
# 定义图像转换操作，包括将图像大小调整为32x32，并将其转换为张量
transform = torchvision.transforms.Compose([torchvision.transforms.Resize((32, 32)),
                                            torchvision.transforms.ToTensor()])

# 加载CIFAR10测试数据集
test_data = torchvision.datasets.CIFAR10(root="dataset", train=False, transform=torchvision.transforms.ToTensor(),
                                         download=True)

# 对图像应用转换操作
image = transform(image)
print(image.shape)

# 创建网络模型
class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        self.model = nn.Sequential(
            nn.Conv2d(3, 32, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 32, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 64, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Flatten(),
            nn.Linear(64*4*4, 64),
            nn.Linear(64, 10)
        )

    def forward(self, x):
        x = self.model(x)
        return x

# 加载已训练的模型
model = torch.load("model/tudui_19.pth", map_location=torch.device('cpu'))
print(model)
# 将图像形状调整为(1, 3, 32, 32)的张量
image = torch.reshape(image, (1, 3, 32, 32))
# 将模型设置为评估模式
model.eval()
# 在不进行梯度计算的情况下进行前向传播
with torch.no_grad():
    output = model(image)
print(output)

# 获取输出中的最大值索引
print(output.argmax(1))
# 打印CIFAR10数据集的类别列表
print(test_data.classes)
# 根据最大值索引获取预测结果
print(f"预测的结果是{test_data.classes[output.argmax(1)]}")

```

