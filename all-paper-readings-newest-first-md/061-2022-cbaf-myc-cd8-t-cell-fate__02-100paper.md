# 《cBAF complex components and MYC cooperate early in CD8+ T cell fate》精读

## 论文信息

- **题目**：cBAF complex components and MYC cooperate early in CD8+ T cell fate
- **作者**：Guo et al.
- **期刊与年份**：Nature，2022
- **DOI**：[10.1038/s41586-022-04849-0](https://doi.org/10.1038/s41586-022-04849-0)
- **PMID / PMC**：[35732731](https://pubmed.ncbi.nlm.nih.gov/35732731/) / [PMC9623036](https://pmc.ncbi.nlm.nih.gov/articles/PMC9623036/)
- **核心数据入口**：[GEO GSE183619](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE183619)
- **研究类型**：小鼠 OT-I CD8 T 细胞体内 pooled CRISPR screen + RNA-seq、ATAC-seq、CUT&RUN、microarray + cBAF inhibitor 与 CAR-T 验证

## 一句话结论

本文在急性感染的体内 CD8 T 细胞分化过程中筛查 337 个表观遗传调控因子，发现 **cBAF 复合体与 MYC 在激活早期协同开放效应分化程序**；遗传删除 ARID1A/SMARCD2 或短暂药理抑制 cBAF 可将细胞偏向记忆命运，并提高小鼠和人 CAR-T 的后续功能。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 主筛选 | Cas9-OT-I CD8 T cells，Lm-Ova infection，体内 effector/memory fate selection |
| library | 337 个 genes × 6 sgRNAs = 2,022 targeting guides；另 200 NTC；总计 2,222 |
| 筛选供者/受者 | 2 个 Cas9-OT-I donor preparations；转入 21 个 Cas9 hosts |
| 转导率 | 约 20% |
| 输入覆盖 | 0.5 million input cells，超过约 200×/sgRNA |
| transfer | 每只 host 约 0.2 million transduced cells |
| 分选 | day 7.5：KLRG1-high CD127-low terminal effector vs KLRG1-low CD127-high memory precursor；另比较 day 36 vs day 7.5 |
| 主公共数据 | GSE183619，101 个 samples，多个 modality |
| GSE 组成 | 28 bulk RNA-seq、30 ATAC-seq、27 CUT&RUN、16 Clariom S microarray，共 101 |
| GEO RAW | 约 18 GB，含 RSEM counts、BED、bigWig 等 processed/raw-derived tracks |
| raw reads | PRJNA761407 / SRA |
| 关键限制 | GEO 主要存放机制验证 omics；完整 CRISPR guide ranking/count 需结合 Fig. 1 Source Data 和 Supplementary materials |

## 1. 研究要解决的问题

初始激活后的 CD8 T 细胞很快在 terminal effector 与 memory precursor 命运之间分叉。染色质重塑发生得早于许多稳定 marker，但哪些表观遗传复合体是驱动命运、哪些只是伴随变化，仍不清楚。

本文聚焦：

1. 哪些 chromatin regulators 在体内决定 effector–memory fate？
2. 它们何时介入，如何与 MYC 等转录/代谢程序协同？
3. 短暂操纵这些节点能否在不永久破坏功能的情况下，提高 CAR-T 的 memory fitness？

## 2. 方法框架：体内表观遗传 CRISPR 筛选与多组学机制解析

### 2.1 focused epigenetic library

- 337 个候选表观遗传/染色质调控 genes。
- 每基因 6 条 sgRNA，共 2,022 targeting guides。
- 200 条 non-targeting controls。
- 总 library：2,222 条 guides。

### 2.2 体内 selection

- 使用 Cas9-OT-I naïve CD8 T cells。
- 转导率约 20%，避免一个细胞多 guide。
- 保存约 0.5 million input cells，覆盖超过 200×/guide。
- 向 21 个 Cas9 hosts 每只转入约 0.2 million transduced T cells。
- Listeria monocytogenes–Ova 感染驱动抗原特异分化。
- day 7.5 分选：
  - terminal effector（TE）：KLRG1-high CD127-low；
  - memory precursor（MP）：KLRG1-low CD127-high。
- 另比较 day 36 memory pool 与 day 7.5 acute pool。
- 每个 pooled sample 尽量回收至少 0.5 million cells，维持约 200× 覆盖。

### 2.3 机制层

围绕 Arid1a、Smarcd2、Brg1/cBAF 和 Myc，作者开展：

- bulk RNA-seq；
- ATAC-seq；
- CUT&RUN；
- microarray；
- genetic KO 与 cBAF inhibitor BD98；
- mouse/human CAR-T 功能验证。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

本文是“**体内 fate screen + 多批次 bulk epigenomics**”，没有 scRNA-seq。公共数据包括：

1. 体内 CRISPR screen 的 guide/gene-level selection 结果，主要在 source/supplementary data。
2. RNA-seq：Arid1a/cBAF/Myc 干预下的表达程序。
3. ATAC-seq：效应/记忆分化和遗传/药物干预下的开放染色质。
4. CUT&RUN：ARID1A、BRG1、MYC 等占位。
5. Clariom S microarray：扩展的处理/时间条件。

GSE183619 的 101 samples 不是一个统一实验；必须按 GSM title 把不同 assay、基因型、时间点、处理和重复拆分。

### 3.2 CRISPR screen 的规模和覆盖

| 参数 | 数值 |
|---|---:|
| target genes | 337 |
| guides/gene | 6 |
| targeting guides | 2,022 |
| NTC | 200 |
| total guides | 2,222 |
| donor preparations | 2 |
| host mice | 21 |
| transduction | ~20% |
| input cells | 0.5 million，>200×/guide |
| cells transferred/host | 0.2 million |
| acute fate time | day 7.5 |
| memory time | day 36 |

主要比较是：

- MP vs TE at day 7.5；
- day 36 memory vs day 7.5 acute populations。

hits 中 cBAF components 形成一致模块，尤其 ARID1A、SMARCD2 等 deletion 促进 memory-associated fate。

### 3.3 GSE183619 的 101 个样本

GEO：[GSE183619](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE183619)

根据 GEO 文件清单和 GSM 编号区段，可将主要样本分成：

| GSM 区段/批次 | modality | 样本数 | 主要内容 |
|---|---:|---:|---|
| GSM5563496–GSM5563515 | bulk RNA-seq | 20 | 早期 cBAF/Arid1a/Myc 等表达程序 |
| GSM5563516–GSM5563524 | ATAC-seq | 9 | MYC-high、MYC-low、naïve 等状态 |
| GSM5563525–GSM5563551 | CUT&RUN | 27 | ARID1A、BRG1、MYC、IgG 等占位 |
| GSM5563552–GSM5563563 | ATAC-seq | 12 | Arid1a KO/WT、Myc KO/WT 等 |
| GSM5592997–GSM5593012 | Clariom S microarray | 16 | 扩展状态/处理表达数据 |
| GSM5959116–GSM5959124 | ATAC-seq | 9 | inhibitor、KO、WT 比较 |
| GSM5966479–GSM5966486 | bulk RNA-seq | 8 | WT 与 inhibitor 等比较 |

这些清晰类别合计 101 个 GEO records 的主体；按 modality 汇总为：

- bulk RNA-seq：28；
- ATAC-seq：30（上述三个区段各为 9、12、9）；
- CUT&RUN：27；
- microarray：16。

上述四类合计 `28 + 30 + 27 + 16 = 101`。正式重分析仍应以下载的 SOFT/Series Matrix 逐条生成 manifest，避免仅凭 GSM 编号推断处理条件。

### 3.4 各 modality 的文件类型

#### bulk RNA-seq

- 样本级 processed RSEM count/expected-count 文件，单文件约 620–646 KB。
- raw FASTQ 可从 SRA/BioProject PRJNA761407 下载。
- 适合比较 cBAF genetic deletion、Myc 状态和 BD98 treatment 的表达程序。

#### ATAC-seq

- 多批次 ATAC-seq。
- processed 文件包含 peaks/BED、coverage bigWig 或计数相关文件，具体随 GSM 不同。
- 可用于分析 cBAF/MYC 是否在早期控制 effector-associated regulatory elements。

#### CUT&RUN

- 27 个样本，包含 ARID1A、BRG1、MYC 和 IgG/input 类 controls。
- processed BED/bedGraph/bigWig 用于 occupancy overlap。
- 分析时必须按 antibody 与 genotype 配对 control；不能把不同抗体 tracks 当 biological replicates。

#### microarray

- 16 个 Affymetrix Clariom S Mouse 样本。
- 与 RNA-seq 是不同批次/平台，适合 pathway consistency，而不宜直接拼接 gene-level expression values。

### 3.5 数据文件总量与下载优先级

- `GSE183619_RAW.tar` 约 18 GB，包含大量 BED/bigWig 和 processed outputs。
- 如果只需要差异表达：先下载样本级 RSEM counts 和 metadata。
- 如果要重画机制图：下载 ATAC/CUT&RUN bigWig 进行 locus browser 可视化。
- 如果要重新 call peaks：再从 SRA 下载 raw FASTQ，所需空间远大于 18 GB。

### 3.6 CRISPR screen 数据不等同于 GSE183619 omics

Supplementary Table 1 的可下载工作簿主要记录约 54 行的 sgRNA/编辑效率等验证信息，并不是完整 2,222-guide abundance matrix。完整 screen ranking/count 需要结合：

- Fig. 1 Source Data；
- 论文 supplementary data；
- Methods 中的 library annotation；
- 如需 raw guide reads，进一步核对 SRA run titles 或联系作者。

因此，GSE183619 的强项是机制多组学，不应误写成“GEO 已提供完整 CRISPR screen count matrix”。

### 3.7 下载方式

#### GEO processed data

```text
https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE183619
```

先下载 Series SOFT/metadata，生成清单：

```bash
GSM | assay | genotype | treatment | time | replicate | processed_file | SRA
```

再按研究目标选择文件，避免一次性下载 18 GB 后才发现 modality 混杂。

#### raw FASTQ

从 BioProject [PRJNA761407](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA761407) 或 GEO SRA links 下载：

```bash
prefetch SRR_ACCESSION
fasterq-dump SRR_ACCESSION --split-files --threads 8
```

ATAC、CUT&RUN 和 RNA-seq 的 read structure/处理流程不同，应分项目建立 pipeline。

#### Nature source/supplementary data

Supplementary Table 1 直链模式：

```text
https://media.springernature.com/original/springer-static/esm/art%3A10.1038%2Fs41586-022-04849-0/MediaObjects/41586_2022_4849_MOESM3_ESM.xlsx
```

Source Data 应从 Nature article 的“Source data”链接逐图下载，并保留 `Fig1` 等原文件名。

### 3.8 下载后建议整理

```text
67_Guo_2022/
├── screen/
│   ├── library_2222.tsv
│   ├── guide_counts_or_source_data/
│   └── gene_rankings/
├── rnaseq/
│   ├── early_cBAF_MYC/
│   └── BD98_validation/
├── atac/
│   ├── MYC_states/
│   ├── Arid1a_Myc_KO/
│   └── inhibitor/
├── cutrun/
├── microarray/
└── metadata/GSE183619_manifest.tsv
```

## 4. 主要生物学发现

### 4.1 cBAF 在命运分叉早期促进效应程序

cBAF components 的缺失使细胞减少 terminal effector commitment、提高 memory precursor representation，说明染色质重塑不是后期结果，而是早期命运驱动层。

### 4.2 cBAF 与 MYC 协同

MYC-high 早期状态与 cBAF-dependent chromatin accessibility/occupancy 相连。ARID1A、BRG1 等 cBAF component 与 MYC 共同支持快速生长和效应分化程序。

### 4.3 短暂 cBAF inhibition 可提高细胞治疗适应性

用 BD98 在激活早期短暂处理，可将 mouse/human CAR-T 偏向更有利的 memory-like 状态，并改善后续抗肿瘤表现。短时 treatment 与永久 gene KO 的影响不完全等价。

## 5. 状态—功能—驱动因子的连接

```text
early antigen activation
→ MYC induction + cBAF chromatin remodeling
→ effector regulatory elements open
→ terminal effector fate

ARID1A/SMARCD2 loss or transient cBAF inhibition
→ effector program restrained
→ memory-precursor state retained
→ improved later antitumor fitness
```

该因果链由体内 screen 定位 driver、ATAC/CUT&RUN 定位染色质机制、RNA/microarray 定位表达程序、CAR-T 实验定位功能结局。

## 6. 对细胞治疗状态导航的启示

- 制造早期存在短暂的 chromatin state decision window。
- 通过短期表观遗传抑制可改变未来命运，而无需永久删除广泛作用的复合体。
- 最佳 memory-like 状态不是完全静止；需在保留扩增/效应和避免 terminal differentiation 之间平衡。
- 多组学可用于定义处理开始和停止的窗口。

## 7. 可复用的分析思路

1. screen 中分别建模 MP/TE 和 late/early selection。
2. ATAC 与 CUT&RUN 以共同 peak universe 做 overlap/occupancy enrichment。
3. 用 MYC-high/low 早期状态检验 cBAF 依赖性。
4. RNA 与 microarray 在 pathway/module 层做跨平台一致性。
5. CAR-T 药理处理按时间窗、剂量、washout 做响应面，不把单点结果视为最优。

## 8. 推荐图版

- 2,222-guide in vivo screen workflow。
- MP vs TE screen ranking，突出 cBAF components。
- cBAF/MYC ATAC + CUT&RUN locus/aggregate plots。
- Arid1a/BD98 对 effector-memory fate 的影响。
- 人 CAR-T short-pulse cBAF inhibition 的功能图。

## 9. 创新价值

- 在生理感染环境中筛选 CD8 fate regulators。
- 将表观遗传 complex-level hits 与 MYC 机制连接。
- 101-sample 多 modality 公共资源支持深度重分析。
- 提出短暂 chromatin modulation 作为细胞产品制造策略。

## 10. 局限性

1. screen 是 337-gene focused library，不是全基因组。
2. 小鼠 OT-I/Lm-Ova 急性感染与人肿瘤慢性抗原环境不同。
3. GSE183619 是多个批次/modality 的合集，不是单一 atlas。
4. 未使用 scRNA-seq，不能解析细胞级 fate trajectory 和稀有亚群。
5. 完整 guide count/ranking 不集中在 GEO，需要 source/supplementary data 拼合。
6. cBAF 广泛参与细胞稳态，抑制剂选择性、剂量与时间窗决定安全性。

## 11. 对本章节的作用

| 综述模块 | 本文贡献 |
|---|---|
| Chart molecular landscape | RNA/ATAC/CUT&RUN/microarray 构建早期 CD8 fate 调控图 |
| Quantify phenotype/function | KLRG1/CD127 定义 TE/MP，体内 abundance 与 CAR-T efficacy |
| Perturb/manipulate states | in vivo CRISPR KO + transient cBAF inhibitor |
| Link transitions with drivers | cBAF–MYC–chromatin accessibility–effector fate |
| Optimize navigation | 定义激活早期短暂干预窗口 |

## 12. 可直接用于综述的观点

> Early chromatin remodeling is a causal control layer of CD8 T-cell fate, not merely a molecular record of completed differentiation.

> In vivo perturbation screening can identify regulators whose effects only emerge under physiological clonal expansion and fate competition.

> Transient inhibition of a chromatin-remodeling complex offers a time-gated strategy to preserve memory potential during cell manufacturing.

## 13. 避免误读

- **library 是 2,222 guides，不是 2,222 genes**：337 genes × 6 + 200 NTC。
- **本文没有 scRNA-seq**，GSE183619 是 bulk RNA/ATAC/CUT&RUN/microarray。
- **101 samples 来自多个批次和 assay**，不能直接合并为一个表达矩阵。
- **Supplementary Table 1 不是完整 screen count matrix**。
- **ARID1A KO 与短暂 BD98 处理不等价**。
- **memory precursor 富集不自动保证长期临床 CAR-T 优势**，仍受肿瘤和受体背景影响。

## 数据与资源链接

- 论文：[Nature](https://www.nature.com/articles/s41586-022-04849-0)
- 全文：[PMC9623036](https://pmc.ncbi.nlm.nih.gov/articles/PMC9623036/)
- GEO：[GSE183619](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE183619)
- BioProject：[PRJNA761407](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA761407)
