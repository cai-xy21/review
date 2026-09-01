# 《Single-cell multiomic dissection of response and resistance to chimeric antigen receptor T cells against BCMA in relapsed multiple myeloma》精读

## 论文信息

- 作者：Maria Rade、Sebastian Grieb、Christian L. Schmid 等
- 期刊：*Nature Cancer*
- 年份：2024；5: 1048–1065
- DOI：[10.1038/s43018-024-00763-8](https://doi.org/10.1038/s43018-024-00763-8)
- PubMed：[PMID 38641734](https://pubmed.ncbi.nlm.nih.gov/38641734/)
- 数据：[GEO GSE234261](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE234261)；BioProject `PRJNA980643`
- 代码：[GitHub](https://github.com/fraunhofer-izi/Rade_Grieb_et_al_2023)

## 一句话结论

10 名 RRMM 患者的纵向 GEX/ADT/TCR/BCR 单细胞图谱显示，BCMA-CAR T 早期失败与 CD39⁺免疫抑制性单核细胞、受抑的 CD8/NK 生态以及高度扩增但功能障碍的 CAR T 克隆相关。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 患者 | 10 名 RRMM | ide-cel 8、cilta-cel 2；CR 5、非 CR 5 |
| 纵向采样 | leukapheresis，day 0/7/14/30/100；BM 约 day 30 | 单细胞集中于 leukapheresis 与约 day 30 |
| 单细胞生物样本 | 25 | PBMC/BMMC；不是 100 个独立样本 |
| 测序细胞 | 179,876 | 中位 8,018/样本，范围 1,559–11,085 |
| QC 后 | 约 97% | 约 17.4 万细胞 |
| 模态 | GEX + ADT + TCR + BCR | 10x 多文库同 barcode 设计 |
| GEO | 100 个 GSM | 25 生物样本 × 4 模态 |
| SRA | 332 experiments，约 4.41 TB | experiments 也不是患者/样本数 |

## 1. 研究要解决的问题

哪些治疗前和治疗后免疫状态区分 BCMA-CAR T 的完全响应与早期失败？作者不仅观察 CAR T，还同时测量外周血和骨髓中的 T、NK、B 与髓系细胞，并加入受体克隆信息。

## 2. 方法框架

- 10 名重度经治 RRMM，ide-cel 或 cilta-cel；
- 多时间点外周血和约 day-30 骨髓；
- 10x GEX、表面 ADT、TCR、BCR 四模态；
- 响应组与非完全响应/早期进展组比较；
- 分析 CAR 克隆扩增、耗竭/细胞毒程序、单核细胞 CD39 及 NK/CD8 功能。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

数据具有三个嵌套层级：

1. **临床纵向样本层**：PB 在 leukapheresis、day 0、7、14、30、100；BM 在 24–79 天附近；
2. **单细胞深描层**：主要取 leukapheresis 与约 day 30 的 PBMC/BMMC，共 25 个生物学样本；
3. **四模态文库层**：每个生物样本拆成 GEX、ADT、BCR、TCR，因此 GEO 显示 100 个 GSM。

图谱覆盖治疗性 CAR T 与内源免疫细胞。CAR 序列/克隆分析是其中一部分，不能把全部约 18 万细胞都称为 CAR T。

### 3.2 多大规模、覆盖哪些临床与组织情境

| 层级 | 规模/内容 | 应如何理解 |
|---|---:|---|
| 患者 | 10 | ide-cel 8、cilta-cel 2 |
| 结局 | 完全响应 5；亚优/非 CR 5 | 非 CR 包括 day 30 未达 CR 或 4 个月内进展 |
| 单细胞生物样本 | 25 | leukapheresis 与约 day 30 的 PB/BM |
| 初始细胞数 | 179,876 | 每样本中位 8,018，范围 1,559–11,085 |
| QC 保留率 | 97% | 约 174,000–175,000 个细胞，精确数应从对象实算 |
| 组织 | PBMC + BMMC | 外周与骨髓环境同时纳入 |
| 模态 | 4 | RNA、ADT、TCR、BCR |

患者数、25 个生物样本、100 个 GEO GSM、332 个 SRA experiments 是四种不同计数层级。论文中任何比例统计都应回到患者层评估不确定性。

### 3.3 GEO/SRA 公共仓库组成

GSE234261 对应 BioProject `PRJNA980643`：

- GEO 当前列出 **100 个 GSM library records**；
- 其结构可还原为 25 个生物样本各 25 个 GEX、25 个 ADT、25 个 BCR、25 个 TCR 记录；
- GEO supplementary 合计约 **1,369 MB**，以样本级 Matrix Market 文件为主：`matrix.mtx.gz`、`barcodes.tsv.gz`、`features.tsv.gz`；
- 原始数据在 SRA；BioProject 当前显示 **332 experiments**，约 **4.41 TB / 13,180 Gb**；
- 代码仓库提供分析脚本及图形复现流程。

100 与 332 的差异主要来自平台对 library/run/file 的拆分方式，不能据此判断有 100 或 332 个生物标本。

### 3.4 如何获取：按目的选择

- **快速重分析表达/蛋白**：从 GEO 下载处理后 Matrix Market 三件套；按 GSM 标题将四模态归并到 25 个样本。
- **重建 V(D)J 与原始 QC**：通过 SRA run selector 导出 332 条记录，先按 BioSample/library strategy 分组；约 4.41 TB，需服务器和分批下载。
- **复现论文图表**：clone GitHub，并固定 commit/软件版本；不要只复制 notebook 输出。
- **分析 CAR clonotype**：先确认哪些 contig 含 CAR construct/可定义 CAR⁺ barcode，再在患者内计算扩增度。

### 3.5 下载后先做什么

1. 生成 25 行的生物样本主表，再把 100 GSM 与 332 experiments 映射其上；
2. 验证各模态 barcode 交集和去重规则；
3. 区分 leukapheresis、PB day 30 与 BM day 30；
4. 记录 ide-cel/cilta-cel 产品差异，但小样本下不做产品优劣结论；
5. 以患者为统计单位比较 CD39 单核、CD8/NK 状态和 CAR clone expansion。

## 4. 主要发现

非响应者富集 CD39⁺免疫抑制性单核细胞，并呈 CD8/NK 功能受抑；高度扩增的 CAR T clonotypes 具有更强细胞毒同时也更明显的耗竭程序。PD-1 等轴被提出为潜在干预节点。

## 5. 与“状态导航”最相关的分析

该研究说明导航系统需要同时观测治疗细胞和宿主生态：相同 CAR T 扩增可能在支持性或抑制性髓系环境中走向不同功能结局。

## 6. 推荐图版

- 队列、采样和四模态设计总览。
- PB/BM 免疫图谱及响应组组成差异。
- CAR clonotype 扩增—耗竭与 CD39⁺单核关联图。

## 7. 创新价值

四模态、双组织、治疗前后设计使细胞状态、受体克隆和微环境可在统一患者坐标中分析；数据与代码均公开。

## 8. 局限性

10 人且混合两种产品；骨髓时间窗为 24–79 天；观察性关联；原始数据量大、仓库记录层级复杂，复现门槛较高。

## 9. 对本章节的作用

适合用于“构建实时优化系统”的输入设计：至少要联合 CAR 克隆、宿主 CD8/NK 与髓系免疫抑制状态。

## 10. 可直接用于综述的观点

> BCMA-CAR T 的早期失败并非单一治疗细胞缺陷，而与 CD39⁺抑制性单核细胞、受抑的 CD8/NK 生态及扩增但功能障碍的 CAR 克隆共同相关（Nature Cancer 2024, Rade）。

## 11. 避免误读

- 不要把 100 个 GSM 或 332 个 experiments 写成患者数。
- 不要把全部 179,876 个细胞称为 CAR T。
- 不要在 n=8 与 n=2 的基础上比较 ide-cel 与 cilta-cel 优劣。
