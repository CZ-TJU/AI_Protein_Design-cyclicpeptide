# cycpep_Kelch：Keap1 Kelch（PDB 3WN7）环肽从头设计

本目录对应靶标 1：**Keap1 Kelch 结构域（PDB 3WN7，链 A）**。目录只保存该靶标特有的配置、输入数据、结果与记录；共享代码位于 `src/`，一键脚本入口为 `scripts/run_all.sh`。

## 1. 数据来源

本目录内容由原始工作目录 `D:\KEAP1` 归档整理而来。所有归档文件的原始路径、用途及未复制的大目录清单见 `data/source_notes.md`。

## 2. 设计流程

### 2.1 RFdiffusion + ProteinMPNN（第一批）

1. **骨架生成（RFdiffusion）**：以 3WN7 链 A 为唯一固定靶标，按 HS1 的 27 个热点残基约束、去噪强度 noise = 1，分长度生成环肽骨架；checkpoint 使用 `complex_base_ckpt` / `complex_beta_ckpt`。长度趋势探索覆盖 **8–15 aa**（HS1），各长度筛出 8–9 个骨架。
2. **热点重构验证（HS2）**：将偏界面表层的残基替换为深口袋残基（18 个，见 `configs/hotspots_HS2.txt`），限定 **8–11 aa** 验证结合深度是否受热点选择影响。
3. **序列设计（ProteinMPNN）**：对每个骨架生成多条候选序列，取低分序列进入 AlphaFold3。各记录中“每骨架生成条数”不完全一致，确切值需以原始 flags 为准。
4. **复合物预测（AF3）**：预测蛋白–环肽复合物并提取 ipTM；26 条高 ipTM（≥约 0.75）候选进入 Rosetta 评价。

### 2.2 BoltzGen 骨架–序列联合生成（第二批）

以同一 HS1 热点为约束，生成 **10–12 aa** 环肽 10 条；随后同样进入 AF3 ipTM 初筛、RosettaScript relax/评分和 simple_cycpep_predict 游离态构象分析。

### 2.3 能量与构象筛选

- **RosettaScript**：每条候选 relax 约 1000 个构象，计算环化前后 ddG 与 contact molecular surface，并结合 RMSD–ddG 分布判断是否形成能量漏斗。
  - `mpirun -np 10 rosetta_scripts.mpi.linuxgccrelease @flags_cycpep_target_relax`
  - `mpirun -np 50 score_jd2.mpi.linuxgccrelease @flags_evaluaton_cycpep`
- **simple_cycpep_predict**：每条候选游离态采样约 5000 个构象，取低能代表构象与 AF3 结合态比较，评估构象预组织。
  - `mpirun -np 25 simple_cycpep_predict.mpi.linuxgccrelease @flags_simple_cycpep_predict`

各步关键参数汇总在 `configs/design_parameters.yaml`。原始工作目录中未找到 RFdiffusion/ProteinMPNN 的完整命令行，因此 `scripts/run_all.sh` 尚未填入可执行命令，待共享代码就绪后补充。

## 3. 目录内容

| 路径 | 内容 |
| --- | --- |
| `configs/` | HS1（27 个残基）、HS2（18 个深口袋残基）、设计长度与关键参数 |
| `data/` | 3WN7/kelch 结构、HS1/HS2 图示与 PyMOL 会话、随机低复杂度对照 AF3 输入、来源映射 |
| `results/results.csv` | 完成 AF3 + Rosetta 计算并进入 BLI 评估的 9 条候选（序列、ipTM、ddG、结合信号） |
| `results/structures/af3_bound/` | 9 条候选对应的 AlphaFold（在线/本地）复合物 CIF |
| `results/structures/free_state/` | simple_cycpep_predict 游离态最低能构象 PDB |
| `results/structures/rosetta_relax/` | 第二批 4 条 BoltzGen 候选的 Rosetta 优化结构 |
| `logs/` | AF3 输入/置信度 JSON 副本、随机种子与运行元数据 |

## 4. 结果要点

- RFdiffusion + ProteinMPNN 候选更容易得到高 ipTM；BoltzGen 候选整体 ddG 更低、接触面积略大，两策略互补。
- 单纯高 ipTM 与 BLI 实验信号并不完全一致（例如 KCP8_20 ipTM = 0.80 但无明显结合信号），因此流程中引入 Rosetta ddG 与游离态构象分析作为二级筛选。
- 在完成 AF3/Rosetta 计算的 9 条候选里，`KCP_BZ_002`、`KCP_BZ_003`、`KCP_BZ_007`、`KCP_BZ_010`、`KCP8_9`、`KCP9_15` 后续显示有可检测的 BLI 结合信号。
- 高置信中间候选（如 KCP9_6，ipTM = 0.87）的游离态/结合态结构也已按原始记录归档在 `results/structures/free_state/` 等位置。

## 5. 复现注意

- 1.7 GB 的完整本地 AF3 输出（每候选 5 个 sample、含 confidences/data JSON）没有进入 Git 目录，仍在 `D:\KEAP1\第一批`；如需完整复现请先回源。
- AlphaFold 在线平台提交的部分候选没有本地输入 JSON，随机种子不可追溯；可追溯的本地输入以 `logs/af3_summary/*_input.json` 为准。
- 原始 docx/PDF 实验记录（BLI 拟合、Rosetta 对比、构象分析等）位于 `D:\KEAP1\实验记录`，是补充图表与判定依据的最终来源。
