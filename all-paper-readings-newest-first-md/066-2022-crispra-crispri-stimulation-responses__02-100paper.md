# 《CRISPR activation and interference screens decode stimulation responses in primary human T cells》精读

## 论文信息

- 作者：Ralf Schmidt、Zachary Steinhart、Madeline Layeghi 等
- 期刊：*Science*
- 年份：2022；375(6580): eabj4008；发表于 2022 年 2 月 4 日
- DOI：10.1126/science.abj4008
- 原文：[Science](https://doi.org/10.1126/science.abj4008)
- PubMed：[PMID 35113687](https://pubmed.ncbi.nlm.nih.gov/35113687/)
- 免费全文：[PMC9307090](https://pmc.ncbi.nlm.nih.gov/articles/PMC9307090/)
- 主 screen SuperSeries：[GEO GSE174292](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE174292)
- CRISPRa Perturb-seq：[GEO GSE190604](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE190604)
- 补充 screen：[GEO GSE190846](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE190846)
- 代码和处理后数据：[Zenodo 10.5281/zenodo.5784651](https://doi.org/10.5281/zenodo.5784651)

## 一句话结论

在同一原代人 T 细胞平台中配对实施全基因组 CRISPRa 与 CRISPRi，并以 IL-2/IFN-γ 高低作为 readout，可把“足量即可增强输出的瓶颈”与“维持输出所必需的组件”区分开；进一步对 70 个候选做约 56,000-cell CRISPRa Perturb-seq，得到扰动如何重编程刺激响应状态的单细胞图谱。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| CRISPRa library | >112,000 sgRNA，>18,800 protein-coding genes | Calabrese A/B；gain-of-function，不是 KO |
| CRISPRi library | Dolcetto A/B genome-wide | loss-of-expression；机制和 KO 不完全相同 |
| 主 readout | CD4 IL-2、CD8 IFN-γ high/low bins | 细胞因子表达是刺激后的终点分选，不是分泌动力学 |
| 主 screen donors | 2 名独立人血供者 | CRISPRa 与 CRISPRi 均做 donor-level reproducibility |
| CRISPRa hits | IL-2 444；IFN-γ 471；共享 171 | hit 数依赖论文阈值，不能当“通路真值” |
| Perturb-seq | 约 56,000 个原代人 T 细胞 | 2 donors；70 个 hits/controls；刺激与未刺激样本 |
| GSE174292 | 68 sample records | GSE174255 CRISPR 52 + GSE174284 bulk RNA 16 |
| GSE190604 | 16 sample records | Perturb-seq；处理矩阵约 950 MB + barcodes/features/guide calls |
| GSE190846 | 16 sample records | 补充 CD4 screen；处理 read-count 表 3.8 MB |
| Zenodo | 26.2 MB `Genome-wide-screens.zip` 等 | 分析脚本与 analyzed Perturb-seq；记录 v1 record DOI |

## 1. 研究要解决的问题

传统 knockout 筛选擅长发现“必需基因”，但不能发现一个在当前状态中低表达、却能在被增强时改善功能的调节器。作者希望在原代人 T 细胞中用互补扰动回答：

1. 哪些基因上调足以增强 IL-2 或 IFN-γ；
2. 哪些基因受抑会降低或增强这些输出；
3. 同一信号网络在 gain-of-function 与 loss-of-function 两侧是否呈对称或非对称响应；
4. 单一候选的改变只是调整 cytokine magnitude，还是把细胞推入不同刺激响应状态。

## 2. CRISPRa/CRISPRi 与 Perturb-seq 框架

### 2.1 两种扰动的互补性

- **CRISPRa**：dCas9-VP64 + SAM 组件激活内源基因；适合找“增强表达是否足够”。
- **CRISPRi**：dCas9-KRAB 抑制转录；适合找“该基因是否必需”或负调节因子。
- **FACS readout**：刺激后分别将 IL-2 或 IFN-γ 分成 high/low bins，测 sgRNA enrichment。

例如 TCR–NF-κB 通路中的 MALT1、BCL10 由 CRISPRi 暴露为必需组件；4-1BB、CD27、CD40、OX40 等 TNFRSF 成员则更多由 CRISPRa 显示“过表达足以增强 IFN-γ”。这不是两个文库谁更好，而是回答不同因果问题。

### 2.2 单细胞跟进

作者选择 70 个 genome-wide CRISPRa screen hits 和 controls，构建 direct-capture-compatible CRISPRa-SAM sgRNA，在 2 名供者的原代人 T 细胞中做 10x 3′ v3.1 + feature barcode Perturb-seq。该层将 guide identity 与每个细胞的全转录组连接，从而区分：

- 广泛增强刺激程序；
- 偏向 IL-2 或 IFN-γ 的选择性程序；
- 改变静息/激活轴；
- 诱导不同 cytokine-expression state 的扰动。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

数据不是一个单一 GEO accession，而是三组 GEO + 一个 Zenodo：

1. **GSE174292**：主 genome-wide CRISPRa/i screen 和 arrayed-hit bulk RNA-seq；
2. **GSE190604**：CRISPRa Perturb-seq 的 GEX、guide calls 与原始 reads；
3. **GSE190846**：补充 CD4⁺ T 细胞 screen；
4. **Zenodo 5784651**：复现代码、处理后 Perturb-seq 对象和 screen 分析文件。

如果只按论文列表中的 `GSE174292` 下载，会漏掉最重要的单细胞数据。论文 Data and materials availability 明确同时列出三项 GEO accession。

### 3.2 多大规模、覆盖哪些实验情境

| 数据块 | 规模 | 组成 |
|---|---:|---|
| genome-wide CRISPRa | >112k guides；>18.8k genes | CD4/IL-2 与 CD8/IFN-γ；high/low；2 donors |
| genome-wide CRISPRi | Dolcetto A/B | 同样围绕 IL-2 与 IFN-γ high/low；2 donors |
| GSE174255 | 52 samples | 主 CRISPR screen；`Other` experiment type |
| GSE174284 | 16 samples | FOXQ1/control、刺激/未刺激等 bulk RNA-seq |
| GSE174292 | 68 samples | 上述两子系列的 SuperSeries |
| GSE190604 | 16 samples；约 56k cells | 2 donors × stimulation/guide-library 设计的 CRISPRa Perturb-seq |
| GSE190846 | 16 samples | supplemental CD4 screen |
| Perturb targets | 70 hits/controls | 不是全基因组单细胞 screen，而是全基因组 FACS screen 后的 focused follow-up |

“约 56,000 cells”是论文分析规模；GEO 的 16 samples 是建库/样本记录层级。两者不能互换。

### 3.3 公共数据包有什么

| 文件/入口 | 体积 | 内容与用途 |
|---|---:|---|
| [GSE174255](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE174255) | 52 samples | 主 CRISPRa/i screen |
| `GSE174255_sgRNA-Read-Counts.xlsx` | 21.3 MB | guide-level counts；快速复做 high/low enrichment |
| [GSE174284](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE174284) | 16 samples | bulk RNA-seq |
| `GSE174284_gene_counts_raw.txt.gz` | 928.3 KB | bulk raw gene count matrix |
| [GSE190604](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE190604) | 16 samples | CRISPRa Perturb-seq |
| `GSE190604_matrix.mtx.gz` | 950.1 MB | 稀疏 GEX count matrix |
| `GSE190604_barcodes.tsv.gz` | 419.4 KB | 细胞条形码 |
| `GSE190604_features.tsv.gz` | 326.8 KB | 基因/feature 注释 |
| `GSE190604_cellranger-guidecalls-aggregated-unfiltered.txt.gz` | 976.4 KB | Cell Ranger guide assignment；需自行做 confidence/QC |
| [GSE190846](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE190846) | 16 samples | supplemental CRISPR screen |
| `GSE190846_supp_CD4_CRISPR_screens_read_counts.tsv.gz` | 3.8 MB | 补充 CD4 screen 计数 |
| [Zenodo 5784651](https://doi.org/10.5281/zenodo.5784651) | 代码包；主 zip 26.2 MB | genome-wide screen 代码、分析文件和 processed Perturb-seq |
| Supplementary Tables 1–2 | 12.2/8.6 MB | CRISPRa/i screen 结果和 hit 注释 |
| Supplementary Tables 3–6 | 约 10–28 KB | guide、验证、secretome 等 |

三个 GEO 页面都可用 SRA Run Selector 获取原始 FASTQ。处理矩阵足以复核多数图，但从原始 reads 重建需要分别处理 sgRNA amplicon、bulk RNA 和 10x GEX/CRISPR feature libraries。

### 3.4 如何获取：按目的选择

#### 路线 A：只复核 genome-wide hit

下载 `GSE174255_sgRNA-Read-Counts.xlsx` 和 Supplementary Tables 1–2。先按 donor、CRISPRa/i、cytokine、high/low 和 Calabrese/Dolcetto set 解析列，再计算 LFC 和基因级聚合。

#### 路线 B：复核 Perturb-seq 状态

下载 GSE190604 的四个处理文件，按 Matrix Market 格式构建对象，并将 guide calls 通过 cell barcode join 到 metadata。或者直接从 Zenodo 取作者处理对象和代码，最快接近论文图。

```python
import scanpy as sc
import pandas as pd

adata = sc.read_mtx("GSE190604_matrix.mtx.gz").T
barcodes = pd.read_csv("GSE190604_barcodes.tsv.gz", header=None)[0]
features = pd.read_csv("GSE190604_features.tsv.gz", sep="\t", header=None)
adata.obs_names = barcodes.astype(str)
adata.var_names = features.iloc[:, 1].astype(str)
```

读取后要检查 MTX 的方向；不同导出可能已经是 cell × feature 或 feature × cell，不能机械 `.T`。

#### 路线 C：严格从 raw reads 重做

分别从三个 accession 导出 SRA run table。Perturb-seq 要把 GEX 与 CRISPR guide feature libraries 按 sample 对齐，用相同 Cell Ranger 版本或明确记录升级差异。

#### 路线 D：完整复现论文分析

使用 Zenodo record `5784651` 而不是论文正文中被截断显示的 `578465`。下载后核对 `Genome-wide-screens.zip` 的 MD5（Zenodo 页面提供），并固定脚本依赖版本。

### 3.5 下载后先做什么

1. screen 数据检查每个 donor 的 high/low library depth 与 sgRNA dropout；
2. 分开 CRISPRa 和 CRISPRi，不能把 LFC 方向直接拼接；
3. Perturb-seq 用 barcode 精确 join guide calls，剔除无 guide、低置信和多 guide 细胞；
4. donor 应作为统计重复，单细胞不能把数万 cells 当数万独立样本；
5. 刺激与未刺激状态分层后再看 perturbation effect；
6. 复现 marker 和状态时保留每个 perturbation 的 cell count，低覆盖候选不要过度解释。

## 4. 主要发现

CRISPRa 与 CRISPRi 共同绘制出刺激到 cytokine 的可调节点：

- CRISPRi 强调 TCR–NF-κB 等必需线路；
- CRISPRa 发现 TNFRSF 共刺激受体和转录因子等“剂量瓶颈”；
- CRISPRa 的 IL-2 与 IFN-γ screen 共有 171 hits，但也存在 cytokine-selective regulators；
- FOXQ1 等非典型 T 细胞调节器可重塑 secretome；
- Perturb-seq 显示不同扰动不仅改变 cytokine 强度，还将细胞置于不同刺激响应状态。

## 5. 从分子扰动到状态导航

这篇论文的重要概念是“同一通路中，necessity 与 sufficiency 不对称”。某基因被 CRISPRi 识别为必需，不代表把它过表达就会进一步增强输出；反之，CRISPRa 命中的共刺激受体可能在基础条件下不是单独必需，却能在表达增加时放大功能。

因此，设计细胞状态导航策略时，应该把扰动算子本身纳入模型：KO、CRISPRi、CRISPRa 和 ORF overexpression 不是可互换的 gene score。

## 6. 推荐图版

- **Fig. 1**：>112k sgRNA 的 CRISPRa IL-2/IFN-γ screen；适合展示 gain-of-function landscape。
- **Fig. 2**：CRISPRa 与 CRISPRi 互补网络；本综述最推荐。
- **Fig. 3**：arrayed validation 与 secretome；适合从单指标转向多功能 phenotype。
- **Fig. 4**：约 56k cells 的 Perturb-seq 状态图谱；适合“link transitions with drivers”。

若只能选一张，选 Fig. 2；若章节重点是单细胞扰动，选 Fig. 4。

## 7. 创新价值

1. 在原代人 T 细胞中成对部署 genome-wide CRISPRa 与 CRISPRi。
2. 将 pooled FACS screen 与 focused Perturb-seq 串联，兼顾尺度和机制分辨率。
3. 把 cytokine 由单一高低值扩展为转录状态和 secretome 重编程。
4. 数据分散但公开较完整：screen counts、raw reads、单细胞矩阵、guide calls、代码均可获得。

## 8. 局限性

1. 主 screen 只有 2 名供者，不能代表完整人群遗传与免疫差异。
2. 读出集中在短期刺激后的 IL-2/IFN-γ，未直接测长期持久性或肿瘤杀伤。
3. Perturb-seq 只覆盖 70 个 hits/controls，不是 genome-wide single-cell screen。
4. CRISPRa 的过表达幅度受基因启动子、染色质和 dCas9 系统影响。
5. cytokine high/low 分箱丢失连续分布信息。
6. 数据来自体外培养，缺少组织和肿瘤微环境的空间约束。

## 9. 对本综述架构的作用

该文是“techniques to perturb/manipulate cell states”的关键方法论文，也连接“quantitatively characterizing phenotypes/functions/markers”：同一扰动先用 FACS 量化 cytokine，再用 secretome 和单细胞转录组扩展表型空间。

它为 real-time optimization 提供候选调节器和状态变量，但本身仍是终点测序；若要进入闭环，需要把 cytokine/marker 换成可连续采样的 reporter 或无损成像 readout。

## 10. 可直接用于综述的观点

> 在原代人 T 细胞中，CRISPRa 与 CRISPRi 分别解析信号网络的“足以增强”与“维持所必需”节点；对筛选命中的 70 个调节器进行约 56,000-cell CRISPRa Perturb-seq 后可进一步区分只改变 cytokine 幅度的扰动与真正重编程刺激响应状态的扰动（Science 2022, Schmidt）。

## 11. 避免误读

- 不要只写 GSE174292；Perturb-seq 在 GSE190604。
- 不要把约 56,000 cells 写成 56,000 donors 或 biological replicates。
- 不要把 70-target Perturb-seq 称为 genome-wide single-cell screen。
- 不要将 CRISPRa hit 与 CRISPRi hit 的方向简单相加。
- 不要把 cytokine high 状态等同于更安全或更持久的治疗状态。
