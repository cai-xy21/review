# 《Genome-wide CRISPR Screens in T Helper Cells Reveal Pervasive Crosstalk between Activation and Differentiation》精读

## 论文信息

- **作者/期刊/年份**：Henriksson et al., *Cell*, 2019
- **DOI**：[10.1016/j.cell.2018.11.044](https://doi.org/10.1016/j.cell.2018.11.044)
- **PMID / PMCID**：30639098 / [PMC6370901](https://pmc.ncbi.nlm.nih.gov/articles/PMC6370901/)
- **代码**：[th2crispr](https://github.com/mahogny/th2crispr)
- **主要数据入口**：BioStudies/ArrayExpress E-MTAB-6300、6285、6276、6292、7258、7260；筛选文库Addgene #104861

## 一句话结论

在原代小鼠Th2细胞的11个全基因组CRISPR筛选中，激活与分化调控高度交织，真正只影响分化而不影响激活的基因很少；PPARG、BHLHE40等因此成为重塑T细胞功能状态的核心节点。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 主筛选体系 | 原代小鼠naïve CD4 T细胞向Th2分化；不是人CAR-T |
| CRISPR文库 | 86,035条sgRNA，低MOI约0.2 |
| 筛选数 | 11个pooled genome-wide screens |
| 读出 | IL-4、IL-13、GATA3、IRF4、XBP1等高/低FACS及存活/掉失 |
| 生物材料规模 | 每重复合并15–30只小鼠；每板约3,000–4,000万细胞 |
| 补充组学 | 小鼠/人Th0-Th2时间序列RNA、ATAC、ChIP；候选KO及过表达RNA/ChIP |
| 人数据 | 脐带血CD4，3位供者，用于时间序列/跨物种验证，不是主CRISPR筛选 |

## 1. 研究要解决的问题

T辅助细胞激活与Th2分化同时发生，传统差异表达难区分某基因是在控制一般激活、选择Th2命运，还是影响存活。作者通过多个表型读出的全基因组扰动矩阵，将基因对IL-4/IL-13、主调控因子和细胞适应度的效应联合比较。

## 2. 实验与分析框架

Cas9小鼠naïve CD4细胞在Th2条件下感染低MOI逆转录病毒sgRNA文库，day 4按细胞因子或转录因子报告信号FACS为high/low，测序sgRNA并计算富集。11个屏幕的效应谱用于网络聚类；候选基因以单独KO转录组、时间分辨RNA/ATAC/ChIP及转录因子过表达验证。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

主体不是单细胞RNA图谱，而是“sgRNA × 11种功能读出”的pooled perturbation atlas。每个数据点反映某sgRNA在FACS高/低或存活群体中的相对丰度。后续RNA/ATAC/ChIP多为bulk时间序列或候选扰动，用于解释筛选命中如何进入Th2调控网络。

### 3.2 多大规模、覆盖哪些生物情境

| 数据层 | 规模/构成 | 用途 |
|---|---:|---|
| 全基因组文库 | **86,035 sgRNA**；单病毒携带1条；MOI约0.2 | 保持单细胞单扰动近似 |
| pooled screens | **11个** | IL-4、IL-13、GATA3、IRF4、XBP1等high/low及fitness/viability维度 |
| 生物重复材料 | 每重复合并15–30只小鼠；每板3,000–4,000万细胞 | 维持文库覆盖度；小鼠池是生物材料单位 |
| 筛选测序 | HiSeq 2500，19-bp single-end | sgRNA abundance；GATA3代表性屏幕中约91%文库达到≥500 reads |
| ATAC时间序列 | 0、2、4、24、48、72 h；小鼠与人Th2 | 早期开放与跨物种比较 |
| RNA时间序列 | 小鼠/人Th0与Th2；人脐带血CD4来自3供者 | 激活与分化表达动力学 |
| 候选扰动 | 单基因KO RNA-seq；PPARG/BHLHE40等过表达RNA/ChIP | 筛选命中机制化 |

“11个screen”不是11个供者，也不是11个单细胞批次；每个screen可能有多个FACS分箱和重复文库。小鼠是主因果筛选体系，人数据用于跨物种时序支持，不能把结论表述成“在人原代T细胞完成了11个全基因组筛选”。

### 3.3 ArrayExpress/BioStudies数据包组成

| accession | 数据内容 | 角色 |
|---|---|---|
| [E-MTAB-6300](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-6300) | 小鼠/人Th0-Th2 RNA-seq时间序列 | 激活/分化转录动力学 |
| [E-MTAB-6285](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-6285) | Th2候选CRISPR KO RNA-seq | 命中基因下游程序 |
| [E-MTAB-6276](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-6276) | Th2 ChIP-seq | GATA3/IRF4/BATF等结合层 |
| [E-MTAB-6292](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-6292) | 小鼠/人Th2 ATAC-seq时间序列 | 动态染色质可及性 |
| [E-MTAB-7258](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-7258) | 转录因子过表达Th2 ChIP-seq | PPARG/BHLHE40等调控占位 |
| [E-MTAB-7260](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-7260) | 转录因子过表达Th2 RNA-seq | 过表达功能程序 |
| E-MTAB-6327 | 既往XBP1 Th2数据 | 外部复用，不是本研究新生成层 |

各study页面提供sample/assay表和原始/处理文件链接；文件数和大小可能随BioStudies迁移更新，下载时应先保存页面manifest而不是依赖旧ArrayExpress FTP路径。

### 3.4 如何获取：按你的目的选择

#### 路线A：复用筛选命中

从PMC补充表和GitHub取得11个screen的sgRNA/gene-level结果及分析代码；适合构建`gene × phenotype`矩阵，无需先下载所有FASTQ。

#### 路线B：重做某一组学

进入对应E-MTAB页面下载`*.sdrf.txt`/样本表和raw/processed文件。先按organism、cell type、time、condition、replicate过滤，再下载；不要一次混合人/鼠或KO/overexpression项目。

#### 路线C：严格复现筛选reads

依据论文数据可用性与补充manifest定位筛选FASTQ，按19-bp guide序列计数并用作者代码/MAGeCK式方法重建high-vs-low富集。先核对文库版本86,035条，避免误用Addgene上其他小鼠CRISPR库。

#### 路线D：获得实验资源

筛选文库可由Addgene #104861定位；载体骨架及其他质粒按论文Key Resources Table核对。涉及购买不属于数据下载，本报告不代为下单。

### 3.5 下载后先做什么

1. 为每个E-MTAB保存SDRF/manifest并校验organism、时间点、重复。
2. 区分主研究新数据与外部E-MTAB-6327。
3. 筛选数据检查每样本guide覆盖、零计数比例、重复相关及high/low分箱方向。
4. RNA/ATAC跨物种比较先做ortholog映射，不能直接按同名峰坐标合并。
5. 以screen为表型轴、mouse pool/replicate为统计轴，不以sgRNA reads充当生物重复。

## 4. 主要发现

- 多数影响Th2因子或GATA3的基因也影响一般T细胞激活，显示两套程序广泛串扰。
- 只有较少基因呈相对“分化专一”效应，提示寻找完全不影响激活的命运开关并不容易。
- PPARG、BHLHE40等形成跨多个读出的核心调控节点；时间序列表观数据帮助定位其作用窗口。

## 5. “状态—功能—驱动”证据链

11个并行功能screen把基因扰动直接连接到细胞因子/主TF/存活，时间RNA/ATAC/ChIP再解释机制，候选KO/过表达提供第二层因果验证。这比仅靠相关图谱更接近可导航控制图。

## 6. 推荐图版

11屏幕相关矩阵、activation-vs-differentiation效应坐标、PPARG/BHLHE40调控网络和人鼠时间ATAC/RNA最有价值。

## 7. 创新价值

不是单表型找hit，而是用多读出扰动谱区分一般激活、分化与适应度；并把筛选与时序多组学整合。

## 8. 局限性

主筛选为小鼠Th2体外体系；pooled abundance是间接表型；高细胞投入依赖小鼠池；人数据主要验证保守性，未证明所有hit在人细胞中同样有效。

## 9. 对本综述架构的作用

非常适合“techniques to perturb/manipulate states”和“link transitions with molecular drivers”，展示多目标筛选如何识别既有效又不过度损害激活/存活的导航靶点。

## 10. 可直接用于综述的观点

> 多表型全基因组扰动揭示，T细胞激活与辅助T细胞分化并非两个独立控制回路；状态工程需要同时评价功能增益、一般激活和适应度代价。

## 11. 避免误读

- 主体是原代小鼠CD4 Th2，不是人CAR-T。
- 11个screen是11种读出/条件，不是11个供者。
- RNA/ATAC/ChIP主要为bulk，不是单细胞multiome。
- E-MTAB-6327是外部复用数据，不计入新生成数据。
