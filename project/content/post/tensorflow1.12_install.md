---
title: "Brisk Guide to Install Tensorflow GPU on Linux (Ubuntu 18.04/18.10)"
date: 2019-02-09
draft: false
---
![Example image](https://images.unsplash.com/photo-1504221507732-5246c045949b?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1050&q=80)
## My system configuration

* AMD Ryzen 2600
* 16 GB DDR4 3200 MHz memory
* GTX 1050 ti 4 GB
* Ubuntu 18.04 Bionic Beaver


## Remove Nvidia Nouveau Driver
### Blacklist the driver
```
$ sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
$ sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
```
### Confirm the content of the new modprobe config file

```
$ cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf
blacklist nouveau
options nouveau modeset=0
```

### Update kernel initramfs
```
$ sudo update-initramfs -u
```

## Reboot
```
$ sudo reboot
```

## Remove Any Other NVIDIA Driver

### Add the graphics driver PPA
```
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt-get update
```
### Purge the driver
```
$ sudo apt-get purge nvidia*
```

### Reboot
```
$ sudo reboot
```

## Install Proprietary Nvidia Driver
```
$ sudo apt update
$ sudo ubuntu-drivers autoinstall
$ sudo reboot
```

After this, ```nvidia-smi``` command will output the GPU status. If it doesn’t, reboot again. You should see something like this:

![nvidia-smi](https://miro.medium.com/max/700/1*Hhf_lBohEU_r1G44d4P4zQ.png)

## Install Tensorflow GPU

### Install Anaconda package
* Install anaconda from here.
* Anaconda already has a newer version 5.3, however, it’s Python 3.7 and that’s not tensorflow-GPU pre-compiled version built with. Don’t use it.
* If you already have anaconda installed on your machine, create a new virtual environment with python 3.6.

```
$ conda create -n tf-gpu python=3.6
$ source activate tf-gpu
```

### Install Tensorflow GPU along with compatible versions of CUDA & CuDNN

This is the key part. Use the following command to install tensorflow-gpu 1.12:
You might look up compatible tensorflow, CUDA and CuDNN versions and change the version number accordingly. Here is what worked for me.

```
conda install \
tensorflow-gpu==1.12 \
cudatoolkit==9.0 \
cudnn=7.1.2 \
h5py
```

### Validate the installation

```
import tensorflow as tf
with tf.device('/gpu:0'):
    a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3],    name='a')
    b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b')
    c = tf.matmul(a, b)

with tf.Session() as sess:
    print (sess.run(c))
```
If succeeded, you’ll see this:

```
tf version = 1.12.0
[[22. 28.]
[49. 64.]]
```
