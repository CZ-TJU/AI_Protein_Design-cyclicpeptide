# cycpep_Kelch 数据来源说明

本目录内容从原始工作目录 `D:\KEAP1` 归档而来，均为复制件，原始文件未做移动或删除。

| 本目录文件 | 原始位置 | 说明 |
| --- | --- | --- |
| `3WN7.pdb` | `D:\KEAP1\3WN7.pdb` | Keap1 Kelch 靶标晶体结构（PDB: 3WN7） |
| `kelch.pdb` | `D:\KEAP1\kelch.pdb` | 本工作流使用的 Kelch 结构/序列文件（链 A 序列同 3WN7） |
| `hotspots/HS1.png` | `D:\KEAP1\HS1.png` | HS1 热点残基示意 |
| `hotspots/HS2.png` | `D:\KEAP1\HS2.png` | HS2 深口袋热点残基示意 |
| `hotspots/Kelch 标记Hotspot.pse` | `D:\KEAP1\Kelch 标记Hotspot.pse` | PyMOL 热点标注会话 |
| `controls/negative_control_*.json` | `D:\KEAP1\第二批\negative_control_*.json` | 随机低复杂度环肽的 AF3 输入（含 SMILES） |

未完整复制的大目录（供按需回源）：

- `D:\KEAP1\第一批`：RFdiffusion 骨架 + ProteinMPNN 序列 + 本地 AF3 预测（约 1.7 GB，含每个候选 5 个 sample 的完整输出）。
- `D:\KEAP1\第二批`：BoltzGen / 1213 深热点候选的 AF3 预测与筛选数据。
- `D:\KEAP1\simple_cycpep_predict`：游离态构象采样的 `.sc`、排序文件及各候选目录。
- `D:\KEAP1\实验记录`：实验记录 docx/pdf，含 BLI、Rosetta、构象分析等。
- `D:\KEAP1\论文`：最终论文/答辩版本，作为方法与结论的文字依据。
