# 《Systematic discovery of CRISPR-boosted CAR T cell immunotherapies》精读

## 论文信息

- 作者：Paul Datlinger、Ekaterina V. Pankevich、Christian D. Arnold 等
- 期刊：*Nature*
- 年份：2025；646: 963–972；在线发表于 2025 年 9 月 24 日
- DOI：10.1038/s41586-025-09507-9
- 原文：[Nature](https://www.nature.com/articles/s41586-025-09507-9)
- PubMed：[PMID 40993398](https://pubmed.ncbi.nlm.nih.gov/40993398/)
- RNA-seq：[GEO GSE266618](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE266618)
- 筛选数据：论文 Supplementary Tables 2、3、5、7、8 及 Source Data
- 载体：[Addgene Datlinger/Bock CELLFIE plasmids](https://www.addgene.org/browse/article/28244767/)

## 一句话结论

CELLFIE 把全基因组 fitness/FACS 筛选、39基因体内 CROP-seq、238种双gRNA组合筛选和3,755条gRNA饱和碱基编辑整合为原代人 CAR-T 工程平台，发现 RHOG、FAS、PRDM1 等增强靶点，其中 RHOG–FAS 双敲在多个 CAR 与异种移植模型中显著提高抗肿瘤效力。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 全基因组库 | Brunello CRISPRko 库；约76,000条gRNA、约19,000基因 | 精确序列与每个 screen 的样本设计以 Supplementary Tables 1–3 为准 |
| genome-wide fitness | 4名供者、2次独立实验；TCR与CAR反复刺激 | day 0/7/14/21取样；主要比较day14 vs day0 |
| genome-wide FACS | 45个排序群体 | readout涵盖CD19 trogocytosis、CD69、FAS、PD-1/LAG3/TIM3等 |
| 体内 pooled screen | 39基因、339条gRNA、20只小鼠 | 8 gRNA/基因并含对照；脾和骨髓，day9/day21 |
| 组合筛选 | 238种双gRNA组合、4名供者、3种CAR | 6个booster、3个essential及safe-harbour对照 |
| 碱基编辑筛选 | 3,755条gRNA；RHOG 1,169、PAC 1,140 | 2名供者、4种base editor；验证小库323条gRNA/5供者 |
| RHOG RNA-seq | GSE266618，60个GSM | 3供者×CD4/CD8×RHOG/对照×5时间点 |
| GEO处理矩阵 | `GSE266618_counts.csv.gz`，约2.70 MB | GEO明确说明未提交人供者原始读段 |
| CRISPR原始/处理数据 | Supplementary Tables + Source Data | 不在GSE266618；不能只下载GEO就声称获得全部筛选数据 |

## 1. 研究要解决的问题

CAR-T 治疗失败常与扩增不足、耗竭、靶细胞识别弱、凋亡和同类相杀有关。已有筛选往往只用一个体外终点，或无法在原代 CAR-T、全基因组规模和动物体内同时保持覆盖。本文目标是建立一条可系统迭代的工程管线：发现单基因 booster，体内优选，组合增强，再用无双链断裂的碱基编辑为临床转化寻找具体突变。

## 2. CELLFIE 方法框架

### 2.1 三组件共递送

CELLFIE 以原代人 T 细胞为起点，同时递送：

- CAR；
- gRNA 库；
- Cas9 或碱基编辑器 mRNA。

作者构建 CROP-seq-CAR 载体，在同一慢病毒中共递送 CAR、gRNA 和选择标记；通过 mRNA 电转规避超大编辑器慢病毒包装问题，并用嘌呤霉素/杀稻瘟菌素双选择提高可筛选细胞比例。全基因组 screen 维持约1,000× gRNA覆盖。

### 2.2 多目标 readout

平台不是只测存活：

1. fitness：反复 TCR 或 CAR 刺激后的 gRNA 富集/耗竭；
2. target recognition：CAR-T 表面获得 CD19（trogocytosis）；
3. activation：CD69；
4. apoptosis/fratricide：FAS、CD19 等；
5. early exhaustion：PD-1、LAG3、TIM3 联合标志；
6. in vivo persistence：白血病小鼠的脾和骨髓中 gRNA/UMI；
7. combination/base-editing：双基因相互作用与蛋白位点级突变。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

本文的核心资源不是一张单细胞转录图谱，而是一组层级化 CRISPR screen：

- gRNA count matrices（plasmid、day0、后续时间点、sorted/unsorted、不同器官）；
- MAGeCK RRA/MLE 结果与 QC；
- in vivo CROP-seq 的 gRNA–UMI 读段，可估计克隆数并生成内部重复；
- 双gRNA的 spacer–iBAR 配对 counts；
- 碱基编辑 gRNA及预测突变、富集和验证结果；
- RHOG knockout vs safe-harbour CAR-T 的 bulk RNA-seq时间序列；
- 小鼠活体生物发光、肿瘤体积、生存和流式 Source Data。

“CROP-seq”在体内部分主要用于从高表达mRNA读取gRNA和UMI，并非对每个细胞做常规10x全转录组测序。不能把它误写成 in vivo Perturb-seq transcriptome atlas。

### 3.2 全基因组 fitness 与 FACS 资源

作者把 Brunello 人全基因组敲除库克隆入 CROP-seq-CAR。CAR-T 来自4名血液供者，分别接受反复 anti-CD3/CD28（TCR）或 CD19⁺ K562（CAR）刺激，day0、7、14、21收样。主fitness模型比较day14与day0，并用essential genes归一化以处理刺激造成的增殖差异。

FACS screen 对多种表面 readout 进行正/负群体分选。Extended Data 显示共45个genome-wide sorted populations；具体供者、CD4/CD8、CAR构型、读出、门限和细胞数在 Supplementary Table 3。筛选得到43个跨readout的优先命中，包括既有免疫检查点与未预期靶点。

### 3.3 体内 CROP-seq：39基因、339 gRNA、20只小鼠

作者从体外筛选选择39个基因，每基因8条gRNA，并加入essential、非essential和safe-harbour对照，共339条gRNA。实验设计为：

- NSG小鼠先注射0.5 million NALM6-luc白血病细胞；
- day5注射1 million pooled CAR-T；
- 4个供者/时间组合，每组5只，共20只；
- day9与day21收集脾和骨髓；
- 从whole-organ RNA反转录读取gRNA与26-nt UMI。

UMI不只是去PCR重复，还用前1–5位构造4、16、64、256或1,024个内部重复，以减轻体内瓶颈和克隆漂移。最佳分析使用16个内部重复。RHOG、FAS、PRDM1在脾和骨髓均显著富集，CDKN2A主要在骨髓富集；前三者在day21合计超过25%的gRNA reads。

### 3.4 组合筛选：238种双gRNA

组合库覆盖6个top booster（FAS、RHOG、PRDM1、CDKN2A、HAVCR2、CTLA4）、3个essential基因（RPL8、PSMB4、POLR2L）及15条safe-harbour gRNA。每基因取2条最佳gRNA，形成238种双gRNA组合；在4名供者、3种CAR（19-BBz、19-28z、GD2-BBz）中比较day12与day0，数据包括：

- Supplementary Table 7a：双gRNA库设计；
- 7b：样本注释；
- 7c：原始/处理counts；
- 7d：完美匹配、库均一性、重组率QC；
- 7e：MAGeCK MLE设计矩阵；
- 7f：模型输出。

### 3.5 碱基编辑：3,755条gRNA与323条验证库

饱和库含3,755条gRNA，其中RHOG基因体1,169条、PAC阳性验证基因1,140条，其余为essential和safe-harbour对照。2名供者分别接受ABEmax、AncBE4max及近PAM-less版本；预测共覆盖3,755种gRNA引导的多类missense、nonsense和splice变化。后续选择323条gRNA在5名供者中验证，并对RHOG局部做靶向测序，把筛选富集连接到具体氨基酸变化。

### 3.6 GSE266618：60个 bulk RNA-seq 样本

GEO系列严格对应RHOG机制研究，而不是全部CRISPR筛选。60个GSM来自完整因子设计：

| 因子 | 水平 |
|---|---|
| 供者 | 3（2女、1男） |
| T细胞亚群 | CD4、CD8 |
| 基因型 | RHOG knockout、safe-harbour edited control |
| 时间 | 0、24、72、168、240 h |

3×2×2×5=60。细胞在与CD19⁺肿瘤细胞刺激后的时间序列中分开分析CD4/CD8。公开文件为 `GSE266618_counts.csv.gz`，约2.70 MB。GEO页面明确写明：由于人供者隐私，raw data未提交；因此可复用的是处理后的count矩阵，而非FASTQ/BAM。

### 3.7 体内验证的规模

RHOG单敲在8名供者的合并验证中使用42只小鼠，对照41只；另有FAS单敲、RHOG–FAS双敲、多CAR和不同剂量验证。主图的双敲比较中标准CAR-T 10只、RHOG KO 10只、FAS KO 9只、双KO 9只。它们是多个独立实验的汇总，不能与20只pooled screen小鼠混为一批。

## 4. 数据页与下载路线

### 4.1 想复现筛选排名

从 Nature 页面下载 Supplementary Tables：

- Table 2：fitness screen；
- Table 3：FACS screen；
- Table 5：in vivo screen；
- Table 7：组合screen；
- Table 8：base-editing screen。

这些表包含样本注释、gRNA counts、MAGeCK结果与部分QC，才是本文筛选数据的主体。Source Data另含各图的流式、活体成像、肿瘤体积和统计数值。

### 4.2 想复现 RHOG 转录机制

从GEO下载：

```bash
wget -c https://ftp.ncbi.nlm.nih.gov/geo/series/GSE266nnn/GSE266618/suppl/GSE266618_counts.csv.gz
```

解压后先检查行名是gene/transcript、列名是否编码cell type/time/genotype/donor，再构建设计式：

```r
~ donor + cell_type + time + genotype + time:genotype
```

论文使用limma并对RHOG KO与standard CAR-T做分时、分CD4/CD8比较。由于只有处理counts，无法从GEO复核比对和read-level QC。

### 4.3 想重建载体或library

Supplementary Table 1给出载体、gRNA、引物、寡核苷酸和抗体；相关CROP-seq-CAR载体已在Addgene列出。组合库还依赖公开 `CROPseq-multi` 设计代码。实际复用需同时确认载体版本、CAR序列、选择标记、UMI结构与编辑器mRNA。

## 5. 主要发现

全基因组和FACS筛选同时回收PDCD1、CTLA4、FAS等已知靶点，支持平台有效。体内筛选把RHOG从众多体外命中中优先出来；尽管先天RHOG缺陷会导致免疫缺陷，RHOG敲除CAR-T在治疗场景中反而扩增更强、保持更多中央记忆样细胞并改善白血病清除。RHOG与FAS双敲产生更强组合效应，提示优化可以沿互补限制模块叠加。

## 6. 从状态扰动到导航策略

CELLFIE提供“候选生成—多目标筛选—体内优选—组合搜索—精细突变”的闭环雏形。它仍不是实时反馈控制，但已把多个可优化目标显式化：fitness、识别、激活、凋亡、耗竭、体内持久性和安全编辑形式。对于本综述，可把它作为离线多目标优化系统的代表。

## 7. 推荐图版

- **Fig. 1**：CELLFIE与全基因组fitness筛选。
- **Fig. 2**：多readout FACS screen，最能体现多目标表型。
- **Fig. 3**：in vivo CROP-seq、UMI内部重复与39基因筛选。
- **Fig. 4**：RHOG/FAS单敲和双敲的体内验证。
- **Fig. 5**：238组合与3,755条碱基编辑库；最适合讲优化空间扩展。

综述中建议Fig. 3 + Fig. 5：前者展示体内筛选瓶颈如何解决，后者展示从单基因到组合与位点级导航。

## 8. 创新价值

1. 在原代人CAR-T上实现高质量全基因组、多readout筛选。
2. 用mRNA型gRNA readout和UMI内部重复解决体内低频与克隆瓶颈。
3. 把单基因发现推进到双基因组合和碱基编辑位点层面。
4. 通过多CAR、多抗原和患者来源细胞验证可迁移性。
5. 筛选表和载体资源较完整，便于方法学复用。

## 9. 局限性

1. pooled体外实验存在旁分泌和群体互作，gRNA表型不完全细胞自主。
2. NSG异种移植缺少完整人免疫微环境，不能预测临床毒性。
3. in vivo 39基因是体外预筛后的候选集，不是全基因组体内无偏筛选。
4. RHOG先天缺陷与工程CAR-T效应方向相反，提示长期安全性不能由短期小鼠疗效替代。
5. GEO只开放处理后的RNA count矩阵，raw RNA reads因隐私未提交。
6. 核心CRISPR数据分散在多个补充Excel和Source Data，而非统一机器可读仓库。
7. 双敲/碱基编辑仍需评估脱靶、染色体结构、制造一致性和临床可控性。

## 10. 对本综述架构的作用

该文横跨“perturb/manipulate cell states”“optimize conditions for navigating T cell states”和“build optimization systems”。它最重要的概念贡献是：不要以单一终点优化CAR-T，而要把状态和功能拆为多个可筛选目标，再用体内表现、组合效应和安全编辑约束候选。

## 11. 可直接用于综述的观点

> CELLFIE 将原代人 CAR-T 的全基因组 fitness 与多表型 FACS 筛选、UMI辅助的体内 CROP-seq、组合敲除和饱和碱基编辑串成工程闭环，发现 RHOG–FAS 双敲可跨CAR构型增强抗肿瘤活性，展示了从单基因状态扰动走向多目标、组合式细胞治疗优化的可行路径（Nature 2025, Datlinger）。

## 12. 避免误读

- 不要把GSE266618当成全部CRISPR screen数据；它只含RHOG机制的bulk RNA counts。
- 不要把in vivo CROP-seq写成单细胞转录组Perturb-seq。
- 不要把39个体内候选说成全基因组体内筛选。
- 不要把RHOG先天免疫缺陷与CAR-T工程敲除的风险/效应简单等同。
- 不要依据异种移植“治愈”直接宣称临床安全有效。
