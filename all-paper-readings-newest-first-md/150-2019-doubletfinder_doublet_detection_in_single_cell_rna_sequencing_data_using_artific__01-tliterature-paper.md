# 019. DoubletFinder: Doublet detection in single-cell RNA sequencing data using artificial nearest neighbors

## 基本信息
- 年份：2019
- 期刊：Cell Systems
- DOI：https://doi.org/10.1016/j.cels.2019.03.003
- PMID/PMCID：`30954475` / `PMC6853612`
- 主题：QC；doublet detection；artificial nearest neighbors；Seurat workflow

## 算法贡献
DoubletFinder 以真实细胞的 count profile 随机配对生成 artificial doublets，把真实与人工 doublets 一起投到表达邻域中，再以每个真实细胞邻域里 artificial neighbors 的比例 `pANN` 给出 doublet ranking。其核心不是外部 hashing/genotype 标签，而是用表达空间中“像人工 doublet”的程度做判别。

## 输入/输出
- 输入：预处理后的 Seurat object、raw UMI matrix、expected doublet count、`pN`、`pK` 和选用 PCs。
- 输出：每个细胞的 `pANN`、singlet/doublet classification、parameter sweep/BCmvn 诊断。
- 局限：对 heterotypic doublets 更强；homotypic doublets 与真实细胞状态更难分。

## 数据与代码
- 代码：<https://github.com/chris-mcginnis-ucsf/DoubletFinder>
- 论文验证依赖已有 PBMC ground-truth 场景，包括 demuxlet 与 Cell Hashing。
- 关键相关数据入口：
  - demuxlet PBMC：`GSE96583`
  - Cell Hashing：`GSE108313`
- 复用意义：适合作为 QC baseline，但免疫活化中间态和双细胞 artifact 的边界仍需谨慎解释。

## 与 T 细胞-人群免疫力的关系
T-cell activation continuum、T/NK 边界和 rare effector-like clusters 都可能被 doublets 污染。DoubletFinder 是 cohort-scale T-cell atlas 中常见的第一道表达型 QC，但其标签不应被当作无误差真值。

## 新算法空间
1. donor/genotype/hash/TCR 与 expression doublet evidence 联合。
2. homotypic immune doublet 与真实 transitory state 的不确定性输出。
3. doublet probability 向 clustering、DE、trajectory 传播。

## 一句话结论
`019` 是表达矩阵 doublet QC 的标准基线论文，方法报告里要同时写它的人工邻域思想与 homotypic 局限。
