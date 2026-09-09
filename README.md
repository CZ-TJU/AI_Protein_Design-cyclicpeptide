## 1. 项目说明

本项目实现“AI 生成—多层级计算筛选—实验候选输出”的环肽从头设计流程，面向 Keap1 Kelch 结构域（PDB 3WN7）与 GCGR（PDB 8JIT）/GIPR 两类靶标分别形成 `cycpep_Kelch` 与 `cycpep_GCGR` 两个靶标目录。除靶标结构与热点约束不同外，两套流程共用同一份计算代码（src/），每个靶标目录只保存该靶标的配置、数据、脚本入口与结果，输出标准化候选清单 results.csv（或 results.xlsx），供后续固相合成与湿实验验证使用。

## 2. 运行环境

本项目运行于本地 Linux 计算服务器，软硬件环境与计算设计工具列表如下。

### 2.1 硬件配置

| 项目 | 配置 |
| --- | --- |
| CPU | Intel® Xeon® 4210 处理器（10 核 / 20 线程，2.2 GHz） |
| 内存 | RECC DDR4 2666，16 GB |
| GPU | NVIDIA RTX 2080 Ti（11 GB 显存，4352 个 CUDA 核心，260 W） |
| 固态硬盘（系统盘） | Intel 企业级 SSD，960 GB（SATA） |
| 机械硬盘（数据盘） | Seagate（ST）企业级 HDD，6 TB（7200 rpm，128 MB 缓存，SATA） |

### 2.2 软件与并行环境

| 类别 | 信息 |
| --- | --- |
| 操作系统 | Linux x86_64【待补充：实际发行版与内核】 |
| Python 解释器 | 【待补充：版本】；依赖清单见 requirements.txt（或 environment.yml） |
| 深度学习环境 | 【待补充：CUDA、驱动版本】 |
| 编译器及运行环境 | C++、Fortran、Python、Java 等编程环境 |
| 并行环境 | MPICH2 等并行计算环境 |
| 科学数学库 | BLAS、ATLAS、LAPACK、ScaLAPACK、FFTW |
| 作业调度与管理 | 安装 PBS 作业调度系统，支持并行作业调度与远程管理 |

### 2.3 计算设计工具

| 工具 | 版本 / 部署方式 |
| --- | --- |
| PyRosetta | PyRosetta-4 2021；具体版本：Release 2024.31 + release |
| RFdiffusion | 本地部署（GitHub: RosettaCommons/RFdiffusion）【版本待补充】 |
| ProteinMPNN | 本地部署（GitHub: dauparas/ProteinMPNN）【版本待补充】 |
| BoltzGen | 本地部署版本【版本/commit 待补充】 |
| AlphaFold3 | 本地部署；另可配合 AlphaFold 在线预测平台（https://alphafoldserver.com/） |
| Rosetta | 本地部署（rosetta_scripts.mpi.linuxgccrelease / score_jd2.mpi.linuxgccrelease）【版本待补充】 |
| simple_cycpep_predict | 本组部署脚本【版本待补充】 |
| 环肽序列 → SMILES 转换 | NovoPro 在线工具（https://www.novoprolabs.com/tools/convert-peptide-to-smiles-string） |

第三方计算设计工具均为开源/科研软件，来源与许可证说明见第 7 节。
