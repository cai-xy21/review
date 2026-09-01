# 《cBAF complex components and MYC cooperate early in CD8+ T cell fate》精读

## 论文信息

- **作者**：Ao Guo, Hongling Huang, Zhexin Zhu 等
- **期刊与年份**：*Nature*, 2022
- **DOI**：10.1038/s41586-022-04849-0
- **本地原文**：[PDF](<D:/research/review/perturbation33references/21-cBAF complex components and MYC cooperate early in CD8+ T cell fate.pdf>)
- **核心数据入口**：[GEO SuperSeries GSE183619](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE183619)

## 一句话结论

在 CD8 T 细胞首次分裂的早期阶段，cBAF 染色质重塑复合体与 MYC 协同建立效应命运；干预 ARID1A/cBAF 可改变早期染色质可及性并提高记忆潜能与抗肿瘤能力。

## 数据护照

| 子系列 | assay | 主要设计 |
|---|---|---|
| GSE183615 | RNA-seq | Myc WT/KO；首次分裂 Arid1a WT/KO |
| GSE183616 | ATAC-seq I | naïve、Myc-high、Myc-low |
| GSE183617 | CUT&RUN | MYC、ARID1A、BRG1 在 WT/KO 背景的占位 |
| GSE183618 | ATAC-seq II | 首次分裂 Arid1a WT/KO；激活 Myc WT/KO |
| GSE184587 | microarray | 体内候选基因 sgRNA 与 NTC 比较 |
| GSE198894 | ATAC-seq III | cBAF 抑制剂 BRD-K98645985 vs DMSO |

## 1. 研究问题

CD8 T 细胞在首次分裂前后就开始分化为不同命运。本文追问：哪些染色质重塑因子与代谢/转录枢纽在这一早期窗口协同决定效应与记忆分化，以及该过程能否被遗传或药理干预以改善免疫治疗？

## 2. 实验设计与方法框架

作者结合体内/体外 CRISPR 筛选、首次分裂细胞分选、Arid1a 或 Myc 缺失、cBAF 药理抑制、RNA-seq、三批 ATAC-seq、CUT&RUN和 microarray。多组学分别测量表达、开放染色质和 MYC/cBAF 占位，从时间上聚焦激活最早期。

## 3. 数据规模与图谱组成

### 3.1 SuperSeries 不是一个单一队列

GSE183619 汇总 **6 个子系列**。每个子系列的 assay、细胞状态、遗传背景和重复结构不同；父系列中的 GSM/文件总量是跨 assay 汇总，不应直接写成同一批小鼠或同一种“单细胞图谱”。本文所有核心组学都是群体层面 assay。

### 3.2 GSE183615：RNA-seq，20 个 GEO 条目

该子系列包含两套表达实验：

- Myc WT 与 Myc-KO 激活 CD8 T 细胞，GEO 各列 **5 个条目**；
- 首次分裂的 Arid1a WT 与 Arid1a-KO 细胞，论文设计为 3 个生物学重复，但部分样本含技术/测序重复，GEO 展开后两组共 **10 个条目**。

因此页面合计 **20 个条目**。这里的条目数不完全等于独立生物学重复数，尤其 Arid1a 模块必须用样本名把 technical replicate 归并。补充文件包括两套 count CSV 及约 12.1 MB 的 RAW.tar，原始数据在 SRA。

### 3.3 GSE183616：ATAC-seq I，9 个样本

比较 naïve、Myc-high、Myc-low 三种 CD8 T 细胞状态，每组 **n=3**，合计 **9 个文库**。该数据定义早期 MYC 水平差异对应的染色质开放状态。处理文件 RAW.tar 约 4.0 GB，含 BED/BIGWIG；原始 SRA 数据体积较大。

### 3.4 GSE183617：CUT&RUN 占位图谱

CUT&RUN 测量 MYC、ARID1A、BRG1 在 WT、Arid1a-KO 或 Myc-KO 等背景的基因组占位，核心比较通常设置两个生物学重复。该子系列用于回答“MYC 与 cBAF 是否共同结合并相互依赖”，而不是做表达量比较。精确文库数和抗体/输入组合应直接从 GSE183617 sample table 导出，避免仅按图版反推。

### 3.5 GSE183618：ATAC-seq II，12 个样本

包含四组，每组 **n=3**：

- 首次分裂 Arid1a WT；
- 首次分裂 Arid1a-KO；
- 激活 Myc WT；
- 激活 Myc-KO。

合计 **12 个 ATAC 文库**。RAW.tar 约 7.6 GB，含 BED/BIGWIG；原始 SRA 数据约百 GB 量级，下载前应确认是否确需重新 peak calling。

### 3.6 GSE184587：体内 microarray

OT-I T 细胞转入 Lm-Ova 体系后，在约第 7.5 天比较候选基因 sgRNA 与非靶向对照（NTC），通常每组约 **4 只小鼠**。GEO 提供 CEL 原始文件和样本表。microarray 的探针空间与 RNA-seq不同，需单独标准化后在基因层面比较。

### 3.7 GSE198894：ATAC-seq III

WT CD8 T 细胞激活期间以 DMSO 或 **1 μM BRD-K98645985**（cBAF 抑制剂）处理，形成药理性 cBAF 干预的 ATAC 复制系列。该模块与 Arid1a-KO 的遗传结果互为参照。精确重复数应以子系列样本页为准，而不把文件数当作鼠数。

### 3.8 推荐下载方式

1. 从 GSE183619 逐一打开六个子系列，导出各自 sample table；先建一张跨系列索引表。
2. RNA 优先下载 count CSV；ATAC/CUT&RUN 优先下载作者提供的 BED/BIGWIG/peak 文件；microarray 下载 CEL 与 platform annotation。
3. 只有需要统一比对和 peak calling 时才从各子系列的 SRA Run Selector 获取 FASTQ。ATAC 原始数据较大，应先估算磁盘空间。
4. 用 `series`、`assay`、`genotype_or_drug`、`state`、`biological_rep`、`technical_rep`、`antibody`、`time` 建表。
5. 任何带技术重复的 RNA 条目先按生物学样本聚合；CUT&RUN 的抗体与 input/control 分开标注。

### 3.9 推荐整合逻辑

先在 RNA 中定义 Arid1a/Myc 依赖基因；在 ATAC 中识别对应开放区；再用 CUT&RUN 判断这些区域是否有 MYC、ARID1A、BRG1 共占位。药理 cBAF 抑制与遗传 Arid1a-KO 应按效应方向比较，而不是直接合并原始矩阵。

## 4. 主要结果

cBAF 组分与 MYC 在激活早期共同占据并开放效应命运相关调控区域。Arid1a/cBAF 干预改变首次分裂细胞的命运程序，减弱过早终末效应化并提高记忆与抗肿瘤潜力。

## 5. 机制理解

TCR 激活诱导 MYC，MYC 与 cBAF 在早期调控元件协作，使染色质进入支持效应分化的开放状态。改变 ARID1A/cBAF 活性会重新分配这一早期开放程序，进而影响后续命运。

## 6. 推荐重点阅读的图

- 筛选中 cBAF 组分命中与首次分裂命运结果。
- Arid1a-KO/Myc-KO RNA 与 ATAC 对比。
- MYC、ARID1A、BRG1 CUT&RUN 共占位图。
- 遗传或药理干预后的记忆形成和肿瘤控制图。

## 7. 创新性

把命运决定定位到首次分裂的早期窗口，并用表达、可及性、蛋白占位和功能实验构成多层因果链；同时展示遗传与药理 cBAF 干预的可操作性。

## 8. 局限性

数据跨多个平台和实验批次；部分 GEO 条目含技术重复；关键机制主要来自小鼠。cBAF 广泛参与细胞稳态，长期或强抑制可能影响扩增、基因组稳定性及其他组织。

## 9. 在综述中的定位

可作为“早期命运窗口的染色质重塑”代表，强调扰动时机与靶点本身同样重要。

## 10. 可直接写入综述的表述

> MYC 与 cBAF 在 CD8 T 细胞首次分裂早期协同塑造效应相关染色质，ARID1A/cBAF 干预可重新引导后续记忆—效应命运并增强抗肿瘤潜力。

## 11. 数据复用建议

建议按子系列建立可审计元数据，优先用作者处理后的 count/peak/track 复现跨组学交集，再决定是否下载大体量 raw reads。整合单位应是基因/调控区域的效应方向，不是把不同 assay 的样本拼成一张矩阵。

## 12. 转化与安全性关注

制造期短暂 cBAF 调节可能比永久删除更可控，但需要确定暴露窗口、残留、扩增产量和长期命运；永久 ARID1A 编辑则需额外评估克隆选择和肿瘤抑制相关风险。

## 13. 避免误读

- GSE183619 是 6 个子系列的集合，不是单一队列。
- GSE183615 的 20 个 GEO 条目含技术重复展开，不能等同于 20 个独立生物学重复。
- RNA、ATAC、CUT&RUN、microarray 应分别处理。
- 本文讨论的是早期命运调控，不等于 cBAF 抑制在所有时段都会提高疗效。

