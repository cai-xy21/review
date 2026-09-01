# 084. Integrated analysis of multimodal single-cell data

## 基本信息
- 年份：2021
- 期刊：Cell
- DOI：https://doi.org/10.1016/j.cell.2021.04.048
- PMID：34062119
- 方法：Weighted Nearest Neighbor (WNN) analysis；Seurat v4 multimodal integration
- 代码/工具：<https://satijalab.org/seurat>；<https://github.com/satijalab/seurat>
- 主题：RNA-protein multimodal analysis；CITE-seq；single-cell multiome；modality weighting；reference mapping

## 为什么重要
这篇是 Seurat v4/WNN 的核心方法论文。它对人群免疫单细胞研究尤其重要，因为 PBMC 和 T cell atlas 常同时包含 RNA、ADT、ATAC 或 V(D)J。WNN 的核心思想是：不同细胞、不同状态下，某一模态的信息量不同，因此不能简单拼接特征或固定权重整合，而要为每个细胞学习 modality-specific neighborhood contribution。

## 数据与研究设计
- 数据性质：方法论文 + 多个多模态示例数据。
- 关键免疫数据：约 211,000 human PBMC CITE-seq reference cells，抗体 panel 可达 228 个 surface proteins。
- 其他示例：RNA+ATAC multiome、跨模态 reference mapping 等。
- 物种/器官：主要示例包括 Homo sapiens PBMC；另含其他公开多模态数据。
- 公开获取：SeuratData/Seurat vignettes 与作者资源提供可复用对象；原始数据多来自配套/公开数据门户和 10x/Cell datasets。

## 核心亮点
1. 提出 WNN：按 cell-specific modality utility 学习 RNA、protein、ATAC 等模态在邻域图中的贡献。
2. 把多模态整合从 feature concatenation 推进到 graph-level adaptive integration。
3. 在 PBMC CITE-seq 中展示 RNA 和 protein 对不同细胞类型/状态的信息量差异。
4. 成为后续 Seurat multimodal workflow 的标准入口。

## 文章中的算法/分析流程
### 1. 单模态预处理
- RNA 通常经过 normalization、variable feature selection、PCA。
- ADT/protein 通常经过 CLR normalization 和 PCA/降维。
- ATAC 可用 LSI 或 gene activity 等表示。

### 2. modality-specific nearest neighbors
- 对每个模态分别计算低维空间和 nearest-neighbor graph。
- 每个细胞在每个模态中都有一组邻居和局部结构。

### 3. cell-specific modality weights
- WNN 评估某个细胞在各模态空间中邻居的一致性和预测能力，为每个细胞估计 RNA weight、protein weight 等。
- 输出不是全局固定权重，而是每个细胞自己的 modality weight。

### 4. weighted nearest-neighbor graph
- 将各模态邻域按 cell-specific weights 融合成 WNN graph。
- 下游 UMAP、clustering、label transfer 都在这个 graph 上运行。

## 代码输入、输出、模型结构和意义
- 输入：Seurat object；RNA assay；ADT assay；ATAC assay 或其他模态的降维结果。
- 输出：modality weights、WNN graph、weighted UMAP、multimodal clusters、reference mapping labels。
- 模型结构：graph-based adaptive integration；不是概率生成模型。
- 意义：能识别“哪个模态对哪个细胞更有用”，非常适合免疫细胞状态细分。

## 与 T 细胞—人群免疫力的关系
- T cell naive/memory/effector/exhausted/Treg 等状态常需要 RNA 与 surface protein 联合区分。
- WNN 可减少单一模态误注释，提升 PBMC/CITE-seq reference 的分辨率。
- 对人群免疫力研究，WNN 是 donor-aware 模型前的重要多模态表征步骤，但自身不显式建模 donor hierarchy、TCR clonotype 或 clinical outcome。

## 对新算法贡献程度
- 直接算法创新：高。
- 数据资源价值：高，尤其是大规模 PBMC CITE-seq reference。
- 对新算法启发：很高。它说明模态贡献应是 cell/state-specific，而不是固定参数。

## 数据可用性
- 主要代码：Seurat v4 WNN workflow，<https://github.com/satijalab/seurat>
- 数据入口：SeuratData/vignettes；PBMC multimodal reference 可通过 Seurat/作者教程获取。
- 数据性质：人 PBMC CITE-seq，约 211k cells，最多 228 surface proteins；另含 RNA+ATAC 示例。
- 复现性评价：软件和教程成熟；不同示例的 raw accession 需按具体数据源回溯。

## 一句话结论
WNN 是多模态单细胞整合的关键算法，它用 cell-specific modality weights 构建融合邻域图；后续新算法空间在于把这种自适应模态权重扩展到 donor、TCR/BCR、HLA 和免疫结局层。
