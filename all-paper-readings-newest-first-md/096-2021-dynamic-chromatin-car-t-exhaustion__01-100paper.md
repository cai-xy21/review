# 《Dynamic chromatin regulatory landscape of human CAR T cell exhaustion》精读

## 论文信息

- **作者/期刊/年份**：Gennert et al., *PNAS*, 2021
- **DOI**：[10.1073/pnas.2104758118](https://doi.org/10.1073/pnas.2104758118)
- **PMID / PMCID**：34285077 / [PMC8325267](https://pmc.ncbi.nlm.nih.gov/articles/PMC8325267/)
- **公共数据**：[GSE168882](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE168882)（SuperSeries）；[GSE168880](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE168880)（Omni-ATAC）；[GSE168881](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE168881)（H3K27ac HiChIP）

## 一句话结论

易耗竭的GD2-HA-28ζ CAR-T在明显表型耗竭之前已经发生染色质重塑；AP-1相关调控模块和可验证的PDCD1增强子由此成为“提前导航”CAR-T状态的候选分子杠杆。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 研究系统 | HA-28ζ（抗GD2、易自发耗竭）与CD19-28ζ（对照）人CAR-T体外培养体系 |
| 关键比较轴 | CAR构型 × CD4/CD8 × naïve/central-memory来源 × 时间 |
| 组学 | **bulk** Omni-ATAC-seq、bulk RNA-seq、H3K27ac HiChIP；不是scATAC-seq |
| GEO规模 | 40个ATAC样本 + 6个HiChIP样本 = SuperSeries中46个样本 |
| 关键时间点 | ATAC：day 0/7/14；HiChIP：day 0/10；论文表型和RNA还覆盖day 10等时间点 |
| 原始数据 | ATAC：[SRP310566](https://www.ncbi.nlm.nih.gov/sra/?term=SRP310566)；HiChIP：[SRP310567](https://www.ncbi.nlm.nih.gov/sra/?term=SRP310567) |
| 处理后数据 | ATAC narrowPeak约50 MB；HiChIP `.hic`约3.0 GB；SuperSeries合包约3.1 GB |

## 1. 研究要解决的问题

作者要区分“耗竭出现后的伴随标志”和“促使耗竭形成的早期调控事件”。核心假设是：若染色质可及性在PD-1等表型显现之前已经分叉，则早期开放的增强子及其转录因子网络可能是可干预的驱动因素。

## 2. 实验与分析框架

人T细胞先按CD4/CD8及naïve/central-memory来源分选，再制造两种CD28-CD3ζ CAR。HA-28ζ因CAR聚集产生tonic signaling并逐渐耗竭，CD19-28ζ作为不易耗竭对照。作者将流式表型、bulk RNA、Omni-ATAC与H3K27ac HiChIP按时间对齐，并以峰差异、motif、共可及/三维互作和CRISPR增强子删除建立“状态—调控元件—基因”的证据链。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这不是逐细胞图谱。每个测序样本均为预先FACS定义亚群的群体测序，因此可比较四类细胞来源，但不能从原始数据中重新发现细胞簇、计算单细胞异质性或克隆轨迹。Omni-ATAC每个样本投入约100,000个CAR-T并拆成两个技术重复（约50,000个核/文库）；bulk RNA每样本约2×10^6个细胞；HiChIP方法注明每重复约500,000个固定细胞。

### 3.2 多大规模、覆盖哪些生物情境

| 数据层 | 构成 | 样本数/规模 | 主要用途 |
|---|---|---:|---|
| ATAC day 0 | CD4/CD8 × naïve/CM来源 × 2重复 | 8 | 制造前基线 |
| ATAC day 7 | 2种CAR × 4类来源 × 2重复 | 16 | 早期调控分叉 |
| ATAC day 14 | 2种CAR × 4类来源 × 2重复 | 16 | 稳定耗竭图景 |
| **ATAC合计** | 三个时间点 | **40** | GSE168880 |
| H3K27ac HiChIP | CD8 naïve-derived；day 0、day 10 CD19-28ζ、day 10 HA-28ζ；各2重复 | **6** | 活性增强子—启动子三维联系，GSE168881 |
| bulk RNA-seq | 相同模型的时间/亚群比较 | 论文分析层 | 转录响应；GEO上述子系列未提供可识别的RNA文库 |

需要特别区分：论文实验还在day 10收集表型/RNA/HiChIP，但GSE168880的ATAC样本只包含day 0、7、14；SuperSeries的46是40个ATAC加6个HiChIP，而不是46个单细胞样本或46位供者。供者来源写作健康人血液，但论文/GEO没有把供者数作为可独立建模的样本轴，因此技术重复不能替代供者重复。

### 3.3 公共数据包有什么

- **GSE168880 / PRJNA714409**：40个GSM；原始FASTQ在SRA SRP310566；`GSE168880_RAW.tar`约50.0 MB，主要是逐样本narrowPeak。
- **GSE168881 / PRJNA714411**：6个GSM；原始数据在SRA SRP310567；`GSE168881_RAW.tar`约3.0 GB，主要是Hi-C/HiChIP `.hic`文件。
- **GSE168882 / PRJNA714410**：上述两个子系列的SuperSeries；`GSE168882_RAW.tar`约3.1 GB，可一次取得峰和互作矩阵。
- 论文补充表提供差异峰、调控模块、基因列表等结果层数据；上述GEO页面未列出bulk RNA-seq FASTQ或表达矩阵，应避免把“sequencing data deposited”扩大解释为所有组学均已公开。

### 3.4 如何获取：按目的选择

#### 路线A：复用作者峰和互作结果

从GEO页面下载`GSE168880_RAW.tar`与`GSE168881_RAW.tar`。narrowPeak可直接用于统一peak set、motif富集和与其他CAR-T ATAC队列交集；`.hic`可用Juicebox或`strawr`读取指定分辨率的接触矩阵。

#### 路线B：从原始reads重做

进入对应SRA Run Selector导出RunInfo，用SRA Toolkit下载FASTQ；ATAC按hg38统一比对、去重复、Tn5校正、峰调用和可重复峰过滤，HiChIP则需明确酶切、配对、重复和距离过滤参数。不要把ATAC与HiChIP的SRP混在同一流程。

#### 路线C：只提取论文结论层数据

优先下载PMC补充表；这一路线适合整理候选增强子、TF模块和差异峰，不适合重新估计样本间变异。

### 3.5 下载后先做什么

1. 用GSM标题解析`time × CAR × CD4/CD8 × origin × replicate`，核对40/6的样本数。
2. 检查narrowPeak是否均为hg38坐标、峰宽和signal列是否一致。
3. 原始ATAC至少复核TSS enrichment、FRiP、片段长度周期性和重复相关性。
4. HiChIP先核对`.hic`可用分辨率，再决定是否需要从FASTQ重建loops。
5. 把技术重复与生物供者分开记录；本数据不宜做供者层统计推断。

## 4. 主要发现

- day 7即出现显著可及性分叉，早于day 10左右清晰的耗竭表型，支持染色质变化具有前驱性质。
- HA-28ζ中AP-1家族及NFE2L1/NFE2L3相关模块异常，连接tonic signaling与耗竭程序。
- 人和小鼠的具体开放位点重叠有限，但通路/调控逻辑有一定保守性，提示跨物种验证应在模块层而非逐峰层进行。
- PDCD1附近候选调控元件经CRISPR删除后可降低PD-1，形成从图谱到因果干预的实例。

## 5. “状态—功能—驱动”证据链

早期开放峰/增强子是状态前兆；后续抑制性受体表达及功能下降是表型；HiChIP把增强子连接到靶基因，CRISPR删除再检验因果。其价值不只是给耗竭贴标签，而是把可导航的分子入口向更早时间点前移。

## 6. 推荐图版

优先使用展示day 7染色质分叉而表型尚未完全分离的时间图、AP-1调控模块、PDCD1增强子互作及删除验证。图注必须注明“FACS亚群bulk ATAC”，避免误写为单细胞轨迹。

## 7. 创新价值

把时间分辨的表观组与三维增强子联系、功能编辑相连；对综述“如何从分子景观走向状态操纵”是非常完整的早期范例。

## 8. 局限性

单一体外tonic-signaling模型不等于患者体内慢性抗原环境；bulk测序掩盖稀有状态；供者层复制信息不足；HA-28ζ与CD19-28ζ同时改变靶抗原/scFv背景，不能把全部差异归因于某个共刺激结构域。

## 9. 对本综述架构的作用

适合放在“charting molecular landscape”向“perturb/manipulate cell states”过渡处：图谱先定位早期调控元件，再用编辑验证可导航性。

## 10. 可直接用于综述的观点

> CAR-T耗竭并非从抑制性受体升高才开始；在易耗竭模型中，染色质调控网络已于表型分离之前重构，使早期增强子和转录因子模块成为预防性干预窗口。

## 11. 避免误读

- 不是scATAC-seq，不存在40个“细胞”或单细胞簇。
- HA-28ζ与CD19-28ζ都是CD28-CD3ζ胞内结构，不是CD28对4-1BB的比较。
- GEO的46个样本只由ATAC和HiChIP构成；不能据此声称bulk RNA原始数据也在该系列中。
- 外部小鼠ATAC数据用于比较，不属于本研究新生成的人CAR-T图谱。
