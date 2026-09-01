# 《Asian diversity in human immune cells》精读（AIDA）

## 论文信息

- 第一作者：K. H. Kock、L. M. Tan、K. Y. Han、Y. Ando 等
- 期刊：*Cell*，2025；188(8): 2288–2306.e24
- DOI：10.1016/j.cell.2025.02.017
- 原文：[Cell](https://www.cell.com/cell/fulltext/S0092-8674(25)00202-8)
- 项目导航：[AIDA resource page](https://prabhakarlab.github.io/AIDA.html)
- 数据：[CELLxGENE collection](https://cellxgene.cziscience.com/collections/ced320a1-29f3-47c1-a735-513c7084d508)

## 一句话结论

AIDA 以 619 名供者、5 个亚洲国家、7 个人群组的 1,265,624 个循环免疫细胞建立亚洲人群参考图谱，显示亚大陆祖源、年龄和性别系统性影响免疫细胞组成、状态与细胞类型特异 eQTL。

## 1. 数据护照

| 维度 | Freeze v2 | 说明 |
|---|---:|---|
| 细胞 | 1,265,624 | 论文完整 AIDA Phase 1 图谱 |
| 供者 | 619 + 6 个技术/共同对照 | 健康循环免疫细胞 |
| 国家 | 5 | 日本、新加坡、韩国、泰国、印度 |
| 人群组 | 7 | 覆盖中国、印度、日本、韩国、马来、泰国等亚群，具体口径见 metadata |
| 技术 | 10x 5′ v2 scRNA-seq | 统一技术有利于人群比较 |
| 遗传数据 | 基因型/eQTL 分析 | 部分访问受各国同意书和数据治理限制 |
| Freeze v1 | 1,058,909 cells；503 donors | 早期公开版本；不要与 v2 混用 |
| TCR/BCR | 生成了 V(D)J 数据，并用于辅助注释 | 每名供者中位数约 986 个高置信 TCR barcode、122 个 BCR barcode；论文重点不是克隆型生物学，也不能据此认为所有 T 细胞均有配对 TRA–TRB |

## 2. 研究设计与方法

- 多国协调采集健康外周血，在共同技术框架下生成 PBMC scRNA-seq。
- 比较细胞类型比例、表达邻域与差异基因，模型中考虑年龄、性别和人群。
- 联合基因型进行细胞类型特异 eQTL，寻找在非亚洲队列中低频或缺失的功能变异。
- 使用共同对照样本连接不同国家/中心的技术批次。

## 3. 到底关注了哪些 T 细胞

这不是只研究某一种 T 细胞，而是对健康外周血中的主要 T 细胞谱系进行统一注释。官方差异表达分析使用的三级标签包括：

| 大类 | 论文/代码中的细胞标签 | 生物学含义与常用标志 |
|---|---|---|
| CD4 T | `CD4+_T_naive` | 初始型；CCR7、SELL、LEF1、TCF7 较高 |
| CD4 T | `CD4+_T_cm` | 中央记忆型；保留归巢/记忆程序 |
| CD4 T | `CD4+_T_em` | 效应记忆型；效应与迁移程序增强 |
| CD4 T | `CD4+_T_cyt` | 细胞毒性 CD4 T；CCL5、细胞毒颗粒相关基因较高 |
| Treg | `Treg` | 调节性 T；FOXP3、IL2RA、CTLA4；内部还可见活化/HLA-DR 与 TCF7 相关差异 |
| CD8 T | `CD8+_T_naive` | 初始型；CCR7、SELL、LEF1、TCF7 较高 |
| CD8 T | `CD8+_T_GZMKhi` | GZMK 高的记忆/过渡效应状态 |
| CD8 T | `CD8+_T_GZMBhi` | GZMB 高的终末细胞毒状态；常伴 CCL5、NKG7、PRF1、GNLY、CX3CR1、FGFBP2 |
| 先天样 T | `MAIT` | 黏膜相关恒定 T；TRAV1-2、SLC4A10、NCR3 等 |
| γδ T | `gdT_GZMKhi` | GZMK 偏高、相对记忆样的 γδ T |
| γδ T | `gdT_GZMBhi` | GZMB 偏高、细胞毒性更强的 γδ T |
| 稀有 T | `dnT` | CD4/CD8 双阴性 T；约占全部细胞 0.04%，属于稀有群体 |

因此，这篇文章对 T 细胞最有价值的地方不是提出一个新的“肿瘤耗竭亚型”，而是在大样本健康亚洲人群中给出 **naive—memory—cytotoxic、Treg、MAIT、γδ T** 的外周血基线。CD8 和记忆 γδ T 中还可见相反的 GZMK–GZMB 连续梯度：GZMB 越高，整体越偏细胞毒，而不是所有细胞都能被绝对切成互斥类别。

## 4. 只有 T 细胞吗

不是。AIDA 是 **PBMC 全免疫图谱**，T 细胞只是其中一部分。主要还包括：

- B 细胞：naive B、IGHM 高/低 memory B、atypical B；
- 髓系细胞：CD14+ monocyte、CD16+ monocyte、cDC1、cDC2、ASDC、pDC；
- NK/ILC：CD16+ NK、CD56+ NK 及 ILC；
- 其他：浆细胞、血小板等小群体。

换言之，论文的问题是“亚洲健康人群的循环免疫系统如何随人群、年龄、性别和遗传背景变化”，并非“T 细胞专项图谱”。

## 5. 关键发现

1. “亚洲人群”不是单一免疫背景；亚大陆内部差异广泛存在于细胞组成和状态。
2. 年龄和性别与人群效应并行，不能只用祖源标签解释差异。
3. 多个细胞类型特异 eQTL 在非亚洲数据库中代表不足，可帮助解释疾病相关变异。
4. AIDA 适合作为疾病队列的健康背景和 ancestry-aware 参考，而不是肿瘤/炎症 T 细胞状态终表。
5. 对 T 细胞而言，naive、memory、cytotoxic、regulatory 和先天样 T 状态的比例/表达邻域会随人群背景变化；例如 CD4 naive T 与自报族群及年龄相关，韩国供者的 Treg 比例偏低。此类结果提示临床“正常范围”和疾病 marker 不宜套用单一人群基线。

## 6. 对 T 细胞研究的价值

- 为 CD4、CD8、γδ 等循环 T 细胞提供大样本健康基线。
- 可检验某个“疾病状态 marker”是否其实受年龄、性别或祖源影响。
- 适合训练亚洲人群的标签转移/基线模型，但实体组织 TRM、TIL 和胸腺发育不在其主要覆盖内。
- 若目标是 TCR–HLA/抗原特异性，仍须另查 VDJ 与 HLA 文件。论文虽有 TCR/BCR barcode，并用其辅助细胞注释，但主线是 GEX + genotype/eQTL，而非抗原特异克隆追踪。

## 7. 它是什么测序，数据具体长什么样

### 7.1 实验数据层次

| 层次 | 数据形式 | 用途 |
|---|---|---|
| 单细胞转录组 | 10x Genomics 5′ v2 GEX；原始数据为 FASTQ，处理后为基因×细胞 UMI 稀疏矩阵 | 聚类、注释、差异表达、细胞状态比较 |
| 单细胞元数据 | 每个 barcode 一行 | 细胞类型、供者、人群、国家、年龄、性别、批次、共同对照等 |
| V(D)J | TCR/BCR contig 或高置信 receptor barcode | 辅助 T/B 细胞身份和受体分析；并非每个细胞都有受体，不能默认 TRA–TRB 成对 |
| 个体基因型 | Illumina GSAv3 等芯片数据，经 QC/填充 | 与单细胞表达联合做 cell-type-specific eQTL |
| 派生结果 | donor×cell type 比例、细胞邻域丰度、pseudobulk 表达、eQTL/共定位统计 | 避免把细胞数误当独立样本，并连接疾病 GWAS |

它是 **单细胞 RNA 测序**，不是空间转录组，也不是肿瘤组织测序；样本来源是健康供者外周血 PBMC。论文还叠加了供者基因型和部分 V(D)J 信息，但没有空间坐标。

### 7.2 下载后的对象

CELLxGENE 公开对象可下载为 AnnData/H5AD。典型结构是：

```text
adata.X       细胞 × 基因的稀疏表达矩阵（UMI/count 或标准化层，须核查具体 layer）
adata.obs     每个细胞的 metadata：细胞标签、donor、population、country、age、sex、batch 等
adata.var     基因 ID、基因名等
adata.obsm    UMAP/低维坐标（若公开对象保留）
```

官方 R 分析代码使用 Seurat 对象，核心注释字段包括 `Annotation_Level1`、`Annotation_Level2`、`Annotation_Level3`，供者字段包括 `DCP_ID`。实际分析前应检查 `.X` 与 `layers` 中哪一个保存 raw counts，不要直接假设 `.X` 是原始 UMI。

### 7.3 TCR 数据应如何理解

- 论文报告每名供者中位数约 **986 个高置信 TCR barcode**，说明确实生成并处理了 V(D)J 数据；
- “有 TCR barcode”不等于“一定有完整、生产性、配对的 TRA+TRB”；需要在 contig 表中按 barcode 检查 chain、productive、high-confidence 字段；
- 官方代码仓库含 `TCR_BCR/get_valid_TCR.R` 和 `get_valid_BCR.R`，说明受体数据经过有效链筛选；
- 但论文没有把克隆扩增、抗原特异性或 TCR–HLA 关系作为主要结论。

## 8. 数据获取

- [CELLxGENE](https://cellxgene.cziscience.com/collections/ced320a1-29f3-47c1-a735-513c7084d508) 可分别下载 Freeze v1 与 v2 的 AnnData。
- 以论文分析为准选 **Freeze v2**；复现旧 Census 结果才选 v1。
- 各国原始/基因型数据访问路径不同，统一入口见 [AIDA resource page](https://prabhakarlab.github.io/AIDA.html)。
- 细胞注释可从 [CellTypist Cell Type Census](https://celltype.info/project/336/dataset/591) 查看；分析代码见 [AIDA Phase 1 GitHub](https://github.com/prabhakarlab/AIDA_Phase1)。
- 原始 10x 5′ GEX FASTQ 和基因型文件按国家分别采用开放或受控访问；公开表达矩阵并不等于所有原始 FASTQ/基因型均可直接下载。
- 分析时必须保留 `donor、population、country、age、sex、batch、control status`；统计按供者进行。

## 9. 推荐图版

- **Graphical abstract / Fig. 1**：国家、人群、供者和百万细胞设计；章节补充页首选。
- **细胞组成与邻域差异图**：讲人群差异不止体现在平均表达。
- **cell-type-specific eQTL 图**：讲基因型如何塑造状态基线。

## 10. PPT 单页格式

**标题**：T 细胞参考图谱必须覆盖人群多样性

**正文**：619 名供者；5 个亚洲国家；1,265,624 个循环免疫细胞；祖源、年龄、性别共同塑造免疫状态和 eQTL。

**配图**：Graphical abstract/Fig. 1 + 一张 cell-type-specific eQTL 图。

**页脚引用**：Cell 2025, Kock。

## 11. 局限性

- 仅外周血，不能代表组织驻留和肿瘤微环境。
- 人群、国家、生活方式和采样中心部分共线，模型难完全拆开。
- 部分数据受控，跨国访问规则不同。
- 百万细胞不能替代对每个人群足够供者数和独立复制。
- PBMC 中缺少组织驻留 T、TIL 和多数器官特异免疫生态；不能把这里的 GZMK/GZMB 状态直接等同于肿瘤中的耗竭轨迹。

## 12. 可直接用于综述

> AIDA 在 619 名供者和 126 万个循环免疫细胞中证明，亚大陆祖源、年龄与性别会系统性改变免疫细胞组成和表达基线，因此跨疾病 T 细胞图谱需要显式建模人群多样性（Cell 2025, Kock）。
