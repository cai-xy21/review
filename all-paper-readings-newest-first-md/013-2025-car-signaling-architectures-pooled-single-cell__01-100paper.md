# 《Dissecting the role of CAR signaling architectures on T cell activation and persistence using pooled screens and single-cell sequencing》精读

## 论文信息

- 作者：Rocío Castellanos-Rueda、Kai-Ling K. Wang、Juliette L. Forster 等
- 期刊：*Science Advances*
- 年份：2025；11(7): eadp4008；发表于 2025 年 2 月 14 日
- DOI：10.1126/sciadv.adp4008
- 原文：[Science Advances](https://www.science.org/doi/10.1126/sciadv.adp4008)
- PubMed：[PMID 39951542](https://pubmed.ncbi.nlm.nih.gov/39951542/)
- 本研究新数据：[GEO GSE262686](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE262686)
- 外部临床参照：[GEO GSE197215](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE197215)
- 补充材料：论文 Figs. S1–S13、Tables S1–S5

## 一句话结论

作者将32种抗HER2 CAR信号架构以TRAC定点整合构成pooled library，在12天反复抗原刺激中联合功能分选、scRNA-seq、20抗体CITE-seq和单细胞CAR条形码测序，获得62,934个带CAR身份的细胞（58,949个平衡后分析细胞），显示膜近端结构域主导状态，CD40最能维持强而持久的反应，CTLA4则偏向长期细胞毒表型。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| CAR library | 32种 | 1个一代、5个二代、25个三代、1个无信号对照 |
| 组合结构域 | CD28、4-1BB、CD40、IL15RA、CTLA4 | 所有CAR共享anti-HER2 scFv、CD28 TMD和CD3ζ |
| 工程方式 | CRISPR/Cas9 HDR定点整合TRAC | 同时敲除TCR并统一CAR转录位置 |
| RAS | 12天，每3天补SKBR3；1:1 E:T | day3/6/9/12功能追踪；day0/6/12做单细胞组学 |
| 单细胞供者 | 2名健康供者 | 不应把多个lane/样本当成独立供者 |
| 带CAR身份细胞 | 62,934 | 质控后且成功scCARseq注释 |
| 平衡后分析细胞 | 58,949 | 对过丰CAR变体下采样后的主图数据 |
| 模态 | GEX + 20-plex ADT + scCARseq | 同一捕获的三类library在GEO各记为GSM |
| GEO规模 | GSE262686，48个GSM | 16个捕获单元×3种library，不是48个生物样本 |
| 处理对象 | `GSE262686_Pre_processed.rds.gz`，约769 MB | 快速复用首选 |
| 外部患者数据 | GSE197215；原文101,326细胞/12名ALL患者 | 本文取61,589细胞用于映射，不是本研究新测 |

## 1. 研究要解决的问题

CAR胞内信号域的种类、数量和排列顺序会改变激活强度、分化、耗竭和持久性，但传统逐个构型实验只能测试少数候选，而且常只测一个终点。本文要回答：

1. 五类共刺激域如何组合成可比较的信号架构空间；
2. 膜近端与膜远端位置是否产生不同状态；
3. 早、中、晚期反复抗原刺激中，构型诱导的状态如何演变；
4. 体外状态能否映射到患者输注产品的临床相关表型。

## 2. 方法框架

### 2.1 32种CAR的系统组合

library含：

- 1个一代CAR：CD3ζ；
- 5个二代CAR：五种共刺激域之一 + CD3ζ；
- 25个三代CAR：五种域在A（膜近端）和B（膜远端）位置的全排列 + CD3ζ；
- 1个无信号CAR（NS-CAR）。

所有构型使用相同anti-HER2 scFv、CD28 hinge/TMD和TRAC定点整合，以减少表达位点和外部识别差异。每个构型在3′UTR含可测序barcode；long-read验证91.3%的barcode与正确CAR序列配对。

### 2.2 反复抗原刺激与多模态读取

CAR-T与HER2⁺ SKBR3按1:1共培养，12天内每3天补靶细胞。day9用CD107a或IFN-γ分选做功能富集；day0、6、12在与SKBR3刺激6小时后进行：

- 10x 3′ scRNA-seq；
- 20种TotalSeq-B抗体的CITE-seq；
- 从同一cDNA扩增CAR 3′UTR barcode的scCARseq。

只有至少2个UMI支持同一CAR身份的细胞才接受注释。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

本文产生的是“构型—时间—单细胞状态”图谱。每个细胞理论上可连接：

1. 转录表达矩阵；
2. 20个表面蛋白ADT；
3. 32种CAR之一；
4. CD4/CD8、供者、RAS时间点；
5. 聚类、gene-set score、pseudotime和功能富集结果。

此外还有bulk层面的CAR barcode深测序、CD107a/IFN-γ FACS、肿瘤细胞生长和流式时间序列。单细胞组学并不直接记录每个细胞的实时杀伤轨迹；其功能连接来自平行pooled screen和群体表型。

### 3.2 32种CAR library组成

| 类别 | 数量 | 构型逻辑 |
|---|---:|---|
| 无信号对照 | 1 | 无胞内信号域，用于识别旁分泌/背景激活 |
| 一代 | 1 | CD3ζ only |
| 二代 | 5 | CD28、4-1BB、CD40、IL15RA或CTLA4 + CD3ζ |
| 三代 | 25 | 5×5有序组合；A为膜近端，B为膜远端 |

三代构型允许同一域重复，因此是25而不是不放回排列20。位置效应是论文核心：膜近端域对转录表型贡献通常大于膜远端域。

### 3.3 单细胞规模

- 2名健康供者；
- 3个主要RAS时间点：day0、day6、day12；
- 每个10x lane目标上样20,000细胞；
- 62,934个细胞通过质控并获得单一CAR身份；
- 为防止反复刺激造成某些CAR克隆过度扩增，每个供者×时间样本中，超过理论均衡比例2倍的变体被下采样，最终58,949个细胞用于主分析；
- 无监督分析得到16个cluster，覆盖CD4/CD8记忆、激活、细胞毒、晚期效应/耗竭样和增殖状态；
- 32种CAR在两供者和各时间点均有覆盖，但覆盖深度不完全相同。

### 3.4 GEO GSE262686 为什么是48个GSM

GEO列出48个GSM，结构是16个单细胞捕获单元分别产生3种library：

| library type | GSM数 | 内容 |
|---|---:|---|
| GEX | 16 | 10x基因表达原始测序 |
| CITE_ADT | 16 | 20种抗体标签读段 |
| CAR/scCARseq | 16 | 单细胞CAR barcode读段 |

16个捕获单元按Donor 09/17、T0/T6/T12及若干技术lane编号（A、B、E、G等）组织。因此48不能解释为48名供者或48个独立条件。

### 3.5 GEO公开文件组成

GEO补充目录提供：

- 每个捕获单元的 `barcodes.tsv.gz`、`features.tsv.gz`、`matrix.mtx.gz`；
- 16个稀疏矩阵，压缩后约35–159 MB/个；
- `GSE262686_Pre_processed.rds.gz`，约769 MB，含作者预处理Seurat对象；
- `GSE262686_CAR_barcode_identity.csv.gz`：CAR barcode与构型映射；
- `GSE262686_Feature_reference.csv.gz`：ADT/feature reference；
- `GSE262686_RAW.tar`约1.8 MB，主要汇总各GSM的assigned CAR barcode CSV，并不等于全部FASTQ；
- 原始测序通过SRA/BioProject `PRJNA1092996`关联。

矩阵文件合计下载量超过1.5 GB；若只做作者层面的复用，优先769 MB RDS。若要重新做细胞过滤、ADT去噪或CAR assignment，应下载原始library/SRA及全部辅助映射文件。

### 3.6 外部GSE197215的正确定位

GSE197215来自Bai等研究：12名儿童ALL患者的CAR-T输注产品，在CD19-3T3、mesothelin-3T3、CD3/CD28和未刺激四条件下做scRNA/CITE-seq，原文共101,326个细胞、120个GSM。本文只下载其中CD19-3T3刺激和未刺激数据，共61,589个细胞（30,484 stimulated +31,105 unstimulated），把本研究的16类表型转移过去并与临床结局比较。

因此，`GSE197215`是external clinical reference，不是本研究32种HER2 CAR library的新数据。

## 4. 如何获取与复用

### 4.1 快速复用作者对象

```bash
wget -c https://ftp.ncbi.nlm.nih.gov/geo/series/GSE262nnn/GSE262686/suppl/GSE262686_Pre_processed.rds.gz
wget -c https://ftp.ncbi.nlm.nih.gov/geo/series/GSE262nnn/GSE262686/suppl/GSE262686_CAR_barcode_identity.csv.gz
wget -c https://ftp.ncbi.nlm.nih.gov/geo/series/GSE262nnn/GSE262686/suppl/GSE262686_Feature_reference.csv.gz
```

```r
library(Seurat)
obj <- readRDS("GSE262686_Pre_processed.rds.gz")
obj
colnames(obj@meta.data)
table(obj$Donor, obj$Time)
table(obj$CAR)
```

字段名应以对象实际内容为准，不要照论文图例猜测。

### 4.2 从稀疏矩阵重建

每个捕获单元下载三件套后用 `Read10X()`读取；ADT可能与GEX保存在feature type中，需检查features第二/三列。随后另行合并CAR assignment，并验证cell barcode前缀/后缀是否与GEX一致。若要严格重跑scCARseq UMI阈值，应使用SRA原始读段，而不是只用最终assignment CSV。

### 4.3 复现临床映射

从GSE197215下载作者提供的4个整合RDS。本文使用CD19-3T3和unstimulated对象，通过Seurat anchors/label transfer投射本研究状态。必须保留patient为统计层级，不能把61,589个细胞当作独立临床重复。

## 5. 主要状态与构型发现

CD40，尤其位于膜近端A位置时，在day9 CD107a/IFN-γ screen和单细胞状态中表现出最强持久性与效应维持。CTLA4虽然是天然抑制受体，其胞内尾嵌入CAR后并非简单抑制，而更偏向持久细胞毒状态；CD28产生强但较短暂激活，4-1BB偏向效应记忆。IL15RA胞内域贡献较弱，可能更像结构间隔而非强共刺激模块。

关键不是给各结构域排单一名次，而是显示域的作用依赖位置、搭档、时间和CD4/CD8背景。

## 6. 时间状态转换

day0至day12，CD8细胞从TCF7/CCR7/LEF1/SELL记忆样状态转向TNFRSF9/TBX21激活、GZMB/PRF1细胞毒，再到HOPX/ENTPD1/LAG3/HAVCR2等晚期效应/耗竭样状态。CAR结构改变各状态的占比与转换速度，但快照、pseudotime和构型富集仍不能直接证明单细胞的真实连续命运。

## 7. 推荐图版

- **Fig. 1**：32种CAR库、TRAC整合与barcode均衡性。
- **Fig. 2**：12天RAS和day9功能筛选。
- **Fig. 3**：58,949细胞的GEX/CITE/CAR图谱；最适合讲状态景观。
- **Fig. 4–5**：结构域位置、时间与CD4/CD8表型关联。
- **Fig. 6**：向ALL患者输注产品的标签映射；适合讲临床桥接及其限制。

若只能选一组，选Fig. 1A + Fig. 3A/B + Fig. 4的domain-position总结。

## 8. 创新价值

1. 把CAR构型身份直接连接到同一细胞的RNA和20-plex蛋白。
2. 用TRAC定点整合降低随机表达位点造成的构型比较偏差。
3. 以时间序列RAS而非单次刺激评估持久性。
4. 用NS-CAR显式估计pooled系统中的旁分泌背景。
5. 公开处理RDS、稀疏矩阵和CAR barcode映射，复用门槛较低。

## 9. 局限性

1. 单细胞组学只有2名供者，供者间泛化有限。
2. 体外RAS缺少肿瘤微环境、空间迁移和免疫抑制网络。
3. pooled培养存在旁分泌和竞争，表型可能不是完全细胞自主。
4. 32种构型是系统但有限的离散库，不能覆盖motif、linker、表达量等连续设计空间。
5. scRNA/CITE快照没有直接测同一细胞的杀伤、细胞因子和长期存活。
6. 向GSE197215的转录相似性映射不能替代所设计CAR的患者实验。
7. 下采样改善构型平衡，但也丢弃真实扩增信息；做fitness分析时应回到未下采样数据。

## 10. 对本综述架构的作用

该文连接“perturb/manipulate cell states”“quantitatively characterizing phenotypes”与“optimize conditions”。它把CAR信号架构视为可调控制输入，把转录/蛋白状态和功能持续性视为输出，并用时间维度评估动态响应，是构建状态导航设计规则的重要案例。

## 11. 可直接用于综述的观点

> 对32种TRAC定点整合的CAR架构进行反复抗原刺激、功能分选和GEX/CITE/CAR条形码联合单细胞测序表明，膜近端共刺激域对T细胞状态的影响大于膜远端域，CD40最能维持强而持续的响应；这说明CAR信号模块的“位置与时间”应作为导航细胞状态的设计变量，而非仅比较共刺激域名称（Science Advances 2025, Castellanos-Rueda）。

## 12. 避免误读

- 不要把48个GSM写成48个独立生物样本；它们是16捕获×3 library。
- 不要把GSE197215写成本研究新生成的数据。
- 不要把58,949个分析细胞与62,934个已注释细胞混用而不说明下采样。
- 不要用pseudotime直接宣称某CAR驱动确定的单向谱系。
- 不要把CD40在本体外体系中的优势直接等同于临床最佳CAR。

