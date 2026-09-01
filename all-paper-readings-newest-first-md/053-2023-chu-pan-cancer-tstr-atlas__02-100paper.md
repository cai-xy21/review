# 《Pan-cancer T cell atlas links a cellular stress response state to immunotherapy resistance》精读

## 论文信息

- **作者**：Chu Y, et al.
- **期刊与年份**：Nature Medicine, 2023
- **DOI**：[10.1038/s41591-023-02371-y](https://doi.org/10.1038/s41591-023-02371-y)
- **原文**：[Nature Medicine](https://www.nature.com/articles/s41591-023-02371-y)
- **PubMed**：[PMID 37248301](https://pubmed.ncbi.nlm.nih.gov/37248301/)
- **开放全文**：[PMC11421770](https://pmc.ncbi.nlm.nih.gov/articles/PMC11421770/)
- **交互数据库**：[Single Cell Research Portal / TCM](https://singlecell.mdanderson.org/TCM/)
- **代码**：[TCM](https://github.com/Coolgenome/TCM)；空间分析参考 [TESLA](https://github.com/jianhuupenn/TESLA)
- **本报告核对日期**：2026-08-23

## 一句话结论

本文整合 27 个单细胞数据集、16 种癌症及正常/非肿瘤组织的 308,048 个高质量 T 细胞，定义 32 个状态并识别 HSPA1A/HSPA1B 高表达的 T cell stress response（TSTR）状态；独立 ICB、基因组、空间转录组和组织学分析显示 TSTR 与淋巴聚集结构及免疫治疗耐受相关。

## 数据护照

| 项目 | 内容 |
|---|---|
| 核心 atlas | 27 个 scRNA 数据集；16 种癌症；9 类正常/非肿瘤组织 |
| 样本/个体 | 486 个样本，324 名个体 |
| 高质量 T 细胞 | 308,048 |
| 广义类型 | CD4、CD8、γδ T、NKT、MAIT、proliferative，共 6 类 |
| 精细状态 | 32 个 T cell states |
| 基因组/临床关联 | 23 个队列、375 名患者，其中 171 名接受 ICB |
| 独立 ICB 单细胞验证 | 6 个队列、4 个癌种、133 名患者、247 个样本 |
| 空间验证 | CosMx、Visium、公开空间转录组与病理图像，覆盖多癌种 |
| 数据发布形态 | 无单一“308,048 细胞总下载包”说明；分散在 EGA、GEO、SCP、空间数据站点与门户 |
| 核心资源 | TCM portal + GitHub code + 论文 Data availability 中各原始 accession |

## 1. 研究问题

泛癌 T 细胞图谱中是否存在被常规聚类忽略、但与免疫治疗耐受相关的共享状态？细胞压力反应是制样伪影、短暂激活还是可重复的组织生态状态？能否利用独立 ICB 队列、空间定位和临床信息验证其意义？

## 2. 方法框架

1. 整合 27 个公开/自有 scRNA 数据集并重新提取 T 细胞。
2. 分层聚类 CD8、CD4、非经典 T 和增殖细胞，定义 32 个状态。
3. 识别以 HSPA1A/HSPA1B 等热休克基因为核心的 TSTR 状态。
4. 在基因组/临床队列及独立 ICB scRNA 队列中测试状态频率与疗效关联。
5. 用 CosMx、Visium、其他空间数据和组织图像确定 TSTR 的组织位置与细胞邻域。

## 3. 数据规模与图谱组成

### 3.1 核心整合 atlas 的口径

| 层级 | 规模 | 说明 |
|---|---:|---|
| scRNA 数据集 | 27 | 跨研究、平台和组织的整合数据 |
| 癌种 | 16 | 多种实体瘤与血液/淋巴系统肿瘤背景 |
| 正常/非肿瘤组织类型 | 9 | 用于区分肿瘤富集与普遍组织状态 |
| 样本 | 486 | 可含同一人的不同组织/时间点 |
| 个体 | 324 | 正确的统计独立层级之一 |
| 高质量 T 细胞 | 308,048 | 论文核心 atlas，不应与门户后续扩容数字混用 |
| 广义 cell types | 6 | CD4、CD8、γδ、NKT、MAIT、proliferative |
| 精细 states | 32 | 分层聚类后统一命名的状态 |

### 3.2 CD8、CD4 与非经典 T 状态组成

**CD8 共 14 个状态/cluster**：

- naïve：c3、c13；
- transitional effector：c0；
- effector：c2、c8、c10、c11；
- central memory（TCM）：c6；
- tissue-resident memory（TRM）：c12；
- stress response（TSTR）：c4；
- interferon-stimulated（TISG）：c5；
- senescent-like（TSEN）：c9；
- precursor exhausted（p-TEX）：c7；
- exhausted（TEX）：c1。

**CD4 共 12 个主要状态**，并对 Treg 再分为 7 个子状态、Tfh 分为 5 个子状态。非经典 T 细胞分为 5 个状态，增殖 T 细胞分为 8 个状态。这里的子聚类数与 32 个统一主状态存在层级关系，不能简单相加为总状态数。

### 3.3 临床与 ICB 验证数据

| 分析层 | 规模 | 用途 |
|---|---:|---|
| 基因组/通路/临床 | 23 个队列、375 名患者 | 关联状态频率、分子背景与结局 |
| 其中 ICB 患者 | 171 | 评估免疫治疗相关性 |
| 独立 ICB scRNA | 6 个队列、4 个癌种、133 名患者、247 样本 | 检验治疗前后及响应者/不响应者的 TSTR 变化 |

独立 ICB 队列包含 GSE123814、GSE179994、GSE173351、GSE169246、SCP1288 和 EGAS00001004809。它们是论文复用的验证数据，不应重复计入 308,048 核心 atlas，除非按作者的具体去重口径重建。

### 3.4 自有数据 accession

论文 Data availability 将新生成/自有数据分散发布：

| 数据 | accession | 访问方式 |
|---|---|---|
| AML | EGA `EGAD00001007672` | 受控访问 |
| lung/LC1 与 LUAD Visium | EGA `EGAS00001005021` | 受控访问；含空间数据 |
| lymphoma LN2 | EGA `EGAS00001006052` | 受控访问 |
| healthy PBMC | EGA `EGAD00001006994` | 受控访问 |
| GBM | GEO `GSE222522` | 开放处理数据/按 GEO 页面下载 |
| BRCA2、LC5、OV、STAD | GEO `GSE222859` | 开放处理数据/按 GEO 页面下载 |
| reactive LN | GEO `GSE203610` | 开放处理数据/按 GEO 页面下载 |

EGA study 与 dataset 的编号层级不同；申请时应从论文给出的 study/dataset 页面确认 Data Access Committee 和可下载文件。

### 3.5 空间数据组成

空间验证不是单一平台：

- **CosMx**：NSCLC、HCC 等公开 Nanostring 数据；
- **Visium**：LUAD（EGAS00001005021）及其他队列；
- **乳腺癌 HER2ST**：公开 GitHub 空间资源；
- **melanoma**：Spatial Research 数据；
- **CSCC**：GSE144240；
- **ccRCC**：GSE175540。

作者将 TSTR signature 映射到空间位置，观察其与淋巴细胞聚集/TLS-like 区域的关联。不同平台的分辨率从单细胞近似到多细胞 spot 不等，不能统一解释为每个点都是一个 T 细胞。

### 3.6 如何获得完整分析数据

本文没有在 Data availability 中给出一个像 Zenodo tarball 那样的“308,048 细胞总对象”。建议按用途分三步：

1. 在 [TCM portal](https://singlecell.mdanderson.org/TCM/) 浏览状态、基因和数据来源，导出门户允许的表格。
2. 从 [TCM GitHub](https://github.com/Coolgenome/TCM) 获取分析代码、对象说明和 accession 映射。
3. 对每个原始队列分别从 GEO/SCP 开放下载，或向 EGA 申请；依据论文 metadata 统一 cell ID、patient、sample、cancer type 和 state label。

按目标选数据：

| 目标 | 最小数据集合 | 主要风险 |
|---|---|---|
| 查看 32 状态标志 | TCM portal/代码中的 marker 表 | 门户版本可能更新 |
| 复现 308,048 atlas | 27 个源数据 + 作者 metadata/code | 下载量大、版本和批次处理复杂 |
| 检验 TSTR 与 ICB | 6 个独立 ICB accession | 要按患者建模，避免细胞伪重复 |
| 复现空间定位 | 对应空间仓库 + signature/code | 平台分辨率不同，需各自 QC |
| 使用自有原始数据 | EGA accession | 需审批/MTA/数据安全流程 |

GEO 示例：

```powershell
# 先在对应 GEO 页面查看 supplementary 文件，再用其 FTP 链接下载。
Start-Process "https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE222859"
```

## 4. TSTR 状态的定义

TSTR 以 HSPA1A、HSPA1B 等热休克/应激反应基因为核心，并在 CD4/CD8 中均可见。作者通过跨数据集复现、临床关联和空间富集来论证它不是单一批次伪影。但组织解离本身也能诱导应激基因，任何复用都应加入解离时间、缺血时间、平台和样本质量协变量。

## 5. TSTR 与 ICB 耐受

在多个独立 ICB 队列中，TSTR 比例在治疗后上升，并在不响应者中更明显；因此作者将其与耐受联系起来。该证据是跨队列关联，不表示 HSPA1A/B 单独造成耐药。TSTR 可能反映高抗原负荷、炎症、代谢压力或组织结构环境。

## 6. 空间组织结构

TSTR 细胞常位于淋巴聚集/TLS-like 区域，提示压力状态可能由局部细胞密度、炎症信号、营养/氧环境或反复刺激共同塑造。空间证据把状态从“表达 cluster”连接到微环境条件，但 Visium spot 的细胞混合仍需去卷积与病理核验。

## 7. 主要生物学发现

- 泛癌 T 细胞可组织为 32 个状态，包含传统记忆/效应/耗竭和 TSTR 等共享状态。
- TSTR 跨 CD4/CD8 和癌种出现，并与 ICB 后增加、不响应相关。
- TSTR 在空间上与淋巴聚集/TLS-like 结构相关。
- 状态—疗效关系同时受细胞内程序和组织生态影响。

## 8. 推荐精读图

- 27 数据集、308,048 细胞和 32 states 总览。
- CD8 14 状态及 TSTR marker 图。
- 6 个独立 ICB 队列的 TSTR 变化图。
- CosMx/Visium 中 TSTR 与淋巴聚集结构的空间图。

## 9. 方法学创新

1. 大规模泛癌整合后进一步系统识别压力反应状态。
2. 以独立 ICB 单细胞队列验证治疗关联。
3. 结合空间组学和病理结构，给状态增加组织生态坐标。
4. 建立可查询门户，支持跨数据集状态比较。

## 10. 局限性

- 27 个源数据平台和制样差异大，应激基因尤其易受预分析过程影响。
- 无单一、静态、完整的 308,048 细胞下载包，复现成本高。
- 部分自有数据受控，需 EGA 审批或 MTA。
- ICB 关联不能证明 TSTR 是耐药的充分或必要原因。
- 空间平台分辨率不一致，TLS-like 定位需病理和蛋白层验证。

## 11. 在综述架构中的位置

本文可作为“**spatial omics**”“**optimize the conditions for navigating T cell states**”与“**build real-time optimization systems**”的连接点：TSTR 可作为制造或体内监控的警戒状态，但需要实时传感器区分可逆短暂压力与稳定耐受程序，并以功能输出而非热休克基因单独闭环控制。

## 12. 可直接写入综述的表述

> Chu 等整合 27 个数据集、324 名个体的 308,048 个 T 细胞，定义 32 个泛癌状态，并在独立 ICB 与空间队列中将 HSPA1A/HSPA1B 高表达的 TSTR 状态连接到治疗后富集、不响应及淋巴聚集结构，为把细胞内压力程序纳入 T 细胞状态导航提供了跨模态依据。

## 13. 避免误读

- 308,048 是论文核心 atlas 口径，不应与门户后续可能扩容的数据量混用。
- Treg 7 子群、Tfh 5 子群等是层级细分，不能与 32 主状态直接相加。
- TSTR 与耐受相关，但不等于 HSPA1A/B 是已验证的耐药因果开关。
- 公开数据分散在多个仓库；门户不等于原始数据永久归档。
