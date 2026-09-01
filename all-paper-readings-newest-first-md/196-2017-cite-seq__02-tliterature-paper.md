# 013. Simultaneous epitope and transcriptome measurement in single cells

## 基本信息
- 年份：2017
- 期刊：Nature Methods
- DOI：https://doi.org/10.1038/nmeth.4380
- PMID/PMCID：`28759029` / `PMC5669064`
- 主题：CITE-seq；多模态单细胞；蛋白+RNA 联合测量
- 可信度：高

## 文章定位
这篇文章是单细胞免疫组学方法史上的里程碑。它首次把表面蛋白信息和 scRNA-seq 放进同一次单细胞实验，使研究者不必在“转录状态”和“免疫表型”之间二选一。

## 亮点
1. 提出 CITE-seq，用寡核苷酸标记抗体实现蛋白与转录组同步测量。
2. 对免疫细胞研究影响极大，因为 T 细胞很多关键亚群只靠 RNA 很难稳定区分。
3. 打开了后来 totalVI、WNN、ECCITE-seq 等多模态算法和实验路线。

## 核心贡献
- 把单细胞实验从 transcriptome-only 推进到 multimodal profiling。
- 大幅提升了免疫细胞状态定义精度，特别是淋巴细胞与活化状态分析。
- 为后续 CITE-seq 生态系统和联合建模算法奠定实验基础。

## 与 T 细胞—人群免疫力的关系
T 细胞亚群通常依赖 CD45RA、CCR7、CD27、PD-1、TIGIT 等表面标志物与 RNA 共同定义。CITE-seq 让 T-cell state 在人群队列中更可解释、更稳定。

## 算法/分析贡献
- 主要贡献是实验方法本身，但它直接定义了新的计算任务：RNA+protein 联合分析。
- 后续多模态建模几乎都以它为起点。
- 对方法论文来说，它的重要性不亚于一篇核心算法论文，因为它重塑了数据结构。
- 数据结构上，RNA UMI matrix 与 antibody-derived tag matrix 共享 cell barcode；后续算法必须同时处理 transcript dropout、ADT 背景、蛋白/RNA 动态范围不一致和多模态邻域构图。

## 对新算法开发的启发
1. 多模态联合表示学习
2. 蛋白背景噪声建模
3. donor-aware multimodal immune profiling

## 数据可用性
- GEO：`GSE100866`
- 数据性质：
  - `Homo sapiens` PBMC/cord blood mononuclear cell 免疫细胞与人/鼠混合实验
  - 代表性 CBMC CITE-seq 数据约 8k 细胞，含 RNA UMI 与 ADT UMI
  - 适合 RNA-protein joint normalization、cell-state annotation 和 modality-denoising benchmark
- 代码/实现：
  - 原论文核心贡献是实验 protocol；后续常用计算实现见 Seurat CITE-seq workflow 与社区工具
  - 原始数据链接：<https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE100866>
- 复用价值：极高

## 影响因子/可信度综合判断
- 期刊层面：Nature Methods
- 社区采用度：极高
- 方法稳定性：高
- 综合判断：**高可信度、基础性方法论文**

## 一句话结论
CITE-seq 是 T 细胞单细胞研究进入“真正多模态时代”的起点。
