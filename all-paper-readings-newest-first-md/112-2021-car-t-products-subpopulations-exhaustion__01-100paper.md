# 《Single-cell transcriptome analysis of CAR T-cell products reveals subpopulations, stimulation, and exhaustion signatures》精读

## 论文信息

- **作者/期刊/年份**：Wang et al., *OncoImmunology*, 2021
- **DOI**：[10.1080/2162402X.2020.1866287](https://doi.org/10.1080/2162402X.2020.1866287)
- **PMID / PMCID**：33489472 / [PMC7801130](https://pmc.ncbi.nlm.nih.gov/articles/PMC7801130/)
- **数据**：[GSE145809](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE145809)，[SRP250485](https://www.ncbi.nlm.nih.gov/sra/?term=SRP250485)
- **代码**：[CARTcells_Notebooks](https://github.com/SharonWang/CARTcells_Notebooks/)

## 一句话结论

同一CAR-T产品内包含多种激活、记忆和耗竭倾向状态，而且检测到CAR转录本的细胞中也只有约一半对抗原表现出明确转录应答，提示产品优化必须关注状态组成与响应比例，而不能只看平均CAR阳性率。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 产品 | APRIL配体型、同时靶向BCMA/TACI的CAR-T研究产品 |
| 供者 | 3位健康供者 |
| 每供者样本 | leukapheresis、未刺激product、抗原过夜刺激product，共3类 |
| GEO | 9个GSM；HiSeq 4000；PRJNA608300 |
| 论文QC后 | 53,191个单细胞；其中CAR product 37,898个 |
| 产品分组 | 未刺激17,163；刺激20,735 |
| 检出CAR转录本 | 8,534：未刺激4,316；刺激4,218 |
| 处理后下载 | `GSE145809_RAW.tar`约177.8 MB，逐样本H5矩阵；原始FASTQ在SRA |

## 1. 研究要解决的问题

制造完成的CAR-T通常用少量流式标志和群体功能判定，但产品内部哪些亚群存在、抗原刺激后哪些细胞真正响应、耗竭signature是否均匀分布并不清楚。作者以制造前leukapheresis作为参照，描绘三个健康供者产品及刺激后状态。

## 2. 实验与分析框架

每位供者形成三联样本：起始leukapheresis、未刺激产品、特异抗原过夜刺激产品。10x scRNA后做整合聚类、差异表达、signature评分，并利用CAR转基因序列在转录本中的检出来区分CAR-detected与未检出细胞，比较其刺激反应。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这是三位健康供者、制造前后及短时刺激的scRNA表达图谱。它不是患者输注后纵向数据，也不是CITE-seq/TCR-seq。CAR RNA“未检出”不能直接等同于细胞表面CAR阴性，因为droplet scRNA存在掉零；CAR-detected更适合作为高置信转录本阳性层。

### 3.2 多大规模、覆盖哪些生物情境

| 层级 | 构成 | 细胞数 | 说明 |
|---|---|---:|---|
| 论文QC后总图谱 | 3供者 × 3样本类型 | **53,191** | 包含leukapheresis中的T及少量非T细胞和产品细胞 |
| CAR product | 未刺激 + 刺激 | **37,898** | 产品主体 |
| 未刺激product | 3供者 | **17,163** | 制造后基线 |
| 刺激product | 3供者 | **20,735** | 特异抗原过夜刺激 |
| CAR RNA检出 | 未刺激4,316 + 刺激4,218 | **8,534** | 用于CAR-detected细胞内的应答比较 |
| leukapheresis T细胞 | donor 1: 3,693；donor 2: 3,763；donor 3: 3,389 | **10,845** | 起始T细胞参照；其余leukapheresis细胞解释总数差额 |

产品图谱分成11个cluster，覆盖静息/记忆、细胞周期、效应/细胞毒、激活/耗竭等状态；cluster 11只有27个树突样细胞，并有少量NK-like污染。GEO摘要记录57,676个转录组，而正文最终分析为53,191个QC后细胞；前者可视为存档/处理阶段总量，不能与最终图谱数混用。

### 3.3 公共数据包有什么

- **GSE145809**：9个GSM，命名对应3位供者的Leukapheresis、Product、Stimulated_product。
- **`GSE145809_RAW.tar`，约177.8 MB**：GEO supplementary processed package，文件类型为H5，可直接读入Seurat/Scanpy；虽文件名含RAW，它在GEO语境下是提交者提供的处理后矩阵包，不等于测序FASTQ。
- **SRA SRP250485**：原始测序reads；用于重跑Cell Ranger或纳入统一预处理。
- **GitHub notebooks**：作者分析笔记，适合核对过滤、整合、图表和signature计算；应固定commit/下载日期以防仓库更新。

### 3.4 如何获取：按你的目的选择

#### 路线A：最快复用单细胞矩阵

从GEO下载`GSE145809_RAW.tar`并解包H5。依据GSM和文件名建立9行sample sheet，在Seurat用`Read10X_h5`或Scanpy用`read_10x_h5`读入；不要先把所有供者合并再丢失样本标签。

#### 路线B：严格统一预处理

在SRA Run Selector导出SRP250485的run表并下载FASTQ，加入CAR转基因序列构建自定义参考，再运行Cell Ranger。若用普通GRCh38而不加CAR序列，将无法复现CAR transcript检测层。

#### 路线C：复现论文图

矩阵与GitHub notebooks配合使用；先核对软件版本和对象字段，再复现QC、整合、cluster marker与signature。必要时从补充表获取作者的基因集。

### 3.5 下载后先做什么

1. 核对9个H5是否形成完整的3×3设计。
2. 分样本统计细胞数、UMI、基因数、线粒体比例和doublet，再比对53,191最终总数。
3. 检查参考基因组是否含CAR转基因、CAR feature名称及计数范围。
4. 保留`donor`为独立重复，刺激效应做供者内配对。
5. 将57,676（GEO描述）和53,191（论文QC）放在不同字段，禁止相加。

## 4. 主要发现

- 产品中存在11个转录cluster，制造并未产生单一均质状态。
- 刺激诱导效应、细胞因子及耗竭相关signature，但反应强度和覆盖比例存在明显细胞间差异。
- 即使限定CAR transcript-detected细胞，也只有约一半显示明确转录应答，说明“表达CAR”与“当下对抗原响应”不是同义。
- 起始材料、供者和制造后状态共同决定产品组成。

## 5. “状态—功能—驱动”证据链

该文对状态与刺激反应连接较强，但对分子驱动的因果证据较弱：抗原刺激是外部扰动，signature是转录读出，却没有对候选调控因子再编辑。因此适合作为“先定量产品状态”的基线，后接CRISPR或培养条件优化研究。

## 6. 推荐图版

产品11-cluster图、刺激前后比例/marker、CAR-detected与未检出细胞的应答比较，以及三位供者并列图最适合说明产品异质性。

## 7. 创新价值

将制造前、制造后和抗原刺激配对；把CAR转录本纳入参考以避免只依赖常规T细胞标志；公开H5和notebook便于复用。

## 8. 局限性

只有3位健康供者；过夜刺激不能代表长期反复刺激；RNA掉零使CAR-detected比例低估真实表面阳性；没有患者疗效、TCR克隆或蛋白层验证。

## 9. 对本综述架构的作用

直接支撑“quantitatively characterizing cell phenotypes/functions/markers”和“optimize conditions”：优化目标不应只是扩大总细胞数，而应提高有利状态及真正响应细胞的比例。

## 10. 可直接用于综述的观点

> 单细胞分析揭示CAR-T产品是由多种记忆、效应、增殖和耗竭倾向状态组成的混合物；因此产品质量需要用状态分布和抗原响应比例而非单一平均指标刻画。

## 11. 避免误读

- GEO的57,676与论文QC后的53,191是不同处理阶段统计。
- 37,898是产品细胞，不含全部leukapheresis细胞。
- CAR RNA未检出不等同于CAR蛋白阴性。
- 9个GSM是3供者×3条件，不是9位供者。
