# 《NaCl enhances CD8+ T cell effector functions in cancer immunotherapy》精读

## 论文信息

- **作者**：Caterina Scirgolea、Rosa Sottile、Marco De Luca 等
- **期刊与年份**：*Nature Immunology*, 2024；25: 1845–1857
- **DOI**：10.1038/s41590-024-01923-9
- **本地原文**：[PDF](<D:/research/review/perturbation33references/27-NaCl enhances CD8+ T cell effector functions in cancer immunotherapy.pdf>)
- **数据包 1**：[Zenodo 10012831](https://zenodo.org/records/10012831)
- **数据包 2**：[Zenodo 11207788](https://zenodo.org/records/11207788)
- **代码**：[GitHub](https://github.com/luglilab/NaCl-enhances-CD8-T-cell-effector-functions-in-cancer-immunotherapy)
- **外部单细胞数据**：[GEO GSE200996](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE200996)

## 一句话结论

在培养期提高 NaCl 可增强 CD8⁺ T 细胞谷氨酰胺利用、IFN-γ 和杀伤功能，同时保留部分干样可塑性；高盐饮食和 NaCl 预处理的细胞在小鼠肿瘤模型中提高抗肿瘤效应，但该结果不能直接解释为患者应增加膳食盐摄入。

## 数据护照

| 数据层 | 规模/组成 | 公开内容 |
|---|---|---|
| 人 CD8 bulk RNA-seq，模块 1 | 3 位供体 × NaCl/尿素对照 | 6 个 R1 FASTQ + `Dataset_1_raw_count_matrix.txt` |
| 小鼠/体内 bulk RNA-seq | 15 个单端 FASTQ；另有 count matrix | Dataset #1 |
| 小鼠肿瘤 scRNA-seq | 4 个 10x 矩阵 + 4 个 QC `.h5ad` + 约 9.43 GB integration `.h5ad.gz` | Dataset #1 |
| 人 CD8 bulk RNA-seq，模块 2 | 3 供体 × Gln± × NaCl/对照 = 12 个 FASTQ | Dataset #2 |
| 时间分辨蛋白质组 | 3 供体 × 2 条件 × 3 时间点 = 18 个 `.raw` | Dataset #2 |
| ATAC-seq/其他 count matrix | `Dataset_3/4_raw_count_matrix.txt` | 两套 Zenodo |
| 数据包规模 | Dataset #1 约 41 个文件；Dataset #2 约 32 个文件 | 单文件从 KB 到 9.4 GB |

## 1. 研究问题

高盐可重塑免疫反应，但其对抗肿瘤 CD8⁺ T 细胞是促进还是损害，以及能否用于细胞治疗制造，缺乏系统证据。作者重点问：NaCl 是否只造成短期效应化，还是能在增强杀伤的同时保留再次扩增能力？

## 2. 实验设计与方法框架

研究在人原代 CD8⁺ T 细胞中比较等渗 NaCl 与尿素/对照，结合转录组、ATAC、蛋白质组和代谢分析；在小鼠中使用高盐饮食、同系肿瘤和 adoptive transfer；并测试 NaCl 预处理的肿瘤特异 T 细胞/工程 T 细胞。机制聚焦 glutamine uptake、代谢重编程和转录调控。

## 3. 数据规模与图谱组成

### 3.1 Zenodo Dataset #1：10012831

记录页给出约 41 个文件，主要由四类数据组成。

#### A. 人 CD8 bulk RNA-seq：6 个 FASTQ

文件为 `NaCl_DonorA/B/C_R1_001.fastq.gz` 与 `Urea_DonorA/B/C_R1_001.fastq.gz`，即 **3 位供体 × 2 个渗透条件 = 6**。尿素是重要的渗透压对照，用于区分 Na⁺/Cl⁻ 特异效应与一般高渗效应。另提供 `Dataset_1_raw_count_matrix.txt`（约 2.4 MB）。

#### B. 体内/小鼠 bulk RNA-seq：15 个 FASTQ

`Lib_CSC-37` 至 `Lib_CSC-60` 共 15 个单端 R1 FASTQ，单文件约 0.78–1.05 GB；条件需从 `README_datainfo.xlsx` 恢复。另有 `Dataset_3_raw_count_matrix.txt`（约 3.4 MB）。

#### C. 小鼠肿瘤单细胞数据

公开 4 组标准 10x 矩阵：`LibCSC01`–`LibCSC04` 的 barcodes/features/matrix；4 个 QC 对象 `Mouse4QC.h5ad.gz`、`Mouse5QC.h5ad.gz`、`Mouse25QC.h5ad.gz`、`Mouse32QC.h5ad.gz`；以及整合对象 `Integration.h5ad.gz`，约 **9.43 GB**。

文件名中的 Mouse4/5/25/32 是样本标识，不是细胞数。最终细胞数应在下载 `.h5ad` 后读取 `n_obs`，不能用矩阵文件体积反推。

### 3.2 Zenodo Dataset #2：11207788

#### A. 谷氨酰胺依赖 bulk RNA-seq：12 个 FASTQ

文件名完整编码设计：Donor A/B/C × `Gplus/Gminus` × Control/NaCl，因此是 **3 × 2 × 2 = 12 个文库**。这套数据专门检验 NaCl 转录效应是否依赖谷氨酰胺，而不是 Dataset #1 的简单重复。

另提供 `Dataset_4_raw_count_matrix.txt`（约 11.4 MB）。

#### B. 时间分辨蛋白质组：18 个 Thermo `.raw`

供体 1447、1471、1472；条件 Control/NaCl；时间 0、15 min、1 h。总计 **3 × 2 × 3 = 18 个 raw 文件**，单文件约 266–277 MB。它刻画的是早期信号/蛋白变化，不是转录组重复。

### 3.3 数据包体积与选择性下载

两套数据均混合原始 FASTQ、原始质谱、处理矩阵和大型单细胞对象。实用下载顺序：

1. 先取两个 `README_datainfo.xlsx` 和四个 raw count matrix；
2. 单细胞复用取 4 个 QC `.h5ad`，机器资源充足再取 9.43 GB integration；
3. 重跑 scRNA 时下载 4 套 MTX 或回到原始测序入口；
4. 机制复现按问题选择 6 个渗透对照 FASTQ、12 个 Gln 交互 FASTQ或 18 个质谱 raw，没必要全量下载。

### 3.4 外部数据 GSE200996

论文还重分析公开 scRNA-seq GSE200996。它不是本文新测 Zenodo 图谱的一部分，主要用于外部验证。报告结果时应把“本文生成”和“公开再分析”分开。

### 3.5 下载后首先核对什么

对 bulk RNA 构建 `~ donor + NaCl + glutamine + NaCl:glutamine`；对质谱以 donor 配对并保留 time interaction；对 scRNA 按小鼠 pseudobulk。`README_datainfo.xlsx` 是将匿名 CSC 编号恢复为生物条件的唯一关键映射，未核对前不应自行猜组。

## 4. 主要结果

NaCl 增强人 CD8⁺ T 细胞 IFN-γ、细胞毒性和肿瘤控制，并在一定条件下保留 TCF1/干样网络。高盐饮食使小鼠肿瘤生长减慢，依赖 CD8⁺ T 细胞。NaCl 促进 glutamine consumption，谷氨酰胺受限会削弱其分子表型。

## 5. 机制理解

NaCl 不是简单把细胞推向终末分化，而是通过离子/渗透感知重构代谢需求，使谷氨酰胺为效应程序提供底物与信号，同时保留部分再次响应能力。不同培养时长、盐浓度和营养背景会决定“增强”还是“压力损伤”。

## 6. 推荐重点阅读的图

- 人供体 NaCl/尿素对照及功能实验。
- bulk RNA/ATAC/蛋白质组的早期机制图。
- glutamine removal 的交互实验。
- 小鼠单细胞 TIL 图谱与高盐饮食疗效。
- adoptive transfer/CAR-T 制造验证。

## 7. 创新性

把 NaCl 从微环境变量转化为可控的 ex vivo 制造扰动，并以多组学证明其不是一般高渗效应；两套 Zenodo 还公开了原始与处理层数据。

## 8. 局限性

盐浓度、暴露时间和培养基成分决定效应，跨实验室复现需严格匹配。高盐饮食有心血管和肾脏风险，不能由小鼠肿瘤结果推导临床饮食建议。单细胞最终细胞规模未在记录首页直接给出。

## 9. 在综述中的定位

适合作为“培养环境离子扰动增强 T 细胞效应”的代表，与代谢底物、细胞因子和药物制造策略比较。

## 10. 可直接写入综述的表述

> NaCl 通过增强谷氨酰胺利用和效应转录程序，提高 CD8⁺ T 细胞细胞毒性，同时在特定培养条件下保留干样可塑性，提示离子环境可作为细胞治疗制造变量。

## 11. 数据复用建议

最有价值的是用 6 个尿素对照文库识别 NaCl 特异基因，再用 12 个 Gln±文库检验依赖性，并用 18 个早期蛋白质组建立时间顺序。单细胞仅用于验证这些程序在 TIL 状态中的分布，统计必须回到小鼠层面。

## 12. 转化与安全性关注

优先考虑短时、可洗脱的 ex vivo NaCl 预处理，而不是系统性高盐暴露。需评估细胞活性、染色体/氧化应激、长期持久性和正常组织杀伤，并建立盐浓度—功能—毒性的治疗窗。

## 13. 避免误读

- NaCl 结果不构成增加膳食盐的临床建议。
- Dataset #1/#2 包含多种 assay，文件数不等于样本数。
- `Integration.h5ad.gz` 的 9.43 GB 是压缩文件体积，不是细胞数量。
- 尿素和 Gln±设计必须分别解释，不能合并为单一两组比较。

