# 《Determinants of response and resistance to CD19 chimeric antigen receptor (CAR) T cell therapy of chronic lymphocytic leukemia》精读

## 论文信息

- 作者：Joseph A. Fraietta、Simon F. Lacey、Elena Orlando 等
- 期刊：*Nature Medicine*
- 年份：2018；24: 563–571
- DOI：10.1038/s41591-018-0010-1
- 原文：[Nature Medicine](https://doi.org/10.1038/s41591-018-0010-1)
- PubMed：[PMID 29713085](https://pubmed.ncbi.nlm.nih.gov/29713085/)
- 全文：[PMCID PMC6117613](https://pmc.ncbi.nlm.nih.gov/articles/PMC6117613/)
- 数据入口：论文 Supplementary Tables/Source Data（处理后表达数据）；未见该论文声明的 GEO/SRA 原始测序 accession

## 一句话结论

在 41 名高风险 CLL 患者中，持续完全缓解对应的输注前/输注产品 T 细胞具有早期记忆与 IL-6–STAT3 程序，而无应答产品偏向终末效应、糖酵解、耗竭、凋亡；CD27+CD45RO−CD8+ 细胞比例具有预测与功能意义。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 临床队列 | 41 名 advanced/high-risk CLL | CR 8、PR 5、转化后复发 PR_TD 3、NR 25 |
| 治疗 | CD19 CAR-T（CTL019，4-1BBζ） | 自体产品，患者/产品是统计单位 |
| 表达数据 | bulk RNA-seq | 不是 single-cell RNA-seq |
| 蛋白/功能 | 多色流式、单细胞 cytokine/polyfunctionality、代谢与小鼠转移 | 多模态但不是统一 cell barcode |
| 纵向 readout | 外周 CAR copy number、B-cell aplasia、临床应答 | qPCR/临床动力学与产品组学相连接 |
| 公开性 | processed counts/figure source in supplements | 原始 FASTQ 未在论文中给出公共 accession |

## 1. 研究要解决的问题

为什么同一类 CD19 CAR-T 在 CLL 中只有部分患者实现长期完全缓解？决定疗效的是患者肿瘤负荷、体内扩增，还是制造前/输注产品中 T 细胞的内在状态？

## 2. 方法框架

- 对 41 名接受 CTL019 的 CLL 患者按临床结局分组；
- 测量体内 CAR copy number 与 B-cell aplasia；
- 对 leukapheresis/infusion product 做 bulk RNA-seq；
- 深度流式量化记忆、耗竭和 CD27/PD-1/IL-6R 表型；
- 单细胞分泌/多功能性、葡萄糖摄取和 STAT3 磷酸化；
- IL-6/STAT3 阻断/补充以及患者来源 CAR-T 的 NSG leukemia model 验证。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这不是“单细胞图谱论文”，而是一套**临床分层的产品属性数据集**：每位患者的 CAR-T 扩增/持久性和应答，连接到产品 bulk transcriptome、流式细胞组成、功能分泌与代谢 readouts。单细胞部分主要是蛋白/分泌功能，不产生 scRNA expression matrix。

### 3.2 队列与结局组成

| 结局 | 人数 | 数据解释 |
|---|---:|---|
| CR | 8 | 持续完全缓解；强扩增与长期功能持久 |
| PR | 5 | 部分缓解 |
| PR_TD | 3 | 初期活性强，后发生 CLL 向侵袭性淋巴瘤转化并复发 |
| NR | 25 | 无应答；多数体内扩增有限 |
| 合计 | 41 | 不同 assay 的可用样本数可能小于 41，应逐图核对 n |

CAR peak expansion 中位数：CR 58,570 copies/μg genomic DNA，PR 13,257，PR_TD 130,258，NR 205。该数是 qPCR copy-number readout，不是细胞数。

### 3.3 分子与功能图谱组成

| 数据层 | 样本/内容 | 主要用途 |
|---|---|---|
| bulk RNA-seq | CR 与 NR 产品/刺激条件 | IL-6/STAT3、memory 对比 exhaustion/glycolysis/apoptosis |
| flow cytometry | premanufacture 与 infusion products | CD27+CD45RO−CD8+、PD-1、IL-6R 等细胞比例 |
| 单细胞功能分泌 | CAR-specific stimulation 后 cytokine/polyfunctionality | 测量“多功能强度”，不是 scRNA cluster |
| 代谢 | glucose analog uptake、2-DG 等 | 检验高糖酵解与晚期效应状态关系 |
| 纵向临床 | CAR qPCR、B-cell aplasia、response | 把产品特征与体内扩增/持久性关联 |
| 动物模型 | CR/NR 患者来源产品 | 验证产品内在抗肿瘤能力 |

### 3.4 数据在哪里

论文和 PMC 的 supplementary files 提供患者特征、临床结局、差异表达/通路、流式门和图源数据。后续综述资料将这套资源概括为“processed counts available in supplementary data”。截至核验，本论文正文的数据可用性信息未给出对应 GEO/SRA accession；因此不应从相似标题的后续 GEO 记录猜测原始数据入口。

特别注意：网络检索中可能出现 `GSE293878`，但该 accession 是 2026 年另一项“小鼠 CAR-T/巨噬细胞 iNOS”研究，与 Fraietta 2018 的 CLL 产品 bulk RNA-seq 无关。

### 3.5 如何获取

#### 路线 A：复用处理后表达和临床表

1. 从[PMC 页面](https://pmc.ncbi.nlm.nih.gov/articles/PMC6117613/)下载 Supplementary Information/Tables；
2. 从 Nature 页面下载 Source Data；
3. 建立 `patient_id–response_group–assay–sample_stage` 映射，核对每个图的实际 n。

#### 路线 B：需要原始 FASTQ

论文没有提供可直接匿名下载的 accession。应联系通讯作者/数据托管方请求原始 RNA-seq，并确认受试者同意与数据使用限制；不能把补充 counts 逆推出原始 reads。

### 3.6 下载后先做什么

先区分 leukapheresis、manufactured infusion product 与 stimulation 后样本；区分患者 response group 和 assay availability。差异表达应以患者为重复，不能将 technical replicate 或单细胞分泌 events 当成独立患者。

## 4. 主要发现

CR 产品富集 early-memory 与 IL-6/STAT3 程序；NR 产品富集 terminal differentiation、glycolysis、hypoxia、exhaustion 与 apoptosis。治疗前较高的 CD27+CD45RO−CD8+ 比例与持续缓解关联。

## 5. 分子驱动验证

加入 IL-6 可增强重复抗原刺激后的 CAR-T 扩增，STAT3 抑制则削弱扩增；2-DG 可推动更早期记忆样表型。去除预测性 CD8 亚群会削弱小鼠肿瘤控制，说明 marker 不只是统计相关。

## 6. 推荐图版

- Fig. 1：临床扩增、B-cell aplasia 与结局；
- 转录组/GSEA 图：CR memory/STAT3 vs NR exhaustion/glycolysis；
- CD27+CD45RO−CD8+ 流式图：产品 biomarker；
- 功能/动物图：从关联到机制验证。

## 7. 创新价值

1. 把临床应答追溯到制造前 T 细胞内在状态。
2. 将转录、表型、功能、代谢与体内动力学整合。
3. 提出可操作的产品富集与制造策略。

## 8. 局限性

1. 回顾性、单中心且不同 assay 的样本量不一致。
2. bulk RNA 无法分解细胞组成与细胞内程序。
3. CLL 和特定 CTL019 制造流程限制外推。
4. 原始测序数据没有清晰的公共 accession，复现受限。

## 9. 对本章节的作用

适合“quantitatively characterizing phenotypes/functions/markers”与“optimize conditions for navigating T-cell states”：它把 early-memory/STAT3 状态转化为产品选择和培养干预变量。

## 10. 可直接用于综述的观点

> CLL 中持久 CAR-T 应答与制造前早期记忆样 CD8 T cells 和 IL-6–STAT3 程序相连，而糖酵解、终末效应、耗竭和凋亡程序富集于无应答产品，提示优化起始细胞与代谢/细胞因子条件可在输注前导航产品状态（Nature Medicine 2018, Fraietta）。

## 11. 避免误读

- 不要把 bulk RNA-seq 写成 scRNA-seq。
- 不要把 qPCR copies/μg 写成 CAR-T 细胞数。
- 不要把 response-associated marker 直接视为跨疾病通用放行标准。
- 不要为该论文编造 GEO accession。

