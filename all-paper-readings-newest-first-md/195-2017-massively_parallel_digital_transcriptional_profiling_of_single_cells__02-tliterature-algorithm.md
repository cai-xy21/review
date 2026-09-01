# Algorithm Report 015

## Paper
Massively parallel digital transcriptional profiling of single cells

## 基本信息
- 年份/期刊：2017, Nature Communications
- DOI：https://doi.org/10.1038/ncomms14049
- PMID/PMCID：28091601 / PMC5241818
- 原始数据：SRA `SRP073767`
- 数据入口：10x public datasets <https://support.10xgenomics.com/single-cell-gene-expression/datasets>
- 代码：68k PBMC 分析代码 <https://github.com/10XGenomics/single-cell-3prime-paper>

## 算法视角定位
这篇论文的主要贡献是高通量 droplet 3' scRNA-seq 平台和标准化计算管线，而不是一个下游统计模型。它的重要性在于把单细胞免疫数据推到数万到数十万细胞尺度，迫使后续算法处理 UMI sparsity、barcode calling、large-scale clustering、ambient RNA、doublets 和 batch/donor effects。

## 数据与任务
- 物种/样本：`Homo sapiens`
- 器官/组织：PBMC、bone marrow mononuclear cells；另有 cell-line/platform benchmark
- 规模：全文约 `250k` single cells across `29` samples；代表性免疫示例为 `68k` fresh PBMC
- 模态：3' UMI-based scRNA-seq
- 任务定义：从 FASTQ 生成 gene-cell UMI matrix，并在大规模 PBMC 上做自动聚类、marker annotation、稀有群体识别和 transplant chimerism 示例分析

## 核心方法结构
### 1. GemCode/10x droplet barcoding
- 每个 GEM/droplet 中包含单细胞、barcoded gel bead、反转录体系。
- cell barcode 标记细胞来源，UMI 标记分子来源。
- 该结构使 gene expression 可以被表示为 sparse gene-by-cell count matrix。

### 2. Cell Ranger 风格上游管线
- read alignment 到 reference transcriptome/genome。
- barcode correction 与 UMI collapsing。
- cell calling 区分真实细胞 barcode 与 empty droplet/background。
- 输出 raw/filtered feature-barcode matrix、QC metrics 和自动降维/聚类结果。

### 3. 大规模 PBMC 计算示例
- 68k PBMC 数据展示了大规模免疫细胞群体分解。
- 作者使用 Seurat 进行 log transform、variable gene selection、PCA、t-SNE 和 cluster annotation。
- 这组数据后来成为单细胞算法教程、benchmark 和 immune atlas 参考中的常见入口。

### 4. 转录组 SNV/chimerism 示例
- 论文还展示从 scRNA-seq reads 中提取 sequence variation，用于 bone marrow transplant donor/host chimerism 推断。
- 这不是 demuxlet 那样完整的人群 demultiplexing 工具，但提示了 genotype signal 可以作为 cell identity 的额外信息源。

## 输入、输出与模型意义
- 输入：FASTQ、cell barcode/UMI reads、reference transcriptome、sample metadata
- 输出：raw matrix、filtered matrix、barcode metrics、gene expression matrix、cluster/projection artifacts
- 模型结构：平台管线型算法；核心是 barcode/UMI error correction、molecule counting、cell calling 和自动聚类流程
- 方法意义：把后续算法的基本输入标准化为 sparse UMI count matrix

## 对新算法开发的贡献程度
- 直接下游算法创新：中等
- 数据规模/平台范式贡献：极高
- 对 T 细胞人群免疫研究贡献：高
- 综合评估：**P0/P1 级平台基础文献**

## 新算法空间
1. End-to-end QC uncertainty：cell calling、ambient RNA、doublet、UMI collapse 的不确定性传到下游 state analysis。
2. Rare T-cell state detection：在大规模稀疏矩阵中识别低频但重要的 activated/exhausted/antigen-experienced states。
3. Platform-aware integration：不同 10x chemistry、测序深度和样本处理差异的显式建模。
4. Population-scale preprocessing：在数百万细胞级别实现 streaming/incremental QC 与 annotation。

## 可纳入 method report 的一句话
10x/GemCode 平台论文让单细胞免疫算法从“小样本表达分析”转向“高通量 sparse UMI matrix 上的可扩展统计建模”，是后续 PBMC/T-cell atlas 与人群免疫队列的上游基础。
