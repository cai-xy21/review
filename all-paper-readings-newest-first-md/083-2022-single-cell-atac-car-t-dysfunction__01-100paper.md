# 《Single-cell ATAC-seq maps the comprehensive and dynamic chromatin accessibility landscape of CAR-T cell dysfunction》精读

## 论文信息

- **作者/期刊/年份**：Jiang et al., *Leukemia*, 2022
- **DOI / PMID**：[10.1038/s41375-022-01676-0](https://doi.org/10.1038/s41375-022-01676-0) / 35962059
- **正文**：[Nature文章页](https://www.nature.com/articles/s41375-022-01676-0)
- **新生成scATAC数据**：[CNP0002442](https://db.cngb.org/search/project/CNP0002442/)，CNGBdb/CNSA
- **复用bulk RNA数据**：[GSE151774](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE151774)

## 一句话结论

抗原刺激后的CAR-T从短时激活进入中间和终末耗竭时，BATF/IRF4主导的可及性网络持续增强；降低BATF或IRF4能改善细胞因子、持久性和肿瘤控制，说明scATAC图谱可用于寻找状态操纵靶点。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 体外模型 | 抗CD19、4-1BB-CD3ζ CAR-T与Nalm6共培养 |
| 体外时间点 | 0 h、6 h、48 h |
| 体外scATAC | 12,617个QC后CAR-T；中位22,232个唯一比对片段/细胞；12个亚群 |
| 临床验证 | 2例抗BCMA CAR-T治疗的复发/难治多发性骨髓瘤患者 |
| 临床scATAC | 峰值扩增期和晚期下降期，QC后10,929个CAR-T |
| 合计 | **23,546个scATAC细胞** |
| 新数据下载 | CNGBdb/CNSA项目CNP0002442；项目内继续按sample/run下载 |
| 外部表达支持 | GSE151774，8个bulk RNA-seq样本，不是本研究scATAC的表达配对层 |

## 1. 研究要解决的问题

作者希望以单细胞染色质层面回答：CAR-T从静息、激活到功能障碍经历哪些状态；体外形成的耗竭调控轴能否在患者输注后CAR-T中观察到；哪些转录因子是关联标志，哪些经过敲低后能改变功能。

## 2. 实验与分析框架

体外anti-CD19/4-1BB CAR-T在Nalm6刺激前、刺激中及抗原清除后做scATAC。作者以LSI/聚类、基因活性、motif deviation和轨迹分析定义状态，再与外部RNA、ChIP-seq及ENCODE表观数据交叉。随后在两例anti-BCMA患者的扩增峰与晚期采血中复现调控特征，并用BATF/IRF4敲低检验功能。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

主体数据是两套scATAC图谱：一套为受控体外抗原刺激时间序列，一套为患者体内纵向验证。每个细胞的直接观测量是开放染色质片段，不是RNA表达；文中“gene activity”由邻近开放信号推断，不能等同于实测转录本。

### 3.2 多大规模、覆盖哪些生物情境

| 图谱层 | 样本/时间构成 | QC后细胞 | 状态组成 |
|---|---|---:|---|
| 体外CD19 CAR-T | 0 h未刺激、6 h刺激中、48 h抗原清除后 | **12,617** | naïve CD4；Th17；Tfh1/Tfh2；naïve CD8 T1/T2；memory CD8；effector CD8；intermediate exhausted；terminal exhausted；mitotic；other/abnormal，共12类 |
| 患者BCMA CAR-T | 2例患者，各2个时段；扩增峰分别约day 10/day 13，晚期约day 29/day 33 | **10,929** | 文中称7类，但正文明确命名6个生物学状态：CD4 Tcm/Tem、CD8 Tcm/Tem/Teff/exhausted；应以作者对象/元数据中的cluster标签确认第7簇 |
| 合计 | 两种CAR靶点、体外与体内 | **23,546** | 可用于调控模块复现，不宜直接合并估计治疗效应 |

体外每细胞中位22,232个唯一比对片段，为判断数据深度的重要指标。临床样本QC包括peak-region fragments 5,000–50,000、blacklist ratio <0.01、nucleosome signal <10、reads in peaks >20%、线粒体比例<20%、TSS enrichment >2；少于10个细胞出现的峰及chrY峰被过滤。

### 3.3 公共数据包有什么

- **CNP0002442**：本研究scATAC项目入口。CNGBdb页面按项目—样本—实验/run组织，适合取得原始测序文件及项目元数据。数据库页面会更新文件清单，下载前应以当前manifest为准；本文不虚构未在论文中列明的文件大小。
- **GSE151774 / SRP266339**：作者复用的既往bulk RNA-seq，共8个样本：control、Nalm6+CAR-T、Nalm6+dasatinib、Nalm6+imatinib，各2重复。处理后`all_samples_FPKM.csv.gz`约726.6 KB、`gene_raw_counts_matrix.csv.gz`约2.5 MB，原始FASTQ在SRA。它用于表达层支持，并非同一细胞的multiome。
- 外部调控层包括BATF/IRF4/JUN ChIP-seq（如GSM2905796、GSM4059676、GSM4059674）及ENCODE开放/组蛋白修饰文件；这些是复用数据，不应计入23,546个新scATAC细胞。

### 3.4 如何获取：按你的目的选择

#### 路线A：下载本研究scATAC

打开CNP0002442，展开sample/run，导出项目文件清单后再批量下载。先保存项目与样本元数据，确保FASTQ能够回填`in vitro/clinical、patient、time point、replicate`。如网页要求登录或专用客户端下载，只记录步骤即可；未经确认不要代为注册账号。

#### 路线B：复用外部bulk RNA矩阵

在GSE151774直接下载两个CSV压缩矩阵；若要重做比对，从SRA Run Selector下载FASTQ。分析时将其标为“external bulk RNA support”，不可与scATAC按细胞配对。

#### 路线C：重做scATAC分析

用对应版本Cell Ranger ATAC生成fragment/peak matrix，再按文中阈值做QC；用Signac/ArchR完成TF-IDF/LSI、聚类、motif deviation和轨迹。为了跨项目比较，建议重新构建统一peak set而不是直接拼接作者峰矩阵。

### 3.5 下载后先做什么

1. 用manifest核对体外三个时间点和临床四个患者-时间样本是否齐全。
2. 统计每库reads、唯一片段、中位TSS enrichment、FRiP和doublet率，确认合计细胞数可接近12,617/10,929。
3. 分开保存实测peak accessibility与推断gene activity。
4. 临床图谱只有2例患者，分析单位不能误设为10,929个独立患者重复。
5. 复现“7类”时检查下载对象的cluster列；正文只清楚列出6个命名状态，不能自行补造第7类名称。

## 4. 主要发现

- 短期刺激后激活相关位点开放；48 h群体出现中间/终末耗竭及持续的BATF/IRF4 motif活性。
- 患者BCMA CAR-T在晚期下降阶段呈更强耗竭相关开放网络，说明体外机制具有一定体内可迁移性。
- BATF或IRF4敲低提高IFN-γ、降低耗竭表型并改善持久性和异种移植瘤控制，超越纯相关图谱。

## 5. “状态—功能—驱动”证据链

scATAC定义可及性状态，motif和外部ChIP把状态指向BATF/IRF4，敲低实验再连接到细胞因子、持续性和抗肿瘤功能。这是综述中“分子图谱—候选驱动—扰动—功能验证”的标准案例。

## 6. 推荐图版

体外三时间点UMAP/轨迹、BATF/IRF4 motif动态、临床峰值与晚期状态比例、敲低后的功能/动物结果最有信息密度。展示时应把体外CD19与临床BCMA两个系统分面。

## 7. 创新价值

将受控时间序列与临床纵向样本结合，并从scATAC推断推进到基因扰动验证；同时给出可操作的单细胞开放染色质QC框架。

## 8. 局限性

临床验证只有2例，不能估计患者间变异或疗效预测能力；体外0/6/48 h时间窗较短；gene activity和motif并非直接表达/结合证据；不同靶点和制造流程带来体系差异。

## 9. 对本综述架构的作用

可同时支撑“charting molecular landscape”“link transitions with molecular drivers”和“perturb/manipulate states”三节，尤其适合作为从静态标志迈向可干预调控网络的例子。

## 10. 可直接用于综述的观点

> 单细胞开放染色质图谱显示，CAR-T功能障碍不是单一终点，而是由抗原刺激后持续增强的转录因子网络塑造；BATF/IRF4扰动证明其中至少部分网络具有因果和工程可操作性。

## 11. 避免误读

- 23,546是两套scATAC图谱的QC后细胞总和，不是患者数或配对multiome细胞数。
- GSE151774是复用的bulk RNA数据，不是CNP0002442的RNA模态。
- 临床数据只有2例，不足以声称建立了普适预后模型。
- 文中“7 subtypes”与正文六个命名状态存在不一致，报告时应保留这一数据字典问题。
