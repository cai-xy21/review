# 《Multi-phenotype CRISPR-Cas9 Screen Identifies p38 Kinase as a Target for Adoptive Immunotherapies》精读

## 论文信息

- **题目**：Multi-phenotype CRISPR-Cas9 Screen Identifies p38 Kinase as a Target for Adoptive Immunotherapies
- **作者**：Gurusamy et al.
- **期刊与年份**：Cancer Cell，2020
- **DOI**：[10.1016/j.ccell.2020.05.004](https://doi.org/10.1016/j.ccell.2020.05.004)
- **PMID / PMC**：[32516591](https://pubmed.ncbi.nlm.nih.gov/32516591/) / [PMC10323690](https://pmc.ncbi.nlm.nih.gov/articles/PMC10323690/)
- **核心数据入口**：[GEO GSE114087](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE114087)
- **研究类型**：原代小鼠 CD8 T 细胞 arrayed multi-phenotype CRISPR–Cas9 kinase screen + p38 抑制剂验证 + bulk RNA-seq + mouse/human adoptive-cell-therapy 功能实验

## 一句话结论

本文在原代 CD8 T 细胞中用四个并行表型——扩增、CD62L、ROS 和 γH2AX——评价 25 个持续性 TCR 磷酸化激酶，鉴定 **MAPK14/p38α** 为兼顾扩增、低氧化/基因毒性压力与记忆样表型的调控节点，并证明短期 p38 抑制可提高多种过继细胞治疗模型的功能。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 筛选形式 | arrayed CRISPR，不是 pooled genome-wide screen |
| 靶点规模 | 25 个 kinase genes；每基因 3 条 sgRNA；另含 non-targeting 与 B2m controls |
| 表型维度 | expansion、CD62L-high、ROS-low、γH2AX-low |
| 筛选重复 | 两次独立 multi-phenotype screens |
| 排名方式 | 每个靶点以第二优 sgRNA 的表现和跨表型 pleiotropy 排名 |
| 主要 hit | Mapk14/p38α |
| RNA-seq | GSE114087，14 个 paired-end bulk RNA-seq 样本 |
| RNA-seq 分组 | day 5：p38 inhibitor 4、vehicle 4；day 10：p38 inhibitor 3、vehicle 3 |
| processed 数据 | 14 个样本级 gene-count 文件；GEO RAW tar 约 2.8 MB |
| raw 数据 | PRJNA464286 / SRP144714，14 个 paired-end runs |
| 关键限制 | 公共 GEO 是 p38 inhibitor 机制验证 RNA-seq，不是 arrayed CRISPR screen 的 raw readout |

## 1. 研究要解决的问题

过继 T 细胞治疗需要同时优化多个经常冲突的性质：快速扩增、维持记忆样状态、降低氧化应激和减少 DNA damage。单一 readout 的筛选可能找到“增殖快但迅速终末分化”的干预，未必提升最终治疗性能。

本文提出：

1. 先从持续 TCR stimulation 的 phosphoproteomics 中选择 kinase candidates；
2. 在原代 CD8 T 细胞中逐一 CRISPR knockout；
3. 同时测量四种表型，对多目标改善的靶点优先排序；
4. 用可转化的小分子 p38 inhibitor 在小鼠和人 T 细胞中验证。

## 2. 方法框架：arrayed 多表型筛选

### 2.1 候选靶点选择

作者并非对全基因组无偏筛选，而是根据持续 TCR signaling 条件下的 phosphoproteomic evidence 选出 25 个 kinase genes。这一前置选择缩小实验规模，也带来 pathway-selection bias。

### 2.2 arrayed CRISPR editing

- 每个基因设计 3 条来自 Brie library 的 exonic sgRNA。
- 包含 non-targeting negative control 和 B2m positive/control target。
- Cas9 RNP/相应原代细胞编辑流程可获得最高约 90% 的 knockout，day 10 典型水平约 75%。
- 每条 sgRNA 在独立孔中处理，因此无需 pooled guide deconvolution。

若按 25 个基因 × 3 guides 加两类单独 controls 计，实验设计至少包含约 77 个 perturbation entries；实际 plate 中 control replicates 应以 supplementary layout 为准。

### 2.3 四个表型

- **Expansion**：细胞数量/扩增能力。
- **CD62L-high**：较少分化、memory-associated 表型。
- **ROS-low**：较低氧化压力。
- **γH2AX-low**：较低 DNA damage response。

作者进行两次独立 screen，并使用第二优 guide 而非最好 guide 代表基因，以降低单条 guide 偶然效应；再根据跨多个表型的综合改善程度选择 pleiotropic hits。

### 2.4 药物与功能验证

使用 p38 inhibitor 在小鼠 CD8、TIL、工程 TCR 和 CAR-T 等体系验证。核心逻辑是短时间窗口内阻断应激/分化信号，而不是永久删除 p38 的全部生理功能。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

本文数据分为三层：

1. **arrayed CRISPR phenotype table**：每条 sgRNA 在四个表型上的流式/扩增值和排序，主要随论文 supplementary/source data 发布。
2. **bulk RNA-seq**：p38 inhibitor 与 vehicle 在 day 5、day 10 的表达变化，存于 GEO。
3. **功能验证数据**：小鼠和人 T 细胞的扩增、表型、代谢/应激、杀伤及体内疗效，主要位于正文和补充材料。

这里没有单细胞 RNA-seq，也不存在 25 个 kinase 对应 25 个 RNA-seq 条件。GSE114087 只回答“小分子 p38 inhibition 后整体转录状态如何改变”。

### 3.2 CRISPR screen 的真实规模

| 项目 | 数量/设计 |
|---|---|
| kinase genes | 25 |
| sgRNA/gene | 3 |
| gene-targeting guides | 75 |
| controls | non-targeting、B2m；具体重复按 plate/source data |
| independent screens | 2 |
| phenotypes | 4 |
| 基因代表值 | 第二优 sgRNA + 跨表型综合排名 |

若仅按 75 guides × 4 phenotypes × 2 screens，至少有 600 个 guide–phenotype–screen 核心测量单元，尚不包括 controls、technical replicates 和时间点。它是小规模、高内容的 arrayed screen，而非依赖 guide sequencing 的 pooled screen。

### 3.3 GSE114087：14 个 bulk RNA-seq 样本

GEO：[GSE114087](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE114087)

| 时间点 | p38 inhibitor | vehicle | 合计 |
|---|---:|---:|---:|
| day 5 | 4 | 4 | 8 |
| day 10 | 3 | 3 | 6 |
| 总计 | 7 | 7 | 14 |

- 物种：Mus musculus。
- 类型：bulk RNA-seq。
- library layout：paired-end。
- 平台：样本记录可在 GEO/SRA 中核验。
- 每个样本提供 gene-count text file，约 204–209 KB。
- `GSE114087_RAW.tar` 约 2.8 MB，主要打包这些 processed count files。
- raw reads：BioProject [PRJNA464286](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA464286)，SRA Study SRP144714。
- 14 个 runs 的规模约为每个 run 2,300 万至 7,000 万 paired-read observations；精确 reads/bases 应以 Run Selector 导出表为准。

### 3.4 GEO 文件结构和用途

GEO 的样本级文件是 **processed gene counts**，适合直接构建 count matrix：

```text
gene_id    count
...
```

实际列名需要按文件读取确认。推荐流程：

1. 从每个 GSM 页面或 `GSE114087_RAW.tar` 解压 14 个 count files；
2. 用 GSM metadata 建立 `condition × day × replicate` 表；
3. 按 gene identifier 外连接为 gene × sample matrix；
4. 使用 DESeq2/edgeR，在模型中加入 day、treatment 与 interaction。

设计式可写为：

```r
design = ~ day + treatment + day:treatment
```

不建议将 day 5 与 day 10 混合后只做 inhibitor vs vehicle，因为这会掩盖时间依赖效应。

### 3.5 Supplementary/source data

论文附件包含多个 `.xlsx` source/supplementary tables，记录：

- 25 个 kinase 和 sgRNA 信息；
- 四表型 screen 数值与排名；
- p38 inhibitor 体外验证；
- 小鼠/人细胞实验及其他扩展结果；
- 引物、抗体或实验资源信息。

这些表是重建 arrayed screen 的主要入口。由于 screen 不依赖 pooled guide sequencing，GEO 不会替代这些 plate-level phenotype tables。

### 3.6 下载方式

#### processed gene counts

进入：

```text
https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE114087
```

下载 `GSE114087_RAW.tar`，解压即可获得样本级 count files。

#### raw FASTQ

从 GEO 页面进入 SRA Run Selector，或从 BioProject PRJNA464286 导出 accession list：

```bash
prefetch SRR_ACCESSION
fasterq-dump SRR_ACCESSION --split-files --threads 8
gzip SRR_ACCESSION_1.fastq SRR_ACCESSION_2.fastq
```

如果只做差异表达，processed counts 通常已足够；只有要统一比对版本、转录本注释或 QC 时才需要几十 GB 级 FASTQ。

#### screen 表格

从 [PMC10323690](https://pmc.ncbi.nlm.nih.gov/articles/PMC10323690/) 的 Supplementary Materials 下载全部 `.xlsx` 和 Supplementary PDF。打开后根据 sheet name 搜索 `screen`、`sgRNA`、`expansion`、`CD62L`、`ROS`、`gammaH2AX` 等字段。

### 3.7 下载后建议整理

```text
64_Gurusamy_2020/
├── arrayed_screen/
│   ├── sgRNA_annotation.tsv
│   ├── phenotype_long.tsv
│   └── gene_rank.tsv
├── rnaseq/GSE114087/
│   ├── processed_counts/
│   ├── raw_fastq/
│   └── sample_metadata.tsv
├── mouse_validation/
├── human_validation/
└── methods_and_resources/
```

把四个表型整理成长表最方便：`screen_id, gene, sgRNA, phenotype, value, rank, control_type`。

## 4. 主要生物学发现

### 4.1 p38 是多表型折中点

Mapk14 loss/p38 inhibition 同时改善扩增、CD62L、ROS 与 γH2AX，而不是只提高单一 readout。这说明 p38 处于 TCR 慢性刺激下应激、分化与增殖的交汇点。

### 4.2 短期药物抑制可改善治疗细胞产品

p38 inhibitor 在扩增阶段使细胞更接近记忆样、低应激状态，并在多种 adoptive therapy 模型中改善功能，显示药理预处理可作为非永久性状态导航手段。

### 4.3 多目标筛选优于单目标最大化

如果只按 expansion 排名，可能获得高增殖但高 ROS/高 DNA damage 的候选。四表型设计显式地寻找 Pareto 更优的细胞状态。

## 5. 状态—功能—驱动因子的连接

```text
sustained TCR signaling
→ p38/MAPK14 activation
→ stress、ROS、DNA-damage and differentiation programs
→ lower expansion / less memory-like state

p38 inhibition
→ CD62L retention + ROS/γH2AX reduction
→ improved expansion and antitumor fitness
```

bulk RNA-seq 给出群体转录机制，但不能判断变化来自所有细胞轻度移动，还是某个亚群比例上升。

## 6. 对细胞治疗状态导航的启示

- 把状态优化定义为多目标问题，而非最大化单一 marker。
- arrayed CRISPR 适合同时做多参数 flow readout。
- 短期、可逆的小分子处理可能比永久 knockout 更适合制造阶段。
- day 5/day 10 数据提示最佳干预窗口需要动态优化。

## 7. 可复用的分析思路

1. 对每条 guide 计算相对 NTC 的标准化 effect size。
2. 用第二优 guide 作为稳健基因评分，降低单 guide off-target。
3. 对四表型做 rank aggregation 或 Pareto-front 分析。
4. RNA-seq 用 treatment × time interaction 分析早晚机制。
5. 将体外制造表型与体内疗效分开验证，避免 surrogate endpoint 过度解释。

## 8. 推荐图版

- 25-kinase、4-phenotype arrayed screen schematic。
- 四表型综合排名图，突出 Mapk14。
- p38 inhibitor 对 CD62L、ROS、γH2AX 和 expansion 的并行影响。
- day 5/day 10 RNA-seq pathway 图。
- mouse/human adoptive immunotherapy efficacy 图。

## 9. 创新价值

- 原代 CD8 T 细胞中可操作的多表型 CRISPR 筛选。
- 用稳健的第二优 guide 与 pleiotropy 排名降低假阳性。
- 从遗传 hit 迅速转化为制造阶段小分子方案。
- 将“低应激、低损伤、记忆样、可扩增”作为联合目标。

## 10. 局限性

1. 只有 25 个预选 kinase，不是无偏全基因组搜索。
2. arrayed screen 的核心 readout 主要在 supplement，缺少统一公共数据库。
3. RNA-seq 是 bulk，无法解析异质性和状态轨迹。
4. p38 广泛参与免疫和应激反应，永久抑制与短期制造期抑制不能等价。
5. 不同抑制剂的选择性、给药窗口和 washout 可能影响结论。
6. 体外四表型只是治疗性能的 proxy，仍需体内验证。

## 11. 对本章节的作用

| 综述模块 | 本文贡献 |
|---|---|
| Quantitatively characterize phenotypes | expansion、CD62L、ROS、γH2AX 四维联合状态 |
| Perturb/manipulate cell states | arrayed CRISPR + 可逆 p38 inhibitor |
| Link transitions with drivers | MAPK14/p38 连接持续 TCR 信号、应激和分化 |
| Optimize navigation conditions | 多目标优化与时间窗优化 |
| Real-time systems | 多参数 flow 可作为迭代反馈，但本文仍为批次式实验 |

## 12. 可直接用于综述的观点

> Multi-phenotype screening reframes T-cell engineering as a multi-objective optimization problem rather than the maximization of a single functional marker.

> Transient pharmacological perturbation during manufacturing can navigate T cells toward a favorable state without permanently rewiring a broadly acting signaling pathway.

> Time-resolved bulk transcriptomics can define treatment windows, but single-cell measurements are still required to resolve whether the population moves coherently or is compositionally selected.

## 13. 避免误读

- **不是 genome-wide pooled screen**：只有 25 个预选 kinases，arrayed 测量。
- **GSE114087 不是 25 个 knockout 的 RNA-seq**：它是 p38 inhibitor vs vehicle、day 5/day 10 的 14-sample bulk RNA-seq。
- **四个表型是 proxy**，不能单独证明体内抗肿瘤效果。
- **p38 genetic knockout 与 transient inhibitor 不等价**。
- **14 个样本不是单细胞数据**，也不能用于细胞亚群轨迹分析。

## 数据与资源链接

- 论文全文：[PMC10323690](https://pmc.ncbi.nlm.nih.gov/articles/PMC10323690/)
- GEO：[GSE114087](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE114087)
- BioProject：[PRJNA464286](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA464286)
