# 016. Comprehensive Integration of Single-Cell Data

## 基本信息
- 年份：2019
- 期刊：Cell
- DOI：https://doi.org/10.1016/j.cell.2019.05.031
- PMID：`31178118`
- 主题：Seurat v3；anchor-based integration；reference mapping
- 可信度：高

## 文章定位
这篇文章是单细胞整合分析的分水岭之一。对人群免疫力研究尤其关键，因为真实世界数据几乎总是跨 donor、跨批次、跨组织、跨平台的，没有整合方法就很难做 atlas 级研究。

## 亮点
1. 提出 anchor-based integration，成为单细胞数据整合的主流路线之一。
2. 除了批次校正，还强调 reference mapping 与 label transfer。
3. 让免疫图谱构建、跨研究整合和公共 reference 复用变得可行。

## 核心贡献
- 给出了统一的整合框架，用于不同数据集间共享结构识别。
- 把参考图谱映射变成实际可落地的分析流程。
- 极大推动了单细胞免疫 atlas 的可比性与可复用性。

## 与 T 细胞—人群免疫力的关系
T 细胞数据常高度受 donor、组织和激活状态影响。Seurat integration 让跨研究比较 T-cell states 成为可能，是人群免疫图谱的基础工具。

## 算法/分析贡献
- 核心是 anchor-based integration 和 label transfer。
- 它不是最深的生成模型，但在实用性和普及度上极强。
- 对方法综述来说，它代表“reference-centric integration”范式。
- 输入通常是多个预处理后的 single-cell objects/embeddings 与共享 features；输出是 anchors、integrated assay/embedding、transferred labels 或跨模态预测值。

## 对新算法开发的启发
1. 从 anchor 到 probabilistic / donor-aware integration
2. 从 cell mapping 到 donor representation mapping
3. 加入不确定性与跨模态扩展

## 数据可用性
- 代码：Seurat，<https://github.com/satijalab/seurat>
- 数据：论文主要复用公开数据集做 cross-technology、cross-modality 与 spatial-reference integration；不是单一新 cohort accession 型论文
- 可复现实操入口：Seurat integration/reference mapping tutorials 与 `SeuratData` 示例数据
- 社区生态成熟
- 复用价值：极高

## 影响因子/可信度综合判断
- 期刊层面：Cell
- 社区采用度：极高
- 算法影响力：极高
- 综合判断：**高可信度、核心整合方法论文**

## 一句话结论
如果你的方法报告要写“单细胞整合方法的主干脉络”，Seurat v3 必须重点写。
