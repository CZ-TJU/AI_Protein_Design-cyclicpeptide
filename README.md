## 1. 项目说明

本项目实现“AI 生成—多层级计算筛选—实验候选输出”的环肽从头设计流程，面向 Keap1 Kelch 结构域（PDB 3WN7）与 GCGR（PDB 8JIT）/GIPR 两类靶标分别形成 `cycpep_Kelch` 与 `cycpep_GCGR` 两个靶标目录。除靶标结构与热点约束不同外，两套流程共用同一份计算代码（src/），每个靶标目录只保存该靶标的配置、数据、脚本入口与结果，输出标准化候选清单 results.csv（或 results.xlsx），供后续固相合成与湿实验验证使用。

## 2. 运行环境

- 操作系统：Linux x86_64【待补充：实际发行版与内核】
- Python 解释器：【待补充：版本】
- 深度学习环境：【待补充：CUDA、驱动版本】；GPU 型号与显存：【待补充】
- 第三方软件（均为开源/科研软件）：
  - RFdiffusion（GitHub: RosettaCommons/RFdiffusion）【版本待补充】
  - ProteinMPNN（GitHub: dauparas/ProteinMPNN）【版本待补充】
  - BoltzGen（本地部署版本）【版本待补充】
  - AlphaFold3 本地版与 AlphaFold 在线预测平台（https://alphafoldserver.com/）
  - Rosetta（rosetta_scripts.mpi.linuxgccrelease / score_jd2.mpi.linuxgccrelease）【版本待补充】
  - simple_cycpep_predict（本组部署脚本）【版本待补充】
  - 环肽序列→SMILES 转换：NovoPro 在线工具（https://www.novoprolabs.com/tools/convert-peptide-to-smiles-string）
