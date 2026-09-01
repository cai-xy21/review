# 《Lineage tracking reveals dynamic relationships of T cells in colorectal cancer》精读

## 论文信息

- **作者**：Zhang L, Yu X, Zheng L, et al.
- **期刊与年份**：Nature, 2018
- **DOI**：[10.1038/s41586-018-0694-x](https://doi.org/10.1038/s41586-018-0694-x)
- **原文**：[Nature](https://www.nature.com/articles/s41586-018-0694-x)
- **PubMed**：[PMID 30479382](https://pubmed.ncbi.nlm.nih.gov/30479382/)
- **开放处理数据**：[GSE108989](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE108989)
- **受控原始数据**：[EGA Study EGAS00001002791](https://ega-archive.org/studies/EGAS00001002791)；[dataset EGAD00001003910](https://ega-archive.org/datasets/EGAD00001003910)
- **代码**：[sscClust](https://github.com/Japrin/sscClust)；[STARTRAC](https://github.com/Japrin/STARTRAC)
- **本报告核对日期**：2026-08-23

## 一句话结论

本文对 12 例初治结直肠癌的血液、邻近黏膜和肿瘤中 11,138 个 T 细胞进行深度全长 scRNA/TCR 测序，以 10,805 个 QC 后细胞和 8,530 个主图细胞构建 20 状态图谱，并提出 STARTRAC 指标定量克隆扩增、组织迁移与状态转换，揭示 Treg、Tfh、耗竭 CD8 等群之间的克隆动力学关系。

## 数据护照

| 项目 | 内容 |
|---|---|
| 队列 | 12 例未经治疗 CRC：4 MSI、8 MSS |
| 组织 | 外周血、邻近正常黏膜、肿瘤 |
| 技术 | FACS 分选 CD8、CD4 helper、Treg；Smart-seq2；HiSeq 4000；TCR αβ 重建 |
| 测序规模 | 11,138 个单 T 细胞；平均每细胞约 125 万条唯一比对 reads |
| QC/TCR | 10,805 个 QC 后细胞；9,878（91.4%）至少有一对 productive αβ TCR |
| 主 atlas | 8,530 个 T 细胞：3,628 CD8、4,902 CD4；20 clusters |
| 主图组织组成 | blood 2,449；normal 1,962；tumor 4,119 |
| 克隆 | 7,274 clonotypes；870 个扩增克隆（≥2 cells）；3,474 个 clonal cells |
| 开放数据 | GSE108989：scRNA counts/TPM/centered，bulk RNA 与 exome mutation 表 |
| 原始数据 | EGA EGAD00001003910，受控 FASTQ |

## 1. 研究问题

CRC 中不同 T 细胞状态如何在血、正常黏膜和肿瘤之间迁移、扩增并相互关联？能否用 TCR 作为天然 lineage barcode，把“表达相似”进一步分解为克隆扩增、组织迁移和状态转换三个定量维度？

## 2. 方法框架

1. 对三组织的 CD8、常规 CD4 和 Treg 单细胞进行深度全长测序。
2. 构建 20 个 CD4/CD8 cluster，并标注组织偏好与功能程序。
3. 重建高比例配对 productive TCR αβ，定义 clonotype。
4. 开发 STARTRAC-expa、STARTRAC-migr、STARTRAC-tran 等指标，分别量化克隆扩增、跨组织共享和跨状态共享。
5. 比较 MSI/MSS、患者和组织背景下的克隆动力学。

## 3. 数据规模与图谱组成

### 3.1 从测序到主 atlas 的筛选层级

| 层级 | 细胞数 | 说明 |
|---|---:|---|
| 已测序 | 11,138 | 12 名患者三组织的 FACS 单 T 细胞 |
| QC 后 | 10,805 | 用于高质量表达/TCR统计 |
| 有 productive αβ TCR | 9,878（91.4%） | 至少获得一对 productive alpha-beta |
| 主 atlas | 8,530 | 20 个主要 CD4/CD8 状态群 |
| 主 atlas CD8/CD4 | 3,628 / 4,902 | 分别形成 8 个 CD8、12 个 CD4 cluster |
| 主 atlas tissue | 2,449 blood；1,962 normal；4,119 tumor | 总和为 8,530 |

### 3.2 20 个状态群

CD8 部分包含初始/记忆、效应、组织驻留/肿瘤富集、耗竭和 MAIT 等 **8 群**；CD4 部分包含初始/记忆、Th17、Tfh、Treg 等 **12 群**。论文最重要的不是把每群视为固定谱系，而是借 TCR 共享量化这些状态之间的动态关系。

### 3.3 克隆图谱

作者得到 **7,274 个 clonotype**。其中 870 个至少包含 2 个细胞，被定义为扩增克隆；这些扩增克隆共覆盖 3,474 个 clonal T cells。STARTRAC 将克隆信息拆为：

- **expa**：同一 clonotype 的扩增程度；
- **migr**：同一 clonotype 跨 tissue compartment 的分布；
- **tran**：同一 clonotype 跨 transcriptional states/clusters 的分布。

这些是基于离散采样的统计指标，不是直接观测到的迁移速度或转化概率。

### 3.4 GSE108989 文件构成

GEO 系列有 **12 个患者级 GSM 记录**，主要文件为：

| 文件 | 约大小 | 内容 |
|---|---:|---|
| `GSE108989_CRC.TCell.S11138.count.txt.gz` | 69.67 MB | 11,138 个单 T 细胞 counts |
| `GSE108989_CRC.TCell.S11138.TPM.txt.gz` | 351.58 MB | 单细胞 TPM |
| `GSE108989_CRC.TCell.S10805.norm.centered.txt.gz` | 368.54 MB | 10,805 个 QC 后细胞的标准化中心化矩阵 |
| `GSE108989_CRC.bulk.S36.count.txt.gz` | 0.56 MB | bulk RNA counts |
| `GSE108989_CRC.bulk.S36.TPM.txt.gz` | 2.12 MB | bulk RNA TPM |
| `GSE108989_CRC.bulk.exome.somaticMutation.txt.gz` | 9.00 MB | bulk 外显子体细胞突变汇总 |

表达矩阵开放；逐细胞原始 FASTQ 位于 EGA study **EGAS00001002791** 下的 dataset **EGAD00001003910**，须申请。TCR 细节需结合论文补充/Source Data；GEO 系列级六个文件中没有一个显式命名的独立 TCR 表。

### 3.5 按目的下载和读取

| 目的 | 首选数据 | 注意事项 |
|---|---|---|
| 重建 20 状态 atlas | `S11138.count` + 元数据 | 必须复现 QC/注释才能得到 8,530 主图子集 |
| 接近原文标准化结果 | `S10805.norm.centered` | 是 10,805 细胞，不是主图 8,530 |
| STARTRAC 克隆分析 | TCR supplementary/source data + metadata | cell ID、patient、tissue、cluster 缺一不可 |
| 原始比对/TCR 重建 | EGAD00001003910 | 受控访问 |
| 联合突变背景 | bulk mutation 文件 | 不是逐 T 细胞 DNA 测序 |

```powershell
Invoke-WebRequest `
  -Uri "https://ftp.ncbi.nlm.nih.gov/geo/series/GSE108nnn/GSE108989/suppl/GSE108989_CRC.TCell.S11138.count.txt.gz" `
  -OutFile "GSE108989_CRC.TCell.S11138.count.txt.gz"
```

## 4. STARTRAC 的价值

STARTRAC 将“同一 TCR 在哪里出现”转化为可比较的指标，使不同 cell state 的克隆扩增、迁移和转换潜力能在患者间汇总。相较只画 trajectory，它增加了天然谱系约束；相较只统计 clone size，它保留了组织和状态上下文。

## 5. 状态关系的核心发现

肿瘤内耗竭 CD8、某些 Treg/Tfh 状态具有明显扩增；同 clonotype 跨 cluster 或跨组织分布提示潜在状态联系和迁移。某些 Treg 群与组织/肿瘤环境高度相关。MSI/MSS 背景会改变免疫克隆结构，但样本量不支持过细亚组断言。

## 6. 为什么不是“真实 lineage tracking”

标题中的 lineage tracking 指 TCR 天然条形码追踪。它不能像连续成像那样看到细胞从正常黏膜移动到肿瘤，也不能单独决定 A 状态先于 B 状态。同一克隆跨状态可能来自共同祖先或并行分化；STARTRAC-tran 是共享程度，不是方向性 transition rate。

## 7. 主要生物学发现

- 20 个 T 细胞状态具有不同克隆扩增和组织分布。
- TCR 共享为 Treg、Tfh、耗竭 CD8 等状态之间的关系提供谱系约束。
- 肿瘤微环境同时塑造克隆选择、组织定位和转录状态。
- MSI/MSS 与 T 细胞生态不同，但需更大队列验证。

## 8. 推荐精读图

- 12 名患者三组织和 20 clusters 总览。
- TCR clone size 与跨组织共享图。
- STARTRAC-expa/migr/tran 定义及状态比较图。
- Treg/Tfh 和 CD8 状态关系图。

## 9. 方法学创新

1. 超过 90% QC 细胞获得 productive αβ TCR，克隆覆盖率高。
2. STARTRAC 把克隆信息分解为扩增、迁移、转换三个维度。
3. 患者内三组织设计降低了克隆共享的跨患者混杂。

## 10. 局限性

- 横断面采样没有真实时间轴和方向性。
- FACS 预分选比例不等于组织内天然细胞比例。
- 主 atlas、QC 后矩阵与测序总数口径不同。
- TCR 表的公开入口不如表达矩阵直接，复现需补充数据。
- 12 名患者不足以稳定解析 MSI/MSS 的所有差异。

## 11. 在综述架构中的位置

适合放在“**techniques for live cell tracking**”的前置章节，作为天然 lineage barcode 方法：它能在人体样本中进行克隆级回溯，但需要与纵向采样、空间定位或实时追踪技术结合才能确定方向和速率。

## 12. 可直接写入综述的表述

> Zhang 等在 12 例 CRC 的 11,138 个深度测序 T 细胞中为 9,878 个细胞获得 productive αβ TCR，并以 STARTRAC 将克隆信息分解为扩增、跨组织迁移和跨状态共享指标，从而为 20 个 CD4/CD8 状态之间的动态关系提供谱系约束。

## 13. 避免误读

- 11,138 是测序数，10,805 是 QC 后数，8,530 是主 atlas 数。
- STARTRAC-migr/tran 是共享指标，不是直接测得的迁移或转化速率。
- FACS 分选会改变 CD4/CD8/Treg 的天然比例。
- bulk exome mutation 表不是单 T 细胞 DNA 数据。
