# LSNN: Long short-term memory Spiking Neural Networks

This repository provides a tensorflow library and a tutorial train a recurrent spiking neural network (ours is called LSNN).
For more details about LSNN see [1]. This model uses a method of network rewiring to keep a sparse connectivity during training, this method is called DEEP R and is described in [2].

In the tutorial `tutorial_sequential_mnist_with_LSNN.py`, you can classify the MNIST digits when the pixels are provided one after the other.
Note that for the purpose of this tutorial, we simplified the task described in [1].

The code was written by Guillaume Bellec and Darjan Salaj at the IGI institute of TU Graz between 2017 and 2018. Anand Subramoney and Arjun Rao helped to improve the implementation.

[1] Long short-term memory and Learning-to-learn in networks of spiking neurons  
Guillaume Bellec*, Darjan Salaj*, Anand Subramoney*, Robert Legenstein, Wolfgang Maass  
NIPS 2018, [[link]](http://papers.nips.cc/paper/7359-long-short-term-memory-and-learning-to-learn-in-networks-of-spiking-neurons) [[arxiv]](https://arxiv.org/abs/1803.09574  )
(\* equal contributions)

[2] Deep Rewiring: Training very sparse deep networks  
Guillaume Bellec, David Kappel, Wolfgang Maass, Robert Legenstein  
ICLR 2018, [[link]](https://openreview.net/forum?id=BJ_wN01C-) [[arxiv]](https://arxiv.org/abs/1711.05136)


## Installation
The code is compatible with python 3.4 to 3.7 and tensorflow 1.7 to 1.12 (CPU and GPU versions).

> You can run the training scripts **without installation** by temporarily including the repo directory
> in your python path like so: `` PYTHONPATH=. python3 bin/tutorial_sequential_mnist_with_LSNN.py`` 

From the main folder run:  
`` pip3 install --user .``  
You can now import the tensorflow cell called ALIF (for adaptive leakey integrate and fire) as well as the rewiring wrapper to update connectivity matrices after each call to the optimizer.
Warning, the GPU compatible version of tensorflow is not part of the requirements by default.
To use GPUs one should also install it:
 ``pip3 install --user tensorflow-gpu``.

## Training and evaluation

To train an LSNN on the sequential MNISt (sMNIST) task, run from the main folder:  
``python3 bin/tutorial_sequential_mnist_with_LSNN.py``  
This will reproduce the result labeled "DEEP R LSNN" in the Figure 1B of [1] with 1ms per pixel.

To train an LSNN on the store-recall task, run from the main folder:  
``python3 bin/tutorial_storerecall_with_LSNN.py``  
This will also plot the input, network spikes, output and thresholds during training.

Use flags to configure plotting and other parameters of the task, network or simulation.

## Troubleshooting

### Possible incompatibility with tf.variable_scope(..., reuse=??)
The LIF and ALIF cells defined in `spiking_models.py` behave almost like any tensorflow cell. However, we define the tensorflow variables during initilization of the cell object.
Instead, the default tensorflow cells define variables during the first call(...) of the cell object.
This leads to some incompatibility with some deep learning framework like Ray for deep RL.
To fix this one has to move the variable initialization into the __call__ method as done for standard tensorflow cells.

### Wrong compilation of tensorflow

If the scripts fail with the following error:
`` Illegal instruction (core dumped) ``

It is most probably due to the lack of AVX instructions on the machine you are using.
A known workaround is to reinstall the LSNN package with older tensorflow version (1.5).
Change requirements.txt to contain:

`` tensorflow==1.5 ``

## Citing LSNN

```
@inproceedings{bellec2018long,
  title={Long short-term memory and learning-to-learn in networks of spiking neurons},
  author={Bellec, Guillaume and Salaj, Darjan and Subramoney, Anand and Legenstein, Robert and Maass, Wolfgang},
  booktitle={Advances in Neural Information Processing Systems},
  pages={787--797},
  year={2018}
}
```

## License

LSNN-official is open source software and is licensed under the [BSD 3-Clause Clear License](https://spdx.org/licenses/BSD-3-Clause-Clear.html).
