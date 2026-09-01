# 《CRISPR screens unveil signal hubs for nutrient licensing of T cell immunity》精读

## 论文信息

- **题目**：CRISPR screens unveil signal hubs for nutrient licensing of T cell immunity
- **作者**：Long et al.
- **期刊与年份**：Nature，2021
- **DOI**：[10.1038/s41586-021-04109-7](https://doi.org/10.1038/s41586-021-04109-7)
- **PMID / PMC**：[34795452](https://pubmed.ncbi.nlm.nih.gov/34795452/) / [PMC8887674](https://pmc.ncbi.nlm.nih.gov/articles/PMC8887674/)
- **核心数据入口**：[GEO GSE160598](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160598)
- **研究类型**：原代小鼠 T 细胞全基因组 CRISPR–Cas9 loss-of-function 筛选 + 聚焦复筛 + microarray、ATAC-seq、单细胞 RNA-seq 机制验证

## 一句话结论

该研究以 **TCR 刺激后的 mTORC1 活性（pS6）** 为功能读出，在原代小鼠 CD4 T 细胞中完成全基因组筛选和聚焦复筛，鉴定出营养转运、GATOR2、SAGA 去泛素化模块和 SWI/SNF 等调控枢纽，并证明 **CCDC101 缺失可通过提高营养转运和 mTORC1 信号重塑 Treg/抗肿瘤免疫状态**。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 主筛选细胞 | 原代小鼠 naïve CD4 T 细胞，Brie sgRNA library，pS6-high / pS6-low 分选 |
| 主筛选库规模 | 19,642 个基因、78,637 条基因靶向 sgRNA，另含 1,000 条 non-targeting controls；总规模约 79,637 条 |
| 主筛选重复 | 2 个生物学重复；低/高剂量 anti-CD3 条件均进行 pS6 极端群体分选 |
| 聚焦复筛 | 417 个候选基因；补充表记录约 9,354 条设计记录；3 个生物学重复 |
| 高置信候选 | 346 个复现候选：286 个正向调控因子、60 个负向调控因子；主筛共报出 417 个候选 |
| 公共数据总入口 | GEO SuperSeries **GSE160598**，52 个样本、6 个 SubSeries |
| microarray | 30 个 Clariom S Mouse 芯片样本：6 + 8 + 16 |
| ATAC-seq | 8 个样本：4 个对照 + 4 个 Ccdc101 sgRNA；约 10 million nucleosome-free reads/样本 |
| scRNA-seq | 4 个 10x 样本；最终 10,202 个细胞，WT 5,281、Ccdc101 条件敲除 4,921 |
| CRISPR readout 测序 | GSE199813，10 个样本；SRA 共约 9,727 万 reads |
| 主要下载方式 | GEO processed files / GEO RAW tar、SRA Run Selector/ENA FASTQ、Nature Supplementary Tables |
| 关键限制 | 主筛选对象主要是 CD4/Treg 体系，不是 CD8 CAR-T；scRNA-seq 是肿瘤免疫生态验证数据，不等同于筛选细胞的单细胞图谱 |

## 1. 研究要解决的问题

T 细胞接受 TCR 信号后，需要把抗原受体信号与氨基酸、葡萄糖等营养条件整合，才能激活 mTORC1、完成增殖和效应分化。已有研究知道若干经典营养感知分子，但缺少一个在原代 T 细胞中、以功能信号为直接读出的系统性遗传图谱。

本文试图回答三个相互衔接的问题：

1. 哪些基因控制 TCR 刺激后 mTORC1 的开启或关闭？
2. 这些基因中，哪些组成可干预的营养“许可”节点，而不是一般性的存活或增殖因子？
3. 操纵这些节点能否改变 Treg 状态和肿瘤免疫反应？

对本综述而言，它位于“**从细胞状态的分子测量走向可控状态转换**”这一段：先用 pS6 将状态量化，再通过 CRISPR 找到驱动该状态的分子节点，最后用多组学和肿瘤模型验证状态后果。

## 2. 方法框架：以 pS6 为状态传感器的两阶段 CRISPR 筛选

### 2.1 主筛选

- 从 Cas9 小鼠获得原代 naïve CD4 T 细胞。
- 以 Brie lentiviral sgRNA library 转导；MOI 约 0.3，约 20% 细胞被转导。
- 每个生物学重复起始约 `2 × 10^8` 个 naïve CD4 T 细胞。
- 激活和扩增后，分别使用低剂量与高剂量 anti-CD3 刺激。
- 根据磷酸化 S6（pS6）进行流式分选，收集最高和最低不超过 10% 的群体。
- 对 sgRNA cassette 测序，以高/低 pS6 群体中的 sgRNA 富集变化推断正、负向 mTORC1 调控因子。

该设计的重要性在于：读出不是“细胞最终活了多少”，而是与营养许可直接相关的 **连续信号状态 pS6**。因此，它更接近对细胞状态坐标轴进行遗传扰动。

### 2.2 聚焦复筛

作者将主筛得到的 417 个候选组成 focused library，在更高覆盖度和 3 个生物学重复下复筛。复筛仍以 pS6-high / pS6-low 为读出，但显著提高了每条 guide 的细胞覆盖度，用于降低全基因组筛选的采样噪声。

### 2.3 机制验证

重点验证包括：

- SAGA complex 成员 **Ccdc101**；
- SWI/SNF 与营养感知相关节点；
- 营养转运、氨基酸摄取和 mTORC1 功能实验；
- Ccdc101 缺失后的转录组、染色质可及性和肿瘤免疫单细胞图谱；
- Foxp3 条件敲除模型中 Treg 状态和抗肿瘤反应。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

本文数据并不是一个单一矩阵，而是由“**筛选层—机制层—组织生态层**”组成：

1. **筛选层**：全基因组和聚焦 CRISPR sgRNA abundance 数据，核心输出是每条 sgRNA 在 input、pS6-high、pS6-low 和不同刺激条件中的计数及基因层统计量。
2. **机制层**：多个 microarray 批次、Ccdc101 扰动的 ATAC-seq，以及差异开放区域/差异表达结果。
3. **组织生态层**：肿瘤浸润免疫细胞的 10x scRNA-seq，用于观察 Treg 特异 Ccdc101 缺失如何改变 CD8、Treg、髓系等免疫群体。

因此，本文可用于绘制“分子驱动因子—信号表型—转录/染色质状态—肿瘤免疫生态”的连接图，但不能把全部 52 个 GEO 样本理解为同一个单细胞 atlas。

### 3.2 主筛选的规模、覆盖度和组成

#### 全基因组 Brie 筛选

- 基因：19,642 个。
- 基因靶向 sgRNA：78,637 条。
- non-targeting sgRNA：1,000 条。
- 总 guide 数：约 79,637 条。
- 生物学重复：2。
- 每个重复起始细胞：约 `2 × 10^8`。
- 转导条件：MOI 0.3，约 20% 转导率。
- input 保存：约 `5 × 10^7` 个细胞，对应约 630 cells/sgRNA。
- 每个 pS6 极端分选样本回收约 `3–9 × 10^6` 个细胞，即约 38–113 cells/sgRNA。
- 条件：低剂量和高剂量 anti-CD3；pS6-high 与 pS6-low 极端群体。

主筛选共提出 **292 个正向调控因子和 125 个负向调控因子**，合计 417 个候选。

#### focused 复筛

- 候选基因：417 个。
- 设计记录：补充表约 9,354 行；包含多条候选 guide 和对照。
- 生物学重复：3。
- 每个重复起始约 `1 × 10^8` 个细胞。
- input 约 `3 × 10^6`，文中给出的覆盖度超过 1,000×。
- 分选后每个样本约 `2–3 × 10^6` 个细胞，覆盖度超过 700×。
- 复现率：约 83%。
- 高置信结果：286 个正向、60 个负向调控因子，共 346 个。

这里需要区分“主筛选 417 个候选”和“复筛 346 个高置信候选”，两者不应互换。

### 3.3 GEO SuperSeries 的 52 个样本如何组成

总入口为 [GSE160598](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160598)。该 SuperSeries 汇集 6 个子系列：

| GEO 子系列 | 数据类型 | 样本数 | 内容 |
|---|---:|---:|---|
| [GSE160550](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160550) | microarray | 6 | steady-state 条件的基因表达芯片 |
| [GSE160552](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160552) | microarray | 8 | TCR stimulation 条件芯片 |
| [GSE160554](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160554) | microarray | 16 | anti-CD3 stimulation 条件芯片 |
| [GSE160593](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160593) | ATAC-seq | 8 | 4 个对照与 4 个 sgCcdc101 样本 |
| [GSE181341](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE181341) | 10x scRNA-seq | 4 | WT 2 个重复、Treg-specific Ccdc101 KO 2 个重复 |
| [GSE199813](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE199813) | CRISPR screen sequencing | 10 | Brie library sgRNA count/readout |

总样本数为 `6 + 8 + 16 + 8 + 4 + 10 = 52`。

### 3.4 每一类公共数据的实际规模

#### microarray：30 个样本

- 平台：Affymetrix Clariom S Mouse。
- 三批样本分别为 6、8、16，共 30 个。
- GEO 提供 CEL 原始文件压缩包；三个子系列的 RAW tar 约 6.7 MB、9.1 MB 和 20 MB。
- 适合复核不同扰动/刺激状态下的基因表达，不适合进行细胞亚群分解。

#### ATAC-seq：8 个样本

- 4 个 non-targeting control，4 个 Ccdc101-targeting 样本。
- 每个 library 约使用 50,000 个细胞。
- 文中目标约为每样本 10 million nucleosome-free reads。
- SRA：BioProject [PRJNA673707](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA673707)，Study SRP290696。
- 8 个 paired-end runs 合计约 10.64 亿 reads、108.6 Gb bases。
- GEO processed matrix：`GSE160593_Ccdc101_ATAC_summarizedCounts.tsv.gz`，约 1.4 MB。
- 作者鉴定约 25,646 个差异开放染色质区域。

#### scRNA-seq：4 个样本、10,202 个细胞

- WT：2 个生物学重复，每个重复由 3–4 只小鼠混池。
- Treg-specific Ccdc101 KO：2 个生物学重复，每个重复由 3–4 只小鼠混池。
- 上机前按 Treg、其他免疫细胞和髓系细胞约 `1:2:1` 混合，以提高关键群体覆盖。
- 10x 每个 channel 目标约 8,000 个细胞。
- 质控后最终细胞：10,202 个。
- WT：5,281 个；KO：4,921 个。
- 总体聚类：14 个 clusters，归纳为约 12 类免疫细胞；CD8 T 细胞进一步分为 4 个亚型。
- SRA：BioProject [PRJNA751600](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA751600)，Study SRP330912。
- 4 个 paired-end runs 合计约 4.58 亿 reads、41.2 Gb bases。
- GEO processed archive 约 104 MB，内含 4 个样本级 tar 文件，通常包括 10x 矩阵相关文件。

#### CRISPR screen sequencing：10 个样本

- GEO：GSE199813。
- BioProject：[PRJNA821534](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA821534)，Study SRP357843/相应 SRA study 可从 GEO 进入。
- 10 个 single-end runs 合计约 97,265,142 reads、4.86 Gb bases。
- 关键 processed file：`GSE199813_LibBrie.sgRNA.count.txt.gz`，约 2.0 MB。

对重分析而言，这个 count 文件通常比从 FASTQ 重新识别 guide 更方便；若要复查去接头、guide 匹配和零计数处理，才需要下载 SRA FASTQ。

### 3.5 Supplementary Tables 的组成

Nature 页面提供的表格是理解筛选设计和统计结果的关键：

| 文件 | 主要内容 | 工作表/规模要点 |
|---|---|---|
| Supplementary Table 1 | 全基因组 Brie 主筛数据 | 两套 pipeline 的 gene-level 与 guide-level 结果；约 19.7k genes、78.6–79.6k guides |
| Supplementary Table 2 | focused library 设计 | 约 9,354 行、6 列 |
| Supplementary Table 3 | focused screen 结果 | gene-level 约 421 行；guide-level 约 2,684 行 |
| Supplementary Table 4 | 验证 sgRNA 序列 | 约 221 行 |
| Supplementary Table 5 | Smarcb1 相关 microarray 差异表达 | 约 2,024 行 |
| Supplementary Table 6 | Ccdc101 相关 microarray 差异表达 | 约 1,121 行 |
| Supplementary Table 7 | mutagenesis primers | 约 221 行 |

主筛 Supplementary Table 1 中，guide-level 两套流程的行数略有差异，是因为不同 pipeline 对 controls、缺失值或 guide 过滤的处理不同；使用时应保留原始 sheet 名和统计列定义。

### 3.6 下载方式

#### 方式 A：直接下载 GEO processed files

进入各 accession 页面，点击 **Series Matrix File(s)** 或 **Supplementary file**。例如：

```text
https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160598
https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160593
https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE181341
https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE199813
```

CRISPR count 的 FTP 直链遵循 GEO 路径规则：

```text
https://ftp.ncbi.nlm.nih.gov/geo/series/GSE199nnn/GSE199813/suppl/GSE199813_LibBrie.sgRNA.count.txt.gz
```

#### 方式 B：SRA Run Selector 下载 FASTQ

从 GEO 页面点击 **SRA Run Selector**，导出 `SraRunTable.txt` 或 accession list，再使用 SRA Toolkit：

```bash
prefetch SRR_ACCESSION
fasterq-dump SRR_ACCESSION --split-files --threads 8
gzip SRR_ACCESSION_*.fastq
```

批量下载前应先在 Run Selector 核对 library layout；ATAC-seq 和 scRNA-seq 为 paired-end，而 GSE199813 的 screen readout 为 single-end。

#### 方式 C：Nature supplementary tables

Supplementary Table 1 的直链示例：

```text
https://media.springernature.com/original/springer-static/esm/art%3A10.1038%2Fs41586-021-04109-7/MediaObjects/41586_2021_4109_MOESM3_ESM.xlsx
```

后续表格的文件编号依次为 `MOESM4` 至 `MOESM9`。若浏览器不能直接预览，应保存为 `.xlsx` 后用 Excel、LibreOffice 或 Python/openpyxl 读取。

### 3.7 下载后建议整理成什么结构

```text
61_Long_2021/
├── paper_and_methods/
├── screen/
│   ├── genomewide_counts/
│   ├── focused_screen/
│   └── library_annotations/
├── microarray/
│   ├── GSE160550/
│   ├── GSE160552/
│   └── GSE160554/
├── atac/GSE160593/
├── scrna/GSE181341/
├── validation_tables/
└── metadata/
    ├── SraRunTable_*.txt
    └── sample_manifest.tsv
```

建议制作一个 `sample_manifest.tsv`，至少包含 accession、modality、genotype、stimulation、replicate、raw/processed、文件名和来源 URL。本文 accession 多、批次多，没有 manifest 很容易把组织 scRNA-seq 与体外筛选样本混在一起。

## 4. 主要生物学发现

### 4.1 营养许可网络比经典 mTOR 通路更广

筛选不仅回收到 mTORC1、氨基酸感知和营养转运的已知节点，还识别到分泌运输、蛋白复合体和染色质调控因子。SEC31A、SCF–FBXW5–SEC13、GATOR2 等结果提示，T 细胞的营养许可同时依赖膜运输、代谢物进入和细胞内信号组装。

### 4.2 CCDC101/SAGA DUB 是可干预的负调控节点

Ccdc101 缺失提高营养转运相关基因表达和 mTORC1 活性。它不是单纯改变一个下游 marker，而是通过染色质/转录调控改变细胞对营养环境的响应能力。

### 4.3 Treg 内的营养状态可以重塑肿瘤免疫生态

Treg-specific Ccdc101 缺失改变肿瘤内 Treg 状态，并伴随 CD8 T 细胞亚群和其他免疫群体变化。scRNA-seq 的作用是验证“操纵一个 Treg 分子节点后，整个免疫生态如何迁移”，而不是发现筛选 hit。

## 5. 细胞状态、功能和分子驱动如何被连接

本文给出一条较完整的因果链：

```text
CRISPR 基因扰动
→ pS6/mTORC1 状态改变
→ 营养转运与染色质可及性改变
→ Treg 功能状态改变
→ 肿瘤中 CD8 与髓系生态改变
→ 抗肿瘤免疫结局改变
```

其中 pS6 是可量化的中间状态，ATAC/microarray 是分子机制层，scRNA-seq 是组织生态层。这种分层非常适合本综述“link cell state/function transitions with molecular drivers”的论述。

## 6. 对细胞治疗状态导航的启示

1. **选择动态信号作为筛选 readout**：比单独使用终点存活更能捕获状态转换节点。
2. **先宽筛、再高覆盖复筛**：417 个初筛候选压缩到 346 个高置信节点，展示了可复现状态控制图谱的构建方式。
3. **营养环境是状态导航的一部分**：细胞内基因工程和培养基条件不能割裂设计。
4. **调节性 T 细胞也可作为工程靶点**：虽然本文不是 CAR-T 工程，但说明通过改变抑制性免疫细胞状态可间接释放 CD8 效应。

## 7. 可复用的分析思路

- 对 sgRNA count 使用 guide-level 与 gene-level 双层统计，并保留两套 pipeline 的一致性证据。
- 将 pS6-high / pS6-low 的 log-fold change 与细胞数覆盖度同时报告。
- 对 focused screen 单独计算复现率，不把初筛排名直接视为最终 hit。
- ATAC-seq 可将 differential accessibility 与附近营养转运基因表达相连。
- scRNA-seq 应在样本层而非细胞层做生物学重复推断；本文只有每组 2 个 pooled biological replicates，不能把 10,202 个细胞当成 10,202 个独立样本。

## 8. 推荐图版

### 主文优先图

- **Fig. 1**：全基因组筛选设计和候选网络，适合说明“功能状态驱动的 CRISPR screen”。
- **focused screen / validation 图**：展示从全基因组 hit 到高置信营养许可节点的收敛过程。
- **Ccdc101 机制图**：连接营养转运、mTORC1 与染色质调控。
- **肿瘤 scRNA-seq 图**：展示 Treg 操纵如何改变 CD8 和肿瘤免疫生态。

### 综述中建议重绘

建议把原文多个 panel 重绘成四层图：

```text
遗传扰动层：Brie CRISPR screen
状态读出层：pS6-high / pS6-low
分子机制层：microarray + ATAC-seq
组织功能层：tumor scRNA-seq + tumor control
```

## 9. 创新价值

- 在原代 T 细胞中以信号状态而非单一生存表型进行全基因组筛选。
- 将全基因组筛选、focused rescreen、多组学和体内免疫模型串成因果验证链。
- 把营养感知从“代谢背景变量”提升为可工程化的 T 细胞状态控制层。
- 公共数据的 modality 较完整，适合二次分析和教学演示。

## 10. 局限性

1. 主筛选主要基于小鼠 CD4 T 细胞，向人 CD8/CAR-T 外推需要再验证。
2. pS6 是 mTORC1 的重要读出，但不能代表完整的代谢和功能状态。
3. scRNA-seq 每组只有 2 个 pooled biological replicates，样本层统计功效有限。
4. SuperSeries 混合了不同实验、不同平台；52 个样本不是一个统一 atlas。
5. guide-level 统计在补充表、processed count 和不同 pipeline 间有口径差异，重分析需先统一 library annotation。
6. 操纵 Treg 营养状态可能带来系统性免疫毒性，不能直接等同于细胞治疗产品内的安全工程。

## 11. 对本章节的作用

| 综述模块 | 本文贡献 |
|---|---|
| Quantitatively characterizing cell phenotypes | 用 pS6 极端分选把营养许可状态转为可筛选表型 |
| Techniques to perturb/manipulate cell states | 原代 T 细胞全基因组 CRISPR loss-of-function + focused rescreen |
| Link state/function transitions with drivers | Ccdc101/SAGA、SWI/SNF、营养运输节点连接到 mTORC1 与免疫功能 |
| Optimize conditions for navigating T cell states | 提示培养营养条件与基因工程应联合优化 |
| Build real-time optimization systems | 提供“动态 phospho-signal 作为在线状态指标”的原型，但尚非实时闭环系统 |

## 12. 可直接用于综述的观点

> Functional CRISPR screens that sort primary T cells by an intracellular signaling state can identify causal regulators of state transitions that would be missed by proliferation- or survival-only readouts.

> Nutrient licensing is not merely a culture variable: it is an engineerable control layer linking transport, chromatin regulation and mTORC1 activity to T-cell function.

> Multi-modal validation is essential for converting a screening hit into a navigable state mechanism: guide enrichment locates the driver, phospho-flow defines the immediate state, chromatin and transcriptomic assays explain the mechanism, and single-cell profiling measures the tissue-level consequence.

## 13. 避免误读

- **不是 CD8 CAR-T 全基因组筛选**：主筛使用原代小鼠 CD4 T 细胞，并重点进入 Treg 机制。
- **不是 52 个样本的单细胞图谱**：GSE160598 是多平台 SuperSeries，只有 4 个样本属于 scRNA-seq。
- **10,202 是质控后细胞数，不是独立生物学重复数**：每个基因型只有 2 个 pooled replicates。
- **417 与 346 含义不同**：417 是主筛候选，346 是 focused screen 中复现的高置信候选。
- **GEO 中的 omics 数据不能替代筛选补充表**：完整 guide/gene ranking 仍需结合 Supplementary Tables 1–3。
- **Ccdc101 的抗肿瘤作用发生在特定遗传和模型背景中**，不能直接推导为临床可用靶点。

## 数据与资源链接

- 论文：[Nature](https://www.nature.com/articles/s41586-021-04109-7)
- 全部公共系列：[GSE160598](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160598)
- ATAC-seq：[GSE160593](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160593)
- scRNA-seq：[GSE181341](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE181341)
- CRISPR screen count：[GSE199813](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE199813)
- ATAC BioProject：[PRJNA673707](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA673707)
- scRNA BioProject：[PRJNA751600](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA751600)
- screen BioProject：[PRJNA821534](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA821534)
