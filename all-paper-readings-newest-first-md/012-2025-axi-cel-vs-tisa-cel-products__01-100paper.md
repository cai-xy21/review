# 《Comparison of axicabtagene ciloleucel and tisagenlecleucel patient CAR-T cell products by single-cell RNA sequencing》精读

## 论文信息

- **作者/期刊/年份**：Yu et al., *Journal for ImmunoTherapy of Cancer*, 2025
- **DOI**：[10.1136/jitc-2025-011807](https://doi.org/10.1136/jitc-2025-011807)
- **PMID / PMCID**：40730421 / [PMC12306477](https://pmc.ncbi.nlm.nih.gov/articles/PMC12306477/)
- **数据**：[GSE297676](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE297676)，BioProject PRJNA1265670

## 一句话结论

57份真实世界商业CAR-T输注产品显示，axi-cel与tisa-cel具有显著不同的状态组成；在axi-cel中，持久缓解更稳定地关联于跨亚群的核糖体/蛋白合成与MYC程序，而不是某一个单独细胞簇。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 队列 | 大B细胞淋巴瘤患者商业输注产品袋洗液，57例 |
| 产品 | axi-cel 39；tisa-cel 18 |
| scRNA | 10x，QC后105,326个细胞 |
| 细胞分配 | axi-cel 78,714；tisa-cel 26,612 |
| 图谱 | 17个cluster：15个T细胞状态 + 2个髓系状态 |
| 疗效 | axi持久缓解15/39；tisa持久缓解4/18 |
| GEO下载 | `RAW.tar`约2.1 GB；`rawcount.rds`约633.8 MB；`meta.rds`约596.4 KB；FASTQ在SRA |

## 1. 研究要解决的问题

axi-cel与tisa-cel在共刺激域和制造周期上不同，但跨产品、带临床结局且样本量足够的单细胞产品比较有限。作者询问：两种商业产品的细胞状态如何不同；哪些产品内特征关联持久缓解；这些特征是特定亚群丰度还是跨状态分子程序。

## 2. 实验与分析框架

研究从输注袋洗液获得57个pre-infusion产品，做10x scRNA、整合聚类和差异表达，将细胞注释与产品、CAR检测、临床缓解和毒性关联。另做体外制造模拟/流式支持制造时间解释，但临床scRNA GEO样本仍为57个患者产品。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这是产品袋残余材料的横断面scRNA图谱，每位患者一个产品样本，并非输注后的纵向外周血。临床标签与样本一一对应，因此患者是独立统计单位。产品内细胞数不同，比较cluster比例或signature时应先聚合到patient/product层，不能把105,326个细胞当作同等独立重复。

### 3.2 多大规模、覆盖哪些生物情境

| 层级 | axi-cel | tisa-cel | 合计 |
|---|---:|---:|---:|
| 患者/产品样本 | 39 | 18 | **57** |
| QC后细胞 | 78,714 | 26,612 | **105,326** |
| 持久缓解 | 15（38%） | 4（22%） | 19 |
| CAR+比例中位数 | 约55% | 约20% | 产品差异显著 |

整合图谱有17个cluster，其中15个为T细胞状态，覆盖naïve/central-memory、effector-memory、cytotoxic、cycling及激活相关程序；另有mregDC和inflammatory antigen-presenting cell（IAC）两个髓系cluster。其存在提示袋洗液中有低丰度非T细胞，但不能把它们解释成CAR-T状态。

### 3.3 公共数据包有什么

- **GSE297676**：57个GSM，平台NextSeq 2000；每个GSM对应一个患者产品，页面附临床/产品元数据。
- **`GSE297676_RAW.tar`约2.1 GB**：GEO提交者的处理后文件集合，主要为MTX/TSV，可逐样本重建稀疏矩阵。
- **`GSE297676_rawcount.rds`约633.8 MB**：合并原始计数对象，最适合R/Seurat快速复用。
- **`GSE297676_meta.rds`约596.4 KB**：细胞及样本元数据；与count对象barcode一一核对。
- 原始reads在SRA/BioProject PRJNA1265670，可用于统一Cell Ranger参考和QC。

### 3.4 如何获取：按你的目的选择

#### 路线A：直接复用论文对象

下载`rawcount.rds`和`meta.rds`，检查对象类型、barcode交集和patient/product/outcome字段，再构建Seurat对象。这是做signature和患者层pseudo-bulk的最快路径。

#### 路线B：逐样本矩阵

下载2.1 GB TAR，按GSM解包MTX/TSV；适合使用Scanpy或保留逐患者独立QC。不要把文件名中的RAW误认为FASTQ。

#### 路线C：从FASTQ重建

由SRA Run Selector导出57样本run表，下载FASTQ并用统一参考重跑。若分析CAR转录本，确认参考是否包含axi/tisa转基因序列及其feature命名。

### 3.5 下载后先做什么

1. 核对57个patient ID、39/18产品分布及105,326个barcode。
2. 比较细胞数和QC指标是否随产品系统性不同，防止测序深度混杂。
3. cluster比例先在患者层计算，再按产品/结局比较。
4. 对疗效分析分别在axi和tisa内做；tisa持久缓解仅4例，功效很低。
5. 将15个T状态和2个髓系cluster分开汇总。

## 4. 主要发现

- axi-cel更偏记忆/细胞毒状态，tisa-cel更偏增殖状态，与两者制造周期和构型差异一致。
- axi-cel中CAR+与CAR−细胞差异明显；tisa-cel中差异较弱，作者将其与较长培养导致总体选择/重塑联系起来。
- axi-cel持久缓解关联核糖体、蛋白合成和MYC程序，并跨多个cluster出现，提示连续分子状态比单簇丰度更稳健。
- 未找到对严重CRS/ICANS稳定预测的单一cluster。

## 5. “状态—功能—驱动”证据链

产品构型/制造是自然扰动，scRNA状态与临床缓解相连，但不是随机比较；MYC/蛋白合成程序是候选质量轴而非已证实可编辑驱动。下一步应在受控制造实验中操纵培养时间、刺激或代谢并验证功能。

## 6. 推荐图版

两产品17-cluster组成、CAR+比例、患者层缓解关联以及跨cluster的ribosome/MYC signature最值得引用。

## 7. 创新价值

真实商业产品、57例患者和直接产品比较；公开患者级矩阵/元数据；从“某亚群越多越好”转向跨状态连续程序。

## 8. 局限性

非随机产品选择和年代/中心/适应证混杂；袋洗液可能与实际输注细胞略有偏差；tisa样本和缓解事件少；横断面产品不能直接揭示体内状态转移。

## 9. 对本综述架构的作用

适合“quantitatively characterizing products”和“optimize conditions”，展示如何把单细胞状态压缩成患者级产品质量指标。

## 10. 可直接用于综述的观点

> 商业CAR-T产品差异不仅体现在构型，也体现在可量化的状态组成；持久疗效关联可以表现为跨多个亚群共享的翻译/生物合成程序，而不一定由某个离散cluster独占。

## 11. 避免误读

- 57是患者/产品数，105,326是嵌套细胞数。
- 本研究比较商业axi-cel与tisa-cel，不等同于纯粹的CD28 vs 4-1BB因果实验。
- 两个髓系cluster不是CAR-T亚群。
- tisa-cel仅4个持久缓解，阴性结果不能解释为确定无关联。
