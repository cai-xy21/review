# 《Pooled Knockin Targeting for Genome Engineering of Cellular Immunotherapies》精读

## 论文信息

- **题目**：Pooled Knockin Targeting for Genome Engineering of Cellular Immunotherapies
- **作者**：Roth et al.
- **期刊与年份**：Cell，2020
- **DOI**：[10.1016/j.cell.2020.03.039](https://doi.org/10.1016/j.cell.2020.03.039)
- **PMID / PMC**：[32302591](https://pubmed.ncbi.nlm.nih.gov/32302591/) / [PMC7219528](https://pmc.ncbi.nlm.nih.gov/articles/PMC7219528/)
- **核心数据入口**：[GEO GSE143417](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE143417)
- **研究类型**：原代人 T 细胞 pooled knock-in（PoKI）screen + targeted barcode sequencing + 10x scRNA-seq + 肿瘤异种移植体内筛选

## 一句话结论

本文将 36 种 kb 级功能构建体以 pooled non-viral knock-in 方式定点整合到人 T 细胞 TRAC 位点，并用构建体 barcode 与单细胞转录组联配，在体外刺激/TGF-β 和体内肿瘤环境中筛选工程模块，鉴定出 **TGFβR2–4-1BB 等可提高适应性和抗肿瘤功能的组合构建体**，建立了“安装功能程序并直接读取状态后果”的 PoKI 平台。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| pooled library | 36 个 barcoded knock-in constructs，约 2–3 kb |
| 定点位点 | TRAC；同时编码 NY-ESO-1 特异 TCR 与候选功能模块 |
| 人供者 | pooled screens 总体涉及 4 个独立 donors；单细胞子实验使用其中的特定 2 donors |
| 体外条件 | IL-2 baseline、anti-CD3/CD28、excess stimulation、CD3 only、TGF-β 等 |
| 体外 PoKI-seq | 两 donors、±TGF-β、各 3 replicates；超过 40,000 个 cells；58% 细胞可分配 1–2 个 construct barcodes |
| 体内模型 | NSG 小鼠 A375 NY-ESO-1 xenograft；输入约 10 million T cells，约 1 million KI-positive |
| 体内回收 | 每鼠约 10,000–20,000 个 TIL（实验描述量级） |
| GEO | GSE143417，共 44 个样本，NovaSeq 6000 |
| GEO 组成 | 24 个体外 GEX/barcode 样本 + 4 个 GFP/mCherry validation barcode 样本 + 16 个体内 GEX/barcode 样本 |
| processed data | `GSE143417_RAW.tar` 约 469.4 MB，含 GEX archives 与 targeted barcode TSV |
| raw data | PRJNA600416 / SRP241166 |
| 关键限制 | pooled donor 在同次电转会出现约 10% template switching，且约 25% knock-in-positive 细胞可能为双等位/多构建体事件 |

## 1. 研究要解决的问题

CRISPR knockout screen 可大规模发现“去掉什么”，CRISPRa screen 可发现“提高哪个内源基因”，但细胞治疗工程经常需要安装复杂的 2–3 kb synthetic receptors、dominant-negative receptors 或 transcription factors。传统做法逐个构建、逐个供者测试，难以搜索组合空间。

本文希望建立一种 pooled knock-in targeting 方法，使每个候选构建体：

- 进入同一个定义明确的基因座；
- 与相同 NY-ESO-1 TCR backbone 配对；
- 通过 barcode 在 bulk 或 single-cell 层追踪；
- 在体外刺激、免疫抑制条件和体内肿瘤环境中竞争；
- 直接连接“安装了什么模块”与“进入什么转录状态/功能状态”。

## 2. 方法框架：PoKI 与 PoKI-seq

### 2.1 36-construct pooled knock-in

- 每个 HDR donor 约 2–3 kb。
- 共有 36 个 barcoded constructs。
- 每个构建体包含统一的 NY-ESO-1 TCR 元件和一个候选调控模块/对照。
- 使用 Cas9 RNP + non-viral HDR donor，将 cassette 定点整合到 TRAC。
- 通过 NY-ESO-1 dextramer 等富集成功 knock-in 的细胞。

### 2.2 bulk PoKI screening

在输入和不同选择条件后扩增 barcode，比较各构建体的 abundance。体外条件包括常规与过量 CD3/CD28 stimulation、CD3-only、IL-2 baseline 和 TGF-β suppression 等。

### 2.3 PoKI-seq

同一个 10x cDNA library 分成两类 readout：

- 常规 3′ gene-expression library；
- 针对 TCR/knock-in barcode 的 targeted amplification library。

通过共享的 10x cell barcode，把某个细胞的转录状态与其 knock-in construct 连接。该设计是 pooled knock-in 与单细胞状态图谱的关键桥梁。

### 2.4 体内筛选

- NSG 小鼠建立 A375 NY-ESO-1 肿瘤。
- 输入约 10 million T cells，其中约 1 million 为 knock-in-positive。
- 约 5 d 后回收肿瘤浸润 T 细胞。
- 比较 input 与 TIL 中 construct barcode abundance，并对部分样本进行 paired GEX/barcode sequencing。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

本文的数据核心是一个三维映射：

```text
construct identity × selection environment × cellular state
```

具体包括：

1. 36 个 HDR construct 的序列、功能类别和 barcode。
2. bulk barcode abundance，评价不同环境中的相对 fitness。
3. 10x gene-expression matrix，描述细胞状态。
4. targeted barcode/TCR reads，将 construct identity 分配给单细胞。
5. 体内 input 与 tumor-infiltrating populations 的构建体富集。

这比普通 scRNA-seq 多一个“已知因果 perturbation”维度；但 construct assignment 不是所有细胞都成功，且部分细胞可能携带两个构建体。

### 3.2 pooled library 与供者规模

- constructs：36。
- cassette：约 2–3 kb。
- pooled screens：总体覆盖 4 个独立 human donors。
- 单细胞 PoKI-seq：重点为 2 donors。
- 体外每个 condition 分选约 500,000 个 NY-ESO-1-positive cells 用于 pooled abundance analysis。
- 每个 donor/condition 对每个 construct 的平均覆盖度超过约 130×。

pooling 方式本身有两个需纳入数据模型的误差来源：

- 约 25% knock-in-positive cells 可出现 biallelic/multiple knock-in；
- 若 donor templates 在电转时已混池，约 10% 可能发生 template switching，使 barcode 与功能 payload 错配。

因此，作者还使用独立 pooling/验证设计评估 switching；二次分析不能默认 barcode assignment 100% 准确。

### 3.3 体外 PoKI-seq 的细胞规模

- donors：2。
- conditions：control/standard 与 TGF-β 抑制环境。
- replicates：每个 donor-condition 组合 3 个重复。
- gene-expression libraries：12。
- targeted barcode libraries：12。
- 总计：24 个 GEO samples。
- 单细胞总量：超过 40,000 个。
- construct assignment：约 58% 细胞能够分配 1 或 2 个 barcodes。
- 其余细胞可能因 barcode reads 不足、低质量或无成功可识别 knock-in 而未分配。

这里的“24 个样本”是 12 个生物学/技术 cell suspensions 的两种 library readout，而不是 24 个独立生物学样本。

### 3.4 体内 PoKI-seq 的组成

GEO 中体内部分有 16 个 samples：

- 8 个 cell suspension/well-level samples；
- 每个样本各有 1 个 gene-expression library 与 1 个 targeted TCR/barcode library；
- 即 8 GEX + 8 barcode = 16。

实验结构包括：

- 2 个 human donors；
- 每 donor 有 2 个 recipient/重复层次；
- matched input 与 in vivo TIL；
- 10x 上机目标约 6,000 cells/condition。

论文描述每只小鼠回收约 10,000–20,000 个 TIL 的量级；实际进入 10x 并通过 QC 的细胞数应从 processed matrix 的 barcode 数逐样本统计，不能用回收细胞数代替最终 scRNA cell count。

### 3.5 GSE143417 的 44 个样本如何组成

总入口：[GSE143417](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE143417)

| 样本区段 | 数量 | 内容 |
|---|---:|---|
| 体外 GEX | 12 | 2 donors × 2 conditions × 3 replicates |
| 体外 targeted barcode/TCR | 12 | 与上述 12 个 GEX 一一配对 |
| GFP/mCherry barcode validation | 4 | 用于验证 barcode/construct tracking |
| 体内 GEX | 8 | input/TIL、donor/recipient 组合 |
| 体内 targeted barcode/TCR | 8 | 与体内 8 个 GEX 一一配对 |
| 总计 | 44 | 24 + 4 + 16 |

- 物种：Homo sapiens。
- 平台：Illumina NovaSeq 6000。
- raw archive：BioProject [PRJNA600416](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA600416)，SRA Study SRP241166。
- processed archive：`GSE143417_RAW.tar`，约 469.4 MB。

### 3.6 processed 文件长什么样

GEO 提供两类 processed outputs：

1. **GEX tar.gz**：通常包含 10x filtered matrix 相关文件，例如 `matrix.mtx.gz`、`barcodes.tsv.gz`、`features.tsv.gz`，具体内部结构需解压确认。
2. **barcode/TCR TSV.gz**：记录 10x cell barcode 对应的 targeted construct/TCR read count 或 assignment 信息。

重建单细胞联配时：

- 先按样本读取 GEX matrix；
- 保留 10x barcode 后缀和 sample identity；
- 读取对应 targeted barcode table；
- 通过完整 cell barcode 合并；
- 对 double-positive construct、低 read assignment 和 switching risk 设置独立标签。

### 3.7 Supplementary Table 1 与非 GEO 数据

Supplementary Table 1 提供 36 个 constructs、gRNA/HDR donor 相关序列和注释，是解释 barcode 的核心 dictionary。

论文还包含：

- bulk amplicon/barcode pooled-screen 数据；
- annotated construct resources；
- custom scripts/部分处理流程。

Data availability 表明 PoKI/scRNA 数据进入 GEO，而部分 bulk amplicon、完整构建体注释或自定义分析资源需结合 supplement 或向作者请求。不能假设 GSE143417 包含所有 bulk screen 原始 FASTQ。

### 3.8 下载方式

#### processed matrices

进入：

```text
https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE143417
```

下载 `GSE143417_RAW.tar`。建议先仅下载约 469.4 MB processed archive，建立 sample mapping 后再决定是否下载全部 SRA raw reads。

#### raw FASTQ

从 PRJNA600416/SRA Run Selector 导出 runs：

```bash
prefetch SRR_ACCESSION
fasterq-dump SRR_ACCESSION --split-files --threads 8
gzip SRR_ACCESSION_*.fastq
```

不同 library 的 read structure 不同：10x GEX 与 targeted barcode library 不能用同一套 Cell Ranger 参数盲目处理。应根据 Methods 和 sample title 为每个 run 标注 `GEX` 或 `targeted`。

#### construct table

从 [PMC7219528](https://pmc.ncbi.nlm.nih.gov/articles/PMC7219528/) Supplementary Materials 下载 Supplementary Table 1 和 Supplementary Methods。

### 3.9 下载后建议整理

```text
66_Roth_2020_PoKI/
├── constructs/
│   ├── construct_dictionary.tsv
│   ├── barcodes.tsv
│   └── hdr_sequences.tsv
├── in_vitro/
│   ├── gex/
│   ├── targeted_barcodes/
│   └── sample_manifest.tsv
├── validation_gfp_mcherry/
├── in_vivo/
│   ├── input/
│   ├── TIL/
│   └── sample_manifest.tsv
├── bulk_poki/
└── qc/
    ├── barcode_assignment.tsv
    └── multi_construct_flags.tsv
```

## 4. 主要生物学发现

### 4.1 TGFβR2–4-1BB 组合在免疫抑制环境中富集

该构建体把 TGF-β 环境输入重新连接到共刺激/生存输出，在 TGF-β 条件和体内肿瘤中表现出优势，体现 synthetic rewiring 而非简单 knockout。

### 4.2 不同构建体产生不同转录状态

PoKI-seq 显示部分构建体推动效应/激活程序，另一些构建体如 TCF7 相关设计形成更 naïve/quiescent 的状态。相同 selection abundance 可能对应不同的状态机制。

### 4.3 体外与体内 fitness 不完全相同

某些模块在强刺激或 TGF-β 体外条件下占优，但在肿瘤浸润环境中的效应不同，强调筛选环境本身决定“最优状态”的定义。

## 5. 状态—功能—驱动因子的连接

PoKI-seq 的核心因果链是：

```text
known barcoded knock-in construct
→ single-cell transcriptomic state
→ abundance under a defined selection environment
→ in vivo infiltration/fitness
```

与普通 scRNA atlas 只能从相关性推测 driver 不同，这里的 perturbation identity 已知，因此能更直接地评估“某工程模块将细胞推向哪种状态”。

## 6. 对细胞治疗状态导航的启示

- 可把多个 kb 级 synthetic programs 置于同一基因座公平比较。
- 将 barcode readout 与 scRNA 联配，可以同时优化 fitness 与状态质量。
- 体外筛选条件应模拟真实抑制环境，但最终仍需体内 selection。
- 下一步可把 PoKI 与 live-cell sensor/closed-loop manufacturing 结合，按实时状态选择最优 construct 和培养条件。

## 7. 可复用的分析思路

1. 构建 `cell × gene` 与 `cell × construct` 两个矩阵。
2. 对 construct assignment 设置 minimum targeted-read threshold。
3. 将双 construct cells 单独建类，不能强行分配唯一 perturbation。
4. differential expression 应以 donor/replicate 为层级，避免把所有 cells 当独立重复。
5. bulk barcode enrichment 与 scRNA state effect 联合排序，例如同时优化 in vivo enrichment、memory score 和 exhaustion score。
6. 对 barcode switching 做敏感性分析。

## 8. 推荐图版

- PoKI pooled knock-in workflow。
- 36 constructs 的功能分类与 barcode 设计。
- 体外不同 stimulation/TGF-β 条件的 construct enrichment。
- PoKI-seq construct-to-state UMAP/heatmap。
- input vs tumor TIL 的体内 enrichment。
- TGFβR2–4-1BB 机制与验证。

## 9. 创新价值

- 把 pooled screening 从小型 sgRNA 扩展到 2–3 kb 功能 cassette。
- 通过统一 TRAC 位点降低随机整合和 copy-number/position effect。
- 将 construct barcode 与 single-cell transcriptome 连接。
- 同时支持体外环境筛选和体内肿瘤筛选。

## 10. 局限性

1. 只有 36 个预选构建体，尚非 genome-scale synthetic program screen。
2. 约 25% biallelic/multiple events 会破坏“一细胞一构建体”的理想假设。
3. template switching 约 10%，需额外设计控制 barcode–payload 对应关系。
4. PoKI-seq 只有约 58% 细胞成功分配 1–2 个 barcodes。
5. 体内模型是短期 NSG/A375 xenograft，不能完全模拟人免疫微环境和长期 persistence。
6. 部分 bulk screen 原始数据和 scripts 不在 GEO 的统一包中。

## 11. 对本章节的作用

| 综述模块 | 本文贡献 |
|---|---|
| Chart molecular landscape | 构建 perturbation-resolved single-cell state map |
| Quantify phenotype/function | construct abundance、scRNA state、体内 TIL fitness |
| Perturb/manipulate states | pooled kb-scale targeted knock-in |
| Link transitions with drivers | 直接把 construct identity 与单细胞状态连接 |
| Optimize navigation | 在刺激、TGF-β、体内肿瘤多个环境中筛选最优程序 |
| Real-time optimization | 提供可条码追踪的反馈架构，但 readout 仍需终点测序 |

## 12. 可直接用于综述的观点

> Pooled knock-in screening expands perturbation space from gene loss to the installation of multi-kilobase synthetic programs at a shared genomic locus.

> Linking construct barcodes to single-cell transcriptomes turns a pooled engineering screen into a causal map from installed program to cellular state.

> The optimal engineered state is environment-dependent; in vitro stimulation, suppressive cytokines and tumors impose distinct fitness landscapes.

## 13. 避免误读

- **44 个 GEO samples 不是 44 个独立 biological replicates**：许多是同一样本的 GEX 与 targeted barcode 两种 library。
- **超过 40,000 是体外 PoKI-seq 细胞量级**，不是全部实验统一的最终细胞数。
- **58% 是可分配 1–2 个 barcodes 的细胞比例**，不是总 knock-in efficiency。
- **约 25% multiple events 与约 10% template switching 是重要偏差来源。**
- **GSE143417 重点是 PoKI/scRNA 数据**，不代表所有 bulk amplicon screen 数据都完整入库。
- **TGFβR2–4-1BB 是特定筛选环境中的优胜构建体**，不应直接宣称适用于所有肿瘤或 CAR/TCR 产品。

## 数据与资源链接

- 论文全文：[PMC7219528](https://pmc.ncbi.nlm.nih.gov/articles/PMC7219528/)
- GEO：[GSE143417](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE143417)
- BioProject：[PRJNA600416](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA600416)
