# Algorithm Report 094

## Paper
Landscape of stimulation-responsive chromatin across diverse human immune cells

## 题录与实现
- DOI：https://doi.org/10.1038/s41588-019-0505-9
- PMID：31548721
- Code/Tool：No single standalone algorithm package is central；data portal and supplementary resources support genome-browser-level reuse。

## 算法视角定位
本文的算法定位是：sorted-cell ATAC-seq/RNA-seq atlas, differential accessibility, TF footprint/motif analysis, stimulation-responsive regulatory region annotation and GWAS overlap。。在 81-94 这一组文献中，它用于补齐 TCR/BCR tooling、sample multiplexing、多模态测量、RNA/ATAC regulatory inference 或 GWAS-to-cell-state enrichment 的方法拼图。

## 输入与输出
- 输入：Human PBMC-derived/sorted immune cell populations and fetal thymus cell types；resting and stimulated conditions for up to 32 immune cell populations；GEO ATAC-seq GSE118189，RNA-seq GSE118165，ChIP-seq GSE126505；portal at Stanford/Pritchard lab。
- 计算输入抽象：feature-by-cell matrix、barcode count matrix、peak/activity matrix、GWAS loci 或 paired multimodal single-cell object。
- 输出：sample labels、doublet calls、multimodal cell object、regulatory programs、chromatin-potential scores、GWAS enrichment statistics 或 state-specific annotations。

## 核心算法贡献
### 1. 数据结构/实验设计转化为算法对象
该论文把原本难以统一分析的实验读出转化为矩阵、metadata、graph 或 enrichment test，使其可以进入 Scanpy/Seurat/Signac/scverse 或专门统计工具。

### 2. 关键计算模块
sorted-cell ATAC-seq/RNA-seq atlas, differential accessibility, TF footprint/motif analysis, stimulation-responsive regulatory region annotation and GWAS overlap。 这是该条目最值得在 method report 中展开的算法或 workflow 层贡献。

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
- DOI 和 accession：https://doi.org/10.1038/s41588-019-0505-9；Human PBMC-derived/sorted immune cell populations and fetal thymus cell types；resting and stimulated conditions for up to 32 immune cell populations；GEO ATAC-seq GSE118189，RNA-seq GSE118165，ChIP-seq GSE126505；portal at Stanford/Pritchard lab。
- 代码：No single standalone algorithm package is central；data portal and supplementary resources support genome-browser-level reuse。
- 复用判断：适合纳入 algorithm report；若 raw human data 位于 EGA/dbGaP，需要权限申请，processed matrices/示例对象更适合作为快速 benchmark。
