# 《In vivo CD8+ T cell CRISPR screening reveals control by Fli1 in infection and cancer》精读

## 论文信息

- 作者：Zeyu Chen、Elizabeth Arai、Omar Khan 等
- 期刊：*Cell*
- 年份：2021；184(5): 1262–1280.e22；在线发表于 2021 年 2 月 25 日
- DOI：10.1016/j.cell.2021.02.019
- 原文：[Cell](https://doi.org/10.1016/j.cell.2021.02.019)
- PubMed：[PMID 33636129](https://pubmed.ncbi.nlm.nih.gov/33636129/)
- 免费全文：[PMC8054351](https://pmc.ncbi.nlm.nih.gov/articles/PMC8054351/)
- 组学总入口：[GEO GSE149839](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE149839)

## 一句话结论

作者建立 OpTICS，以 675 条 sgRNA 聚焦筛选 120 个转录因子在急性/慢性 LCMV 感染中的体内选择，发现 Fli1 是限制效应 CD8⁺ T 细胞分化的转录“safeguard”；Fli1 缺失开放 ETS:RUNX 元件、增强 Runx3 驱动的效应程序，并在多种感染和肿瘤模型中提升保护性免疫。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| OpTICS library | 675 sgRNA；120 TFs | 每个 DNA-binding domain 4–5 guides，含 Pdcd1 positive 和 non-selection controls |
| screen contexts | LCMV Armstrong 与 clone 13 | 急性清除 vs 慢性感染；1、2 周及多个组织 |
| screen cell system | Cas9⁺ P14 CD8⁺ T cells | 单克隆抗原特异性、成熟 T 细胞体内筛选 |
| Fli1 validation | 2 条 sgRNA，约 70–80% editing | 1–2 周提高细胞扩增约 5–20 倍；需结合功能和状态分析 |
| GEO SuperSeries | GSE149839，共 20 samples | GSE149836 ATAC 6 + GSE149837 CUT&RUN 6 + GSE149838 RNA 8 |
| 主 screen count | 不在 GEO | 论文明确 full TF CRISPR screening data 需向通讯作者申请 |
| processed files | ATAC/RNA count + CUT&RUN peaks/bigWig | 原始测序 reads 通过各子系列 SRA 获取 |

## 1. 研究要解决的问题

CD8⁺ T 细胞在急性感染中形成强效应反应，在慢性感染和肿瘤中则进入耗竭。已知许多促进效应或耗竭的 TF，但缺少一种在生理细胞数下、成熟抗原特异性 CD8⁺ T 细胞中直接筛选 TF 的方法。

作者问：

1. 哪些 TF 是效应/耗竭分化的促进或限制因子；
2. 同一 TF 在 acute 与 chronic antigen 中是否一致；
3. 删除 Fli1 是否只提高扩增，还是重塑效应谱系；
4. Fli1 如何在染色质层限制 Runx3；
5. 这种干预是否保留 memory/precursor，而不是以长期持续为代价。

## 2. OpTICS 筛选框架

### 2.1 平台

OpTICS（optimized T cell in vivo CRISPR screening）使用 Cas9⁺P14 T 细胞和 retroviral sgRNA，以较生理性的转移细胞数进入 LCMV infection。作者先优化转导和回收，再构建 120-TF domain-focused library。

675 条 sgRNA 不是全基因组文库。每个 TF 通常由 4–5 条针对 DNA-binding domain 的 guides 表征，因此扰动更可能破坏 TF 功能域。

### 2.2 多条件 readout

筛选覆盖：

- LCMV Armstrong：急性感染；
- LCMV clone 13：慢性感染；
- 约 1 周和 2 周时间点；
- PBMC、spleen、liver、lung 等组织。

sgRNA enrichment/depletion 可识别限制或促进 T 细胞积累的 TF。Fli1、Erg、Atf6、Irf2 等是限制效应反应的 hits；Batf、Irf4、Myc 等则表现为强负选择，符合其对 T 细胞扩增/功能的必需性。

### 2.3 机制组学

围绕 Fli1，作者做：

- bulk RNA-seq：sgFli1 vs control；
- ATAC-seq：Fli1 KO vs WT；
- CUT&RUN：FLI1 binding；
- Runx3 overexpression/interaction；
- acute/chronic viral、Listeria、influenza 和肿瘤模型验证。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

公开 GEO 仅包括围绕 Fli1 的机制组学，不包含 675-guide OpTICS 的完整 screen count。论文 Data and Code Availability 明确写明：RNA-seq、ATAC-seq 和 CUT&RUN 在 GEO；full transcription factor CRISPR screening data 需联系作者。

因此该文存在一个重要可复现边界：

- 可公开重做 Fli1 的表达、开放染色质和结合位点分析；
- 不能仅凭 GEO 完整重排 120 个 TF 的 screen hits。

### 3.2 多大规模、覆盖哪些生物情境

| 子系列 | 样本数 | 组成 |
|---|---:|---|
| [GSE149836](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE149836) | 6 | Fli1 KO vs WT CD8⁺ T cell ATAC-seq |
| [GSE149837](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE149837) | 6 | LCMV Cl13 day 8 FLI1 CUT&RUN 与 IgG |
| [GSE149838](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE149838) | 8 | sgFli1 5 replicates vs WT/control 3 replicates bulk RNA-seq |
| GSE149839 | 20 | 上述 3 个子系列总和 |

公开组学都来自小鼠 CD8⁺ T 细胞，重点是慢性 LCMV 情境。论文的肿瘤、Listeria、influenza 功能实验主要以流式、病原负荷和生存为 readout，不构成额外 GEO 转录组样本。

### 3.3 公共数据包有什么

| 文件/资源 | 体积 | 内容与用途 |
|---|---:|---|
| `GSE149839_RAW.tar` | 2.9 MB | SuperSeries 聚合的 BED；与子系列可能重复 |
| `GSE149836_RAW.tar` | 2.9 MB | ATAC BED files |
| `GSE149836_combUnionReadsWithLabels.txt.gz` | 1.5 MB | ATAC union regions/read matrix，适合快速差异可及性 |
| `GSE149837_peak_pool_Cl13_Fli1.final.bed.gz` | 19.7 KB | pooled FLI1 peaks |
| `GSE149837_peak_pool_Cl13_Fli1.final.bed.anno.txt.gz` | 113.5 KB | 注释后的 FLI1 peaks |
| `GSE149837_pool_Cl13_Fli1.clean.bw` | 310.4 MB | FLI1 CUT&RUN track |
| `GSE149837_pool_Cl13_Fli1.clean.subtractIgG.bw` | 407.2 MB | IgG-subtracted track |
| `GSE149837_pool_IgG.bw` | 557.2 MB | IgG control track |
| `GSE149838_Zeyu_Fli1koRNAseq_rawcounts.csv.gz` | 243.4 KB | 8-sample RNA raw count matrix |
| Supplementary Tables 1–2 | xlsx | 单 guides、120-TF library 与 screen 摘要；full count data 不公开 |

三个 bigWig 合计超过 1.2 GB，远大于 SuperSeries 页面上 2.9 MB 的 TAR；后者只聚合 BED，不代表全部 processed files。

### 3.4 如何获取：按目的选择

#### 路线 A：复核 Fli1 differential expression

下载 `GSE149838_Zeyu_Fli1koRNAseq_rawcounts.csv.gz`，建立 5 vs 3 的设计并复核 effector gene program。需从 GSM metadata 确认 sample label，而不是只按列数分组。

#### 路线 B：复核开放染色质和 FLI1 binding

用 GSE149836 的 union read matrix 做差异可及性；用 GSE149837 的 BED/bigWig 看 FLI1 占位、ETS:RUNX motif 和具体基因位点。大规模 motif/peak 重做则从 SRA 获取 FASTQ。

#### 路线 C：复现主 OpTICS 排名

先下载 Supplementary Table 2 获取文库构成和论文展示结果；若需要完整 675-guide × sample count matrix，根据论文说明联系通讯作者。报告中应明确这是“available upon request”，不能写成 GEO 开放。

#### 路线 D：原始组学

从每个子系列的 SRA Run Selector 导出 accession；ATAC、CUT&RUN、RNA 分别运行不同流程，并固定 mouse genome build 与 blacklist。

### 3.5 下载后先做什么

1. 将 GSE149836/7/8 分开建 manifest，再按 gene locus 连接；
2. 检查 BED/bigWig 的 genome build；
3. CUT&RUN 用 IgG control，不能只看 pooled FLI1 track；
4. RNA 用 count model，并以 sample 为单位统计；
5. ATAC 与 RNA 的 replicate 数不同，不做简单逐样本相关；
6. 任何全 screen 的定量复现结论都标注数据需申请。

## 4. 主要发现

Fli1 在 acute 与 chronic infection 中均限制 T_EFF 分化。两条 sgRNA 将 Fli1 editing 做到约 70–80%，可在 1–2 周使抗原特异性细胞扩增提高约 5–20 倍。

Fli1 缺失并未像 Tox 缺失那样耗尽 exhaustion precursor，而是增强效应输出并保留 memory/precursor。该扰动在 LCMV、Listeria、influenza 和多个肿瘤模型中提高保护性免疫。

## 5. 状态与分子 driver

RNA/ATAC/CUT&RUN 支持：FLI1 结合效应相关 cis-regulatory elements，并与 Runx1 等因子共同限制 ETS:RUNX 位点开放；Fli1 缺失后这些元件更可及，使 Runx3 更有效推动 T_EFF program。

因果模型为：

`FLI1 occupancy → constrained ETS:RUNX accessibility → limited RUNX3 effector drive`；

删除 Fli1 则解除这一染色质 safeguard。它改变的是 lineage commitment 的可及性门槛，而不只是某个单一 effector gene。

## 6. 推荐图版

- **Fig. 1**：OpTICS 设计、120-TF screen 与 Fli1 命中；适合方法页。
- **Fig. 2–3**：Fli1 KO 对 acute/chronic T cell state 的影响。
- **Fig. 4–5**：RNA/ATAC/CUT&RUN 与 ETS:RUNX/Runx3 机制；本综述最推荐。
- **Fig. 6–7**：感染和肿瘤保护。

若只能选一张，选 Fig. 5（机制）；若强调筛选平台，选 Fig. 1。

## 7. 创新价值

1. 在生理性成熟 CD8⁺ T 细胞数下进行体内 focused TF screen。
2. 同时比较急性和慢性抗原情境以及多组织/时间点。
3. 将 TF screen 命中连接到 CUT&RUN binding、ATAC accessibility 和 Runx3 function。
4. 找到一种增强效应但不明显牺牲 precursor/memory 的干预。

## 8. 局限性

1. 文库只有 120 TF，并非 genome-wide。
2. P14/LCMV 仍是单一 TCR 和模型感染。
3. sgRNA enrichment 主要反映积累，混合扩增、存活、迁移和分化。
4. full screen data 需申请，独立定量复现受限。
5. Fli1 是广泛造血 TF，长期编辑的安全性和谱系副作用未知。
6. 组学是 bulk，无法直接解析所有细胞亚群异质性。

## 9. 对本综述架构的作用

该文可用于“link cell state/function transitions with molecular drivers”：FLI1 不是状态 marker，而是控制 ETS:RUNX 染色质门的 driver。它也展示小型、机制聚焦文库在体内可优于全基因组文库的覆盖和可解释性。

由于没有活细胞连续观测，不能给出同一细胞 T_EFF/T_EX 转换速度；其证据是不同时间点与条件的群体终点。

## 10. 可直接用于综述的观点

> OpTICS 通过 675 条 sgRNA 聚焦筛选 120 个转录因子，发现 FLI1 在急性和慢性抗原环境中作为效应分化的染色质 safeguard；Fli1 缺失提高 ETS:RUNX 元件可及性并释放 RUNX3 效应程序，从而增强感染和肿瘤保护而不明显耗竭 precursor pool（Cell 2021, Chen）。

## 11. 避免误读

- 不要把 OpTICS 称为全基因组筛选；它是 120-TF focused screen。
- 不要把 675 写成基因数；它是 sgRNA 总数。
- 不要声称 full screen count 已在 GEO 公开。
- 不要把细胞积累单独解释为效应分化增强；机制需要 RNA/ATAC/CUT&RUN 支撑。
- 不要将小鼠 Fli1 deletion 的疗效直接外推到临床安全性。
