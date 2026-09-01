# 《TOX is a critical regulator of tumour-specific T cell differentiation》精读

## 论文信息

- 作者：Ana C. Scott、Frank Dündar、Patricia Zumbo 等
- 期刊：Nature
- 年份：2019；571: 270–274
- DOI：10.1038/s41586-019-1324-y
- 原文：[Nature](https://www.nature.com/articles/s41586-019-1324-y)
- PubMed：[PMID 31207604](https://pubmed.ncbi.nlm.nih.gov/31207604/)
- 全文：[PMC7698992](https://pmc.ncbi.nlm.nih.gov/articles/PMC7698992/)
- GEO SuperSeries：[GSE126974](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126974)
- bulk ATAC-seq：[GSE126970](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126970)
- bulk RNA-seq：[GSE126973](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126973)

## 一句话结论

在自发性肝癌持续抗原模型中，TOX 由慢性 TCR/NFAT 信号诱导，驱动肿瘤特异 CD8 T 细胞的抑制受体和染色质分化程序；删除 TOX 会破坏该程序，却不能恢复完整效应功能并导致细胞难以持久，说明耗竭分化是保护性适应而非 dysfunction 的单一原因。

## 数据护照（先看这一表）

| 维度 | 内容 | 分析提醒 |
|---|---|---|
| 模型 | TCRTAG 自发性肝癌；OT-I 肿瘤旁观者对照 | 固定 TCR 和抗原 |
| 分子层 | bulk RNA-seq、bulk ATAC-seq | 无 scRNA/scATAC |
| SuperSeries | GSE126974，33 samples | 14 ATAC + 19 RNA |
| ATAC | GSE126970，14 samples | processed 包约 5.3 GB，多为 bigWig |
| RNA | GSE126973，19 samples | processed 包约 68.9 MB |
| 关键比较 | TCRTAG vs OT-I；early vs late；WT vs Tox cKO | 多因子设计，必须按 GSM 重建 |
| 差异规模 | 2,347 DEGs；19,071 differential accessible peaks | TCRTAG 与 OT-I 的群体比较 |
| 外部数据 | GSE30962、GSE64407、GSE89308 | 复用数据，不计入 33 个新样本 |

## 1. 研究要解决的问题

TOX 在肿瘤特异 T 细胞中升高，但其作用可能有两种相反解释：

- TOX 导致 dysfunction，删除即可恢复；
- TOX 建立慢性抗原下的适应性分化，使细胞虽受抑制但能存续。

论文用 tumour-recognizing 与 tumour-ignorant T cells 的同环境比较，以及 Tox conditional deletion，区分抗原驱动、肿瘤环境和 TOX 本身的效应。

## 2. 方法框架

### 2.1 抗原特异与旁观者对照

在同一自发性 SV40 Tag 肝肿瘤环境中比较：

- TCRTAG：识别肿瘤抗原，经历持续 TCR 刺激；
- OT-I：其 OVA 抗原在肿瘤中不存在，作为旁观者/非相关特异性对照。

这种设计比简单比较“肿瘤内 vs 外周”更好地区分抗原经历。

### 2.2 状态分层和遗传扰动

作者分析早期 day 8–9 和晚期 day 20–21，并用 PD-1、LAG-3、CD39 高/低等表型分选。Tox conditional KO 和 overexpression 用于测试必要性/充分性；另用 Listeria、LCMV 和既有数据作参照。

## 3. 数据规模与图谱组成

### 3.1 GSE126974 SuperSeries

[GSE126974](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126974) 共 33 个 bulk 样本：

| 子系列 | 技术 | 样本数 | 处理后包 |
|---|---|---:|---:|
| GSE126970 | bulk ATAC-seq | 14 | 约 5.3 GB |
| GSE126973 | bulk RNA-seq | 19 | 约 68.9 MB |
| 合计 | 两种 bulk assay | 33 | SuperSeries RAW.tar 约 5.4 GB |

大体积主要来自逐样本 bigWig 覆盖文件，不意味着原始 FASTQ 只有 5.4 GB。

### 3.2 GSE126970 bulk ATAC

[GSE126970](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126970) 的 14 个样本覆盖：

- tumour-specific TCRTAG 与 tumour-ignorant OT-I；
- early 与 late tumour stages；
- PD-1/LAG-3/CD39 表型分层；
- WT 与 Tox-deficient 条件。

GSE126970_RAW.tar 当前约 5.3 GB，主要为 bigWig 和相关处理后信号。bigWig 适合浏览基因组轨道和复图，但不包含每个 fragment 的细胞/文库级信息，也不能替代 FASTQ 做统一 peak calling。

下载：

~~~bash
wget -c \
  https://ftp.ncbi.nlm.nih.gov/geo/series/GSE126nnn/GSE126970/suppl/GSE126970_RAW.tar
~~~

若只关注 TOX、Pdcd1、Tcf7 等位点可直接加载 bigWig；若做差异可及性，建议下载原始 reads 并统一生成 consensus peak matrix。

### 3.3 GSE126973 bulk RNA

[GSE126973](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126973) 的 19 个 bulk RNA 样本覆盖相似但不完全相同的状态和扰动：

- TCRTAG vs OT-I；
- early/late；
- WT vs Tox deletion；
- TOX-related control/perturbation 条件。

GSE126973_RAW.tar 当前约 68.9 MB，主要为 gene-level processed TXT。RNA 与 ATAC 样本数不同，不能假定 14 个 ATAC 都有严格一一对应 RNA 文库。

下载：

~~~bash
wget -c \
  https://ftp.ncbi.nlm.nih.gov/geo/series/GSE126nnn/GSE126973/suppl/GSE126973_RAW.tar
~~~

### 3.4 33 个样本的设计层级

为防止误分组，建议从 GEO series matrix/GSM characteristics 建 manifest：

| 字段 | 例子 |
|---|---|
| assay | RNA / ATAC |
| TCR/model | TCRTAG / OT-I |
| antigen relationship | tumour-recognizing / irrelevant |
| time | day 8–9 / day 20–21 |
| phenotype gate | PD-1、LAG-3、CD39 high/low |
| genotype | WT / Tox cKO |
| perturbation | control / overexpression |
| replicate | biological replicate |

这些因素不一定完全交叉。不能用简单 2×2 设计臆造不存在的重复。

### 3.5 差异规模

在 tumour-recognizing TCRTAG 与同肿瘤 OT-I 对照之间，作者报告：

- 2,347 个差异表达基因；
- 19,071 个差异可及区域。

这些是特定比较和阈值下的 bulk 结果，不是“TOX 直接靶基因数”，也不全由 TOX 单独造成。持续抗原、状态、细胞组成和时间都会贡献。

### 3.6 原始数据下载

GEO 页面提供 SRA 链接。完整重处理步骤：

1. 分别进入 GSE126970 与 GSE126973；
2. 通过 SRA Run Selector 导出 RunInfo；
3. 检查 paired/single-end、read length 和 library layout；
4. 下载原始 reads；
5. RNA 统一比对/定量，ATAC 统一去重复、Tn5 shift 和 peak calling；
6. 以 biological replicate 做差异分析。

下载处理后 SuperSeries 全包：

~~~bash
wget -c \
  https://ftp.ncbi.nlm.nih.gov/geo/series/GSE126nnn/GSE126974/suppl/GSE126974_RAW.tar
~~~

全包约 5.4 GB，解包后预留至少 10–15 GB。若只需要 RNA，不必下载大体积 ATAC bigWig。

### 3.7 外部复用数据

论文还引用：

- GSE30962：慢性病毒耗竭表达参照；
- GSE64407：NFAT1 ChIP 等调控参照；
- GSE89308：前述肿瘤 dysfunction bulk ATAC/human validation。

这些 accession 用于支持 TOX/NFAT 与耗竭程序保守性，不能计入 GSE126974 的 33 个新生成样本。

## 4. 核心机制

### 4.1 持续抗原而非肿瘤位置本身驱动 TOX

TCRTAG 和 OT-I 位于相同肿瘤环境，但只有识别肿瘤抗原的 TCRTAG 细胞强烈进入 TOX/抑制程序，说明持续 cognate TCR 信号是关键。

### 4.2 TOX 建立耗竭分化程序

TOX 与 NFAT 网络相关，促进抑制受体和耗竭相关染色质状态。Tox 缺失使这一程序明显受损。

### 4.3 删除 TOX 不等于恢复

Tox-deficient tumour-specific cells 仍不能形成完整、高质量效应反应，并且长期存续下降。由此可见：

- dysfunction 不是 TOX 的单一输出；
- TOX-dependent exhaustion program 可能保护细胞免受持续刺激损伤；
- 工程上完全移除 TOX 有显著持久性风险。

## 5. 关键图表怎么读

- TCRTAG vs OT-I：很好地控制肿瘤位置，但 TCR 本身、前体状态也可能有差异。
- WT vs KO：KO 的存活选择会影响 bulk RNA/ATAC。
- 19,071 peaks：是条件差异，不等于直接 TOX binding sites。
- marker 高低分选：群体仍可能含内部异质性。

## 6. 创新点

1. 在同一肿瘤中使用 cognate 与 irrelevant TCR 对照。
2. 将 TOX 因果扰动与 RNA/ATAC 连接。
3. 证明耗竭程序与 dysfunction 不能简单等同。
4. 揭示删除抑制命运因子可能牺牲持久性。

## 7. 局限性

1. bulk 测序不能解析 progenitor/terminal Tex 细胞比例。
2. 固定 TCR 模型可能混入受体内在差异。
3. KO 后细胞减少造成 survivor bias。
4. 早晚时间点仍是群体横断面。
5. 小鼠肝癌模型未覆盖人 CAR-T/TCR-T 制造环境。
6. direct TOX targets 需要 CUT&RUN/ChIP 和增强子编辑进一步证明。

## 8. 对本综述的作用

该论文特别适合讨论“导航目标不能单维优化”：

- 低抑制受体不一定等于高质量状态；
- 去除 TOX 可短期减弱耗竭表型，却降低适应和持久性；
- 应优化 TOX 动态、TCR 刺激剂量/持续时间和下游存续网络，而不是永久完全关闭。

## 9. 可直接写进综述的表述

> 在同一肿瘤中，只有识别持续抗原的 TCRTAG 细胞进入 TOX 依赖的抑制受体和染色质程序；Tox 缺失破坏该程序却未恢复正常效应分化，反而削弱持久性，表明耗竭是慢性刺激下的保护性适应状态。

## 10. 最容易误读的地方

- 33 是 bulk RNA/ATAC libraries，不是单细胞。
- 2,347 DEGs 和 19,071 peaks 不都是 TOX 直接靶标。
- OT-I 是同肿瘤中的 irrelevant-antigen 对照。
- TOX deletion 不会自动生成 memory-like T cells。
- 5.4 GB 主要是处理后 bigWig，不代表完整原始数据体积。
