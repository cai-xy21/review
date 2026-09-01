# 《Distinct cellular dynamics associated with response to CAR-T therapy for refractory B-cell lymphoma》精读

## 论文信息

- 作者：Nicholas J. Haradhvala、Michael B. Leick、Kathleen Maurer 等
- 期刊：*Nature Medicine*
- 年份：2022；28: 1848–1859
- DOI：10.1038/s41591-022-01959-0
- 原文：[Nature Medicine](https://doi.org/10.1038/s41591-022-01959-0)
- PubMed：[PMID 36097221](https://pubmed.ncbi.nlm.nih.gov/36097221/)
- 全文：[PMCID PMC9509487](https://pmc.ncbi.nlm.nih.gov/articles/PMC9509487/)
- processed matrices：[GEO GSE197268](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE197268)
- raw reads：[dbGaP phs002922.v1.p1](https://www.ncbi.nlm.nih.gov/projects/gap/cgi-bin/study.cgi?study_id=phs002922.v1.p1)
- 代码：[GitHub](https://github.com/getzlab/Haradhvala_et_al_2022)

## 一句话结论

在 32 名 LBCL 患者、105 个纵向样本和 602,577 个高质量细胞中，tisa-cel 应答对应稀有 CD8 central-memory-like clone 的显著扩增，而 axi-cel 无应答产品富集 CAR-Treg；不同 CAR 设计具有不同有效状态动力学。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 患者 | 32 | axi-cel 19；tisa-cel 13 |
| 样本 | 105（论文分析）；GEO 109 records | 部分患者有重复输注/额外时点，record ≠ 患者 |
| 细胞 | 602,577 高质量 cells | 含 CAR-T 与 host PBMC，多种分选组分 |
| 主要时点 | baseline、IP、day 7；少数 day 14/30/retreatment | day 7 捕获扩增峰 |
| scTCR | 与 5′ GEX 配对 | 用于 IP 到 day 7 克隆扩增 |
| processed download | `GSE197268_RAW.tar` 3.0 GB | TAR of TAR；raw reads 不在 GEO |
| raw access | dbGaP phs002922 | 受控访问 |
| 临床观察 | 6 月内 15/32 进展/复发 | 产品特异分析样本数更小 |

## 1. 研究要解决的问题

axi-cel（CD28 costimulation）与 tisa-cel（4-1BB costimulation）为什么在患者体内形成不同扩增/分化动力学？哪些产品内稀有状态通过克隆扩增决定 response 或 relapse？

## 2. 方法框架

- baseline PBMC、infusion product、day 7 CAR+ / CAR− fractions 做 10x 5′ scRNA/scTCR；
- 在参考基因组加入 axi-cel/tisa-cel transgene，以 CAR reads 分类；
- pseudobulk 比较 product/time/response，避免单细胞伪重复；
- TCR 克隆频率连接 IP 与 day 7；
- RNA 读段推断 CD45RA/RO isoform；
- 体外 suppression 与 NSG lymphoma 模型验证 CAR-Treg 功能。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这是**CAR-T 产品 + 治疗前后宿主免疫系统 + TCR 克隆**的纵向图谱。它不仅测 CAR+ cells，也测 baseline/day7 CAR− PBMC，因此可比较产品内在状态、入体后的 CAR-T 演化和宿主免疫背景。

### 3.2 队列与样本组成

| 层级 | 规模/内容 | 应如何理解 |
|---|---:|---|
| patients | 32 LBCL | axi-cel 19、tisa-cel 13 |
| analysis samples | 105 | 论文明确的 scRNA/scTCR 样本数 |
| GEO records | 109 | 包含 retreatment/额外 records；下载时按 metadata 筛回论文分析集合 |
| baseline | 20 samples | day −30 12、day −5 4、day 0 4 |
| infusion products | 31 | 一位缺失/不合格应按 supplement 核对 |
| day 7 | 29 patients | 其中 22 有 CAR-sorted fractions；另有 unsorted |
| high-quality cells | 602,577 | 混合 T/B/NK/myeloid/DC/macrophage 等 |

### 3.3 图谱的细胞组成

IP 与 day 7 CAR+ 以 T cells 为主；baseline 和 day 7 CAR− 包含宿主 T cells、monocytes/macrophages、B cells、dendritic cells 等。T-cell 子图进一步分 CD4/CD8、naive、EM、TEMRA、memory-like、Treg 等。

以公开图中可核对的子集为例：baseline 20,361 T cells、day 7 CAR− 46,750、day 7 CAR+ 10,010 被用于 CD45 isoform/亚型比较。这些是特定分析子集，不等于 602,577 全图谱。

### 3.4 克隆与产品特异动力学

tisa-cel 应答者在 day 7 显示源自 IP 稀有 CD8 central-memory-like clones 的显著扩增；axi-cel 应答者更异质，IP 到 day 7 的 lineage shift 较弱。axi-cel NR 产品中 CAR-Treg 较多，且其抑制 conventional CAR-T 扩增的能力由体外与小鼠实验支持。

### 3.5 GEO/dbGaP 文件组成

`GSE197268`：

- 109 GSM records，平台 NovaSeq 6000；
- `GSE197268_RAW.tar` 约 3.0 GB，包含逐样本处理后 TAR/matrices；
- BioProject `PRJNA809638`；GEO 页面注明 raw sequencing 走 controlled dbGaP；
- GitHub 提供 preprocessing、pseudobulk、T-cell、reference refinement 与 figure code；
- Supplementary Tables 1–2 提供临床和 cell-level/sample metadata；作者 README 另指向 clustered AnnData objects。

### 3.6 如何获取

#### 路线 A：直接做下游分析

下载 3.0 GB GEO TAR，解包两层 archive；合并 cell-level metadata 与 clinical tables。优先使用作者 clustered AnnData 复现 UMAP/标签，再从 raw count matrices 验证。

#### 路线 B：重跑原始数据

申请 dbGaP `phs002922`，获批后下载 FASTQ；构建包含 GRCh38 + 对应 CAR construct 的 reference，不能只对人参考比对后再推断所有 CAR+ cells。

#### 路线 C：只研究 TCR

提取 productive paired αβ clonotypes，按 patient 和 timepoint 计算 clone frequency；只比较在 IP/day7 均达到最低 T-cell 数的患者，避免 coverage 偏差。

### 3.7 下载后先做什么

先区分 product、timepoint、sorting strategy、CAR construct 和 response。检查 105 analysis samples 与 109 GEO records 的差异。CAR RNA classification 有 dropout；应结合 flow sort/metadata，而不是把无 transgene read 直接视为 CAR−。

## 4. 主要发现

day 7 CAR-T 具有强激活/增殖程序，但不同产品从 IP 到体内的状态转换不同。tisa-cel 的 CD8 dominance 和 memory-like clone expansion 与 response 相关；axi-cel 中较高 CAR-Treg 与无应答/复发相关。

## 5. 功能验证

作者将 CAR-Treg 加入 conventional CAR-T 体系，观察到抑制扩增；在 lymphoma mouse model 中，25% CAR-Treg 可促进复发/减弱控制。该验证把 rare-state association 提升为可干预的功能机制。

## 6. 推荐图版

- Fig. 1：32 人、602,577-cell 全景；
- Fig. 2：产品/时间的 pseudobulk 演化；
- Fig. 3–4：tisa/axi response 与克隆动力学；
- CAR-Treg 图与 Extended Data 8：状态—功能因果验证。

## 7. 创新价值

1. 同一框架比较两种商业 CAR 设计。
2. 联合产品、host PBMC、纵向 scRNA 与 TCR。
3. 发现并功能验证 CAR-Treg 作为治疗失败状态。

## 8. 局限性

1. 产品分组后患者数有限，且 observational comparison 有混杂。
2. 主要追踪到 day 7，长期组织状态不足。
3. CAR RNA dropout 与 flow sorting 不完全一致。
4. 小鼠中 25% Treg 比例高于多数临床产品情境。

## 9. 对本章节的作用

这是“link transitions with molecular drivers”与“optimize navigation conditions”的主力文献：TCR 追踪证明哪些产品状态真正扩增，功能实验指出应减少的 CAR-Treg 状态。

## 10. 可直接用于综述的观点

> 不同 CAR costimulatory design 具有不同的有效状态路径：tisa-cel 应答可由产品中稀有 memory-like CD8 clones 的体内扩增驱动，而 axi-cel 失败可与功能性 CAR-Treg 富集相关，因此状态优化必须针对具体 CAR 架构和动力学而非使用单一通用 marker（Nature Medicine 2022, Haradhvala）。

## 11. 避免误读

- 不要把 109 GSM 写成 109 名患者。
- 不要把全部 602,577 cells 写成 CAR-T。
- 不要把 transgene RNA 阴性当作真实 CAR 阴性。
- 不要把 axi-cel/tisa-cel 差异完全归因于 costimulatory domain，制造和人群也不同。

