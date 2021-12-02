# CUPBJet-HPC-Docs

Jet集群自 2021 年底建成，拥有 8 台计算节点和 16 颗第二代英特尔至强金牌 5218R 处理器，同时拥有两台深度学习节点，配备8块 NVIDIA Tesla T4 和 2块 NVIDIA Tesla V100， 可以应对众多复杂的科学计算问题。


| 队列  | CPU | GPU | Memory | 数量|节点号|
| ----- | ----- |----- |----- |----- |----- |
| login   | Intel Xeon Silver 4210R (2.4GHz 10C) * 2     | -  |128 GB  | 1  |ln01|
| compute | Intel Xeon Gold 5218R (2.1GHz 20C) * 2 | -  |384 GB  | 8  |n01-n08|
| gpu     | Intel Xeon Gold 5218R (2.1GHz 20C) * 2 | NVIDIA Tesla T4 (2560C 16GB) * 8  | 256 GB  | 1  |g01 |
| gpu     | Intel Xeon Gold 4214R   (2.4GHz 12C) * 2 | NVIDIA Tesla V100 (5120C TC640 32GB) * 2  | 256 GB  | 1  |g02|
| all     | -  |-  |-  |-  |-|


操作系统：CentOS Linux release 7.9.2009

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

## 连接外网
terminal输入`firefox`然后任意打开网页进入石油大学网络认证窗口，输入学校密码连接


## 运行软件

### 可用节点

### 提交作业

#### 交互模式
```bash
srun -t 1:00:00 -n20 -N1 -p compute --pty /bin/bash
```

#### 批量模式
```bash
sbatch myjob.slurm
```

## 参考材料
https://hpc.sjtu.edu.cn/Item/docs/Pi_GetStarted.pdf
