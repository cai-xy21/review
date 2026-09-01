# 《A longitudinal single-cell atlas to predict outcome and toxicity after BCMA-directed CAR T cell therapy in multiple myeloma》精读

## 论文信息

- **作者/期刊/年份**：Rade et al., *Cancer Cell*；online 2025，卷期2026
- **DOI**：[10.1016/j.ccell.2025.10.014](https://doi.org/10.1016/j.ccell.2025.10.014)
- **PMID**：41349540
- **正文**：[ScienceDirect文章页](https://www.sciencedirect.com/science/article/pii/S1535610825004484)
- **完整研究数据**：[Zenodo 14732727](https://zenodo.org/records/14732727)
- **代码**：[Rade_et_al_CAR_2025](https://github.com/fraunhofer-izi/Rade_et_al_CAR_2025)
- **早期子队列GEO**：[GSE234261](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE234261)

## 一句话结论

ide-cel与cilta-cel在扩增动力学和CAR-T状态上不同；cilta-cel富集延迟扩增的CD4细胞毒/EOMES程序，而非完全缓解患者的CD8效应和线粒体程序受损，纵向多组学由此连接产品、宿主生态、疗效与毒性。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 临床队列 | 61例RRMM：ide-cel 34、cilta-cel 27 |
| 有单细胞数据 | 57/61例，135份PBMC |
| 时间层 | leukapheresis、late（约治疗后1月）、very late（约治疗后3月） |
| 多组学 | 10x 5′ scRNA + TCR + BCR + ADT/表面蛋白 |
| 规模 | 约440,000个QC后细胞；每样本中位2,977（范围601–7,937）；约160,000个T细胞 |
| 完整数据包 | Zenodo约25.9 GB，9个文件 |
| GEO边界 | GSE234261仅早期10名参与者/首批测序，不代表完整61人队列 |

## 1. 研究要解决的问题

两种BCMA CAR-T在疗效、扩增动力学和毒性上存在差别，但单次采样难以区分输注前状态、扩增后状态与宿主免疫变化。作者构建真实世界纵向multi-omic atlas，寻找完全缓解、CRS/ICANS等结局的细胞状态和分子关联。

## 2. 实验与分析框架

61例复发/难治MM接受ide-cel或cilta-cel；其中57例在白细胞单采及治疗后两个窗口获得135份PBMC，使用10x 5′ GEX、VDJ和feature barcode。作者联合RNA、ADT、TCR/BCR克隆与临床指标（扩增、sBCMA、CRP、结局/毒性），比较产品和疗效组并构建预测关联。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这是外周血纵向多组学，不是骨髓空间图谱，也不是所有模态都在每个细胞完整配对。RNA和ADT由同一液滴barcode连接；TCR/BCR只在具有可识别VDJ的相应淋巴细胞中可用。研究同时测量CAR-T和宿主免疫细胞，因此总细胞数远大于CAR-T数。

### 3.2 多大规模、覆盖哪些生物情境

| 层级 | 规模/构成 | 注释 |
|---|---:|---|
| 入组患者 | **61** | ide-cel 34，cilta-cel 27；完全缓解率约38% vs 78% |
| 单细胞患者 | **57** | 4例没有纳入单细胞图谱 |
| PBMC样本 | **135** | leukapheresis、late、very-late；存在缺失，非57×3完整设计 |
| QC后细胞 | **约440,000** | 每样本中位2,977，范围601–7,937 |
| T细胞 | **约160,000** | 进一步区分CAR/native、CD4/CD8和功能状态 |
| 组学层 | GEX、ADT、TCR、BCR | ADT提供表面蛋白；VDJ提供克隆层 |

治疗后窗口在队列中大致覆盖late day 19–64（平均约31）和very late day 63–169（平均约102）；复用时必须以Zenodo multiplexing/sample表中的患者实际采样日为准，而不是把所有人固定成day 30/day 100。cilta-cel扩增峰约day 14，ide-cel约day 7，是临床动力学统计，不是单细胞采样时间点本身。

### 3.3 Zenodo公共数据包有什么

Zenodo 14732727当前为开放记录，总计约25.9 GB、9个文件：

| 文件 | 大小 | 内容/用途 |
|---|---:|---|
| `seurat_objects.tar.gz` | 10.8 GB | 主要分析对象；含全细胞`05_seurat_harmony_all.Rds`、T细胞`06_seurat_harmony_t_all.Rds`等 |
| `cellranger_gex_adt.tar.gz` | 6.0 GB | GEX+ADT Cell Ranger输出；因隐私不含原始TCR/BCR序列输出 |
| `4_3_2_R_packages.tar.gz` | 4.5 GB | R 4.3.2软件包环境 |
| `singularity-rstudio-4-3-2.sif` | 3.4 GB | 可复现实验环境镜像 |
| `souporcell_latest.sif` | 1.1 GB | souporcell容器 |
| `souporcell.tar.gz` | 24.7 MB | 样本去卷积/基因型相关输出 |
| `featurer_reference_ADT_10x.tar` | 26.1 KB | ADT feature reference |
| `Table_cellranger_multi_omics.xlsx` | 33.9 KB | Cell Ranger多组学样本/运行表 |
| `Table_multiplexing_info.xlsx` | 35.0 KB | multiplex、患者/时间点映射 |

VDJ对象为保护隐私移除了序列，并对clone ID做假名化；因此开放对象仍可研究克隆大小/动态，但不能从中恢复患者可识别的TCR/BCR序列。

### 3.4 GEO子集与下载路线

#### 路线A：完整研究的快速复用

从Zenodo优先下载10.8 GB Seurat对象和两个XLSX映射表；若只研究T细胞，可先读取`06_seurat_harmony_t_all.Rds`，避免加载全部对象。下载前预留至少两倍压缩包空间用于解包。

#### 路线B：从GEX/ADT处理输出重建

下载6.0 GB `cellranger_gex_adt.tar.gz`，依据两张XLSX重建sample sheet，重新做QC/整合。适合验证Harmony或ADT归一化，但不提供隐私受限的原始VDJ序列。

#### 路线C：环境级复现

下载R package archive或Singularity镜像，并配合GitHub代码。镜像很大，只有严格复现时才必要；一般再分析无需下载全部25.9 GB。

#### 路线D：GSE234261的正确用法

GSE234261来自早期报告，只覆盖首10名参与者/第一批测序，页面含约100个按PB/BM、时间和GEX/ADT/BCR/TCR拆分的GSM。它可用于小子集原始reads/矩阵测试，但不能用来声称已下载本篇完整57例单细胞队列。

### 3.5 下载后先做什么

1. 先读两个XLSX，建立`patient × product × actual day × modality × batch`映射。
2. 核对57例、135样本、约44万细胞及每样本601–7,937的范围。
3. 检查RNA/ADT barcode一致性、VDJ可用比例和clone ID假名化规则。
4. 疗效/毒性模型必须在患者层切分，禁止细胞级随机训练测试泄漏。
5. 将GSE234261标为subset，并记录它与Zenodo全队列的重叠患者/批次。

## 4. 主要发现

- cilta-cel相较ide-cel扩增峰更晚，并富集CD4细胞毒/EOMES相关CAR-T状态。
- 未完全缓解患者的CD8 CAR-T显示效应不足、线粒体功能障碍及TIGIT/CD38相关程序。
- pDC在非B细胞中呈较高BCMA；治疗后B细胞、pDC和浆细胞减少，显示靶向影响宿主生态。
- sBCMA下降与CAR-T扩增及炎症指标相关；状态、动力学与临床毒性/疗效形成联合图景。

## 5. “状态—功能—驱动”证据链

纵向multiome把RNA状态、表面蛋白、克隆、扩增和临床终点连接起来，但多数“driver”仍是关联。EOMES、TIGIT/CD38和线粒体程序是可测试的调节轴，需在制造/扰动体系中验证因果。

## 6. 推荐图版

两产品扩增曲线、纵向免疫图谱、CD4细胞毒/EOMES状态、CR与非CR的CD8功能程序，以及sBCMA/CRP/细胞扩增关联最值得引用。

## 7. 创新价值

真实世界较大队列、三时间层和RNA/蛋白/VDJ联合；数据包同时提供分析对象、处理输出、映射表和可复现环境，公开层级非常完整。

## 8. 局限性

产品非随机分配；外周血不能完全代表骨髓肿瘤微环境；样本时间窗较宽且缺失不平衡；VDJ序列因隐私移除；预测关联仍需独立队列前瞻验证。

## 9. 对本综述架构的作用

是“build real-time optimization systems”的临床原型：把多时间点状态、产品类型、可溶性标志和毒性组合为动态监测输入，但距离实时闭环干预仍有一步。

## 10. 可直接用于综述的观点

> BCMA CAR-T疗效与毒性来自产品、CAR-T内在状态和宿主免疫生态的动态耦合；纵向RNA、表面蛋白和克隆联合测量比单一峰值扩增更接近可用于状态导航的监控框架。

## 11. 避免误读

- 61是总队列，57有单细胞数据；135不是每人完整三个时间点。
- 约44万是全PBMC图谱，不是全部CAR-T。
- GSE234261只覆盖早期10人子集；完整数据以Zenodo 14732727为主。
- 开放VDJ对象已去序列/假名化，不能用于公开TCR序列检索。
