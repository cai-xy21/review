# 《In vivo CRISPR screening in CD8 T cells with AAV–Sleeping Beauty hybrid vectors identifies membrane targets for improving immunotherapy for glioblastoma》精读

## 论文信息

- **题目**：In vivo CRISPR screening in CD8 T cells with AAV–Sleeping Beauty hybrid vectors identifies membrane targets for improving immunotherapy for glioblastoma
- **作者**：Ye et al.
- **期刊与年份**：Nature Biotechnology，2019
- **DOI**：[10.1038/s41587-019-0246-4](https://doi.org/10.1038/s41587-019-0246-4)
- **PMID / PMC**：[31548728](https://pubmed.ncbi.nlm.nih.gov/31548728/) / [PMC6834896](https://pmc.ncbi.nlm.nih.gov/articles/PMC6834896/)
- **核心数据入口**：[BioProject PRJNA553676](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA553676)
- **研究类型**：AAV–Sleeping Beauty hybrid vector 原代 CD8 T 细胞体内 pooled CRISPR screen + GBM 模型 + bulk RNA-seq、TCR-seq、scRNA-seq 和 editing validation

## 一句话结论

本文用 AAV–Sleeping Beauty（SB100x）hybrid vector 解决原代 T 细胞体内 pooled screening 的稳定递送难题，以 7,628-element cell-surface library 在胶质母细胞瘤模型中筛选，鉴定 **PDIA3、MGAT5 等膜相关负调控节点**；PDIA3 缺失提高 CD8 T 细胞肿瘤浸润与细胞毒功能，并通过 bulk/scRNA 等数据获得机制支持。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 递送系统 | AAV donor 搭载 Sleeping Beauty transposon，SB100x 实现稳定整合 |
| targeted library | 1,657 个 cell-surface/membrane-related genes × 4 sgRNAs = 6,628 targeting guides；另 1,000 NTC；总计 7,628 |
| 主筛起始细胞 | 每次约 2×10^7 naïve Cas9 CD8 T cells，约 1×10^11 AAV particles |
| functional MOI | 约 0.65；约 48% 细胞获得 functional sgRNA |
| 体内 screens | medium polyclonal 7 mice；short OT-I 3 mice；long OT-I 9 mice；总计 19 只用于三类 screen |
| 统计结果 | 33 条 enriched sgRNAs，FDR 0.2%；RIGER gene ranking |
| 验证 hits | Pdia3、Mgat5 等 |
| scRNA-seq | sgPdia3 与 vector/control 两个样本；每样本上机目标 10,000 cells，最终 QC cells 未在数据页摘要明确给出 |
| 公开测序 | PRJNA553676，共 30 个 SRA runs，混合 amplicon/editing、splinkerette、bulk RNA、TCR-seq 与 scRNA-seq |
| screen abundance | 定量 guide enrichment 主要在 Supplementary Tables S3–S5/source data；SRA run titles 未显示一个可直接认定为完整 pooled-screen readout 的独立集合 |
| 关键限制 | library 是 membrane-focused 而非 genome-wide；论文 Results 有一处写 1,658 genes，Methods/library arithmetic 支持 1,657 targeted genes |

## 1. 研究要解决的问题

在体外筛选容易维持 library coverage，但 T 细胞进入体内后要经历扩增、迁移、肿瘤浸润和长期抗原压力。若直接把慢病毒 pooled library 用于原代 T 细胞，递送效率、稳定表达和体内覆盖常成为瓶颈。

本文有两个目标：

1. 建立可在原代 CD8 T 细胞中稳定、规模化递送 sgRNA library 的 AAV–SB hybrid system；
2. 在 GBM 环境中筛查删除哪些膜蛋白能提高 T 细胞体内 persistence/infiltration 和抗肿瘤功能。

## 2. 方法框架：AAV–SB hybrid vector 与三类体内筛选

### 2.1 递送设计

- AAV 高效进入原代 T 细胞。
- sgRNA cassette 位于 Sleeping Beauty transposon 两端反向重复序列之间。
- SB100x transposase 将 cassette 从 episomal AAV donor 转入基因组，提供稳定表达。
- 通过 splinkerette PCR 等方法检查 insertion-site distribution。

### 2.2 Surf library

- 根据细胞表面/膜相关基因集合选择 1,657 个 targets。
- 每基因 4 条 sgRNA，共 6,628 条 targeting guides。
- 另含 1,000 条 NTC。
- 总计 7,628 条 elements。
- library cloning coverage 至少约 60×。

论文 Results 叙述中有一处写 1,658 genes，但 `6,628 ÷ 4 = 1,657`，Methods 也写 1,657；因此本报告采用 1,657 targeted genes。

### 2.3 三类体内 screen

- **medium-duration polyclonal screen**：7 只小鼠，约 day 26 endpoint。
- **short OT-I screen**：3 只小鼠，约 day 20 endpoint。
- **long OT-I screen**：9 只小鼠，约 day 92 endpoint。

总计 19 只动物构成三类体内 selection。不同 screen 的 T-cell receptor repertoire、时间长度和选择压力不同，结果需要看跨 screen 一致性，而非简单合并 counts。

### 2.4 命中验证

作者选择 Pdia3、Mgat5 等候选进行单独 sgRNA 验证，测量 GBM 中肿瘤浸润、细胞毒性、转录状态和治疗效果，并对 Pdia3 条件开展 bulk/scRNA 分析。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

本文的数据包括：

1. **vector engineering**：AAV titer、functional MOI、转座整合与 insertion-site 数据。
2. **three in vivo pooled screens**：input/TIL 或不同时间条件下 sgRNA enrichment。
3. **hit validation**：单基因编辑效率和功能。
4. **bulk RNA-seq**：Pdia3-edited 与 control CD8 T cells。
5. **TCR-seq**：受体 repertoire 前后变化。
6. **scRNA-seq**：Pdia3-edited 与 control 的单细胞状态。

PRJNA553676 把多种 library strategy 放在同一 BioProject 下。下载时必须按 run title/experiment strategy 分类，不能把 30 个 runs 全部交给同一个 RNA-seq pipeline。

### 3.2 library 与递送规模

| 参数 | 数值 |
|---|---:|
| targeted genes | 1,657 |
| sgRNAs/gene | 4 |
| targeting sgRNAs | 6,628 |
| NTC | 1,000 |
| total library elements | 7,628 |
| naïve Cas9 T cells/screen transduction | 2×10^7 |
| AAV particles | 1×10^11 |
| functional MOI | ~0.65 |
| functional sgRNA-positive cells | ~48% |
| library cloning representation | ≥60× |

AAV preparation titer 约 `1.4 × 10^12 vg/ml`。functional MOI 和 vector genomes 不是同一个概念：前者由功能性 sgRNA/细胞效应估计，后者反映物理颗粒数量。

### 3.3 三个 screen 的动物与时间规模

| screen | T-cell context | mice | endpoint |
|---|---|---:|---:|
| medium | polyclonal Cas9 CD8 | 7 | ~day 26 |
| short | OT-I Cas9 CD8 | 3 | ~day 20 |
| long | OT-I Cas9 CD8 | 9 | ~day 92 |
| 总计 |  | 19 |  |

GBM 脑组织中回收的 TIL 量级较低：vector/control 平均约 `5 × 10^4`，Surf library 条件约 `1.25 × 10^5`。低细胞数会造成 guide bottleneck，因此作者以跨 screen 富集和 RIGER ranking 提高稳健性。

### 3.4 筛选结果数据

- 严格阈值下获得 33 条 enriched sgRNAs，FDR 0.2%。
- 通过 RIGER 将多条 guide 聚合到 gene-level ranking。
- 主要 quantitative tables 位于 Supplementary Tables S3–S5/source statistics，例如每次 screen 的 guide enrichment 和 cross-screen ranking。
- source statistics 文件在论文中以类似 `original data_v8.xlsx` 的形式提供。

当前可核验的 SRA run titles 主要指向验证性 sequencing modalities，未发现一个可直接、无歧义标记为全部 Surf screen readout 的独立 FASTQ 集合。因此如需复现 screen ranking，优先使用 Supplementary Tables S3–S5；如需从 raw reads 重新计数，应逐 run 核对 Methods 或联系作者。

### 3.5 PRJNA553676：30 个 runs 如何组成

BioProject：[PRJNA553676](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA553676)

依据 ENA/SRA run titles，可归纳为：

| modality | runs | 主要内容 |
|---|---:|---|
| targeted amplicon/editing validation | 18 | sgRNA editing/indel 等验证 |
| splinkerette sequencing | 2 | Sleeping Beauty insertion-site analysis |
| bulk RNA-seq | 6 | 3 个 vector/control 与 3 个 sgPdia3 |
| TCR-seq | 2 | treatment 前后/对应条件 repertoire |
| scRNA-seq | 2 | vector/control 与 sgPdia3 |
| 总计 | 30 | 多种 library strategy 混合 |

run 数是按标题分类的文件级统计；正式下载时应导出 `SraRunTable.csv`，保留 `library_strategy`、`library_layout`、`sample_title` 和 `fastq_ftp` 字段。

### 3.6 bulk RNA-seq

- 样本数：6。
- 分组：vector/control 3，sgPdia3 3。
- 物种：mouse CD8 T cells。
- 用途：解释 Pdia3 deletion 后的效应/代谢/激活程序。
- raw FASTQ 位于 PRJNA553676。

3 vs 3 的统计功效有限，应以 pathway/module 和独立功能实验交叉验证，不宜仅凭单个低表达基因下结论。

### 3.7 scRNA-seq

- 样本数：2：vector/control 与 sgPdia3。
- 10x 每个样本上机目标：10,000 cells。
- 最终通过 QC 的细胞数：论文摘要和 BioProject 页面未给出统一明确数字，应从 Cell Ranger filtered barcodes/论文 Methods 或原始 matrix 实际统计。
- 用途：比较 Pdia3 editing 后的细胞状态组成和功能程序。

由于每个条件只有 1 个 scRNA library，细胞不是生物学重复。差异分析更适合描述性 state mapping，不能用大量细胞替代独立动物/制备批次。

### 3.8 下载方式

#### 导出 run manifest

从 BioProject PRJNA553676 进入 SRA Run Selector，下载 `SraRunTable.csv`。先按 `LibraryStrategy` 和 `Sample Name` 分类：

```python
import pandas as pd

run = pd.read_csv("SraRunTable.csv")
print(run.groupby(["LibraryStrategy", "Sample Name"]).size())
```

#### FASTQ

```bash
prefetch SRR_ACCESSION
fasterq-dump SRR_ACCESSION --split-files --threads 8
gzip SRR_ACCESSION_*.fastq
```

amplicon、TCR、scRNA 和 bulk RNA 必须使用不同 pipeline。对于 10x scRNA，还需要从 SRA metadata/Methods 确定 read structure 和 chemistry。

#### Supplementary screening tables

从 [PMC6834896](https://pmc.ncbi.nlm.nih.gov/articles/PMC6834896/) 的 Supplementary Materials 下载 Supplementary Tables S1–S5 和 source data。重点寻找：

- library gene/guide annotation；
- medium/short/long screen guide abundance；
- RIGER gene ranking；
- validation sgRNA/primer information。

### 3.9 下载后建议整理

```text
69_Ye_2019/
├── surf_screen/
│   ├── library_7628.tsv
│   ├── medium_screen/
│   ├── short_OTI/
│   ├── long_OTI/
│   └── RIGER_rank.tsv
├── vector_validation/
│   ├── amplicon/
│   └── splinkerette/
├── pdia3_validation/
│   ├── bulk_rna/
│   └── scRNA/
├── tcrseq/
└── metadata/PRJNA553676_manifest.tsv
```

## 4. 主要生物学发现

### 4.1 AAV–SB 支持原代 T 细胞体内 pooled screening

AAV 负责高效递送，SB100x 负责稳定整合，使 guide 在长期体内 selection 中得以保留，是本文的重要方法学贡献。

### 4.2 膜相关负调控因子可提高 GBM T-cell fitness

Pdia3、Mgat5 等 hit 表明细胞表面/膜蛋白加工相关节点影响 T 细胞在 GBM 中的浸润、存活和效应。

### 4.3 Pdia3 deletion 提升细胞毒程序

单独验证显示 Pdia3 editing 增强抗肿瘤功能，bulk/scRNA 为状态变化提供支持。但由于 scRNA 每组仅一个 library，机制结论主要依赖多种独立功能实验汇合。

## 5. 状态—功能—驱动因子的连接

```text
membrane-gene CRISPR perturbation
→ altered signaling/trafficking/protein-processing state
→ in vivo expansion and GBM infiltration selection
→ cytotoxic transcriptional state
→ tumor control
```

与体外筛选相比，本文直接让肿瘤迁移和长期抗原环境参与 hit 定义，因此找到的是“能在体内复杂选择压力下存活/富集”的驱动因子。

## 6. 对细胞治疗状态导航的启示

- 递送技术决定可搜索的 perturbation space。
- 体内筛选能捕获 migration、persistence 和微环境适应，不能由短时体外 killing 替代。
- focused surfaceome library 对寻找可药物化/抗体化靶点尤其有价值。
- 长短期 screen 应联合，区分急性效应与持久性优势。

## 7. 可复用的分析思路

1. 每个 screen 独立计算 guide enrichment，再做 rank aggregation。
2. 评估每只动物的 library bottleneck 和 NTC variance。
3. 将 4 guides/gene 用 RIGER/MAGeCK 等聚合，不依赖单 guide。
4. 验证时同时测 editing efficiency、TIL recovery、cytotoxicity 和 tumor control。
5. scRNA 只做状态描述，动物/制备重复层的统计用独立功能实验支持。

## 8. 推荐图版

- AAV–SB hybrid vector 和 stable integration schematic。
- 7,628-element Surf library 与三类 in vivo screen 设计。
- cross-screen hit/rank plot，突出 Pdia3/Mgat5。
- Pdia3 validation 与 GBM tumor-control 图。
- Pdia3 bulk/scRNA state change 图。

## 9. 创新价值

- 解决原代 T 细胞体内 pooled CRISPR library 的稳定递送问题。
- 通过短、中、长期三类体内 selection 识别稳健 hits。
- 聚焦 surfaceome，提高可转化靶点的比例。
- 将体内遗传筛选与单细胞状态验证连接。

## 10. 局限性

1. library 仅覆盖约 1,657 个膜相关 genes，不是 genome-wide。
2. 体内回收细胞数低，guide bottleneck 明显。
3. 筛选发生在小鼠 GBM/OT-I 等模型，向人 CAR-T 外推需验证。
4. SRA 中多种 assay 混合，screen raw readout 的公共映射不够直接。
5. scRNA 每条件只有一个样本，不能作为独立重复统计。
6. transposon insertion 虽近随机，仍需长期 insertional safety 评估。

## 11. 对本章节的作用

| 综述模块 | 本文贡献 |
|---|---|
| Chart molecular landscape | Pdia3 条件的 bulk/scRNA 状态描述 |
| Quantify phenotype/function | guide enrichment、TIL recovery、cytotoxicity、tumor control |
| Perturb/manipulate states | AAV–SB stable in vivo CRISPR delivery |
| Link transitions with drivers | surface gene loss → in vivo fitness/cytotoxic state |
| Optimize navigation | 比较短/中/长期体内选择环境 |

## 12. 可直接用于综述的观点

> In vivo T-cell screening requires a delivery system that preserves perturbation identity through expansion, trafficking and prolonged tumor selection.

> Surfaceome-focused libraries trade genome-wide breadth for a higher density of pharmacologically and therapeutically actionable targets.

> Short- and long-duration in vivo screens reveal different fitness landscapes and should be integrated rather than treated as interchangeable replicates.

## 13. 避免误读

- **采用 1,657 个 targeted genes**：6,628 targeting guides / 4；论文 Results 的 1,658 为口径不一致。
- **总 library 是 7,628 elements**，包括 1,000 NTC。
- **33 是显著 enriched sgRNAs，不是 33 个独立 genes。**
- **PRJNA553676 的 30 个 runs 混合多种 assay**，不能统一当作 RNA-seq。
- **scRNA 每样本目标 10,000 cells 不是最终 QC cell count。**
- **完整 screen abundance 主要在 supplementary/source data**，不能假设所有 raw guide reads 已明确存入 SRA。

## 数据与资源链接

- 论文：[Nature Biotechnology](https://www.nature.com/articles/s41587-019-0246-4)
- 全文：[PMC6834896](https://pmc.ncbi.nlm.nih.gov/articles/PMC6834896/)
- BioProject：[PRJNA553676](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA553676)
