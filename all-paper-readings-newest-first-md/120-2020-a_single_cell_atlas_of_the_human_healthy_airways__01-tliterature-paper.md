# 044. A Single-Cell Atlas of the Human Healthy Airways

## 基本信息
- 年份：2020
- 期刊：American Journal of Respiratory and Critical Care Medicine
- DOI：https://doi.org/10.1164/rccm.201911-2199OC
- PMID：32726565
- 主题：healthy airway atlas；anatomical gradient；epithelial reference；mucosal domain shift

## 为什么保留
这篇不是以 T cells 为核心，而是为 healthy human airways 提供解剖轴参考。它对后续 airway infection、mucosal immunity 和 tissue transfer algorithms 有价值，因为它证明同一器官系统内的 location 与 sampling method 已足以显著改变单细胞观测。

## 数据与研究设计
- 供者：10 名健康在世志愿者
- 样本：35 个 airway samples
- 解剖范围：nose 到 airway tree 12th division
- 细胞：77,969 个 10x 3' scRNA-seq profiles
- 细胞组成：89.1% epithelial、6.2% immune、4.7% stromal
- 采样：forceps biopsies 与 brushings，覆盖 nasal、tracheal/carina、intermediate bronchi、distal airway regions

## 主要贡献
1. 建立 healthy airway single-cell reference。
2. 描述鼻腔与 tracheobronchial airway 之间同类 epithelial cells 的 region-specific expression。
3. 细化 ionocytes、pulmonary neuroendocrine、brush/NREP-positive 与 KRT13-associated states。
4. 为 disease airway dataset 的 anatomical proxy 选择提供基线。

## 与 T 细胞和人群免疫力的关系
- 免疫细胞不是本文主体，但 healthy mucosal baseline 决定感染/炎症中的 T-cell context 如何解释。
- 对 population immunity，anatomical site 与 sampling method 是比普通 batch 更强的 confounder/biology mixture。

## 算法与分析视角
- 贡献主要是 anatomy-aware atlas design。
- 适合用于 site-aware integration、healthy-to-disease mapping、sampling-method robustness evaluation。
- 不含 TCR/V(D)J，也没有作者专用模型仓库。

## 数据可用性
- GEO：https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE143868
- GEO accession：`GSE143868`
- BioProject：`PRJNA601924`
- raw EGA study：`EGAS00001004082`
- raw EGA dataset：`EGAD00001005714`
- EGA 数据性质：35 samples；forceps 46,791 cells；brush biopsy 31,178 cells
- 作者专用代码：本轮未定位到

## 对新算法开发的启发
1. anatomy-aware batch correction
2. healthy-to-disease reference mapping with uncertainty
3. brush/biopsy recovery bias correction
4. epithelial-immune coupled mucosal baselines

## 可信度与边界
- 可信度：中高
- 强项：健康供者、明确 anatomy gradient、GEO 与 EGA accessions
- 边界：T-cell 相关性间接；主要是 epithelial atlas

## 一句话结论
`044` 是 airway tissue baseline，而不是 T-cell 算法论文；它保留的理由是为 site-aware mucosal single-cell modeling 提供真实基准。
