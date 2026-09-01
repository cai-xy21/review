# 《Durable response to CAR T is associated with elevated activation and clonotypic expansion of the cytotoxic native T cell repertoire》精读

## 论文信息

- **作者/期刊/年份**：Cheloni et al., *Nature Communications*, 2025
- **DOI**：[10.1038/s41467-025-59904-x](https://doi.org/10.1038/s41467-025-59904-x)
- **PMID / PMCID**：40410132 / [PMC12102275](https://pmc.ncbi.nlm.nih.gov/articles/PMC12102275/)
- **GEO**：[GSE290722](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE290722)
- **开放矩阵**：[Zenodo 15007741](https://zenodo.org/records/15007741)
- **代码**：[ZUMA1_Single_Cell_DLBCL_Atlas](https://github.com/ivlachos/ZUMA1_Single_Cell_DLBCL_Atlas)

## 一句话结论

axi-cel长期缓解不仅与CAR-T本身相关，还与患者原生细胞毒T/NK状态及其TCR克隆扩增相关；这把疗效机制从“工程细胞单体”扩展为“输注细胞与宿主免疫生态共同导航”。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 临床队列 | ZUMA-1（NCT02348216）LBCL患者32例 |
| 有结局分组 | 29例：长期响应13、早期复发8、无响应8；另3例结局未分类 |
| 纵向样本 | 92份PBMC；leukapheresis、4周、6月、12月 |
| scRNA+scTCR | 10x 5′ v2；QC后405,775个细胞 |
| GEO条目 | 184个GSM = 92份生物样本 × RNA/TCR两种文库 |
| 开放性 | GEO只给处理后数据；患者隐私相关原始reads需DUA申请 |
| Zenodo | 约1.9 GB：normalized count RDS约1.9 GB、metadata RDS约11.9 MB |

## 1. 研究要解决的问题

CAR-T疗效研究常只看输注产品或CAR阳性细胞，但患者原有免疫系统可能决定持久缓解。作者利用ZUMA-1长期随访样本，追踪非CAR原生T细胞的状态和克隆，询问哪些基线/治疗后免疫特征与长期响应相连。

## 2. 实验与分析框架

32例患者在白细胞单采、治疗后4周、6个月、12个月取得PBMC，做10x 5′ scRNA与配对scTCR。作者建立73个细胞群的全免疫图谱，再深入CD8和CD4各14个亚群，比较长期响应、早期复发和无响应；用TCR clonotype追踪克隆扩增，并以留一法评估探索性预测模型。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这是患者外周免疫系统的纵向scRNA+scTCR图谱，不是输注产品图谱。5′ GEX与VDJ来自同一10x样本，细胞barcode可连接表达和TCR；“native T cell”指未被判定为CAR-T的宿主T细胞。对克隆扩增的解释应依赖VDJ质量、clone定义和时间点抽样深度。

### 3.2 多大规模、覆盖哪些生物情境

| 层级 | 规模/组成 | 说明 |
|---|---:|---|
| 患者 | 32 | 29有明确疗效组：LtR>1年13；relapse<6月8；1月SD/PD无响应8 |
| PBMC生物样本 | **92** | 每人并非四时间点都完整，形成不平衡纵向设计 |
| GEO文库条目 | **184** | 每份PBMC分别提交1个RNA和1个TCR GSM，不是184份独立PBMC |
| QC后细胞 | **405,775** | 73个细胞群、9大谱系 |
| T细胞 | **264,059（65%）** | CD8和CD4各细分14亚群 |
| B细胞 | 8,255（2%） | 外周B细胞层 |
| NK/ILC | 46,955（11.6%） | 与细胞毒免疫一起分析 |
| 单核细胞 | 66,480（16.4%） | 主要髓系层 |
| DC | 6,228（1.5%） | 另有pDC、HSC/MPP及极少mast/erythrocyte/platelet |

10x每通道装载约8,000–10,000个细胞；目标深度约50,000 read pairs/cell用于GEX、20,000用于TCR。小谱系包括mast 305、erythrocyte 700、platelet 475；正文对pDC和HSC/MPP的1,641计数表述合并/不够清晰，复用时应以开放metadata中的最终标签逐类计数。

### 3.3 公共数据包有什么

- **GSE290722**：184个样本条目，平台NovaSeq 6000，BioProject PRJNA1229630。
- **`GSE290722_RAW.tar`约871.4 MB**：逐样本CSV处理后数据。
- **`GSE290722_metadata.csv.gz`约9.3 MB**：细胞/样本/临床元数据。
- **原始FASTQ不在GEO公开下载**：因患者隐私，需按论文说明向Kite/Gilead（medinfo@kitepharma.com）提交数据使用协议；对方称目标在60天内决定，且审批具有裁量性。报告下载方式不等于替用户发送申请。
- **Zenodo 15007741，约1.9 GB**：`metadata.RDS`约11.9 MB、`normalized_count_data.RDS`约1.9 GB，适合无需原始reads的再分析。
- **GitHub**：分析脚本与图谱代码；应同时记录release/commit。

### 3.4 如何获取：按你的目的选择

#### 路线A：最快进行状态/克隆复用

从Zenodo下载两个RDS并按barcode连接；从GEO补充metadata核对样本和结局字段。先查看对象维度和TCR列是否已包含clone ID/链信息，再决定是否需要逐GSM CSV。

#### 路线B：逐样本重建处理后对象

下载GEO 871.4 MB TAR和metadata。依据GSM标题把RNA/TCR配成92对；以patient和timepoint为主键，而不是把GSM当生物重复。

#### 路线C：申请原始数据

只有在必须重跑Cell Ranger或审计VDJ reads时才需要DUA。先准备研究目的、数据安全和机构信息；涉及发送申请前需用户再次确认。

#### 路线D：复现论文分析

结合Zenodo对象和GitHub脚本，先固定R包版本，再复现大谱系、T细胞亚群、clone expansion和患者级统计。探索性模型应严格在患者层交叉验证。

### 3.5 下载后先做什么

1. 核对32名患者、92份PBMC和184个GSM的三层关系。
2. 建立缺失时间点矩阵，不能当作完整32×4设计。
3. 检查RNA barcode与TCR barcode匹配率、productive chain及doublet/multiplet定义。
4. 所有疗效统计先pseudo-bulk到patient-timepoint。
5. 分开CAR/native标签，并确认其判定规则；TCR克隆大不等于天然排除CAR来源。

## 4. 主要发现

- 长期响应者具有更高的原生细胞毒/促炎T和NK活化特征。
- 长期响应与原生TCR clonotype扩增相关，支持宿主免疫重建参与持久肿瘤控制。
- 扩增克隆未被证明具有特定病毒抗原特异性，不能把克隆扩增简单解释为病毒再激活。
- 探索性模型在23例可用患者中以LOOCV评估（LtR 9 vs R/NR 14），样本量限制其临床泛化。

## 5. “状态—功能—驱动”证据链

纵向表达状态与TCR克隆提供“状态转变+谱系持续”的关联证据，疗效提供临床功能端点；但没有对原生克隆进行抗原鉴定或扰动，故更接近机制假说生成而非因果证明。

## 6. 推荐图版

全免疫73群图、CD8/CD4亚群、不同结局的时间动态、TCR克隆扩增与状态对应，以及患者级模型流程最适合引用。

## 7. 创新价值

长达12个月的纵向scRNA/scTCR；把宿主原生免疫纳入CAR-T疗效框架；处理后矩阵和代码公开，隐私边界说明清楚。

## 8. 局限性

样本不平衡且后期样本更可能来自存活/响应者；原始数据受限；克隆特异性未知；小规模模型存在过拟合和选择偏倚。

## 9. 对本综述架构的作用

适合“link cell state/function transitions with molecular drivers”和“live/longitudinal tracking”的临床层案例：真正需要导航的不只是CAR-T，还包括宿主免疫生态。

## 10. 可直接用于综述的观点

> 持久CAR-T疗效可能依赖工程细胞与宿主原生T/NK反应的协同；纵向单细胞表达和TCR克隆追踪显示，原生细胞毒状态及克隆扩增可构成额外的疗效轴。

## 11. 避免误读

- 184个GEO条目来自92份PBMC的RNA/TCR双文库，不是184份生物样本。
- 405,775个细胞不是独立患者重复。
- 原始FASTQ不是公开下载；公开的是处理后对象，原始数据需DUA。
- 克隆扩增不等于已经证明肿瘤抗原特异性。
