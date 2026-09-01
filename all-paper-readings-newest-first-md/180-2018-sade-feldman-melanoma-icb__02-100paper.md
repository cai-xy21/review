# 《Defining T cell states associated with response to checkpoint immunotherapy in melanoma》精读

## 论文信息

- **作者**：Sade-Feldman M, Yizhak K, Bjorgaard SL, et al.
- **期刊与年份**：Cell, 2018
- **DOI**：[10.1016/j.cell.2018.10.038](https://doi.org/10.1016/j.cell.2018.10.038)
- **原文**：[Cell](https://www.cell.com/cell/fulltext/S0092-8674(18)31394-1)
- **PubMed**：[PMID 30388456](https://pubmed.ncbi.nlm.nih.gov/30388456/)
- **开放全文**：[PMC6641984](https://pmc.ncbi.nlm.nih.gov/articles/PMC6641984/)
- **开放处理数据**：[GSE120575](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE120575)
- **受控原始数据**：[dbGaP phs001680.v1.p1](https://www.ncbi.nlm.nih.gov/projects/gap/cgi-bin/study.cgi?study_id=phs001680.v1.p1)
- **本报告核对日期**：2026-08-23

## 一句话结论

本文在 32 名接受免疫检查点抑制治疗的转移性黑色素瘤患者、48 份肿瘤活检中分析 16,291 个高质量 CD45+ 单细胞，发现响应病灶富集 TCF7/IL7R 记忆样 CD8 状态，进展病灶富集耗竭状态，并用 TCR、WES、ATAC-seq 和独立多重免疫荧光队列支持“可自我更新前体样—终末耗竭”状态平衡与疗效相关。

## 数据护照

| 项目 | 内容 |
|---|---|
| 患者/活检 | 32 名患者，48 份转移性黑色素瘤活检 |
| 治疗 | 35 份 anti-PD-1、11 份 anti-CTLA-4+anti-PD-1、2 份 anti-CTLA-4 相关活检 |
| 疗效口径 | 病灶级 17 responder、31 non-responder |
| 纵向结构 | 11 名患者有纵向活检；20 名各 1 份；1 名有 2 份非标准配对活检 |
| scRNA-seq | 19,392 个 CD45+ 细胞完成测序；16,291 个通过 QC；Smart-seq2 |
| T 细胞重点 | 6,350 个 CD8 T 细胞；2 个大类、进一步 6 个 CD8 状态群 |
| 配套组学 | 20 对肿瘤-正常 WES；5 名患者分选 CD39/TIM3 亚群做 ATAC-seq；scRNA 来源 TCR |
| 独立验证 | 33 名患者、43 份样本的多重免疫荧光 |
| 开放数据 | GSE120575：TPM 矩阵和细胞/患者元数据 |
| 受控数据 | phs001680.v1.p1：原始 scRNA/WES/ATAC 等，须 dbGaP 审批 |

## 1. 研究问题

免疫检查点抑制治疗前后，哪些肿瘤浸润 T 细胞状态与病灶响应相关？响应相关状态是由单一标志物还是细胞群比例决定？TCR 克隆、染色质开放性和组织蛋白定位能否为转录状态提供正交证据？

## 2. 方法框架

1. 从治疗前或治疗中/后的活检中 FACS 分选 CD45+ 细胞并做 Smart-seq2。
2. 建立 11 个广义免疫群，再重点重聚类 6,350 个 CD8 T 细胞。
3. 将样本按病灶是否缩小划分 responder/non-responder，比较状态比例与 signature。
4. 从 scRNA-seq 重建 TCR，分析克隆扩增与状态分布。
5. 用 WES 检查抗原呈递/免疫逃逸事件，用 ATAC-seq比较 CD39+TIM3+ 与双阴性 CD8，用多重 IF 验证 TCF7+ 与耗竭标志细胞的组织比例。

## 3. 数据规模与图谱组成

### 3.1 临床队列与活检构成

核心队列是 **32 名患者、48 份肿瘤活检**。病灶级反应定义为 CR/PR 或影像学回缩，共 17 份；稳定/进展病灶归为 non-responder，共 31 份。该口径是“活检病灶级”，不等价于 17 名响应患者和 31 名不响应患者。

治疗构成按活检计为 35 份 anti-PD-1、11 份联合 anti-CTLA-4+anti-PD-1、2 份 anti-CTLA-4。11 名患者有纵向样本，因此不同活检并非全部统计独立；复用数据时应保留 patient ID 并采用患者层级建模。

### 3.2 单细胞与图谱层级

| 层级 | 规模 | 组成/用途 |
|---|---:|---|
| 已测序 CD45+ | 19,392 | FACS 获得的肿瘤免疫细胞 |
| QC 后细胞 | 16,291 | 主免疫图谱输入；平均/中位测序深度约 140 万 paired-end reads/细胞、约 2,588 genes/细胞 |
| 广义免疫群 | 11 | 2 个 B、2 个髓系及 7 个 T/NK/NKT 相关群 |
| CD8 T 细胞 | 6,350 | 核心治疗响应分析 |
| CD8 大类 | CD8_G 与 CD8_B | CD8_G 偏记忆/TCF7/IL7R；CD8_B 偏细胞毒/耗竭 |
| CD8 精细群 | 6 | 进一步解析记忆样、细胞毒、耗竭、增殖等状态 |

### 3.3 正交数据的规模

| 数据 | 规模 | 作用 |
|---|---:|---|
| WES | 20 个患者的肿瘤-正常配对 | 检查 B2M/HLA 等免疫逃逸与新抗原背景 |
| ATAC-seq | 5 名患者 | 比较 CD39+TIM3+ 与 CD39−TIM3− 分选 CD8 群的染色质状态；详见补充表 S5 |
| TCR | 从 Smart-seq2 重建 | 克隆表见补充表 S6；关联扩增与状态 |
| 多重免疫荧光 | 独立 33 名患者、43 份样本 | 在组织中验证 TCF7+ 与耗竭标志细胞比例/空间共现 |

这些数据不是对所有 16,291 个细胞完整配对的四模态测量，必须分层描述。

### 3.4 GSE120575 公开文件

GEO 有 **48 个 GSM 活检记录**，与 48 份临床活检相对应。主要系列级文件为：

- `GSE120575_Sade_Feldman_melanoma_single_cells_TPM_GEO.txt.gz`（文件名可能在页面中缩写显示），约 **120.85 MB**：单细胞 TPM 矩阵。
- `GSE120575_patient_ID_single_cells.txt.gz`，约 **0.08 MB**：细胞到患者/样本等元数据映射。

公开 GEO 适合状态复现，但逐细胞 raw FASTQ、WES、ATAC 及受保护临床信息需从 **phs001680.v1.p1** 申请。补充表中的 TCR/ATAC 汇总不应与受控原始文件混为一谈。

### 3.5 按目的下载

| 目的 | 数据入口 | 关键操作 |
|---|---|---|
| 复现 CD8 状态和疗效关联 | GSE120575 TPM + metadata | 以患者为统计单位，不能把每个细胞当独立重复 |
| 重做 counts-based 标准化 | dbGaP raw 数据 | GEO 仅公开 TPM，需受控原始数据 |
| TCR 克隆—状态 | 补充表 S6；深度复现申请 dbGaP | 对齐 cell ID 和 clonotype |
| 染色质状态 | 补充表 S5 + dbGaP ATAC | 只有 5 名患者，避免过度泛化 |
| 组织蛋白验证 | 论文多重 IF 数据/图像 | 独立队列，不与 scRNA 逐细胞配对 |

```powershell
Invoke-WebRequest `
  -Uri "https://ftp.ncbi.nlm.nih.gov/geo/series/GSE120nnn/GSE120575/suppl/GSE120575_patient_ID_single_cells.txt.gz" `
  -OutFile "GSE120575_patient_ID_single_cells.txt.gz"
```

下载 TPM 时建议从 GEO supplementary 区复制页面显示的精确 FTP 文件名，以避免版本后缀变化。

## 4. 响应相关的 T 细胞状态

响应病灶富集 TCF7、IL7R、CCR7 等记忆/前体样程序，非响应病灶更富集 CD39、TIM3、PD-1 等耗竭相关程序。作者进一步提出两类状态比例比单个基因更有预测力。TCF7+ 并不代表完全未激活；它更接近保留增殖、自我更新潜力的抗原经历细胞。

## 5. 克隆与染色质证据

TCR 结果说明相同克隆可存在于不同 CD8 状态，支持状态可塑性。ATAC-seq 显示 CD39/TIM3 分选群具有不同调控景观，为转录差异提供表观遗传证据。但两者仍缺少同一细胞的时间序列，不能直接绘制确定的状态转换路线。

## 6. 抗原呈递与疗效混杂

部分 non-responder 可能存在 B2M/HLA 等抗原呈递缺陷。由此，低响应不一定只由 T 细胞自身状态造成；即便增加 TCF7+ 前体样 T 细胞，如果肿瘤无法呈递抗原，治疗仍可能失败。这一点非常适合综述中讨论“导航 T 细胞状态的外部边界条件”。

## 7. 主要生物学发现

- TCF7/IL7R 记忆样 CD8 状态与 ICB 响应相关。
- 终末耗竭/细胞毒状态在进展病灶更常见。
- 两类状态的相对比例比单一标志更能概括治疗反应生态。
- TCR、ATAC 与多重 IF 提供了不同层次的支持。

## 8. 推荐精读图

- 队列设计及 11 个免疫群总览。
- CD8_G/CD8_B 与 6 个精细状态图。
- responder/non-responder 状态比例图。
- TCF7 多重 IF 和 ATAC-seq 验证图。

## 9. 方法学创新

1. 将单细胞状态与真实 ICB 病灶反应相连。
2. 用 TCR、ATAC、WES、组织蛋白多层验证同一状态模型。
3. 从单标志预测转向“状态比例/生态平衡”指标。

## 10. 局限性

- 活检、治疗方案和时间点异质，且有重复患者样本。
- 疗效是病灶级关联，不能直接证明 TCF7 状态导致响应。
- 细胞状态比例受取样部位、解离效率和肿瘤纯度影响。
- 公共 GEO 只有处理后 TPM 与基础元数据，完整复现需 dbGaP。
- ATAC 和 WES 仅覆盖主队列子集。

## 11. 在综述架构中的位置

适合用于“**optimize the conditions for navigating T cell states**”：优化目标不应仅是提高效应分子，而应维持可持续的 TCF7+ 前体/记忆样库，同时限制过早终末耗竭；还应同步保证抗原呈递等外部条件。

## 12. 可直接写入综述的表述

> 在 32 名黑色素瘤患者的 48 份 ICB 相关活检中，Sade-Feldman 等从 16,291 个高质量 CD45+ 单细胞识别出与响应相关的 TCF7/IL7R 记忆样 CD8 状态，并通过 TCR、ATAC-seq 和独立组织成像支持“前体样—终末耗竭”状态平衡，而非单一 checkpoint 表达，是重要疗效关联指标。

## 13. 避免误读

- 17 responder 与 31 non-responder 是活检/病灶数，不是患者数。
- TCF7+ 是疗效相关状态，不是本文证明的充分因果条件。
- 多组学不是对每个细胞同时测得。
- 状态比例可能被抗原呈递缺陷和取样差异混杂。
