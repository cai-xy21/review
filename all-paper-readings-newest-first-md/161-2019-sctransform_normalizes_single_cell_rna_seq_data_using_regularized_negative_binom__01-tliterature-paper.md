# 018. Normalization and variance stabilization of single-cell RNA-seq data using regularized negative binomial regression

## 基本信息
- 年份：2019
- 期刊：Genome Biology
- DOI：https://doi.org/10.1186/s13059-019-1874-1
- PMID/PMCID：`31870423` / `PMC6927181`
- 目录更正：原目录把工具名写进标题；正式题名以本页标题为准
- 主题：SCTransform；normalization；variance stabilization；regularized negative-binomial regression

## 算法贡献
SCTransform 不再把 normalization 主要写成 library-size scaling 加 log transform，而是对每个基因拟合以 sequencing depth 为协变量的 NB regression，再用 Pearson residual 作为 variance-stabilized 表示。其关键点是跨相近丰度基因做参数正则化，避免低表达基因的 per-gene NB fit 过拟合。

## 输入/输出
- 输入：UMI count matrix，可附加待回归 technical covariates。
- 输出：Pearson residual 表示、corrected counts/assay 层、variable-feature ranking 与供 PCA/聚类/整合使用的标准化矩阵。
- 实现：`sctransform` R package，Seurat 中有直接接口。

## 与 T 细胞-人群免疫力的关系
PBMC/T-cell cohort 的 cell depth、RNA content 与 activation state 往往相互纠缠。SCTransform 是很多下游 state discovery、integration 和 differential testing 的前处理分界点，因此其归一化假设会影响稀有 T-cell state 是否被保留。

## 数据可用性
- 论文示例含 10x human PBMC 数据，图 1 报告 33,148 PBMC。
- 该论文重点是通用预处理模型，示例数据主要来自公开 benchmark，而非单一新 cohort accession。
- 代码：<https://github.com/satijalab/sctransform>

## 新算法空间
1. normalization 与 ambient RNA/doublet uncertainty 联合建模。
2. 在 donor-aware immune cohort 中区分技术 depth 与真实 activation-linked RNA content。
3. RNA、ADT、TCR 多模态前处理的一致不确定性表达。

## 一句话结论
`018` 是单细胞预处理算法必须写透的基础论文，尤其要说明 Pearson residual 表示改变了下游 T-cell state 分析的统计入口。
