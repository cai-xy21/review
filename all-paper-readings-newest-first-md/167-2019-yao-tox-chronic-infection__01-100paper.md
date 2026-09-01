# 《Single-cell RNA-seq reveals TOX as a key regulator of CD8+ T cell persistence in chronic infection》精读

## 论文信息

- 作者：Chen Yao、Hong-Wu Sun、Nianshuang Lacey 等
- 期刊：Nature Immunology
- 年份：2019；20: 890–901
- DOI：10.1038/s41590-019-0403-4
- 原文：[Nature Immunology](https://www.nature.com/articles/s41590-019-0403-4)
- PubMed：[PMID 31209400](https://pubmed.ncbi.nlm.nih.gov/31209400/)
- 全文：[PMC6588409](https://pmc.ncbi.nlm.nih.gov/articles/PMC6588409/)
- GEO SuperSeries：[GSE119943](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE119943)
- scRNA-seq：[GSE119940](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE119940)
- H3K27ac ChIP-seq：[GSE119941](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE119941)
- bulk RNA-seq：[GSE119942](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE119942)

## 一句话结论

通过急性 LCMV Armstrong 与慢性 Clone 13 感染的 16,042 个 P14 CD8 T 单细胞，作者发现 progenitor-like exhausted cells 在免疫反应高峰前已与 memory precursor 分叉；TOX 所在转录模块和 H3K27ac 程序对慢性感染中的前体维持与长期存续至关重要。

## 数据护照（先看这一表）

| 维度 | 内容 | 分析提醒 |
|---|---|---|
| 模型 | P14 CD8；LCMV Arm vs Cl13 | 固定病毒特异 TCR 的小鼠模型 |
| scRNA 主分析 | 16,042 cells | 8 个 WT libraries：2 infection × 2 days × 2 replicates |
| scRNA GEO | GSE119940，10 libraries | 另有 2 个 day 7 bone-marrow chimera WT/ToxKO libraries |
| 时间点 | day 4.5、day 7 | 捕获峰值前/峰值附近早期分叉 |
| scRNA clusters | 主要分析中多个 clusters | cluster 10 progenitor-like 116 cells，属于稀有群 |
| H3K27ac ChIP | GSE119941，16 samples | 4 状态 × 2 IP replicates，另有相应 input |
| bulk RNA | GSE119942，34 samples | 多时间点、感染和 genotype 条件 |
| SuperSeries | GSE119943，60 samples | 10 + 16 + 34 |
| 处理后总包 | 约 673.8 MB | 包含 bigWig、MTX、TSV 等 |

## 1. 研究要解决的问题

慢性感染中 progenitor exhausted T cells 是持续反应和 checkpoint therapy 应答的来源，但它们何时与正常 memory precursor 分叉、由什么调控仍不清楚。论文问：

1. 急性和慢性感染在早期单细胞状态上何时分离；
2. progenitor-like Tex 是否在高峰前已经出现；
3. 哪些共表达模块和染色质标记驱动这一状态；
4. TOX 是否维持而非单纯抑制慢性感染 CD8 T cells。

## 2. 方法框架

### 2.1 急慢性感染时间对照

转移 P14 CD8 T cells 后感染：

- LCMV Armstrong：急性；
- LCMV Clone 13：慢性；
- day 4.5 和 day 7 分选 P14 cells；
- 每个 infection×time 组合 2 个生物重复。

### 2.2 多层组学和扰动

- 10x scRNA-seq：早期状态和稀有 progenitor-like cluster；
- WGCNA：定义 49 个共表达模块；
- bulk RNA-seq：扩展时间/基因型比较；
- H3K27ac ChIP-seq：比较活性增强子/启动子景观；
- Tox KO/bone marrow chimera：测试存续和状态形成。

本文没有 ATAC-seq；其表观遗传层是 H3K27ac ChIP-seq。

## 3. 数据规模与图谱组成

### 3.1 GSE119943 SuperSeries

[GSE119943](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE119943) 共 60 个样本：

| 子系列 | 技术 | 样本/文库数 | processed 包 |
|---|---|---:|---:|
| GSE119940 | 10x scRNA-seq | 10 | 约 346.7 MB |
| GSE119941 | H3K27ac ChIP-seq | 16 | 约 327.1 MB |
| GSE119942 | bulk RNA-seq | 34 | 约 6.9 MB 合计 |
| 合计 | 三层数据 | 60 | GSE119943_RAW.tar 约 673.8 MB |

60 是文库/样本记录，不是 60 只小鼠或 60 个单细胞样本。

### 3.2 GSE119940 scRNA：主发现 8 个文库

主 scRNA 设计为：

| 感染 | 时间 | biological replicates | 作用 |
|---|---:|---:|---|
| Armstrong | day 4.5 | 2 | 急性早期 |
| Clone 13 | day 4.5 | 2 | 慢性早期 |
| Armstrong | day 7 | 2 | 急性峰值附近 |
| Clone 13 | day 7 | 2 | 慢性峰值附近 |

这 8 个 WT 文库整合后得到 16,042 个细胞，是论文标题所指的主要 single-cell discovery dataset。

此外，GSE119940 还有 2 个 day 7 bone-marrow chimera 文库：

- WT P14；
- Tox KO P14。

因此 GEO 页面是 10 个 scRNA libraries，而主文发现分析的精确规模是 8 个 WT libraries、16,042 cells。不能把两者简单混写为“10 个样本产生 16,042 个细胞”，除非下载对象重新统计确认。

### 3.3 单细胞 cluster 组成

论文主要 cluster 中，与慢性感染 progenitor 分化特别相关的群包括：

| cluster | 细胞数 | 解释 |
|---|---:|---|
| cluster 5 | 1,357 | 慢性感染相关早期状态之一 |
| cluster 7 | 834 | 另一慢性感染/分化状态 |
| cluster 10 | 116 | progenitor-like exhausted 稀有群 |

cluster 10 只有 116 个细胞，结论依赖 marker、module、后续 bulk/KO 和功能验证，而不能只靠聚类稳定性。

### 3.4 GSE119940 下载内容

[GSE119940](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE119940) 的 GSE119940_RAW.tar 当前约 346.7 MB，逐样本包含：

- Matrix Market 稀疏表达矩阵；
- genes/features TSV；
- barcodes TSV。

下载：

~~~bash
wget -c \
  https://ftp.ncbi.nlm.nih.gov/geo/series/GSE119nnn/GSE119940/suppl/GSE119940_RAW.tar
~~~

原始 reads 对应 SRA study SRP161709、BioProject PRJNA490748。当前 BioProject 统计：

- 10 个 SRA experiments；
- 约 215 Gbases；
- 归档量约 0.11 TB。

处理后 MTX 足以重建表达对象；若需重做比对和 ambient RNA/QC，下载 SRA。

### 3.5 GSE119941 H3K27ac ChIP

[GSE119941](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE119941) 共 16 个样本。设计可概括为 4 种 T 细胞状态，每种 2 个 H3K27ac IP biological replicates，并有相应 input，总计 8 IP + 8 input。

主要状态覆盖：

- naive；
- acute effector；
- acute memory；
- chronic exhausted。

GSE119941_RAW.tar 当前约 327.1 MB，主要为 bigWig。原始 reads 对应 SRP161708。

H3K27ac 表示活性调控区域富集，不等于 chromatin accessibility；不要将其写成 ATAC-seq。

### 3.6 GSE119942 bulk RNA

[GSE119942](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE119942) 共 34 个 bulk RNA 样本，覆盖不同时间点、急/慢性感染、WT/Tox 相关条件。GEO 当前处理后文件包括约：

- 1.9 MB TXT.gz；
- 4.2 MB XLSX；
- 771 KB TXT.gz。

原始 reads 对应 SRP161707。34 个样本的完整分组需由 GSM characteristics 建立，不能用 scRNA 的 2×2 设计套用到所有 bulk 样本。

### 3.7 三层数据如何对应

| 层 | 能回答 | 不能单独回答 |
|---|---|---|
| scRNA | 稀有群、状态异质性、早期分叉 | 直接染色质因果 |
| bulk RNA | 更广时间/基因型的平均转录 | 亚群比例与单细胞轨迹 |
| H3K27ac ChIP | 活性调控景观 | 开放性、TF 结合的单细胞异质性 |

三层数据是同一生物问题的互补实验，不是每个单细胞的 RNA+ChIP 配对多组学。

### 3.8 下载与重建

处理后 scRNA：

~~~python
import scanpy as sc

adata = sc.read_10x_mtx(
    "GSM_directory",
    var_names="gene_symbols",
    cache=True
)
adata.obs["sample"] = "GSM..."
adata.obs["infection"] = "Arm"
adata.obs["day"] = "4.5"
adata.obs["replicate"] = "1"
~~~

合并 10 个文库前应：

- 给 barcode 添加 sample 前缀；
- 区分 8 个 WT discovery 与 2 个 WT/KO chimera；
- 按论文阈值复现 QC；
- 以 mouse/replicate 而非 cell 做条件统计；
- 验证最终是否得到 16,042 个主分析细胞。

完整原始下载：

~~~bash
curl -L \
  "https://trace.ncbi.nlm.nih.gov/Traces/sra-db-be/runinfo?acc=SRP161709" \
  -o SRP161709_runinfo.csv
~~~

## 4. 核心单细胞发现

### 4.1 高峰前已经分叉

day 4.5 和 day 7 的单细胞状态显示，慢性感染 progenitor-like cells 在反应高峰前已与急性感染 memory precursor 走向不同程序。耗竭不是在效应结束后才被动出现。

### 4.2 49 个共表达模块

WGCNA 得到 49 个模块，分离增殖、效应、记忆和慢性感染相关程序。TOX 所在模块约含 200 个基因；作者结合既有调控数据提出约 90 个潜在直接靶基因。

200 是模块成员数，90 是候选直接调控集，均不是本文新做单细胞 ChIP 得到的“确定 TOX targets”。

### 4.3 Progenitor-like cluster

116 个 cluster 10 细胞表现出 progenitor-like 特征，并与长期存续相关。这一小群体是后来 Tpex 概念的重要早期证据，但其 marker 与现代 TCF1+ progenitor exhausted 定义应结合后续文献更新。

## 5. TOX 的因果作用

Tox 缺失使慢性感染中的 P14 cells 难以维持，并破坏 progenitor-like/耗竭程序。与同期两篇 Nature 论文一致，TOX 是慢性刺激下的适应性命运节点；删除它不等于获得持久、高功能 memory。

## 6. 关键图表怎么读

- 早期 UMAP/cluster：显示状态分离，不直接证明谱系方向。
- 116-cell cluster：稀有群，需结合 replicate 分布和后续功能证据。
- WGCNA：模块是共表达相关，不全是直接 TF targets。
- H3K27ac：bulk 群体平均，不能定位到 cluster 10 单细胞。
- KO chimera：强因果证据，但 KO 后存活差会引入选择偏差。

## 7. 创新点

1. 在高峰前用 scRNA 捕获急慢性感染分叉。
2. 将稀有 progenitor-like Tex 与 TOX 模块连接。
3. 结合 scRNA、bulk RNA、H3K27ac 和 KO。
4. 强调 TOX 对 persistence 的必要性。

## 8. 局限性

1. scRNA 仅两个早期时间点。
2. 主要模型为固定 TCR 的小鼠 LCMV。
3. 16,042 个细胞来自有限生物重复。
4. cluster 10 只有 116 个细胞。
5. H3K27ac 和 bulk RNA 不是单细胞配对。
6. 论文未使用 ATAC，不能直接描述开放染色质。

## 9. 对本综述的作用

该论文把“状态转变”和“分子驱动”接在一起：

- 时间：day 4.5–7 即出现命运偏转；
- 状态传感：早期 progenitor-like module；
- 驱动：TOX；
- 功能目标：慢性刺激下 persistence；
- 工程含义：干预窗口应早于终末耗竭，并避免以牺牲前体池为代价。

## 10. 可直接写进综述的表述

> 急性与慢性 LCMV 的 16,042 个早期单细胞转录组显示，progenitor-like exhausted CD8 T cells 在免疫反应高峰前即与 memory precursor 分叉；TOX 模块及 H3K27ac 程序对该前体池和长期存续必不可少。

## 11. 最容易误读的地方

- GSE119940 有 10 个文库，但 16,042 个主分析细胞来自其中 8 个 WT 文库。
- GSE119943 的 60 是 scRNA、ChIP 和 bulk RNA 样本总数。
- 本文表观遗传数据是 H3K27ac ChIP，不是 ATAC。
- 49 个模块不是 49 个细胞类型。
- 约 90 个基因是候选直接靶标，不是全部由本文 ChIP 直接验证。
