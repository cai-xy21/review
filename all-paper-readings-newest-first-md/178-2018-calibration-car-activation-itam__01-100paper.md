# 《Calibration of CAR activation potential directs alternative T cell fates and therapeutic potency》精读

## 论文信息

- 作者：Feucht J, Sun J, Eyquem J, Ho YJ, Zhao Z, Leibold J, Dobrin A, Cabriolu A, Hamieh M, Sadelain M
- 期刊：*Nature Medicine* 25:82–88；在线发表于 2018 年 12 月 17 日
- DOI：[10.1038/s41591-018-0290-5](https://doi.org/10.1038/s41591-018-0290-5)
- PubMed：[PMID 30559421](https://pubmed.ncbi.nlm.nih.gov/30559421/)
- 开放全文：[PMC6532069](https://pmc.ncbi.nlm.nih.gov/articles/PMC6532069/)
- RNA-seq：[GEO GSE121226](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE121226)
- 原始 reads：[SRA SRP165725](https://www.ncbi.nlm.nih.gov/sra/?term=SRP165725)；[BioProject PRJNA496405](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA496405)

## 一句话结论

在 TRAC 定点整合的 CD19-28ζ CAR 中，将三个 CD3ζ ITAM 只保留膜近端第一个（1XX）可把过强激活校准到兼具效应和记忆的区间，获得更慢分化、更低耗竭、更强持久性和肿瘤清除；一个 ITAM 并非都等价，其相对位置同样决定命运。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| RNA-seq Series | GSE121226 | 论文新产生的 bulk RNA 数据 |
| GEO 样本 | 27 个 GSM | 9 个 CAR 样本 + 18 个参考亚群样本 |
| CAR 条件 | TRAC-1928ζ、TRAC-1XX、TRAC-XX3 | 每个技术三重复；均为 naive T 起始、CD19 刺激后 |
| 参考亚群 | TN、TSCM、TEFF | 每类 6 个重复；用于把 CAR 状态映射到已知命运程序 |
| 测序 | HiSeq 4000，PE50 | 平均 30.6 million paired reads/样本 |
| 处理文件 | `GSE121226_Feucht_DESeq2_normalized.txt.gz` | 适合快速重做 PCA/GSEA；差异分析最好从 counts/raw 开始 |
| 体外/体内 | 连续刺激、杀伤、细胞因子、NALM6-luc、rechallenge | 图源数据可请求作者；不在 GEO 表达矩阵中 |

## 1. 研究要解决的问题

经典 1928ζ CAR 同时含 CD28 和 CD3ζ 的 3 个 ITAM，能快速产生效应，但容易牺牲持久性。作者不是简单比较“有无共刺激”，而是问：**激活势能是否存在最优区间，能否用 ITAM 剂量与位置工程把细胞导向更有治疗价值的命运？**

## 2. ITAM 校准框架

### 2.1 构建设计

以 1928ζ 为母体，将 CD3ζ 三个 ITAM 的关键 tyrosine 置换为 phenylalanine，产生：

- **1XX**：仅膜近端 ITAM1 可用；
- **X2X**：仅 ITAM2 可用；
- **XX3**：仅远端 ITAM3 可用；
- **X23**：保留 ITAM2/3；
- **1928ζ**：三个 ITAM 均可用。

先用 retroviral 系统快速筛选，再将关键构建定点整合至 TRAC，控制插入位置和表达差异。

### 2.2 readout

短期杀伤、连续抗原刺激、流式分化/耗竭、细胞因子、NALM6-luc stress test、长期生存和 tumor rechallenge 与 bulk RNA-seq 共同评价“效应—记忆—耗竭”平衡。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

公开核心是一套 **27 个 bulk RNA-seq 样本的命运参照图谱**。作者把三种 CAR 构建的 CD8⁺ T 细胞与分选得到的 naive、stem-cell memory 和 effector CD8⁺ T 细胞放在同一表达空间中，从而回答不同 ITAM 设计更像哪种天然命运。

RNA 获取流程为：sorted naive T 细胞定点编辑 7 天后，将 TRAC-1928ζ、TRAC-1XX、TRAC-XX3 用 irradiated 3T3-CD19 刺激 24 h，再磁选 CD8⁺；每个 CAR 技术三重复。TN/TSCM/TEFF 各 6 个样本作为参考。

### 3.2 多大规模、覆盖哪些生物情境

| 组别 | GEO 样本数 | 生物意义 |
|---|---:|---|
| TRAC-1928ζ | 3 | 强效应、较快分化与耗竭参照 |
| TRAC-1XX | 3 | 膜近端单 ITAM；治疗最优构建 |
| TRAC-XX3 | 3 | 远端单 ITAM；信号不足/功能衰减 |
| TN | 6 | naive 参考 |
| TSCM | 6 | stem-cell memory 参考 |
| TEFF | 6 | effector 参考 |
| 合计 | 27 | 9 CAR + 18 reference |

每个 RNA 样本平均约 30.6 million paired-end reads，最多 4.69% ribosomal reads，mRNA bases 平均 69.3%。这是一套深度较高但样本数较小的 bulk 数据，适合 pathway-level 比较，不适合训练复杂预测模型。

体内部分覆盖：

- 5×10^4、1×10^5 或 5×10^5 CAR⁺ T 细胞的 NALM6 模型；
- 低剂量 stress test；
- D17 组织回收和长期生存；
- tumor rechallenge；
- 部分每组 n=5 小鼠或依 figure legend 的规模。

### 3.3 GEO 数据包有什么

| 文件/入口 | 内容 | 用途 |
|---|---|---|
| `GSE121226_Feucht_DESeq2_normalized.txt.gz` | 27 样本的 DESeq2 normalized expression | PCA、heatmap、signature/GSEA 快速复现 |
| GSE121226 family SOFT/MINiML | GSM 标题、source、characteristics、SRA link | 正确重建 CAR/reference 分组 |
| SRP165725 | 27 个样本原始 FASTQ | 统一重新比对与 count quantification |
| PRJNA496405 | BioProject 级元数据 | 批量关联 BioSample/SRA |

论文还复用了外部耗竭 signature（例如 GSE41867）和既有 T cell subset gene sets。它们是分析参考，不是 GSE121226 新产生的数据。

### 3.4 如何获取：按目的选择

#### 路线 A：快速复现命运映射

下载 normalized matrix 和 SOFT 元数据，将 27 列按 CAR/TN/TSCM/TEFF 分组，重做 PCA、hierarchical clustering 和基因集评分。保持技术重复结构，不要将 27 列解释为 27 位供者。

#### 路线 B：重做差异表达

从 SRA Run Selector 导出 run table，下载 FASTQ；STAR 对齐、featureCounts/HTSeq 计数后，用 DESeq2 以 construct 为主要设计。技术重复不足以支撑 donor-level 泛化，P 值应与效果量和功能实验共同解释。

#### 路线 C：重做外部 signature 分析

单独下载 GSE41867 或从作者 Supplementary Methods 获取 signature 定义。报告中明确区分“论文产生的 GSE121226”和“论文用于 GSEA 的外部数据”。

#### 路线 D：体内/流式原始数据

论文 Data availability 说明 raw figure data 可向通讯作者申请。GEO 只覆盖 RNA-seq，不含逐鼠 BLI、raw FCS、rechallenge 纵向表。

### 3.5 下载后先做什么

```r
expr <- read.delim("GSE121226_Feucht_DESeq2_normalized.txt.gz",
                   row.names = 1, check.names = FALSE)
stopifnot(ncol(expr) == 27)
```

然后从 SOFT 而非列名猜测生成 metadata。若直接对 normalized matrix 做 PCA，先 `log2(x+1)` 并检查作者文件是否已转换；不要对已 log 数据再次取 log。

## 4. 主要生物学发现

- 单 ITAM 构建并非简单减弱版 1928ζ：1XX 明显优于 XX3；
- 1XX 在低剂量 NALM6 模型中快速清瘤并形成长期记忆；
- 1928ζ 形成强早期效应但更快出现耗竭和持久性不足；
- XX3 更接近 naive-like/低触发状态，但长期细胞毒功能不足；
- 1XX 保留 TCF7、BCL6、LEF1、KLF2、CCR7、SELL、IL7R 等记忆相关程序，同时维持有效杀伤；
- 1XX 的优势是“平衡”而不是最大化静态 stemness。

## 5. 信号剂量与位置共同定义命运

如果只有“ITAM 数量”重要，1XX、X2X、XX3 应相近；实际 1XX 最佳，说明膜近端到远端的空间位置改变 signalosome 组装和信号质量。该结果支持将 CAR 激活势能视为多维参数：

```text
activation potential = ITAM number × ITAM position × receptor expression × antigen history
```

因此，优化不能只比较 1、2、3 个 ITAM 的总量。

## 6. 功能—状态映射

RNA-seq 将 1928ζ、1XX、XX3 映射到 TEFF、TSCM、TN 参考，但这种“更像”不等同于细胞身份完全相同。真正治疗价值来自配对功能：1XX 同时保留长期扩增、细胞毒、多因子分泌和 rechallenge 保护。

## 7. 推荐图版

- **Fig. 1**：ITAM 构建设计与 stress test，是最清晰的工程—疗效图。
- **Fig. 2–3**：TRAC knock-in 后的分化、持久性和耗竭。
- **Fig. 4**：bulk RNA 命运映射，最适合“molecular drivers”章节。

如果只能选一组，选 Fig. 1 + Fig. 4。

## 8. 创新价值

1. 将 CAR signaling strength 从定性概念转为可设计的 ITAM 参数。
2. 证明膜近端单 ITAM 可优于完整 CD3ζ。
3. 使用 TRAC 定点整合降低表达位点混杂。
4. 连接受体结构、转录命运、长期功能和体内疗效。

## 9. 局限性

1. RNA-seq 是 bulk、样本量小且以技术重复为主。
2. 只深入比较 1928ζ/1XX/XX3，其他靶点和 costimulatory domains 的最佳校准可能不同。
3. 健康供者和 NSG 模型无法替代患者制造物和免疫完整微环境。
4. 强调“1XX 最优”可能掩盖抗原密度、肿瘤负荷和组织环境依赖。
5. 体内/流式 raw data 不在 GEO，完整复算需申请。
6. 外部 signature 映射不能证明真实谱系来源。

## 10. 对本章节的作用

该文是“**优化导航条件**”的核心例子：状态不应朝单一方向最大化，而应把激活调到效应与记忆之间的治疗最优区间。它也提示实时系统的控制目标可能不是最低 exhaustion score，而是长期杀伤与自我更新的多目标最优。

## 11. 可直接用于综述的观点

> 在 TRAC 定点整合的 CD19-28ζ CAR 中，保留膜近端单个 ITAM 的 1XX 构建比完整三 ITAM 或远端单 ITAM 构建更好地平衡效应和记忆程序，并在低剂量肿瘤模型中获得持久清除，表明 CAR 激活的强度与拓扑位置共同决定 T 细胞命运（Nature Medicine 2019, Feucht）。

## 12. 避免误读

- 不要写成“ITAM 越少越好”。
- 不要把 XX3 的 naive-like 程序理解为更优 stemness。
- 不要把 27 个 RNA 样本当成 27 位供者。
- 不要把 GSE41867 等外部 signature 写成本文产生的数据。
- 不要忽视所有关键构建均在 TRAC 位点受控表达这一前提。

## 13. 数据复用优先级

优先下载 GSE121226 normalized matrix 和 SOFT，重做 Fig. 4 的命运映射；需要严谨差异表达时再取 SRP165725 raw reads。体内剂量—疗效和 rechallenge 数据若用于定量模型，应从 Source Data/作者处取得，而不是从图像手工数字化。
