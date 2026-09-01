# Algorithm Report 086

## Paper
MULTI-seq: sample multiplexing for single-cell RNA sequencing using lipid-tagged indices

## 题录与实现
- DOI：https://doi.org/10.1038/s41592-019-0433-8
- PMID：31209384
- Code/Tool：MULTI-seq demultiplexing scripts/workflow commonly distributed through Gartner lab/GitHub resources；算法可复现于 barcode count matrix。

## 算法视角定位
本文的算法定位是：lipid-modified oligo barcode labeling + barcode UMI matrix thresholding/classification + singlet/doublet/negative calls。在 81-94 这一组文献中，它用于补齐 TCR/BCR tooling、sample multiplexing、多模态测量、RNA/ATAC regulatory inference 或 GWAS-to-cell-state enrichment 的方法拼图。

## 输入与输出
- 输入：人和鼠细胞/组织示例；Nature 论文页显示 mouse immune-cell 示例 n=10,427 cells；原文提供 source data 和 supplementary protocol。
- 计算输入抽象：feature-by-cell matrix、barcode count matrix、peak/activity matrix、GWAS loci 或 paired multimodal single-cell object。
- 输出：sample labels、doublet calls、multimodal cell object、regulatory programs、chromatin-potential scores、GWAS enrichment statistics 或 state-specific annotations。

## 核心算法贡献
### 1. 数据结构/实验设计转化为算法对象
该论文把原本难以统一分析的实验读出转化为矩阵、metadata、graph 或 enrichment test，使其可以进入 Scanpy/Seurat/Signac/scverse 或专门统计工具。

### 2. 关键计算模块
lipid-modified oligo barcode labeling + barcode UMI matrix thresholding/classification + singlet/doublet/negative calls 这是该条目最值得在 method report 中展开的算法或 workflow 层贡献。

### 3. 对 T 细胞/人群免疫的建模价值
- 可把 donor、sample、activation、clonotype、protein phenotype、chromatin accessibility 或 GWAS risk variants 变成可建模变量。
- 对 T cell immunity 的意义不是只提高可视化，而是为 immune state prediction、clone-state coupling 和 variant-to-function mapping 提供输入。

## 新算法贡献程度评估
- 直接算法/方法创新：中到高。
- 数据/benchmark 价值：中到高。
- 对我们新算法选题启发：高。

## 局限
- 多数条目仍是 measurement/workflow 或 enrichment layer，尚未统一建模 donor hierarchy、TCR sequence、HLA、multimodal phenotype 和 clinical outcome。
- 多模态数据常有缺失、噪声结构不同、feature 空间不一致的问题。
- 人群遗传和单细胞状态之间多为富集或关联，距离因果/预测模型仍有空间。

## 可发展的方向
1. Donor-aware multimodal VAE/graph model。
2. Clone-aware RNA/protein/ATAC joint representation。
3. Stimulation-aware regulatory trajectory model。
4. GWAS/HLA-to-T-cell-state prediction with uncertainty。
5. Multiplexing-aware doublet/batch/ambient correction unified model。

## 数据可用性评估
- DOI 和 accession：https://doi.org/10.1038/s41592-019-0433-8；人和鼠细胞/组织示例；Nature 论文页显示 mouse immune-cell 示例 n=10,427 cells；原文提供 source data 和 supplementary protocol。
- 代码：MULTI-seq demultiplexing scripts/workflow commonly distributed through Gartner lab/GitHub resources；算法可复现于 barcode count matrix。
- 复用判断：适合纳入 algorithm report；若 raw human data 位于 EGA/dbGaP，需要权限申请，processed matrices/示例对象更适合作为快速 benchmark。
