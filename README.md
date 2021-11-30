# CUPBJet-HPC-Docs

Jet集群自 2021 年底，双精度浮点数理论性能 2.1 PFlops，拥有 658 台双路节点和 1316 颗第二代英特尔至强金牌 6248 处理器，配以英特尔 Omni-Path 架构的 100 Gbps 高速网络互连，以及全闪存的 NVMeLustre 存储系统，体现强大的计算能力和先进的设计理念。

π 集群上的 AI 计算平台是国内高校计算力能最强的人工智能计算平台。该平台由 8 台 DGX-2 组成，每台 DGX-2 配备 16 块 NVIDIA Tesla V100，深度学习张量计算能力可以达到 16 PFLOPS；通过搭载 NVIDIA NVSwitch 创新技术，GPU 间带宽高达 2.4 TB/s。AI 计算平台采用可扩展架构，使得模型的复杂性和规模不受传统架构局限性的限制，从而可以应对众多复杂的人工智能挑战。

π 集群上的 ARM 计算平台基于 ARM 处理器构建，是国内首台基于 ARM 处理器的校级超算，共 100 个计算节点，与 π 2.0 集群实现共享登录、共享 Lustre 文件系统和共享 Slurm 作业调度系统，完美融入现有超算系统。ARM 超算单节点配备 128 核（2.6 GHz）、256 GB 内存（16 通道 DDR4-2933）、240 GB 本地硬盘，节点间采用 IB 高速互联，挂载 Lustre 并行文件系统。

| 队列  | CPU | GPU | Memory | 数量|节点号|
| ----- | ----- |----- |----- |----- |----- |
| login   | Intel Xeon Silver 4210R (2.4GHz 10C) * 2     | -  |128 GB  | 1  |ln01|
| compute | Intel Xeon Gold 5218R (2.1GHz 20C) * 2 | -  |384 GB  | 8  |n01-n08|
| gpu     | Intel Xeon Gold 5218R (2.1GHz 20C) * 2 | NVIDIA Tesla T4 (2560C 16GB) * 8  | 256 GB  | 1  |g01 |
| gpu     | Intel Xeon Gold 4214R   (2.4GHz 12C) * 2 | NVIDIA Tesla V100 (5120C TC640 32GB) * 2  | 256 GB  | 1  |g02|
| all     | -  |-  |-  |-  |-|


## 登陆
* 登录节点 IP 地址（或主机名）为 **10.4.60.233**
* SSH 端口为 **2277**
```bash
$ ssh user@10.4.60.233 -p 2277
```

## 文件传输
MobaXterm 可以直接FTP连接进行下载和上传

## 文件系统
介绍 /share/, /share/home

## 运行软件

### 可用节点

### 提交作业

#### 交互模式
```bash
srun -t 1:00:00 -n20 -N1 -p compute --pty /bin/bash
```
--pty /bin/bash

#### 批量模式
