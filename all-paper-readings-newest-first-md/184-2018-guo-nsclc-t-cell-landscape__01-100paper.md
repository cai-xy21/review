# 《Global characterization of T cells in non-small-cell lung cancer by single-cell sequencing》精读

## 论文信息

- **作者**：Guo X, Zhang Y, Zheng L, et al.
- **期刊与年份**：Nature Medicine, 2018
- **DOI**：[10.1038/s41591-018-0045-3](https://doi.org/10.1038/s41591-018-0045-3)
- **原文**：[Nature Medicine](https://www.nature.com/articles/s41591-018-0045-3)
- **PubMed**：[PMID 29942094](https://pubmed.ncbi.nlm.nih.gov/29942094/)
- **开放处理数据**：[GSE99254](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE99254)
- **受控原始数据**：[EGA EGAS00001002430](https://ega-archive.org/studies/EGAS00001002430)
- **代码**：[sscClust](https://github.com/Japrin/sscClust)
- **本报告核对日期**：2026-08-23

## 一句话结论

本文在 14 例未经治疗的非小细胞肺癌中，对肿瘤、邻近肺和血液来源的 12,346 个 T 细胞进行全长单细胞转录组与 TCR 重建，解析出 16 个 CD4/CD8 状态群，并提出 CXCL13+、LAYN+ 耗竭 CD8 及 GZMK+“pre-exhausted”群之间的克隆联系与临床意义。

## 数据护照

| 项目 | 内容 |
|---|---|
| 队列 | 14 例未治疗 NSCLC：11 例腺癌、3 例鳞癌 |
| 组织 | 外周血、邻近正常肺、肿瘤 |
| 实验 | FACS 单 T 细胞；全长 scRNA-seq；TCR α/β 重建 |
| 测序 | 12,346 个细胞；平均每细胞约 104 万条唯一比对 read pairs |
| QC 后矩阵 | 11,769 个细胞 × 12,415 个基因 |
| 主图展示 | 9,055 个细胞构成 16 群 |
| 亚群 | 7 个 CD8；7 个常规 CD4 + 2 个 Treg，即 9 个 CD4 群 |
| TCR | 10,202 个最终细胞获得完整 TCRαβ 信息；用于克隆扩增/跨状态分析 |
| 开放数据 | GSE99254 处理后 counts、TPM、normalized-centered 矩阵 |
| 原始数据 | EGA EGAS00001002430，受控访问 |

## 1. 研究问题

NSCLC 的 T 细胞状态在血、邻近肺和肿瘤之间如何分布？肿瘤内耗竭 CD8 是否存在前体/过渡状态？TCR 克隆共享能否识别不同状态间的亲缘联系，并用于解释患者预后或治疗靶点？

## 2. 方法框架

1. 从 14 名患者三类组织中 FACS 分选单 T 细胞并深度全长测序。
2. 以 sscClust 等流程分别构建 CD8、常规 CD4 和 Treg 图谱。
3. 用标志基因、组织富集和功能程序定义 16 个状态群。
4. 重建 TCR α/β，量化克隆大小、跨组织和跨 cluster 共享。
5. 结合 TCGA/公开队列评估状态 signatures 与临床结局的关联。

## 3. 数据规模与图谱组成

### 3.1 三个常被混淆的细胞数

| 数字 | 对象 | 为什么不同 |
|---:|---|---|
| 12,346 | 实际完成单细胞测序的 T 细胞 | 实验输入/测序总量 |
| 11,769 | 最终标准化表达表中的细胞 | 经过 QC 与分类后保留；矩阵为 12,415 genes × 11,769 cells |
| 9,055 | 论文主聚类图中的细胞 | 用于展示 16 个主要 CD4/CD8 群的分析子集 |

这三个数分别代表测序、最终数据表和主图分析子集，不能互换。平均每细胞约 **1.04 million uniquely mapped read pairs**，体现板式全长测序的深度，也使 TCR 重建成为可能。

### 3.2 16 个 T 细胞群的组成

CD8 图谱包含 7 群，典型标志包括：

- **CD8-C1-LEF1**：初始/中央记忆样；
- **CD8-C2-CD28**：共刺激/记忆样；
- **CD8-C3-CX3CR1**：循环效应细胞；
- **CD8-C4-GZMK**：过渡/“pre-exhausted”相关；
- **CD8-C5-ZNF683**：组织驻留相关；
- **CD8-C6-LAYN**：耗竭/肿瘤富集；
- **CD8-C7-SLC4A10**：MAIT。

CD4 部分为 7 个常规 CD4 群和 2 个 Treg 群，共 9 群。因而论文有时写作“7 CD8 + 9 CD4”，其中后者包含 Treg。

### 3.3 TCR 图谱的规模

最终数据中 **10,202 个细胞**获得完整 TCR αβ 信息。主 16 群的早期统计中，8,038 个细胞有 TCR：约 5,015 个独特克隆、3,023 个属于重复/扩增克隆；克隆大小约 2–75 个细胞。作者报告约 9%（158/1,669）的 CD8 克隆跨 cluster 出现，说明同一克隆可占据不止一个转录状态。

### 3.4 GSE99254 文件构成

GEO 公开的是处理后表达数据，主要包括：

| 文件 | 约大小 | 内容 |
|---|---:|---|
| `GSE99254_NSCLC.TCell.S12346.count.txt.gz` | 67.81 MB | 12,346 个测序细胞的 count 表 |
| `GSE99254_NSCLC.TCell.S12346.TPM.txt.gz` | 326.77 MB | 12,346 个细胞的 TPM 表 |
| `GSE99254_NSCLC.TCell.S11769.norm.centered.txt.gz` | 342.43 MB | 11,769 个 QC/分类后细胞的标准化中心化矩阵 |

系列有 14 个患者级 GSM 记录。TCR 表和精细元数据需结合补充表/论文资源；逐细胞原始 FASTQ 与受保护临床信息在 EGA，而不是 GEO supplementary 区。

### 3.5 按目的下载与读取数据

| 目的 | 推荐文件/仓库 | 注意事项 |
|---|---|---|
| 从头标准化、聚类 | `S12346.count` | 需复现 QC 才能得到 11,769/9,055 子集 |
| 接近原文热图/聚类 | `S11769.norm.centered` | 已经过作者处理，不适合当 raw count |
| 定量表达浏览 | `S12346.TPM` | 文件大，内存读取需预留数 GB |
| TCR 克隆分析 | 补充数据；原始重建需 EGA | 先确认 cell ID 与表达矩阵一致 |
| 重跑比对/TCR assembly | EGAS00001002430 | 需审批和 EGA 客户端 |

```powershell
Invoke-WebRequest `
  -Uri "https://ftp.ncbi.nlm.nih.gov/geo/series/GSE99nnn/GSE99254/suppl/GSE99254_NSCLC.TCell.S12346.count.txt.gz" `
  -OutFile "GSE99254_NSCLC.TCell.S12346.count.txt.gz"
```

```python
import pandas as pd
counts = pd.read_csv(
    "GSE99254_NSCLC.TCell.S12346.count.txt.gz",
    sep="\t", index_col=0, compression="gzip"
)
print(counts.shape)
```

## 4. “Pre-exhausted”与耗竭状态

CD8-C4-GZMK 被解释为与耗竭相关的过渡状态，而 CD8-C6-LAYN 呈现更强的 checkpoint/耗竭程序。二者之间的转录相似、组织富集和部分 TCR 共享共同支持状态关联。需要注意，“pre-exhausted”是由静态图谱和克隆重叠推定的生物学模型，不是逐细胞实时观察到的转换轨迹。

## 5. 克隆扩增与组织分布

血液中的 CX3CR1+ 效应群、肺组织驻留群和肿瘤耗竭群具有不同克隆扩增模式。跨 cluster 的同一 TCR 提供谱系约束，使纯粹基于表达相似度的轨迹更可信；但跨组织克隆共享也可能来自并行分化或共同祖先，不能自动推断迁移方向。

## 6. 临床关联

作者利用状态 signature 在外部 bulk 队列中评估预后关联，并提出“pre-exhausted/ exhausted”平衡可能比单独耗竭标志更有信息量。这种外推会受到 bulk 细胞比例与肿瘤纯度影响，适合生成假说，不等同于单细胞前瞻性临床验证。

## 7. 主要生物学发现

- NSCLC T 细胞至少包含 16 个可区分群，并呈显著组织偏好。
- LAYN+ CD8 群体现肿瘤富集的耗竭程序；GZMK+ 群与其存在克隆联系。
- 同一克隆可跨状态出现，表明克隆身份与功能状态不是一一对应。
- Treg 也存在功能差异明显的亚群，肿瘤富集状态具有独特标志。

## 8. 推荐精读图

- **Fig. 1**：队列、三组织取样和 16 群全景。
- **CD8 状态图**：GZMK、ZNF683、LAYN 等群及组织分布。
- **TCR 克隆共享图**：本文连接状态与亲缘关系的核心证据。
- **临床关联图**：用于理解 signature 外推的价值与边界。

## 9. 方法学创新

1. 以深度全长 scRNA-seq 同时获取表达和高比例 TCR αβ。
2. 三组织配对设计把循环、正常组织驻留和肿瘤状态放在同一坐标系。
3. 将克隆共享用于约束“pre-exhausted—exhausted”状态模型。

## 10. 局限性

- 14 例患者仍不足以覆盖 NSCLC 分子和治疗异质性。
- 未治疗横断面队列不能说明免疫治疗后的状态变化。
- 高测序深度不等于实时状态追踪；方向性仍是推断。
- 处理矩阵与主图子集规模不同，复现时容易发生细胞数口径错误。
- bulk 预后外推可能混入细胞比例和肿瘤纯度效应。

## 11. 在综述架构中的位置

适合放在“**link cell state/function transitions with molecular drivers**”之前，作为“利用 TCR 克隆给静态状态图加入谱系约束”的典型案例；也可用于说明 LAYN、GZMK、CX3CR1、ZNF683 等标志如何把组织位置、分化与功能联系起来。

## 12. 可直接写入综述的表述

> Guo 等在 14 例初治 NSCLC 中测序 12,346 个单 T 细胞，并在最终数据中为 10,202 个细胞获得完整 TCR αβ 信息；7 个 CD8 和 9 个 CD4/Treg 群揭示了组织特异状态，以及 GZMK+ 过渡群与 LAYN+ 耗竭群之间的克隆联系。

## 13. 避免误读

- 12,346、11,769 与 9,055 分别是测序总数、最终矩阵数和主图子集数。
- “pre-exhausted”不等于已被实时证明的必经前体。
- 跨 cluster TCR 共享不决定状态转换的方向。
- GEO 提供处理矩阵；原始测序数据在受控 EGA。
