# TH Praxistage
## Energy benchmarking for Deep Learning Workflows with CodeCarbon

In this repository you will find the tutorial and solution notebook for the "TH Praxistage" student session. This repository is public and will remain so even after the TH Praxistage so that you can work through it once again, if, for example, the pace in the session was too fast for you.

In order for you to follow along on your own machine, there are a few things you have to do first:

- Set up your machine for CodeCarbon.
- Set up a python environment with all the dependencies we'll need for the tutorial notebook and CodeCarbon.

`The only accurate way of tracking the energy usage with CodeCarbon is on a Linux machine with a Nvidia GPU. On Windows the CPU RAPL files do not exist, resulting in CodeCarbon using a fallback approximation for CPU Power (50% of the CPU Model TDP) which is not really representative. At the same time, non-Nvidia GPUs are not yet supported, i.e. GPU power won't be measure at all if you have an iGPU or AMD GPU.`

### i) Code Carbon Setup on Linux with Nvidia GPU

If you have a Linux machine and Nvidia GPU, there are a two things you need to set up:
- Install Nvidia drivers for your GPU and a compatible CUDA version, cuDNN for the Deep Learning part of this tutorial.
- Give global read access to the Intel RAPL files.

#### **1) Setup Nvidia driver, CUDA and cuDNN**
- [Install CUDA 12](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)
- [Install cuDNN 9](https://docs.nvidia.com/deeplearning/cudnn/latest/installation/linux.html)

#### **2) Setup read access to Intel RAPL files**
Due to a [security issue](https://www.cve.org/CVERecord?id=CVE-2020-8694) all Linux distributions changed the permissions to the Intel RAPL file. You need root access to read the file. As a work around you can either give permanent global read access to the files or temporary global read access that vanishes once you reboot your machines. 

The temporary option for our usecases in this tutorial is enough and probably also for you the more convenient option without the risk of forgetting to change back the permissions yourself.

##### a) Temporary Option
```
sudo chmod -R a+r /sys/class/powercap/intel-rapl
```

##### b) Permanent Option
```
sudo apt install sysfsutils
```
Add these lines to `/etc/sysfs.conf` *(Needs root permission, i.e. open file with sudo)*: 
```
mode class/powercap/intel-rapl:0/energy_uj = 0444
mode class/powercap/intel-rapl:0:0/energy_uj = 0444
mode class/powercap/intel-rapl:0:1/energy_uj = 0444
```
After making these changes to the file you need to reboot, then you're ready to go.


### ii) Python virtual environment setup

```
## Create python virtual environment
python3 -m venv .venv

## Activate the environment
# For Windows
.\.venv\Scripts\activate
# For Linux
source ./.venv/bin/activate

## Install dependencies into the environment
pip install -r requirements.txt
```