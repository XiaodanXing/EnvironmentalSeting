# EnvironmentalSettings
Some problems I encountered when setting up my enviroment and my solutions

Basic information about my device: Genforce RTX 3090 NVIDIA-SMI 460.106.00  Driver Version: 460.106.00   CUDA Version: 11.2  Ubuntu x86
![图片](https://user-images.githubusercontent.com/30890745/166502799-6de7cb43-cab1-4638-b224-08be32758f8f.png)


## Nvidia Driver Update

## Update Cuda and Cudnn
Credit to [Best practice for upgrading CUDA and cuDNN for tensorflow](https://stackoverflow.com/questions/50213021/best-practice-for-upgrading-cuda-and-cudnn-for-tensorflow) and [Install CUDA 11.2, cuDNN 8.1.0, PyTorch v1.8.0 (or v1.9.0), and python 3.9 on RTX3090 for deep learning](https://medium.com/analytics-vidhya/install-cuda-11-2-cudnn-8-1-0-and-python-3-9-on-rtx3090-for-deep-learning-fcf96c95f7a1)

### Install NVIDIA DRIVER

Step 1: Remove original cuda files. **Check /usr/local/ to see if there's any folder named "cuda" left.** 

```
sudo apt-get --purge remove cuda
sudo apt-get autoremove
dpkg --list |grep "^rc" | cut -d " " -f 3 | xargs sudo dpkg --purge
```

Step 2: Then setting up the Unbuntu by

```
sudo apt-get update
sudo apt-get upgrade -ysudo apt-get install -y build-essential cmake unzip pkg-config
sudo apt-get install -y libxmu-dev libxi-dev libglu1-mesa libglu1-mesa-dev
sudo apt-get install -y libjpeg-dev libpng-dev libtiff-dev
sudo apt-get install -y libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
sudo apt-get install -y libxvidcore-dev libx264-dev
sudo apt-get install -y libgtk-3-dev
sudo apt-get install -y libopenblas-dev libatlas-base-dev liblapack-dev gfortran
sudo apt-get install -y libhdf5-serial-dev graphviz
sudo apt-get install -y python3-dev python3-tk python-imaging-tk
sudo apt-get install -y linux-image-generic linux-image-extra-virtual
sudo apt-get install -y linux-source linux-headers-generic
```

Step 3: Add graphics ppa
```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
```

Step 4: Search available drivers
```
ubuntu-drivers devices
```
![图片](https://user-images.githubusercontent.com/30890745/166504728-1a30e940-f5b3-49cc-a7e6-7510e3743943.png)


Step 5: Install the driver with the best version. I choose driver-460 here because of the requirement of StyleGAN2-ada-pytorch. Please choose 510 if you are working on a RTX3090 device.
```
sudo apt-get install nvidia-driver-460
```

Step 6: Reboot your computer after installation. After rebooting, verify the driver installation by

```
nvidia-smi
```


### Install CUDA-toolkit
Step 1. Install CUDA-toolkit by

```
wget https://developer.download.nvidia.com/compute/cuda/11.2.2/local_installers/cuda_11.2.2_460.32.03_linux.run
sudo sh cuda_11.2.2_460.32.03_linux.run

```

***Remember: DO NOT check the option of installing the driver!!!***

In Yifan's blog this line is written in bold and italic texts. I guess this is super important but I do not know what will happen if the driver is installed at the same time.
![图片](https://user-images.githubusercontent.com/30890745/166521580-5262001d-ca62-4ad1-ba48-772688e53cfa.png)

After the installation, you will see the result as below.
![图片](https://user-images.githubusercontent.com/30890745/166521626-06dd5009-dc95-475c-a152-cc2832460ede.png)

Step 2. Update enviromental variable

Set environmental variables in the `~/.bashrc` by
```
cd ~
vim ./.bashrc
```
Then paste below lines into your bashrc file. 
'''
export PATH=/usr/local/cuda-11.2/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-11.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
export CUDA_HOME=/usr/local/cuda
'''
Remember to update your bashrc file by
```
source ./.bashrc
```
