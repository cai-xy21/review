# 015. Massively parallel digital transcriptional profiling of single cells

## 基本信息
- 年份：2017
- 期刊：Nature Communications
- DOI：https://doi.org/10.1038/ncomms14049
- PMID/PMCID：`28091601` / `PMC5241818`
- 主题：10x/GemCode droplet scRNA-seq；Cell Ranger；large immune population profiling

## 文章定位
这篇文章把高通量商业 droplet scRNA-seq 平台和自动化 count pipeline 推到可大规模复用的层面。对 PBMC/T-cell cohort 而言，它不是直接提出下游免疫算法，而是决定了后续算法面对的数据形态：UMI count matrix、barcode calling、自动聚类和数万细胞尺度。

## 核心贡献
1. 描述可在一次实验中对数万细胞做 3' digital counting 的 GEM/Gel bead workflow。
2. 给出平台级灵敏度、稀有群体检测与 PBMC 大规模 profiling benchmark。
3. 展示用转录组 sequence variation 做 transplant donor/host chimerism 识别的分析例子。

## 与 T 细胞-人群免疫力的关系
- 论文直接测了约 68k PBMC，成为后续 immune atlas、T-cell state annotation 和大规模预训练/benchmark 的常用数据形态。
- 它说明人群免疫算法必须同时处理平台化测量误差、cell calling、UMI sparsity 和大规模计算约束。

## 数据与代码
- 论文报告约 250k single cells across 29 samples；免疫示例为约 68k human PBMC。
- 物种/组织：以 `Homo sapiens` PBMC、bone marrow mononuclear cells 为关键展示场景，另含 cell-line/platform benchmark。
- 数据获取：SRA `SRP073767`；10x public datasets 入口 <https://support.10xgenomics.com/single-cell-gene-expression/datasets>；论文代码 <https://github.com/10XGenomics/single-cell-3prime-paper>。
- 代码/管线：Cell Ranger。
- 管线输入：FASTQ、barcode/UMI reads、reference transcriptome。
- 管线输出：filtered/raw feature-barcode matrices、barcode metrics、clustering/projection artifacts。
- 模型意义：把 barcode assignment、UMI collapsing、gene counting 和后续 large-scale clustering 的上游标准化为平台流水线。

## 新算法空间
1. 平台偏差与 donor effect 联合校正。
2. rare T-cell state 在高吞吐稀疏矩阵中的稳健发现。
3. cell calling、ambient RNA、doublet、downstream uncertainty 的端到端传播。

## 一句话结论
`015` 是人群免疫单细胞算法的上游平台基石，方法报告里应把它写成“数据生成范式改变算法问题”的论文。
