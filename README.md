# TT-BUDA TEST Models

## Contents
- [Introduction to PyBuda](#test-models)
- [Installation TT-BUDA](#test-models)
- [Tests](#docs)
- [Test Models](#build)




# Introduction Pybuda

PyBuda ™ is a compute framework used to develop, run, and analyze ML workloads on Tenstorrent hardware.


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


### 
## PCI Driver Installation ( Tenstorrent AI Kernel-Mode Driver )

**Supported Hardware:**

- [Introduction to PyBuda](#)
- [Installation TT-BUDA](#)

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

