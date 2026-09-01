# 《The PTPN2/PTPN1 inhibitor ABBV-CLS-484 unleashes potent anti-tumour immunity》精读

## 论文信息

- **作者**：Christina K. Baumgartner、Hakimeh Ebrahimi-Nik、Arvin Iracheta-Vellve 等
- **期刊与年份**：*Nature*, 2023；622: 850–862
- **DOI**：10.1038/s41586-023-06575-7
- **本地原文**：[PDF](<D:/research/review/perturbation33references/24-The PTPN2PTPN1 inhibitor ABBV-CLS-484 unleashes potent anti-tumour immunity.pdf>)
- **多组学 SuperSeries**：[GEO GSE237378](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE237378)
- **蛋白结构**：[PDB 7UAD](https://www.rcsb.org/structure/7UAD)

## 一句话结论

口服活性位点抑制剂 ABBV-CLS-484（AC484）同时抑制 PTPN2/PTPN1，放大肿瘤细胞的 IFN 感知与免疫细胞的 JAK–STAT/TCR 信号，在抗 PD-1 不敏感的小鼠肿瘤中重塑髓系、NK 和 CD8⁺ T 细胞生态并产生强抗肿瘤作用。

## 数据护照

| 子系列 | 数据类型 | 样本数 | 核心组成 |
|---|---|---:|---|
| GSE237371 | 肿瘤细胞 bulk RNA-seq | 24 | WT/DKO × CTRL/AC484/IFNγ/联合；每格 3 重复 |
| GSE237374 | bulk tumor TCRβ-seq | 44 | vehicle 14、anti-PD-1 15、AC484 15 |
| GSE237375 | CD45⁺ TME scRNA-seq | 28 | B16 12 + KPC 16；vehicle/anti-PD-1/AC484 |
| GSE237376 | 分选 CD8⁺ TIL ATAC-seq | 12 | Slamf6⁺/Tim3⁺ × 3 治疗 × 2 重复 |
| GSE237377 | 分选 CD8⁺ TIL bulk RNA-seq | 12 | 与 ATAC 完全同构 |
| **GSE237378 合计** | 5 个子系列 | **120** | 同一 SuperSeries 的 GSM 总数 |

## 1. 研究问题

遗传删除 PTPN2 或 PTPN1 能提高抗肿瘤免疫，但磷酸酶活性位点长期被认为难以药物化。作者试图证明：一个可口服、同时作用肿瘤与免疫细胞的小分子是否能把遗传学表型转化为系统性免疫治疗。

## 2. 实验设计与方法框架

研究从结构和酶学确证 AC484 对 PTPN2/PTPN1 的结合与选择性，随后在肿瘤细胞、T 细胞、NK 细胞和髓系细胞中测试 IFN/JAK–STAT 与抗原受体信号，最后在 B16、KPC 等免疫治疗耐受模型中开展单药和 anti-PD-1 联合实验。多组学把肿瘤细胞内在效应、TME 组成、TCR 克隆和 CD8⁺ TIL 的转录/染色质状态分开归档。

## 3. 数据规模与图谱组成

### 3.1 SuperSeries 的层级结构

GSE237378 是索引页，不是一张 120 样本的同质矩阵。它由五个 assay 不同、统计单位不同的子系列组成。下载和分析必须进入子系列；直接把 120 个 GSM 拼成一个表达矩阵没有意义。

### 3.2 肿瘤细胞内在转录组：GSE237371，24 个文库

设计为 **2 个 PTPN2/PTPN1 基因背景 × 4 个处理 × 3 个重复 = 24**：

- 基因背景：B16 WT 与 PTPN2/PTPN1 double knockout；
- 处理：CTRL、AC484、IFNγ、AC484 + IFNγ；
- 每格 3 个生物重复。

该模块用于判断 AC484 的转录效应是否依赖 PTPN2/PTPN1，以及药物是否与 IFNγ 协同。GEO 提供 `GSE237371_est_counts.txt.gz`，原始 reads 在 SRA。

### 3.3 TCRβ 克隆组：GSE237374，44 个肿瘤样本

从 B16 整块肿瘤 RNA 分析 TCRβ repertoire：vehicle 14、anti-PD-1 15、AC484 15，共 44。提供约 1.3 MB 的 `GSE237374_bulk_df.csv.gz` 处理表和 SRA 原始数据。

这是 bulk TCRβ repertoire，不是单细胞配对 TRA/TRB；能分析克隆多样性和扩增，但不能把每条 TCR 精确映射到单细胞转录状态。

### 3.4 TME 单细胞图谱：GSE237375，28 个样本

| 模型 | 条件与重复 | 样本数 |
|---|---|---:|
| B16F10 + GVAX | vehicle 4、AC484 4、anti-PD-1 4 | 12 |
| KPC | vehicle 5、AC484 6、anti-PD-1 5 | 16 |
| **合计** |  | **28** |

每个样本为肿瘤分离的 CD45⁺ 细胞单细胞 RNA-seq。GEO 提供 `GSE237375_RAW.tar`（约 833.3 MB，H5 文件），原始 reads 亦可从 SRA 下载。B16 和 KPC 的重复数不完全平衡，建模时需保留模型×治疗交互，而不是简单合并。

### 3.5 CD8⁺ TIL 转录—染色质配对设计：各 12 个文库

GSE237376（ATAC-seq）和 GSE237377（bulk RNA-seq）均采用：

**2 个状态 × 3 个治疗 × 2 个重复 = 12**。

- 状态：Slamf6⁺ progenitor-exhausted 与 Tim3⁺ terminally exhausted；
- 治疗：untreated、anti-PD-1、AC484；
- 重复：A、B。

ATAC 提供 `GSE237376_ATACseq_rawCounts.txt.gz`（约 2.2 MB），RNA 提供 `GSE237377_RNAseq_rawCounts.csv.gz`（约 1.2 MB）。两套设计同构，适合做状态×治疗的联合解释，但不应声称它们一定来自同一只动物的严格配对 aliquot，除非 GSM 元数据进一步确认。

### 3.6 结构与非测序数据

人 PTPN2 与抑制剂复合物的原子坐标和衍射数据在 PDB `7UAD`。药效、流式、肿瘤体积等其余数据主要在论文 Source Data；Data availability 对这些数据仅写明可向作者申请。

### 3.7 推荐下载方式

1. 先下载 GSE237378 的 MINiML/SOFT，建立 120 个 GSM 到 5 个子系列的映射。
2. 单细胞优先取 28 个 H5；从样本标题恢复模型和治疗，按小鼠层面做比例统计。
3. bulk RNA/ATAC 优先取处理矩阵；需要重新比对时再从各 BioProject 下载 FASTQ。
4. TCR 先取 `bulk_df.csv.gz` 做克隆统计；若要重跑 clonotyping，再下载 44 个 SRA run。
5. 结构机制从 PDB 7UAD 获取坐标，而不是在 GEO 中寻找。

## 4. 主要结果

AC484 增强肿瘤细胞 IFN 反应和抗原呈递，同时提高 NK/CD8⁺ T 细胞活性。单细胞图谱显示肿瘤免疫环境被“点燃”，CD8⁺ TIL 的 progenitor/terminal 状态和染色质程序改变；多种 anti-PD-1 耐受模型出现显著控制。

## 5. 机制理解

PTPN2/PTPN1 是多个磷酸化信号的共同刹车。AC484 通过双重解除制动，既提高肿瘤细胞对 IFN 的可见性，又增强免疫细胞 JAK–STAT 和抗原受体输出，形成跨细胞类型的正反馈。正因作用广泛，疗效与炎症毒性也来自同一机制。

## 6. 推荐重点阅读的图

- 结构/酶学和 7UAD：证明“可药物化”。
- 多模型肿瘤控制与 anti-PD-1 对照。
- B16/KPC CD45⁺ 单细胞图谱。
- Slamf6⁺ 与 Tim3⁺ TIL 的 RNA/ATAC 联合图。
- TCR 多样性与克隆扩增图。

## 7. 创新性

首次把活性位点 PTPN2/PTPN1 双抑制推进为可口服肿瘤免疫药物，并用 120 个多组学样本拆解肿瘤细胞内在与免疫细胞内在贡献。

## 8. 局限性

临床相关性主要仍是小鼠证据；PTPN2/PTPN1 广泛表达，治疗窗与系统炎症是核心风险。TCR 为 bulk β链，不能直接连接到单细胞状态。各子系列重复数有限且部分不平衡。

## 9. 在综述中的定位

适合作为“药理性解除细胞内免疫检查点”的代表，与 T 细胞特异 PTPN2 编辑、肿瘤细胞 PTPN2 缺失及抗体型 ICB 对照。

## 10. 可直接写入综述的表述

> ABBV-CLS-484 通过同时抑制 PTPN2/PTPN1，协同增强肿瘤细胞 IFN 感知与 CD8⁺ T/NK 细胞效应信号，在检查点阻断耐受模型中把免疫“冷”肿瘤转化为炎症性微环境。

## 11. 数据复用建议

可用 28 个 scRNA-seq 样本做模型分层的 pseudobulk；用 12+12 个 RNA/ATAC 样本做状态×治疗交互；再以 44 个 bulk TCR 样本检验克隆集中度是否与单细胞 CD8 状态比例相关。整合时统计单位始终是小鼠/样本，不是细胞或 peak。

## 12. 转化与安全性关注

临床应重点监测自身免疫、细胞因子升高、肝毒性和广泛 IFN 相关不良反应，并评估与 PD-1 阻断联用是否缩窄治疗窗。论文所述药物已进入 NCT04777994，但本研究不能替代临床疗效结论。

## 13. 避免误读

- GSE237378 的 120 是五种 assay 的 GSM 合计，不是 120 个独立小鼠的单一队列。
- bulk TCRβ 不是配对单细胞 TCR。
- AC484 的作用不是只发生在 T 细胞，也包含肿瘤、NK 和髓系细胞。
- 小鼠抗 PD-1 耐受模型中的强疗效不能直接等同于人群应答率。

