# Algorithm Report 084

## Paper
Integrated analysis of multimodal single-cell data

## 题录与实现
- DOI：https://doi.org/10.1016/j.cell.2021.04.048
- PMID：34062119
- Method：Weighted Nearest Neighbor (WNN)
- Code：<https://github.com/satijalab/seurat>

## 算法视角定位
WNN 是 graph-level multimodal integration algorithm。它不是把 RNA、ADT、ATAC 简单拼接，而是为每个细胞估计各模态对局部邻域结构的贡献，再构建 weighted nearest-neighbor graph。

## 输入与输出
- 输入：Seurat object 中多个 assay 或降维表示，例如 RNA PCA、ADT PCA、ATAC LSI。
- 输出：cell-specific modality weights、WNN graph、weighted UMAP、multimodal clusters、reference mapping。

## 核心算法贡献
### 1. modality-specific neighbor graph
先在每个模态独立计算邻域结构，保留 RNA/protein/ATAC 各自的信号。

### 2. cell-specific modality weighting
对每个细胞估计不同模态能否更好解释其局部邻居关系。输出权重随细胞和状态变化，而非全局常数。

### 3. weighted graph fusion
把单模态邻域按权重融合，形成 WNN graph，并用于聚类、可视化和 label transfer。

## 新算法贡献程度
- 直接算法创新：高。
- 多模态免疫分析价值：很高。
- 数据资源价值：高，尤其是 PBMC CITE-seq reference。

## 局限与机会
- WNN 是局部图整合，不是显式 likelihood model。
- donor、batch、TCR/BCR、clinical outcome 仍作为 metadata 后处理。
- 可扩展方向：donor-aware WNN、clone-aware WNN、uncertainty-calibrated modality weights。

## 数据可用性评估
- PBMC CITE-seq reference：约 211,000 human PBMC cells，最多 228 protein markers。
- 代码：Seurat v4/v5。
- 复用性：很高；raw accession 需按具体 vignette 数据源回溯。
