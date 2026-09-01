# 《Pan-cancer single-cell landscape of tumor-infiltrating T cells》精读

## 论文信息

- **作者**：Zheng L, Qin S, Si W, et al.
- **期刊与年份**：Science, 2021
- **DOI**：[10.1126/science.abe6474](https://doi.org/10.1126/science.abe6474)
- **原文**：[Science](https://www.science.org/doi/10.1126/science.abe6474)
- **PubMed**：[PMID 34914499](https://pubmed.ncbi.nlm.nih.gov/34914499/)
- **新测数据**：[GSE156728](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE156728) / [PRJNA658913](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA658913)
- **完整整合对象与代码**：[Zenodo 10.5281/zenodo.5461803](https://doi.org/10.5281/zenodo.5461803)
- **历史交互门户**：[PanC_T](http://cancer-pku.cn:3838/PanC_T/)
- **本报告核对日期**：2026-08-23

## 一句话结论

本文整合 21 种癌症、316 名供体/患者的 397,810 个高质量 T 细胞，建立 17 个 CD8 与 24 个 CD4 meta-cluster 的泛癌图谱，并结合 TCR、RNA velocity、转录因子和临床结局揭示多条通向耗竭的路径及肿瘤反应性 CD4 状态，为跨癌种定义可导航 T 细胞坐标系提供了标志性资源。

## 数据护照

| 项目 | 内容 |
|---|---|
| 覆盖 | 21 种癌症；316 名患者/供体；整合新测与既往公开数据 |
| 核心规模 | 397,810 个高质量 T 细胞 |
| 状态图谱 | 17 个 CD8 meta-clusters；24 个 CD4 meta-clusters |
| 模态 | scRNA；大量细胞配套 TCR；部分 10x 数据支持 RNA velocity；配套 bulk/WES/临床信息依队列而异 |
| 主要结论 | 多条 CD8 耗竭路径；TOX/PRDM1 泛癌关联；SOX4/FOXP3 等路径偏好；CD4 肿瘤反应性候选状态 |
| 新测数据入口 | GSE156728/PRJNA658913，只覆盖本研究新生成队列，不是 397,810 全部细胞 |
| 全量再分析入口 | Zenodo 23.5 GB，含表达、元数据、TCR、velocity、外部数据和代码 |
| 数据格式 | RDS 为主：SingleCellExperiment、Seurat、TCR/metadata R objects |

## 1. 研究问题

跨癌种能否建立统一的 TIL T 细胞状态参考？CD8 耗竭是否只有一条线性路线？哪些转录因子是共享或路径偏好的候选驱动？CD4 中哪些状态更可能富集肿瘤反应性克隆？状态构成能否对患者分型和预后提供信息？

## 2. 方法框架

1. 汇集本研究新测 10x/Smart-seq2 数据和多篇已发表人癌单细胞数据。
2. 对 CD8、CD4 分开做跨队列整合和 meta-clustering。
3. 用标志基因、组织富集、TCR 克隆扩增与共享、RNA velocity 标注状态关系。
4. 分析 regulon/转录因子与分化路径，提出多条耗竭路线。
5. 将状态频率用于患者免疫分型，并与临床结局相关联。

## 3. 数据规模与图谱组成

### 3.1 核心整合图谱

| 层级 | 规模 | 说明 |
|---|---:|---|
| 癌种 | 21 | 跨实体瘤与血液/淋巴相关肿瘤背景，具体队列技术不完全一致 |
| 患者/供体 | 316 | 包含新测与公开数据；同一供体可能贡献多个样本 |
| 高质量 T 细胞 | 397,810 | 论文整合 atlas 的核心口径 |
| CD8 meta-clusters | 17 | 初始/记忆、效应、驻留、NK-like、Tc17、Treg-like、耗竭等 |
| CD4 meta-clusters | 24 | 初始/记忆、Th、Tfh、Treg、IFNG+ 等状态 |

图谱不是一个统一实验批次：不同队列使用 10x 3′/5′、Smart-seq2 等平台，组织解离、分选、临床阶段和治疗背景均不同。整合模型提供共同坐标，但细胞比例比较必须控制数据集/患者效应。

### 3.2 多条 CD8 耗竭路径

作者提出主要路径可经 effector-memory 与 tissue-resident-memory 相关状态走向 terminal exhaustion；另有较少见的 KIR+ NK-like、Tc17 和 CD8 Treg-like 路线。TOX 与 PRDM1 在多癌种耗竭程序中较普遍，SOX4、FOXP3 等对特定路径更偏好。这里的“路径”综合表达邻近、TCR 共享和 velocity 等证据，仍不是直接的单细胞时间序列。

### 3.3 CD4 图谱与患者分型

CD4 中 IFNG+ TFH/TH1 和 TNFRSF9+ Treg 被提出为肿瘤反应性候选状态。基于状态构成，作者区分 Tex-high/Trm-low 与 Tex-low/Trm-high 等免疫类型；后者在相应分析中表现出更好的生存关联。该分型是队列关联，不能直接作为所有癌种统一临床阈值。

### 3.4 GSE156728：只包含新测部分

GSE156728 有 **48 个 GEO 样本记录**、21 个系列级补充文件，总体约 **0.34 GB**。主要为不同癌种/组织的 CD4 与 CD8 10x 表达对象：

| 组别 | CD4 文件约大小 | CD8 文件约大小 |
|---|---:|---:|
| BCL | 8.02 MB | 6.82 MB |
| BC | 5.98 MB | 7.79 MB |
| ESCA | 24.28 MB | 21.14 MB |
| MM | 6.63 MB | 15.85 MB |
| PACA | 7.40 MB | 10.40 MB |
| RC | 16.56 MB | 26.78 MB |
| THCA | 41.43 MB | 55.58 MB |
| UCEC | 22.95 MB | 34.24 MB |

另有：

- `10X_VDJ.merge` 约 **10.64 MB**；
- `RAW.tar` 约 **15.51 MB**；
- bulk exome mutation 约 **4.87 MB**；
- bulk RNA counts 约 **4.39 MB**；
- 单细胞 metadata 约 **1.27 MB**。

这些文件是本研究新生成的子队列，**不是完整 397,810 细胞整合图谱**。若只下载 GSE156728，无法重建全部 21 癌种 reference。

### 3.5 Zenodo 完整资源包

Zenodo 版本 **v20210906** 总大小约 **23.5 GB**，是复现整合图谱的优先入口：

| 归档 | 大小 | 内容 |
|---|---:|---|
| `data.expression.tar.gz` | 14.0 GB | 各数据集 SingleCellExperiment、CD4/CD8 integration 和 Seurat 对象 |
| `data.external.tar.gz` | 5.2 GB | 论文复用/比较的外部数据 |
| `data.velo.tar.gz` | 4.2 GB | RNA velocity 所需对象/结果 |
| `data.metaInfo.tar.gz` | 60.5 MB | 逐细胞、样本和患者元数据及状态频率 |
| `data.tcr.tar.gz` | 16.8 MB | cell-level TCR/clonotype 数据 |
| `code.tar.gz` | 31.9 MB | 主要分析代码 |

关键解包路径包括：

```text
data/expression/CD4/integration/int.CD4.S35.sce.merged.rds
data/expression/CD8/integration/int.CD8.S35.sce.merged.rds
data/expression/CD4/byDataset/*.sce.rds
data/expression/CD8/byDataset/*.sce.rds
data/expression/CD4/integration/CD4.thisStudy_10X.seu.rds
data/expression/CD8/integration/CD8.thisStudy_10X.seu.rds
data/expression/zhangLab/S.10X.sce.rds
data/expression/zhangLab/BCL.10X.sce.rds
data/expression/zhangLab/MM.10X.sce.rds
data/expression/zhangLab/S3.SS2.sce.rds
data/tcr/byCell/tcr.zhangLab.comb.flt.rds
data/metaInfo/panC.freq.all.meta.tb.rds
```

### 3.6 按目的选择数据与读取

| 目的 | 推荐下载 | 说明 |
|---|---|---|
| 浏览新测样本 | GSE156728 | 体量小，但覆盖不完整 |
| 复现 397,810 atlas | Zenodo expression + metaInfo | 至少约 14 GB 压缩数据，解压后更大 |
| TCR—状态分析 | Zenodo tcr + expression + metaInfo | 以 cell ID 连接，注意不同数据集 TCR 覆盖率 |
| RNA velocity | Zenodo velo + expression | 只对有 spliced/unspliced 的相应数据集适用 |
| 完整代码复现 | Zenodo code | 需匹配旧版 R/Bioconductor/Seurat 环境 |

```powershell
# 从 Zenodo 页面获取每个文件的当前下载链接后，可分卷下载；建议校验 md5。
curl.exe -L -C - -o data.metaInfo.tar.gz `
  "https://zenodo.org/records/5461803/files/data.metaInfo.tar.gz?download=1"
```

```r
library(SingleCellExperiment)
sce <- readRDS("data/expression/CD8/integration/int.CD8.S35.sce.merged.rds")
sce
```

## 4. TCR、velocity 与状态方向

TCR 共享提供克隆亲缘，velocity 提供局部转录动力趋势，两者结合比单独 pseudotime 更强。但 velocity 依赖剪接动力学假设，不同癌种/平台的质量不一；TCR 共享也没有方向。多证据一致时可称“支持路径”，不宜写成已直接观察到的确定谱系。

## 5. 候选分子驱动

TOX、PRDM1 是跨癌种耗竭相关候选调控因子；SOX4、FOXP3 等显示路径偏好。论文以调控网络、表达和状态关联为主，并非在全部癌种逐一做遗传扰动。因此这些分子适合作为后续 perturb-seq/CRISPR 的候选，不是泛癌因果定论。

## 6. 主要生物学发现

- 人肿瘤 TIL 可映射到共享但比例不同的泛癌状态坐标。
- CD8 耗竭存在主路径和多个少见路径，而非单一线性终点。
- CD4 中存在更可能富集肿瘤反应性的 IFNG+ TFH/TH1 与 TNFRSF9+ Treg。
- 状态组合可以进行患者免疫分型并关联生存。

## 7. 对细胞治疗导航的价值

该资源可作为工程 T 细胞的参考坐标：将制造前、制造中和回输后的细胞投射到 41 个 CD4/CD8 meta-cluster，监控是否偏向可持续 memory/Trm 还是 terminal Tex。更进一步，可把 TOX/PRDM1 等作为扰动轴，把状态频率用作优化目标。

## 8. 推荐精读图

- 泛癌数据来源和 397,810 细胞整合总览。
- 17 CD8、24 CD4 meta-cluster 图。
- 多条 CD8 耗竭路径及 TCR/velocity 支持图。
- 患者免疫分型与生存关联图。

## 9. 方法学创新

1. 以近 40 万 T 细胞建立跨 21 癌种统一坐标。
2. 联合 TCR、velocity 和 regulon，而非仅做批次整合。
3. 数据包同时发布逐数据集对象、整合对象、元数据、TCR、velocity 和代码。

## 10. 局限性

- 公共队列平台、临床阶段和组织处理高度异质。
- 并非全部细胞都有 TCR、velocity 或临床结局。
- 批次校正可能削弱真实癌种差异，也可能残留技术差异。
- 许多“驱动因子”是推断候选，缺少泛癌系统扰动验证。
- 历史交互门户当前可用性可能变化，长期复现应依赖 Zenodo/GEO。

## 11. 在综述架构中的位置

这是“**charting T cell molecular landscape by single-cell omics**”的核心泛癌 reference，也能自然引出“**build real-time optimization systems**”：先建立稳定 reference state space，再把连续采样的细胞映射到该空间并根据目标状态闭环调整培养/刺激条件。

## 12. 可直接写入综述的表述

> Zheng 等整合 21 种癌症、316 名供体的 397,810 个 T 细胞，建立 17 个 CD8 与 24 个 CD4 meta-cluster，并联合 TCR、RNA velocity 和调控网络提出多条耗竭路径；其 23.5 GB Zenodo 包提供表达、元数据、TCR、velocity 和代码，是泛癌 T 细胞状态导航的重要参考坐标。

## 13. 避免误读

- GSE156728 只含新测子队列，不是完整 397,810 细胞 atlas。
- 41 个 meta-cluster 是 CD8 17 + CD4 24，不能当作互斥谱系总数。
- 路径由多种计算证据支持，但不是活细胞直接追踪。
- 生存关联是队列层面结果，不是现成的临床决策阈值。
