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
  
<br>




