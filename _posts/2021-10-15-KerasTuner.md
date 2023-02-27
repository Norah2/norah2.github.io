---
layout: post
title: 使用 Keras Tuner 对神经网络进行超参数调优
date: 2021-10-15
description: 使用 Keras Tuner 对神经网络进行超参数调优
categories: Python
tags: Python DL
author: NMt
mathjax: true
---

* content
{:toc}

分享一个小工具，在神经网络中使用 Keras Tuner 进行超参数调优。  

<div style='display: none'>
@@@@
</div>





{% raw %}
## 介绍  

在神经网络中有很多超参数，手动调整超参数非常困难。因此，我们可以使用Keras Tuner，这使得调整神经网络的超参数变得非常简单。就机器学习中看到的网格搜索或随机搜索一样。


## 超参数  

开发深度学习模型是一个迭代过程，从初始架构开始，然后重新配置，直到获得可以在时间和计算资源方面有效训练的模型。

调整的这些设置称为超参数，你有了想法，编写代码并查看性能，然后再次执行相同的过程，直到获得良好的性能。

因此，有一种方法可以调整神经网络的设置，称为超参数，找到一组好的超参数的过程称为超参数调整。

超参数调整是构建中非常重要的部分，如果不完成，则可能会导致模型出现重大问题，例如花费大量时间、无用参数等等。

超参数通常有两种类型：

* 基于模型的超参数：这些类型的超参数包括隐藏层的数量、神经元等。  

* 基于算法：这些类型会影响速度和效率，例如梯度下降中的学习率等。  

对于更复杂的模型，超参数的数量会急剧增加，手动调整它们可能非常具有挑战性。

Keras 调优器的好处在于，它将有助于完成最具挑战性的任务之一，即只需几行代码即可非常轻松地进行超参数调优。


## Keras tuner  

Keras tuner是一个用于调整神经网络超参数的库，可在Tensorflow中的神经网络实现中选择最佳超参数。

安装 Keras tuner：

```python
pip install keras-tuner
```

**超参数在开发一个好的模型中起着重要的作用，它可以产生很大的差异，防止过度拟合，在偏差和方差之间进行良好的权衡**，等等。

## 使用Keras Tuner调整超参数  

首先，我们将开发一个基线模型，然后我们将使用 Keras tuner 来开发我们的模型。我将使用 Tensorflow 进行实现。

* 步骤1（下载并准备数据集）  

```python  
from tensorflow import keras # importing keras
(x_train, y_train), (x_test, y_test) = keras.datasets.mnist.load_data() # loading the data using keras datasets api
x_train = x_train.astype('float32') / 255.0 # normalize the training images
x_test = x_test.astype('float32') / 255.0 # normalize the testing images
```

* 步骤2（开发基线模型）  

现在，我们将使用有助于识别数字的 mnist 数据集构建我们的基线神经网络，因此让我们构建一个深度神经网络。

```python
model1 = keras.Sequential()
model1.add(keras.layers.Flatten(input_shape=(28, 28))) # flattening 28 x 28 
model1.add(keras.layers.Dense(units=512, activation='relu', name='dense_1')) # you have 512 neurons with relu activation
model1.add(keras.layers.Dropout(0.2)) # we added a dropout layer with the rate of 0.2
model1.add(keras.layers.Dense(10, activation='softmax')) # output layer, where we have total 10 classes
```

* 步骤3（编译和训练模型）  

现在，我们已经建立了我们的基线模型，现在是时候编译我们的模型并训练模型了，我们将使用 Adam 优化器，学习率为 0.0，为了训练，我们将运行我们的模型 10 个时期，验证分割为 0.2 。

```python
model1.compile(optimizer=keras.optimizers.Adam(learning_rate=0.001),
            loss=keras.losses.SparseCategoricalCrossentropy(),
            metrics=['accuracy'])
model1.fit(x_train, y_train, epochs=10, validation_split=0.2)
```


* 步骤4（评估我们的模型）  

现在我们已经训练好了，现在我们将在测试集上评估我们的模型，看看模型的性能。  

```python
model1_eval = model.evaluate(img_test, label_test, return_dict=True)
``` 

## 使用Keras Tuner调整模型  

* 步骤1（导入库）  

```python
import tensorflow as tf
import kerastuner as kt
```

* 步骤2（使用 Keras Tuner 构建模型）  

现在，设置一个超模型（你为超调设置的模型称为超模型），将使用模型构建器函数定义为超模型，可以在下面的函数中看到该函数返回带有调整过的超参数的编译模型。

在下面的分类模型中，将微调模型超参数，即几个神经元以及 Adam 优化器的学习率。

```python
def model_builder(hp):
  '''
  Args:
    hp - Keras tuner object
  '''
  # Initialize the Sequential API and start stacking the layers
  model = keras.Sequential()
  model.add(keras.layers.Flatten(input_shape=(28, 28)))
  # Tune the number of units in the first Dense layer
  # Choose an optimal value between 32-512
  hp_units = hp.Int('units', min_value=32, max_value=512, step=32)
  model.add(keras.layers.Dense(units=hp_units, activation='relu', name='dense_1'))
  # Add next layers
  model.add(keras.layers.Dropout(0.2))
  model.add(keras.layers.Dense(10, activation='softmax'))
  # Tune the learning rate for the optimizer
  # Choose an optimal value from 0.01, 0.001, or 0.0001
  hp_learning_rate = hp.Choice('learning_rate', values=[1e-2, 1e-3, 1e-4])
  model.compile(optimizer=keras.optimizers.Adam(learning_rate=hp_learning_rate),
                loss=keras.losses.SparseCategoricalCrossentropy(),
                metrics=['accuracy'])
  return model
```

在上面的代码中，这里有一些注意事项：

Int()方法来定义密集单元的搜索空间。设置最小值和最大值以及在这些值之间递增时的步长。

学习率的Choice()方法。在超调时定义要包含在搜索空间中的离散值。

* 步骤3  实例化tuner并调整超参数  

使用HyperBand Tuner，它是一种为超参数优化而开发的算法。它使用自适应资源分配和提前停止来快速收敛到高性能模型。

可以在此处(https://arxiv.org/pdf/1603.06560.pdf)阅读有关此直觉的更多信息。

Hyperband 通过计算 1 + log_factor(max_epochs) 并将其四舍五入到最接近的整数来确定要在括号中训练的模型数量。

```python  
# Instantiate the tuner
tuner = kt.Hyperband(model_builder, # the hypermodel
                     objective='val_accuracy', # objective to optimize
max_epochs=10,
factor=3, # factor which you have seen above 
directory='dir', # directory to save logs 
project_name='khyperband')
# hypertuning settings
tuner.search_space_summary() 
Output:- 

# Search space summary
# Default search space size: 2
# units (Int)
# {'default': None, 'conditions': [], 'min_value': 32, 'max_value': 512, 'step': 32, 'sampling': None}
# learning_rate (Choice)
# {'default': 0.01, 'conditions': [], 'values': [0.01, 0.001, 0.0001], 'ordered': True}
```

* 步骤4（搜索最佳超参数）  

```python
stop_early = tf.keras.callbacks.EarlyStopping(monitor='val_loss', patience=5)
# Perform hypertuning
tuner.search(x_train, y_train, epochs=10, validation_split=0.2, callbacks=[stop_early])
best_hp=tuner.get_best_hyperparameters()[0]
```

* 步骤5（使用最佳超参数重建和训练模型）  

```python
# Build the model with the optimal hyperparameters
h_model = tuner.hypermodel.build(best_hps)
h_model.summary()
h_model.fit(x_train, x_test, epochs=10, validation_split=0.2)
```

现在，评估这个模型:

```python
h_eval_dict = h_model.evaluate(img_test, label_test, return_dict=True)
```

## 使用和不使用超参数调优的比较  

基线模型性能：

```python
BASELINE MODEL:
number of units in 1st Dense layer: 512
learning rate for the optimizer: 0.0010000000474974513
loss: 0.08013473451137543
accuracy: 0.9794999957084656
HYPERTUNED MODEL:
number of units in 1st Dense layer: 224
learning rate for the optimizer: 0.0010000000474974513
loss: 0.07163219898939133
accuracy: 0.979200005531311
```

如果看到基线模型的训练时间比这个超参数调整模型多，那是因为它的神经元较少，所以速度更快。

超参数模型更健壮，可以看到基线模型的损失和超调模型的损失，所以可以说这是一个更健壮的模型。



转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2021/10/15/KerasTuner/) 

<!--本文用到的链接-->

[pt_01]: 
[link_01]: 

{% endraw %}
