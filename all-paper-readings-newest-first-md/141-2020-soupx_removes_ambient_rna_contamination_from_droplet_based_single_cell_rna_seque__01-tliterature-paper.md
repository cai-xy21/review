# 020. SoupX removes ambient RNA contamination from droplet-based single-cell RNA sequencing data

## 基本信息
- 年份：2020
- 期刊：GigaScience
- DOI：https://doi.org/10.1093/gigascience/giaa151
- PMCID：`PMC7763177`
- 主题：ambient RNA；contamination fraction；droplet QC/decontamination

## 算法贡献
SoupX 把 droplet scRNA-seq 中的 cell-free mRNA contamination 显式建模为 background soup。算法先利用 empty droplets 构造 ambient expression profile，再估计细胞或 cluster 的 contamination fraction，最后对 observed counts 做污染校正。

## 输入/输出
- 输入：raw/unfiltered droplet count matrix、filtered cell matrix、cluster/marker 信息或自动估计所需表达信号。
- 输出：ambient profile、contamination estimate、adjusted expression counts。
- 关键假设：空滴能代表 soup；某些基因在目标细胞群中不应真实表达，因而可用于估计污染。

## 数据与代码
- 代码包：<https://github.com/constantAmateur/SoupX>
- 论文复现脚本：<https://github.com/constantAmateur/ambientRNA_paper>
- 论文还提供含代码和数据的 Docker reproduction image。
- 论文展示数据包含 mouse/human droplet examples 与 fetal liver 等环境污染明显的数据场景；该条目重点是 QC 算法，不是单一 T-cell cohort accession。

## 与 T 细胞-人群免疫力的关系
在 PBMC 或炎症样本中，高表达 cytokine、immunoglobulin、erythroid 或 lysis-derived transcripts 会把 T-cell state 注释推偏。SoupX 提供 ambient correction baseline，尤其影响 rare activated T-cell clusters 的 marker 解释。

## 新算法空间
1. ambient RNA 与 true paracrine activation signal 的区分。
2. donor/sample-aware soup profile 与跨批次污染传播建模。
3. RNA、ADT、V(D)J QC 的联合 contamination uncertainty。

## 一句话结论
`020` 是 ambient RNA 校正的核心基线，T-cell cohort 里应把它视为上游不确定性处理而不是机械去噪步骤。
