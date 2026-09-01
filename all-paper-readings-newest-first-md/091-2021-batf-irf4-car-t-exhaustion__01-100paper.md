# 《BATF and IRF4 cooperate to counter exhaustion in tumor-infiltrating CAR T cells》精读

## 论文信息

- **题目**：BATF and IRF4 cooperate to counter exhaustion in tumor-infiltrating CAR T cells
- **作者**：Seo et al.
- **期刊与年份**：Nature Immunology，2021
- **DOI**：[10.1038/s41590-021-00964-8](https://doi.org/10.1038/s41590-021-00964-8)
- **PMID / PMC**：[34282330](https://pubmed.ncbi.nlm.nih.gov/34282330/) / [PMC8319109](https://pmc.ncbi.nlm.nih.gov/articles/PMC8319109/)
- **核心数据入口**：[GEO GSE154747](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE154747)
- **外部比较数据**：[GEO GSE88987](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE88987)，为既往研究数据，不是本文新生成
- **研究类型**：CAR-T transcription-factor overexpression/KO validation + bulk RNA-seq、ATAC-seq、ChIP-seq + 小鼠实体瘤和人 CAR-T 验证

## 一句话结论

本文发现提高 **BATF** 表达可与 **IRF4** 协同重塑染色质和转录程序，使肿瘤浸润 CAR-T 在维持效应功能的同时降低 TOX/抑制性受体相关耗竭状态，并显著改善实体瘤控制；BATF–IRF4 相互作用而非 BATF 单纯高表达是这一状态转换的核心。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 干预形式 | retroviral BATF overexpression；BATF-HKE interaction mutant；Batf knockout/IRF4 依赖验证 |
| 筛选性质 | 小型 transcription-factor candidate screen/验证，不是 genome-wide CRISPR screen |
| 小鼠模型 | B16F0-hCD19、MC38-hCD19 等实体瘤；CD19 CAR-T |
| 人细胞验证 | human CD19 CAR-T |
| 公共 SuperSeries | GSE154747，共 64 个小鼠样本 |
| modalities | ATAC-seq、ChIP-seq、bulk RNA-seq |
| 64 样本组成 | 初始 8 ATAC + 12 ChIP + 8 RNA；restimulation 18 RNA + 18 ChIP |
| raw/processed | GSE154747 RAW tar 约 20.1 GB，含大量 bigWig；raw reads 为 PRJNA647363 |
| 关键补充表 | 体内 RNA 11,512 genes；体外 ATAC DAR 31,390 regions；体内 ATAC DAR 35,921；体外 RNA DGE 265；gene-near BATF ChIP peaks 125 |
| 外部数据 GSE88987 | 44 个样本：36 ATAC + 8 RNA，来自 2016 年 Scott-Browne 等研究 |
| 关键限制 | 本文无 scRNA-seq；bulk 组学的状态变化可能同时包含细胞内重编程与群体组成选择 |

## 1. 研究要解决的问题

实体瘤中的 CAR-T 很快进入功能受损的 exhaustion-like state。提高 AP-1 family activity 是潜在策略，但 AP-1 因子既可能促进效应程序，也可能加速耗竭；单独增加某个因子是否有益取决于其 partner、剂量和染色质环境。

本文要回答：

1. 哪个转录因子能让肿瘤浸润 CAR-T 保持功能？
2. BATF 是否通过 IRF4 建立一种区别于典型终末耗竭的状态？
3. 这种转录状态是否有对应的染色质可及性和 occupancy 变化？
4. 结果能否从小鼠模型延伸到人 CAR-T？

## 2. 方法框架：因子过表达、互作突变与多组学

### 2.1 候选因子与功能筛选

作者在 JUN、MAFF、BATF 等候选转录因子中进行初步比较，发现 BATF 对 CAR-T tumor control 特别突出。之后围绕 BATF 展开系统验证，而不是继续扩展成 genome-scale screen。

### 2.2 因果验证

- BATF overexpression vs pMIG vector control。
- BATF-HKE mutant：削弱 BATF 与 IRF4 的功能性协同/互作。
- Batf genetic deficiency/相关验证。
- 检测 IRF4、TOX、抑制性受体、效应因子、细胞因子和体内扩增。

### 2.3 组学设计

- ATAC-seq：比较 BATF 与 control 的染色质可及性。
- ChIP-seq：BATF 与 IRF4 occupancy，以及 restimulation/突变体条件。
- bulk RNA-seq：体外和肿瘤浸润状态，以及短时 restimulation response。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

GSE154747 是一个多组学 SuperSeries，描述：

```text
BATF program × environment (in vitro vs tumor) × assay (RNA/ATAC/ChIP)
```

其核心是群体水平的表达、开放染色质和转录因子结合图谱。它不是单细胞 atlas，也没有 live-cell tracking。BATF、HKE、pMIG 与 restimulation 的样本属于不同批次/问题，不能全部放入一个简单 treatment contrast。

### 3.2 GSE154747 的 64 个样本如何组成

总入口：[GSE154747](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE154747)

#### 初始 BATF vs pMIG 实验：28 个样本

| assay | 设计 | 样本数 |
|---|---|---:|
| ATAC-seq | BATF/pMIG × in vitro/TIL × 2 replicates | 8 |
| ChIP-seq | BATF/pMIG × anti-BATF/anti-IRF4/input × 2 replicates | 12 |
| bulk RNA-seq | BATF/pMIG × in vitro/TIL × 2 replicates | 8 |
| 小计 |  | 28 |

#### restimulation/HKE 实验：36 个样本

| assay | 设计 | 样本数 |
|---|---|---:|
| bulk RNA-seq | BATF/HKE/pMIG × 0 h/6 h × 3 replicates | 18 |
| ChIP-seq | BATF/HKE/pMIG × anti-IRF4/anti-BATF/input × 2 replicates | 18 |
| 小计 |  | 36 |

总计：`28 + 36 = 64`。

GEO 的子系列包括 [GSE154742](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE154742)（ATAC-seq）、[GSE154743](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE154743)（ChIP-seq）和 [GSE154745](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE154745)（RNA-seq）。

### 3.3 ATAC-seq 数据规模

- 初始 ATAC：8 个样本。
- BATF vs pMIG，in vitro vs tumor-infiltrating CAR-T，各 2 replicates。
- 论文报告体外约 32,035 个 accessible regions。
- BATF overexpression 相对 control 增加可及性的区域约 640 个，而显著降低可及性的区域约 8 个。
- Supplementary Table 中：
  - in vitro differential accessible regions：约 31,390 行；
  - in vivo/TIL differential accessible regions：约 35,921 行。

“32,035 accessible regions”与“31,390/35,921 table rows”代表的 universe/比较口径不同，不应直接互相替换。

### 3.4 ChIP-seq 数据规模

- 初始 BATF/pMIG 条件：12 个样本。
- antibody/input：anti-BATF、anti-IRF4、input。
- restimulation/HKE：18 个样本，覆盖 BATF、HKE、pMIG 以及 BATF/IRF4/input readouts。
- 合计 ChIP-related samples：30。
- Supplementary Table 提供约 125 个 gene-near BATF ChIP peaks 的聚焦注释表。

ChIP 数据用于证明 BATF 与 IRF4 共同占位，并用 HKE mutant 检验互作依赖。input 是背景对照，不是生物学条件重复。

### 3.5 RNA-seq 数据规模

- 初始 BATF/pMIG × in vitro/TIL：8 个样本。
- BATF/HKE/pMIG × 0 h/6 h × 3 replicates：18 个样本。
- 合计 bulk RNA-seq：26 个样本。
- Supplementary Table：
  - 体内 RNA expression table 约 11,512 genes × 9 columns；
  - 体外 differential gene expression 聚焦结果约 265 rows × 9 columns。

0 h/6 h restimulation 数据可以分析 BATF 对动态转录响应的影响；体外/TIL 数据则回答环境依赖。两者不应无批次校正直接合并。

### 3.6 公共文件与体量

- `GSE154747_RAW.tar` 约 20.1 GB。
- GEO 文件清单包含大量 `.bigWig` processed tracks；主系列页面可见约 27 个 bigWig entries，其他 processed outputs 分布在子系列。
- raw reads：BioProject [PRJNA647363](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA647363)。
- 对只想重画 heatmap 的用户，优先下载 Supplementary Tables 和 RNA expression matrices；对 locus-level chromatin 图再下载 bigWig；重新 peak calling 才需要 SRA FASTQ。

### 3.7 Supplementary Tables 的具体组成

一个主要 supplementary workbook（约 3.44 MB）包含：

| sheet | 行列规模 | 内容 |
|---|---:|---|
| Supplementary Table 1 | 11,512 × 9 | in vivo/TIL RNA expression |
| Supplementary Table 2 | 31,390 × 18 | in vitro ATAC differential accessibility |
| Supplementary Table 3 | 35,921 × 18 | in vivo ATAC differential accessibility |
| Supplementary Table 4 | 265 × 9 | in vitro RNA differential expression |
| Supplementary Table 5 | 125 × 19 | gene-near BATF ChIP peaks |

下载直链模式：

```text
https://media.springernature.com/original/springer-static/esm/art%3A10.1038%2Fs41590-021-00964-8/MediaObjects/41590_2021_964_MOESM3_ESM.xlsx
```

### 3.8 GSE88987 是什么

[GSE88987](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE88987) 是 Scott-Browne 等 2016 年研究产生的既往 CD8 T-cell chronic/acute stimulation 数据，本文将其用于比较。

- 总样本：44。
- ATAC-seq：36。
- RNA-seq：8。
- processed matrices 包括约 136.8 KB 的 average TPM 表和约 2.4 MB 的 normalized count 表。
- raw data：PRJNA349462。

报告本文数据时必须把 GSE154747 标为“generated in this study”，把 GSE88987 标为“external comparator”。

### 3.9 下载方式

#### GEO processed

```text
https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE154747
```

按子系列下载 ATAC、ChIP、RNA，先保存 series matrix/SOFT metadata。

#### raw FASTQ

```bash
prefetch SRR_ACCESSION
fasterq-dump SRR_ACCESSION --split-files --threads 8
```

Run list 从 PRJNA647363 导出。务必在 manifest 中标出 assay 和 input/antibody。

#### external comparator

仅在需要复现 chronic/acute comparison 时下载 GSE88987；不要与 GSE154747 放在同一 `generated_data` 文件夹。

### 3.10 下载后建议整理

```text
68_Seo_2021/
├── generated_GSE154747/
│   ├── atac_GSE154742/
│   ├── chip_GSE154743/
│   ├── rna_GSE154745/
│   └── manifest.tsv
├── supplementary_tables/
├── external_comparator/
│   └── GSE88987/
├── functional_validation/
└── constructs/
    ├── BATF/
    └── BATF_HKE/
```

## 4. 主要生物学发现

### 4.1 BATF overexpression 改善肿瘤浸润 CAR-T 功能

BATF 工程细胞在实体瘤中表现出更好的扩增、效应和肿瘤控制，同时耗竭相关抑制性受体/TOX 程序降低。

### 4.2 BATF 的作用依赖 IRF4 协同

HKE mutant 和 occupancy 数据表明，BATF 需要与 IRF4 形成合适的转录调控复合体。BATF 不是简单以“越多越好”的方式工作，其 partner availability 决定输出。

### 4.3 染色质状态被主动重塑

ATAC/ChIP 表明 BATF 增加一组调控区域的开放性，并与 IRF4 共同占位。这些变化连接到 effector function、survival 和 exhaustion counter-program。

## 5. 状态—功能—驱动因子的连接

```text
BATF overexpression
→ BATF–IRF4 cooperative occupancy
→ selected chromatin regions become accessible
→ effector/survival program maintained, TOX/exhaustion program reduced
→ tumor-infiltrating CAR-T expansion and control improved
```

HKE mutant 是关键因果对照：如果只有 BATF abundance 而没有正确 IRF4 cooperation，状态导航效果显著减弱。

## 6. 对细胞治疗状态导航的启示

- transcription factor engineering 应考虑 partner stoichiometry 和 motif context。
- “抗耗竭”不是简单回到 naïve 状态，而可能是维持高效应同时阻断终末耗竭。
- RNA、ATAC 与 ChIP 的联合可把 marker 变化推进到调控机制。
- 未来可用可调表达回路控制 BATF 剂量和时间，减少持续过表达风险。

## 7. 可复用的分析思路

1. RNA：分别分析 in vitro/TIL 与 0 h/6 h dynamics。
2. ATAC：建立 consensus peak set，计算 BATF-dependent DAR。
3. ChIP：BATF/IRF4 co-occupancy 与 nearby gene expression 联合。
4. 用 HKE mutant 作为机制 perturbation，而非只做相关性分析。
5. 对 GSE88987 做独立 batch-aware comparator，不直接合并 raw counts。

## 8. 推荐图版

- BATF candidate screen/体内 tumor-control 图。
- BATF vs pMIG TIL phenotype 与 exhaustion markers。
- ATAC differential accessibility 图。
- BATF/IRF4 ChIP co-occupancy 与 HKE mutant 图。
- 人 CAR-T 验证图。

## 9. 创新价值

- 发现 BATF–IRF4 可建立高功能、低终末耗竭的 CAR-T 状态。
- 用 interaction mutant 将 TF abundance 与 cooperative mechanism 区分。
- 64-sample RNA/ATAC/ChIP 公共资源完整支持调控机制。
- 从小鼠实体瘤延伸到人 CAR-T。

## 10. 局限性

1. 前置候选 screen 规模小，不是无偏 genome-wide discovery。
2. 全部组学为 bulk，无法解析稀有亚群和单细胞状态轨迹。
3. BATF 持续过表达的剂量、安全性和长期命运仍需评估。
4. 模型使用人工 hCD19 实体瘤，抗原和微环境与临床实体瘤不同。
5. GSE154747 包含多个 assay/批次，不能直接统一标准化。
6. GSE88987 是外部数据，不能算入本文新生成的 64 个样本。

## 11. 对本章节的作用

| 综述模块 | 本文贡献 |
|---|---|
| Chart molecular landscape | RNA/ATAC/ChIP 描绘 BATF–IRF4 抗耗竭调控网络 |
| Quantify phenotype/function | 抑制性受体、TOX、细胞因子、扩增、肿瘤控制 |
| Perturb/manipulate states | TF overexpression、interaction mutant、KO |
| Link transitions with drivers | BATF–IRF4 occupancy → accessibility → function |
| Optimize navigation | 指向 TF 剂量、partner 和表达时序优化 |

## 12. 可直接用于综述的观点

> Countering exhaustion does not necessarily require suppressing effector differentiation; BATF–IRF4 cooperation can sustain function while redirecting the exhaustion-associated regulatory landscape.

> Transcription-factor engineering must account for partner availability and cooperative occupancy, not only expression level.

> Combining RNA-seq, ATAC-seq and ChIP-seq connects an engineered factor to state, regulatory mechanism and function, even though single-cell resolution remains absent.

## 13. 避免误读

- **本文没有 scRNA-seq。**
- **GSE154747 是 64 个 bulk multi-omic samples**，不是 64 个单细胞样本或 64 个生物学重复。
- **GSE88987 是外部既往数据**，不是本文新数据。
- **BATF-HKE 的意义是破坏协同机制**，不能当作简单低表达 BATF 对照。
- **BATF overexpression 的“抗耗竭”不表示细胞回到 naïve 状态。**
- **20.1 GB GEO archive 主要是 bigWig 等轨道文件**，并非单一 count matrix。

## 数据与资源链接

- 论文：[Nature Immunology](https://www.nature.com/articles/s41590-021-00964-8)
- 全文：[PMC8319109](https://pmc.ncbi.nlm.nih.gov/articles/PMC8319109/)
- SuperSeries：[GSE154747](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE154747)
- ATAC：[GSE154742](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE154742)
- ChIP：[GSE154743](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE154743)
- RNA：[GSE154745](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE154745)
- Raw BioProject：[PRJNA647363](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA647363)
- External comparator：[GSE88987](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE88987)
