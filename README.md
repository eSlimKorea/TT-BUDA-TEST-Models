## Contents
- **Introduction to PyBuda**
- **Installation TT-BUDA**
  - HugePages Setup
  - PCI Driver Installation
  - Backend Compiler Dependencies
  - PyBuda Installation
- **Test**
- [Test Models](#build)




# Introduction Pybuda

PyBuda is a compute framework used to develop, run, and analyze ML workloads on Tenstorrent hardware.


Moving forward, we plan to test various AI models using TT-Buda to evaluate its capabilities and  on Tenstorrent hardware.


# Installation TT-BUDA

## OS Compatibility
Tenstorrent software currently supports the following operating systems:

- **Ubuntu 20.04 LTS (Focal Fossa)**
- **Ubuntu 22.04 LTS (Jammy Jellyfish)**

## Compatibility Matrix

| OS          | Python | PyTorch                 | Driver      | Firmware       |
|-------------|--------|-------------------------|-------------|----------------|
| Ubuntu 22.04 | 3.10.12 | 2.1.0+cpu.cxx11.abi    | ttkmd_1.29  | fw_v80.10.0.0 |
| Ubuntu 20.04 | 3.8.10  | 2.1.0+cpu.cxx11.abi    | ttkmd_1.29  | fw_v80.10.0.0 |


## HugePages Setup
1. Download latest setup_hugepages.py script.
   
Download the latest  **`setup_hugepages.py`** for the HugePages setup script. for HugePages setup script.
```bash
wget https://raw.githubusercontent.com/tenstorrent/tt-metal/main/infra/machine_setup/scripts/setup_hugepages.py
```
<br>


2. Run first setup script and Reboot
   
```bash
sudo -E python3 setup_hugepages.py first_pass
reboot
```
<br>


 3. Run second setup script & check setup.
   
```bash
sudo -E python3 setup_hugepages.py enable && sudo -E python3 setup_hugepages.py check
```
<br>

---


## PCI Driver Installation ( Tenstorrent AI Kernel-Mode Driver )

**Supported Hardware:**

- Grayskull
- Wormhole
<br>

**Install** ( You must have dkms installed. )
driver will auto-load next boot
  ```bash
  dnf install dkms
  reboot

  lsmod |grep -i tens
  tenstorrent            49152  0
  ```
<br>


**Uninstall** ( You must have dkms installed. )
  ```bash
 sudo modprobe -r tenstorrent
 sudo dkms remove tenstorrent/1.29 --all
  ```
<br>

---

## Backend Compiler Dependencies

Instructions to install the Tenstorrent backend compiler dependencies on a fresh install of Ubuntu Server 20.04 or Ubuntu Server 22.04.

For both operating systems run the following commands:

```bash
apt update -y
apt upgrade -y --no-install-recommends
apt install -y build-essential curl libboost-all-dev libgl1-mesa-glx libgoogle-glog-dev libhdf5-serial-dev ruby software-properties-common libzmq3-dev clang wget python3-pip python-is-python3 python3-venv
```
<br>

for Ubuntu 20.04:

```bash
apt install -y libyaml-cpp-dev
```

for ubuntu 22.04:

```bash
wget http://mirrors.kernel.org/ubuntu/pool/main/y/yaml-cpp/libyaml-cpp-dev_0.6.2-4ubuntu1_amd64.deb
wget http://mirrors.kernel.org/ubuntu/pool/main/y/yaml-cpp/libyaml-cpp0.6_0.6.2-4ubuntu1_amd64.deb
dpkg -i libyaml-cpp-dev_0.6.2-4ubuntu1_amd64.deb libyaml-cpp0.6_0.6.2-4ubuntu1_amd64.deb
rm libyaml-cpp-dev_0.6.2-4ubuntu1_amd64.deb libyaml-cpp0.6_0.6.2-4ubuntu1_amd64.deb
```
<br>

---


## PyBuda Installation

**It is strongly recommended to use virtual environments for each project utilizing PyBuda and Python dependencies.**

### 1. Download .zip package and unzip to find the pybuda, tvm and torchvison wheel files


- **Grayskull && Ubuntu 20.04 LTS (Focal Fossa)**
  
  ```bash
  wget https://github.com/tenstorrent/tt-buda/releases/download/v0.19.3/pybuda-gs-v0.19.3-ubuntu-20-04-amd64-python3.8.zip
  ```

- **Wormhole && Ubuntu 20.04 LTS (Focal Fossa)**
  
  ```bash
  wget https://github.com/tenstorrent/tt-buda/releases/download/v0.19.3/pybuda-wh.b0-v0.19.3-ubuntu-20-04-amd64-python3.8.zip
  ```

- **Grayskull && Ubuntu 22.04 LTS (Focal Fossa)**
  
  ```bash
  wget https://github.com/tenstorrent/tt-buda/releases/download/v0.19.3/pybuda-gs-v0.19.3-ubuntu-22-04-amd64-python3.10.zip
  ```

- **Wormhole && Ubuntu 22.04 LTS (Focal Fossa)**
  
  ```bash
  wget https://github.com/tenstorrent/tt-buda/releases/download/v0.19.3/pybuda-wh.b0-v0.19.3-ubuntu-22-04-amd64-python3.10.zip
  ```
<br>

### 2. Create your Python environment in desired directory and Activate environment

```bash
python3 -m venv env   ( env is example )
source env/bin/activate
```
<br>

### 3. Pip install PyBuda, TVM and Torchvision whl files

```bash
pip install --upgrade pip==24.0
pip install pybuda-<version>.whl tvm-<version>.whl torchvision-<version>.whl
```

ex..
```bash
pip install packages/pybuda-0.1.240917+dev.wh.b0.12bb84b-cp38-cp38-linux_x86_64.whl packages/tvm-0.14.0+dev.tt.0840d6bef-cp38-cp38-linux_x86_64.whl packages/torchvision-0.16.0+fbb4cc5-cp38-cp38-linux_x86_64.whl
```
<br>

> [!NOTE]
> The pybuda-<version>.whl file contains the PyBuda library, the tvm-<version>.whl file contains the latest TVM downloaded release, and the torchvision-<version>.whl file bundles the torchvision library.

<br>

Install Debuda (Optional)

```bash
pip install debuda-<version>.whl
```

---
<br>
<br>

# Test

This code tests the integration of PyTorch with PyBuda to verify if a simple PyTorch model can run on Tenstorrent hardware through PyBuda. It defines a sample model with two weight parameters and a forward method that performs matrix multiplication. The function test_module_direct_pytorch() runs this model using PyBuda and prints the output, confirming the PyBuda installation.

<br>

```bash
import pybuda
import torch


# Sample PyTorch module
class PyTorchTestModule(torch.nn.Module):
    def __init__(self):
        super().__init__()
        self.weights1 = torch.nn.Parameter(torch.rand(32, 32), requires_grad=True)
        self.weights2 = torch.nn.Parameter(torch.rand(32, 32), requires_grad=True)
    def forward(self, act1, act2):
        m1 = torch.matmul(act1, self.weights1)
        m2 = torch.matmul(act2, self.weights2)
        return m1 + m2, m1


def test_module_direct_pytorch():
    input1 = torch.rand(4, 32, 32)
    input2 = torch.rand(4, 32, 32)
    # Run single inference pass on a PyTorch module, using a wrapper to convert to PyBuda first
    output = pybuda.PyTorchModule("direct_pt", PyTorchTestModule()).run(input1, input2)
    print(output)
    print("PyBuda installation was a success!")


if __name__ == "__main__":
    test_module_direct_pytorch()
```


## Output

```bash
[PyBuda Tensor: tensor([[[18.7812, 16.9844, 15.6641,  ..., 15.6406, 20.7422, 17.6328],
         [15.9688, 15.7461, 14.2578,  ..., 16.0000, 18.1250, 18.2188],
         [17.2812, 14.9492, 14.1875,  ..., 14.1992, 16.7891, 16.9844],
         ...,
         [20.4062, 17.4766, 16.8984,  ..., 16.3672, 20.9688, 18.7969],
         [19.1094, 17.7188, 17.2656,  ..., 15.0156, 18.5156, 16.6562],
         [18.2422, 16.9609, 15.7422,  ..., 15.7969, 18.8047, 17.2969]],

        [[14.7891, 13.3125, 12.9062,  ..., 12.1992, 15.3047, 13.9062],
         [17.0078, 15.3711, 13.3008,  ..., 15.0508, 18.0469, 16.3906],
         [18.0781, 14.7344, 14.4492,  ..., 14.4609, 17.8203, 17.8516],
         ...,
         [15.8828, 15.6875, 15.3242,  ..., 13.3242, 15.5234, 13.9219],
         [15.6953, 15.0703, 13.4336,  ..., 12.9453, 16.4844, 15.8047],
         [15.0742, 13.9219, 14.5000,  ..., 13.4727, 16.1953, 14.8828]],

        [[14.9336, 13.8828, 11.9805,  ..., 12.5625, 14.7344, 13.4180],
         [17.9219, 16.6484, 15.3398,  ..., 14.5742, 19.2422, 16.9219],
         [18.4297, 17.4922, 16.3906,  ..., 16.1719, 19.6016, 17.3750],
         ...,
         [17.7969, 15.4375, 15.6562,  ..., 13.7656, 18.6719, 17.2188],
         [16.2891, 15.0469, 14.3438,  ..., 13.9492, 16.4062, 15.0078],
         [17.8594, 16.5938, 16.5547,  ..., 15.3047, 18.4688, 15.7891]],

        [[16.5078, 15.6484, 15.0312,  ..., 14.3867, 16.9453, 16.3203],
         [17.5938, 15.7891, 15.6641,  ..., 14.1328, 18.4688, 17.0859],
         [15.2578, 14.7148, 12.0547,  ..., 12.9648, 16.1562, 15.6797],
         ...,
         [16.4219, 17.0625, 15.1016,  ..., 14.1172, 18.0703, 17.4453],
         [18.5547, 15.3477, 15.2852,  ..., 14.3750, 17.7656, 16.9766],
         [16.0938, 15.7969, 14.8750,  ..., 13.2812, 16.8594, 14.9609]]],
       requires_grad=True), DataFormat.Float32, PyBuda Tensor: tensor([[[ 8.0703,  8.1094,  7.6523,  ...,  7.2969, 10.3984,  8.2969],
         [ 8.5156,  7.9375,  8.1641,  ...,  9.1562, 10.1250,  9.7188],
         [ 7.8594,  7.1406,  6.4648,  ...,  6.6367,  7.6523,  8.1562],
         ...,
         [ 9.5078,  9.1562,  7.9141,  ...,  8.5234, 10.3281,  9.7422],
         [ 9.2578,  9.6094,  8.5156,  ...,  7.7773,  9.5391,  8.3516],
         [ 8.8672,  9.0312,  7.9688,  ...,  7.7383,  9.9062,  8.5625]],

        [[ 6.7031,  6.6172,  6.3750,  ...,  6.0312,  7.0586,  6.1211],
         [ 7.2930,  7.6719,  5.8867,  ...,  7.1914,  8.3594,  7.3359],
         [ 7.8359,  7.6094,  7.0469,  ...,  7.7383,  9.4688,  8.9297],
         ...,
         [ 7.8594,  8.5781,  7.6094,  ...,  7.1680,  8.1484,  7.3867],
         [ 5.9141,  6.1250,  5.4883,  ...,  4.8594,  6.5000,  5.7422],
         [ 7.5117,  7.9375,  8.0859,  ...,  7.8594,  9.2031,  8.3828]],

        [[ 7.7656,  7.6172,  6.7422,  ...,  6.8203,  8.2266,  7.3672],
         [ 8.3828,  9.0469,  7.8555,  ...,  7.0391,  9.8594,  8.9062],
         [ 9.1016,  8.4375,  7.8711,  ...,  7.8906,  9.6875,  8.0234],
         ...,
         [ 8.5156,  7.6211,  7.5234,  ...,  6.5078,  9.5391,  8.2109],
         [ 8.1250,  8.0312,  7.0312,  ...,  7.5977,  8.9531,  8.1406],
         [ 7.9961,  7.8398,  7.3867,  ...,  7.0469,  9.0859,  6.9062]],

        [[ 6.9883,  7.9570,  7.0273,  ...,  7.1406,  7.8633,  7.8086],
         [ 7.3008,  7.4414,  7.1406,  ...,  6.6602,  8.2188,  7.3750],
         [ 6.9531,  7.9258,  6.3633,  ...,  6.6719,  8.0469,  7.5430],
         ...,
         [ 8.2188,  8.1953,  7.7031,  ...,  7.3555,  9.3281,  8.6328],
         [ 8.1406,  7.6328,  7.6797,  ...,  6.6523,  8.3281,  8.2656],
         [ 7.4219,  8.7031,  6.7812,  ...,  6.4531,  8.8984,  7.9258]]],
       requires_grad=True), DataFormat.Float32]
PyBuda installation was a success!

```


