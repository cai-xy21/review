# 《Cross-tissue immune cell analysis reveals tissue-specific features in humans》精读

## 论文信息

- 第一作者：Cecilia Domínguez Conde、Chuan Xu、Lorna B. Jarvis、Daniel B. Rainbow、Sarah B. Wells（共同第一作者）
- 通讯作者：Kourosh Saeb-Parsy、Joanne L. Jones、Sarah A. Teichmann
- 期刊：*Science*
- 年份：2022；376(6594): eabl5197
- DOI：10.1126/science.abl5197
- 原文：[Science](https://www.science.org/doi/10.1126/science.abl5197)
- 开放全文：[PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC7612735/)
- 图谱主页：[Cross-tissue Immune Cell Atlas](https://www.tissueimmunecellatlas.org/)
- 在线浏览：[Broad Single Cell Portal SCP1845](https://singlecell.broadinstitute.org/single_cell/study/SCP1845/cross-tissue-immune-cell-analysis-reveals-tissue-specific-features-in-humans)
- 原始测序数据：[BioStudies/ArrayExpress E-MTAB-11536](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-11536)
- 分析代码：[Zenodo 10.5281/zenodo.6334988](https://doi.org/10.5281/zenodo.6334988)
- CellTypist：[GitHub](https://github.com/Teichlab/celltypist)

## 一句话结论

作者联合12名成人供者的16种组织、约36万个单细胞转录组及αβ TCR、γδ TCR和BCR测序，建立健康人体跨组织免疫参考图谱；结果表明，免疫细胞身份不仅由谱系决定，还受到组织位置和克隆历史塑造，同一T细胞克隆可跨多个器官并呈现不同的效应记忆与组织驻留状态。

## 数据护照（先区分四个数字）

| 层级 | 规模 | 含义 |
|---|---:|---|
| 供者 | 12 | 多组织来自同一供者，是控制个体差异的核心设计 |
| 组织 | 16 类（公开标签可见 17 个解剖名称） | 组织类别与数据门户标签口径略有差异 |
| QC 后全部细胞 | 357,211 | 含免疫及少量非免疫/质控保留对象 |
| 免疫细胞 | 329,762 | 论文免疫图谱的主体 |
| 基因 | 36,601 | 公共全局 AnnData 的变量规模 |
| 模态 | 10x 5′/3′ scRNA、TCRαβ、TCRγδ、BCR、流式、smFISH、qPCR | 并非每个细胞、样本或组织都具备全部模态 |

## 1. 研究要解决什么问题

外周血容易获取，因此人类免疫学长期以血液为主要研究材料。但血液只是免疫系统的一个流动窗口，无法代表淋巴器官、黏膜、肝脏、肺和肠道中的组织驻留免疫状态。

论文解决四个相互关联的问题：

1. 健康人体不同组织中存在哪些可重复的免疫细胞类型和状态；
2. 同一种免疫细胞进入不同组织后，会产生哪些组织适应程序；
3. 同一个TCR或BCR克隆是否会跨越不同组织与转录状态；
4. 如何建立可迁移、可更新的跨组织免疫细胞注释体系。

## 2. 研究设计

### 2.1 供者与取样

- 12名成年去世器官供者；
- 同一供者尽可能采集多个组织，从设计上减少年龄、性别、病史、用药和个体遗传背景造成的混杂；
- 6名供者采用剑桥统一组织处理流程；另外6名采用与不同组织适配、但尽量协调的处理流程；
- 研究对象主要代表非肿瘤成人组织，但去世器官供者并不等同于完全健康的普通人群。

论文称覆盖16种组织。公开对象中可见17个组织/解剖区标签，是因为空肠被进一步拆分为上皮和固有层两个区室：

| 缩写 | 组织或区室 |
|---|---|
| BLD | 血液 |
| BMA | 骨髓 |
| SPL | 脾脏 |
| LLN | 肺引流淋巴结 |
| MLN | 肠系膜淋巴结 |
| LNG | 肺 |
| LIV | 肝脏 |
| THY | 胸腺 |
| SKM | 骨骼肌 |
| OME | 网膜 |
| CAE | 盲肠 |
| DUO | 十二指肠 |
| ILE | 回肠 |
| TCL | 横结肠 |
| SCL | 乙状结肠 |
| JEJEPI | 空肠上皮区 |
| JEJLP | 空肠固有层 |

并非每名供者都贡献全部组织。因此，比较细胞比例或表达时必须保留`donor × tissue`结构，不能把所有细胞直接视为独立重复。

### 2.2 实验层

研究包含以下数据模态：

1. **scRNA-seq/GEX**：用于细胞类型、状态和组织适应程序分析；
2. **αβ TCR V(D)J**：用于常规T细胞的克隆扩增与跨组织共享；
3. **γδ TCR V(D)J**：使用作者基于Cell Ranger并结合Dandelion构建的定制流程重新注释；
4. **BCR V(D)J**：用于B细胞克隆分布、同种型和体细胞突变分析；
5. **smFISH**：验证CRTAM⁺ CD8⁺ T细胞的组织位置；
6. **流式细胞术和qPCR**：验证CD8记忆状态、γδ T细胞以及部分髓系发现。

scRNA-seq使用10x Genomics平台：剑桥样本包括5′ v1和5′ v2，哥伦比亚样本包括3′ v3；TCRαβ、TCRγδ和BCR V(D)J文库只在5′ GEX样本中构建。测序平台为Illumina HiSeq 4000或NovaSeq 6000。

## 3. 数据规模：几个数字为什么不同

| 统计口径 | 细胞数 | 含义 |
|---|---:|---|
| 摘要中的近似规模 | 约360,000 | 对整体项目规模的四舍五入描述 |
| 严格质控后的全部细胞 | 357,211 | 包括免疫与少量非免疫细胞 |
| 最终免疫细胞图谱 | 329,762 | 论文主体和公开Single Cell Portal显示的免疫细胞对象 |
| 基因数 | 36,601 | Broad Single Cell Portal公开对象的特征数 |

因此，“约36万个免疫细胞”并不严谨。推荐写法是：

> 研究获得357,211个质控后细胞，其中329,762个进入跨组织免疫细胞图谱。

作者的基础过滤标准包括：

- 少于1,000个UMI的细胞剔除；
- 少于600个检测基因的细胞剔除；
- 使用Scrublet识别doublets；
- hashtag样本使用Hashsolo拆分；
- Cell Ranger 6.1.1进行比对和定量；
- Scanpy 1.6.0用于标准化、降维和聚类等下游分析。

## 4. 数据资产详细说明

### 4.1 全局免疫图谱

最终免疫图谱包含329,762个细胞和36,601个基因，覆盖：

- T细胞与先天样淋巴细胞；
- B细胞、浆细胞和B细胞前体；
- 单核细胞、巨噬细胞和树突状细胞；
- NK、ILC3、肥大细胞；
- 骨髓造血祖细胞、红系和巨核细胞；
- 少量跨谱系doublet类别。

CellTypist在该数据中首先给出43个细粒度预测亚型，作者再结合聚类、marker、组织分布及V(D)J信息人工校正；最终研究重点描述41类免疫细胞，并将这些标签反馈到CellTypist，使更新后的参考体系达到101种细胞类型/状态。

### 4.2 CellTypist训练参考与本研究数据不是同一件事

CellTypist初始训练集整合：

- 19项既有单细胞研究；
- 20种人体组织；
- 两级注释体系；
- 低层级/高分辨率模型包含91个协调后的免疫细胞标签。

这部分是**训练参考**，用于建立跨组织自动注释器；12名供者的329,762个免疫细胞是**论文新产生的目标图谱**。不能把19项训练研究的细胞数加入本论文的新测细胞数。

模型使用随机梯度下降训练的逻辑回归分类器。优势是速度快、每个细胞类型对应的基因权重可解释；局限是参考库外的新状态可能被强制分到最相近的已知类别，所以作者仍保留人工检查和新亚型发现环节。

### 4.3 T与先天淋巴细胞数据

作者定义18类T/ILC状态，包括：

- `Tnaive/CM_CD4`、活化的`Tnaive/CM_CD4`；
- `Teffector/EM_CD4`；
- `Tfh`、`Tregs`、`Trm_Th1/Th17`；
- `Tnaive/CM_CD8`；
- `Trm_gut_CD8`、`Tem/emra_CD8`、`Trm/em_CD8`；
- `MAIT`；
- `Trm_Tgd`、`Tgd_CRTAM+`；
- NK和ILC相关状态及增殖群。

公开数据将T/ILC表达对象与TCR对象分开提供，使用户可以只下载转录组，也可以下载已经连接GEX与受体序列的数据。

### 4.4 TCR数据是什么样的

αβ TCR对象将单细胞转录状态、组织、供者和受体序列连接起来。链配对分析显示：

- T细胞中约50%–60%具有一对主要受体链；
- 约5%–20%表现为orphan chain，即仅捕获一侧链；
- 约5%–10%存在extra chain；
- extra α/VJ链多于extra β/VDJ链，与TCRβ更严格的等位排斥相符。

作者根据相同CDR3核苷酸序列定义克隆关系，并区分：

- 单细胞clonotype；
- 扩增clonotype：>1个细胞；
- 高度扩增clonotype：>20个细胞。

必须注意：这里的TCR覆盖仅来自5′样本，因此不能用T细胞表达对象的总细胞数直接作为“有配对TCR的细胞数”。进行复用时应在`adata_TILC_TCR_onlyseq.h5ad`内统计productive paired chains，而不是从全文总细胞数推算。

### 4.5 γδ TCR数据

标准10x/Cell Ranger流程主要面向αβ TCR。作者针对γδ链开发定制分析流程，并用Dandelion重新注释contigs。数据支持：

- `Tgd_CRTAM+`主要携带可检测的productive γδ TCR；
- MAIT中的TRAV1-2富集；
- TRAV1-2⁺细胞的TRAJ使用具有组织差异，例如TRAJ33偏脾和肝、TRAJ12偏肝、TRAJ29/TRAJ36偏空肠；
- MAIT的TCRβ V基因使用比传统认识更具多样性。

### 4.6 BCR数据

BCR对象连接B细胞转录状态、组织、免疫球蛋白同种型、克隆身份和突变信息。作者发现：

- 浆细胞克隆更倾向组织局限；
- 经典记忆B细胞克隆更容易跨组织分布；
- 不同组织具有不同的抗体同种型组成；
- B细胞状态和克隆分布共同反映局部免疫生态。

这部分不是T细胞章节的主线，但能说明该论文是完整的适应性免疫克隆图谱，而不仅是T细胞聚类文章。

## 5. 公开文件、体积与用途

官方图谱站点提供以下H5AD文件。体积为2026年8月读取服务器HTTP元数据得到的近似下载体积：

| 官方文件 | 体积 | 内容 | 推荐用途 |
|---|---:|---|---|
| [`global.h5ad`](https://cellgeni.cog.sanger.ac.uk/pan-immune/global.h5ad) | 5.13 GB | 全局免疫图谱的处理后对象 | 浏览细胞类型、组织分布、embedding和注释 |
| [`CountAdded_PIP_global_object_for_cellxgene.h5ad`](https://cellgeni.cog.sanger.ac.uk/pan-immune/CountAdded_PIP_global_object_for_cellxgene.h5ad) | 10.23 GB | 带count层的全局免疫对象 | 重新归一化、差异表达、二次建模 |
| [`t-cells.h5ad`](https://cellgeni.cog.sanger.ac.uk/pan-immune/t-cells.h5ad) | 3.01 GB | T细胞与先天淋巴细胞处理后对象 | T细胞状态和组织分布分析首选 |
| [`CountAdded_PIP_T_object_for_cellxgene.h5ad`](https://cellgeni.cog.sanger.ac.uk/pan-immune/CountAdded_PIP_T_object_for_cellxgene.h5ad) | 6.00 GB | 带count层的T/ILC对象 | T细胞重新分析与差异表达 |
| [`adata_TILC_TCR_onlyseq.h5ad`](https://cellgeni.cog.sanger.ac.uk/pan-immune/adata_TILC_TCR_onlyseq.h5ad) | 2.97 GB | GEX注释与TCRαβ序列相连的对象 | clonotype、扩增和跨组织共享 |
| [`adata_TILC_TCRgd_onlyseq.h5ad`](https://cellgeni.cog.sanger.ac.uk/pan-immune/adata_TILC_TCRgd_onlyseq.h5ad) | 495 MB | GEX与γδ TCR序列对象 | γδ T细胞和组织特异受体分析 |
| [`TICA_B_BCR.h5ad`](https://cellgeni.cog.sanger.ac.uk/pan-immune/TICA_B_BCR.h5ad) | 1.10 GB | B细胞GEX与BCR对象 | B细胞克隆、同种型和突变分析 |

“Download”与“Download Raw”的区别主要是后者加入可供重新分析的count信息；这里的“raw”通常指原始计数层，不等于FASTQ。真正的原始测序reads应从E-MTAB-11536获取。

### 5.1 按目的选择下载

#### 只想看图谱和marker

使用[Single Cell Portal](https://singlecell.broadinstitute.org/single_cell/study/SCP1845/cross-tissue-immune-cell-analysis-reveals-tissue-specific-features-in-humans)在线浏览，或者下载`global.h5ad`。

#### 只研究T细胞状态

下载`t-cells.h5ad`。如果需要自己重新归一化或进行严谨的供者级差异表达，下载带count层的`CountAdded_PIP_T_object_for_cellxgene.h5ad`。

#### 研究TCR克隆扩增和跨组织共享

下载`adata_TILC_TCR_onlyseq.h5ad`；γδ T细胞另下载`adata_TILC_TCRgd_onlyseq.h5ad`。

#### 从FASTQ重新处理

从[E-MTAB-11536](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-11536)下载原始数据与样本信息，按5′/3′化学版本分别运行Cell Ranger。该路线数据量和计算成本最高，但能自主决定比对参考、QC、VDJ过滤和doublet策略。

#### 复现论文代码

下载[Zenodo代码包](https://doi.org/10.5281/zenodo.6334988)，v1.1压缩包约57.7 MB；同时查看[GitHub仓库](https://github.com/Teichlab/TissueImmuneCellAtlas/tree/v1.1)。CellTypist工具本身使用独立仓库。

### 5.2 下载后检查对象

```python
import scanpy as sc

adata = sc.read_h5ad("t-cells.h5ad", backed="r")
print(adata)
print(adata.obs.columns.tolist())
print(list(adata.layers.keys()))
print(list(adata.obsm.keys()))
```

读取TCR对象后，先检查受体相关字段和缺失率：

```python
tcr = sc.read_h5ad("adata_TILC_TCR_onlyseq.h5ad", backed="r")

print(tcr)
print([x for x in tcr.obs.columns
       if "clone" in x.lower() or "chain" in x.lower()])
```

H5AD字段会随导出版本变化，不应凭论文图注假定具体键名。先检查`.obs`、`.layers`和`.obsm`，再决定用哪个表达层和clonotype字段。

## 6. CellTypist方法的意义

CellTypist不是生成式基础模型，而是以协调后的参考标签训练逻辑回归分类器。其价值在于：

1. 同时覆盖血液、淋巴器官和非淋巴组织；
2. 输出细胞类型预测及可解释的marker权重；
3. 计算成本低，适合百万细胞级快速注释；
4. 支持通过新图谱持续更新参考标签。

但它不能替代新状态发现：若训练参考中不存在某个组织特异状态，分类器仍可能把它归入最近的既有类别。论文中γδ T细胞亚型的人工修正，正说明“自动注释 + 专家复核 + 独立验证”才是可靠流程。

## 7. T细胞核心发现

### 7.1 CD8记忆状态存在清楚的组织分区

作者识别三类主要CD8记忆状态：

| 状态 | 代表基因 | 主要组织 | 生物学解释 |
|---|---|---|---|
| `Trm_gut_CD8` | CCR9、ITGAE、ITGA1 | 空肠、回肠等肠道 | 肠道归巢与组织驻留 |
| `Tem/emra_CD8` | CX3CR1、细胞毒程序 | 血、骨髓、肺、肝等血液丰富组织 | 循环效应记忆/TEMRA |
| `Trm/em_CD8` | CRTAM | 脾、骨髓、淋巴结、部分肺 | 淋巴组织偏好的驻留/效应记忆状态 |

该结果说明，相同的“CD8记忆T细胞”标签会掩盖不同的迁移、驻留和组织适应程序。

### 7.2 同一克隆可以跨组织、跨状态

TCR分析发现：

- 扩增克隆主要位于组织驻留记忆群，包括CD8 TRM和CD4 Th1/Th17；
- 高度扩增的克隆中，多数分布于至少5种组织；
- 同一个CD8克隆可同时包含`Tem/emra_CD8`和`Trm/em_CD8`细胞；
- `Trm_gut_CD8`克隆可在多个肠道区域共享；
- 少数CD4克隆同时出现Treg和效应表型，但频率较低。

最稳妥的解释是：克隆身份与转录状态并非一一对应，组织环境可在共同克隆背景上塑造不同表型。TCR共享不能单独确定状态转换方向。

### 7.3 γδ T和MAIT也具有组织适应

- `Trm_Tgd`表达CCR9、ITGAE和ITGA1，主要分布在肠道；
- `Tgd_CRTAM+`表达CRTAM、IKZF2和ITGAD，偏脾、骨髓和肝；
- MAIT的TRAJ和TRBV使用存在组织差异。

这说明“组织塑造受体库与状态”的结论不仅适用于常规αβ CD8 T细胞。

## 8. 空间证据应该如何理解

论文使用smFISH检测CD3D、CD8A和CRTAM，在肝脏和肺引流淋巴结中验证CRTAM⁺ CD8⁺ T细胞。该证据的价值是证明scRNA-seq定义的状态确实存在于原位组织，而不完全是解离产物。

但该研究没有生成全转录组空间坐标，也没有系统分析细胞邻域。因此，应称为：

> 单细胞跨组织图谱，并辅以smFISH原位验证。

不应称为“空间转录组图谱”。

## 9. 其他免疫谱系发现

虽然本章节关注T细胞，论文还有三项重要的全免疫发现：

1. **髓系细胞**：巨噬细胞和树突状细胞表现出强烈组织适应，部分状态只有结合多个组织才容易被分辨；
2. **B细胞**：浆细胞和记忆B细胞具有不同的跨组织克隆分布模式；
3. **组织免疫邻域**：不同组织由不同免疫谱系组合构成，例如骨髓富含祖细胞，脾和淋巴结富含B细胞但髓系组成不同，肠道富含TRM和浆细胞。

这些发现支撑一个更广的结论：T细胞状态必须放在完整组织免疫生态中解释。

## 10. 推荐图版

### 首选：Fig. 4

Fig. 4同时包含：

- T/ILC UMAP与marker；
- 各状态的组织分布；
- CD3D/CD8A/CRTAM smFISH；
- clonotype size；
- 跨组织TCR克隆重叠。

最适合支撑：

> T-cell state is jointly shaped by tissue context and clonal identity.

### 图谱方法：Fig. 1

适合介绍：

- 12名供者和跨组织取样；
- scRNA-seq与αβ TCR、γδ TCR、BCR联合测序；
- CellTypist训练和自动注释框架；
- 全局免疫图谱。

### 全局资源：Fig. 5

适合章节收束，展示不同组织的免疫细胞组成以及可持续更新的CellTypist参考体系。

如果只能选择一张，选**Fig. 4**；若要讲“图谱如何建立”，使用**Fig. 1 + Fig. 4**。

## 11. 在“Charting T cell molecular landscape”章节中的位置

该文应承担“健康跨组织基线”角色：

```text
Science 2022, Domínguez Conde
健康人体：组织位置塑造T细胞状态，克隆可跨器官分布
                       ↓
Science 2021, Zheng / Nature Medicine 2023, Chu
肿瘤环境：泛癌TIL状态、耗竭与TSTR应激程序
                       ↓
Nature Methods 2025, Xue
跨炎症和癌症：统一CD8参考空间与状态间克隆共享
                       ↓
空间组学研究
状态位于何处、与哪些细胞形成邻域
```

它补充的不是更多肿瘤细胞，而是一个关键参照：肿瘤中的TRM、效应和应激程序，必须与正常器官固有的组织适应区分。

## 12. 创新价值

1. 通过同一供者多组织采样，提高了跨组织比较的可信度；
2. 同时覆盖转录组、αβ TCR、γδ TCR和BCR；
3. 将细胞状态、组织位置和克隆身份连接在同一图谱中；
4. 建立可解释、可持续更新的CellTypist免疫注释框架；
5. 提供公开H5AD、原始测序数据、交互式浏览器和代码，复用门槛较低。

## 13. 局限性

1. 只有12名成人器官供者，人口学多样性和统计功效有限；
2. 各供者并非拥有完全相同的组织组合，部分组织只来自少数供者；
3. 两套组织处理策略、5′/3′化学版本和两个采集中心引入技术异质性；
4. V(D)J只存在于5′样本，TCR/BCR覆盖与GEX覆盖不相等；
5. 去世供者及器官获取过程可能产生缺血、应激和终末期生理影响；
6. scRNA-seq依赖组织解离，会损失脆弱细胞并改变即时表达；
7. 自动注释受训练参考限制，部分新状态仍依赖人工修正；
8. smFISH只验证少量基因和位置，不能替代全转录组空间组学；
9. TCR克隆共享证明共同克隆身份，但不能确定迁移方向、抗原或状态转换顺序。

## 14. 可直接用于综述的表述

> 对12名成人供者16种组织的单细胞转录组和V(D)J联合分析表明，CD8⁺记忆T细胞可分为具有不同组织分布的肠道驻留、循环效应记忆和CRTAM⁺驻留/效应记忆状态；高度扩增的TCR克隆可跨越多个器官并呈现不同转录表型，说明T细胞状态必须在组织位置和克隆身份的共同背景下解释（Science 2022, Domínguez Conde）。

## 15. PPT单页格式

### 标题

**T-cell state is jointly shaped by tissue context and clonotype**

### 三条正文

- 12名供者、16种组织、329,762个免疫细胞；
- 联合scRNA-seq、αβ/γδ TCR、BCR与smFISH；
- 同一TCR克隆可跨多个组织，并呈现不同的CD8记忆/驻留状态。

### 配图

- 主图：Fig. 4A–D，T/ILC状态和组织分布；
- 辅图：Fig. 4G–H，克隆扩增和跨组织共享；
- 如需原位证据：Fig. 4E–F，CRTAM⁺ CD8⁺ T细胞smFISH。

### 图下注

> Healthy cross-tissue immune atlas: scRNA-seq + V(D)J + smFISH  
> **Science 2022, Domínguez Conde**

## 16. 避免误读

- 不要把329,762个免疫细胞写成全部T细胞；
- 不要把摘要的约36万与最终免疫图谱细胞数混用；
- 不要把同一供者多组织设计写成所有供者都有全部16种组织；
- 不要把H5AD中的raw count层写成FASTQ；
- 不要把克隆跨组织共享直接解释为迁移方向；
- 不要把smFISH验证写成空间转录组；
- 不要把CellTypist预测当成不需要人工复核的真值。
