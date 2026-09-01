# 《Integrative mapping of human CD8+ T cells in inflammation and cancer》精读

## 论文信息

- 作者：Ziwei Xue、Lize Wu、Ruonan Tian 等
- 期刊：*Nature Methods*
- 年份：2025；22: 435–445；在线发表于 2024 年 11 月 29 日
- DOI：10.1038/s41592-024-02530-0
- 原文：[Nature Methods](https://www.nature.com/articles/s41592-024-02530-0)
- PubMed：[PMID 39614111](https://pubmed.ncbi.nlm.nih.gov/39614111/)
- 模型与代码：[GitHub](https://github.com/WanluLiuLab/scAtlasVAE)；[文档](https://scatlasvae.readthedocs.io/)
- 论文引用的数据版本：[Zenodo v1](https://zenodo.org/records/12542577)
- 持续更新的数据入口：[Zenodo concept DOI](https://doi.org/10.5281/zenodo.10472913)（自动指向最新版）
- 可复现分析笔记：[作者在线 notebooks](https://huarc.net/notebook/scatlasvae)

## 一句话结论

scAtlasVAE 将 68 项研究、961 份样本和 42 种疾病条件中的 1,151,678 个 CD8⁺ T 细胞整合到统一参考空间，并利用配对 TCR 克隆扩增与共享关系解析 18 个亚型及 GZMK⁺、ITGAE⁺、XBP1⁺ 三类耗竭状态。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| CD8 T 细胞 | 1,151,678 | 全图谱细胞数，不等于都有 GEX + 成对 TCR |
| 样本 | 961 | 样本、供者、研究是三个不同层级 |
| 研究 | 68 | accession 与是否含 scTCR 见 Supplementary Tables 1–2 |
| 条件 | 42 | 健康、感染、炎症、自免、癌症、irAE |
| 统一状态 | 18 | 人工可解释的参考标签，不是唯一真值 |
| 独特 TCR 字符串 | 498,679 | 通常含 TRA/TRB 字段 + `individual_id`；缺失链也可能形成字符串 |
| 全图谱 GEX | `.h5ad`，19,957 genes 或 4,000 HVGs | 差异表达用 all-genes；映射/可视化优先 HVG 对象 |
| 克隆关系 | `clone_subtype.csv` 等 | 先过滤链完整性，再区分 cell、clonotype 与 paired clonotype |

## 1. 研究要解决的问题

现有 CD8⁺ T 细胞研究常局限于单一癌种、组织或炎症疾病；不同研究又使用不同平台、命名体系和过滤标准，使“同名不同态”与“同态不同名”并存。

论文同时解决三个问题：

1. 如何在百万细胞尺度整合跨研究 scRNA-seq；
2. 如何保留真正的疾病、组织和状态差异；
3. 如何利用配对 TCR 把转录相似性提升为克隆关系证据。

## 2. scAtlasVAE 方法框架

### 2.1 模型结构

scAtlasVAE 是变分自编码器。其核心不是“VAE”名称本身，而是对批次信息的结构化处理：

- batch-invariant encoder 学习尽量不受研究/样本来源影响的潜在生物表示；
- batch-dependent decoder 在重建表达时显式接收批次信息；
- 可在无监督或提供细胞标签的监督模式下训练；
- 学到的参考潜在空间可用于 query-to-reference transfer learning。

### 2.2 评价维度

论文不仅评估批次混合，还评估细胞类型保留、跨图谱整合和标签转移，并与 scVI、scANVI、scPoli、SCALEX 等方法比较。

必须注意：批次混合与生物保真存在张力。任何单一 benchmark 分数都不能证明模型保留了所有疾病特异程序。

## 3. 数据规模与图谱组成

- 1,151,678 个 CD8⁺ T 细胞；
- 961 份样本；
- 68 项研究；
- 42 种疾病或生理条件；
- 覆盖外周血、健康组织、炎症组织、实体瘤和免疫检查点治疗相关不良事件；
- 部分细胞具有配对 TCRαβ 信息。

作者在统一图谱中定义 18 个 CD8⁺ T 细胞亚型，覆盖初始、中央记忆、效应记忆、TEMRA、组织驻留、黏膜相关、增殖和耗竭等程序。

### 3.1 数据到底是什么

这不是一套新测的单中心队列，而是作者从 **68 项公开或授权研究**中汇总、质控并统一注释的 CD8⁺ T 细胞参考图谱。核心数据层包括：

1. **单细胞转录组（GEX）**：每个细胞的基因表达矩阵，是聚类、状态注释、差异表达和参考映射的主体。
2. **细胞与样本元数据**：将细胞追溯到供者、样本、组织、疾病、治疗状态和原始研究。
3. **TCR 信息**：在原研究提供免疫受体测序时，记录 TCR 克隆关系，用于克隆扩增和跨状态克隆共享分析。不是全部 115 万个细胞都有配对 TCR。
4. **作者训练好的潜在表示与模型**：可直接把新队列映射到图谱，而不必从头训练百万细胞模型。
5. **独立 query 队列**：用于测试标签迁移和参考映射，覆盖多种肿瘤及 MAIT 数据。

公开文件主要使用 `.h5ad`，即 Scanpy/AnnData 的 HDF5 格式。通常包含表达矩阵 `.X`、逐细胞注释 `.obs`、基因信息 `.var`，并可能在 `.obsm` 中保存低维表示；实际字段应以下载后检查结果为准。

### 3.2 多大规模、覆盖哪些生物情境

| 层级 | 规模/内容 | 应如何理解 |
|---|---:|---|
| 细胞 | 1,151,678 个 CD8⁺ T 细胞 | 论文完整分析图谱的规模 |
| 样本 | 961 份 | 一个供者可贡献多个组织或时间点，不能把样本数等同于供者数 |
| 来源研究 | 68 项 | 跨平台、跨实验室整合，批次校正是核心问题 |
| 条件 | 42 种疾病或生理条件 | 包括健康、感染、炎症/自身免疫、癌症、irAE 等 |
| 细胞状态 | 18 个主要 CD8⁺ 亚型 | 统一注释体系，不代表唯一的自然分类 |

疾病覆盖很广，代表性内容包括：

- 健康外周血与多种健康组织；
- HIV、CMV、疟疾、Lyme disease、COVID-19 感染或疫苗等感染情境；
- 溃疡性结肠炎、类风湿关节炎、银屑病关节炎、强直性脊柱炎、川崎病/MIS-C 等炎症或自身免疫病；
- 黑色素瘤、NSCLC、乳腺癌、肾癌、头颈癌、结直肠癌、卵巢癌、胰腺癌、胃癌、前列腺癌、胶质瘤等实体瘤；
- 多发性骨髓瘤、AML、B-ALL、T-LGLL 等血液肿瘤；
- 检查点抑制剂相关结肠炎、关节炎等 irAE。

精确到每项研究和样本的来源应查论文的 Supplementary Tables：

- **Supplementary Table 1**：68 项研究的文献、疾病、组织、平台、是否具有 scTCR 以及 GEO/SRA/ArrayExpress/EGA 等 accession；
- **Supplementary Table 2**：样本级映射，字段包括 Sample ID、Individual ID、Tissue、Disease、Disease Status、Treatment、Sex、Age、Cell Number、GEX/TCR 对应 GSM 和 accession；
- **Supplementary Table 3**：整合方法 benchmark；
- **Supplementary Table 4**：细胞亚型 marker genes。

### 3.3 Zenodo 公共数据包有什么

截至 2026 年 8 月，Zenodo 最新公开版本为 v2，共 **21 个文件，约 10.82 GB（10.07 GiB）**。最重要的文件如下：

| 文件 | 体积 | 内容与用途 |
|---|---:|---|
| `huARdb_v2_GEX.CD8.all_genes.h5ad.gz` | 3.49 GB | 作者标注的全基因原始矩阵，19,957 个基因；适合差异表达和重新建模 |
| `huARdb_v2_GEX.CD8.hvg4k.h5ad` | 2.99 GB | 4,000 个高变基因的核心参考对象；一般复用首选 |
| `huARdb_v2_GEX.CD8.hvg4k.X_gex.npy` | 45.0 MB | 已计算的 scAtlasVAE GEX 潜在表示；适合直接可视化或下游分析 |
| `huARdb_v2_GEX.CD8.clone_subtype.csv` | 106 MB | TCR 克隆与细胞亚型关系；做克隆扩增/共享分析时下载 |
| `huARdb_v2_GEX.CD8.hvg4k.supervised.model` | 13.8 MB | 全图谱监督模型权重；用于 query-to-reference 映射 |
| `huARdb_v2_GEX.CD8.Tex.hvg4k.h5ad` | 978 MB | Tex 子集的 4,000 HVG 数据；用于精细研究三类 Tex |
| `huARdb_v2_GEX.CD8.hvg4k.Tex.supervised.model` | 9.1 MB | Tex 高分辨率注释模型 |
| `adata_cd8_chu.h5ad` | 219 MB | Chu 等 pan-cancer T-cell atlas 的 CD8⁺ 子集，用于外部图谱比较 |
| `adata_cd8_zheng.h5ad` | 633 MB | Zheng 等 TCellLandscape 的 CD8⁺ 子集，用于外部图谱比较 |

其余 query 数据包括肝癌、HNSCC 肿瘤与 PBMC、黑色素瘤、乳腺癌、RCC、NSCLC、TNBC、结直肠癌及两套 MAIT 数据，单文件约 18 MB–778 MB。这些文件的作用不是扩充主图谱规模，而是验证跨队列映射和标签迁移。

注意：Zenodo 页面文字曾把 Tex 文件名写成主图谱文件名；实际应以文件列表中的 `huARdb_v2_GEX.CD8.Tex.hvg4k.h5ad` 为准。

### 3.4 如何获取：按你的目的选择

#### 路线 A：快速浏览或映射自己的数据

从[最新版 Zenodo 入口](https://doi.org/10.5281/zenodo.10472913)下载：

1. `huARdb_v2_GEX.CD8.hvg4k.h5ad`；
2. `huARdb_v2_GEX.CD8.hvg4k.X_gex.npy`；
3. `huARdb_v2_GEX.CD8.hvg4k.supervised.model`；
4. 如需 TCR，再加 `huARdb_v2_GEX.CD8.clone_subtype.csv`；
5. 如需精细 Tex 注释，再加 Tex `.h5ad` 和 Tex model。

这是最实用的组合。只做参考映射时没有必要先下载全基因矩阵和全部 query 队列。

#### 路线 B：完整重做表达分析

下载 `huARdb_v2_GEX.CD8.all_genes.h5ad.gz`、4k HVG 对象、clone CSV，并从论文页面下载 Supplementary Tables 1–4。全基因文件适合重新选 HVG、差异表达或验证 marker；4k HVG 文件适合复现作者的参考空间。

#### 路线 C：严格对应论文发表时版本

使用论文 Data availability 中引用的[Zenodo v1：10.5281/zenodo.12542577](https://doi.org/10.5281/zenodo.12542577)。若目的是现在复用资源，则使用 concept DOI 指向的 v2；v2 增加了预计算 latent embedding 和独立 Tex 对象等文件。报告中应写明具体 version DOI，避免“同一 DOI、不同文件集”的复现歧义。

#### 路线 D：回到每项原始研究

先用 Supplementary Table 1 找到研究 accession，再用 Supplementary Table 2 将样本对应到 GEX 与 TCR 的 GSM/accession。原始数据分散在 GEO、SRA、ArrayExpress、GSA-human、EGA 和 10x Genomics 等平台。

论文明确说明，Zenodo 公共图谱排除了两套受控访问数据。因此，公开包不一定逐细胞等同于作者内部使用的完整 1,151,678 细胞集合。受控部分需根据 Supplementary Table 1 的 accession，向原始数据库及数据访问委员会申请；不能仅靠 Zenodo 获取。表中可明确追溯的一例是 AML 数据的 EGA study `EGAS00001004894`。

### 3.5 下载后先做什么

用 Scanpy 读取 4k HVG 对象并检查真实字段，不要凭文件说明假定表达层和元数据键名：

```python
import scanpy as sc

adata = sc.read_h5ad("huARdb_v2_GEX.CD8.hvg4k.h5ad")
print(adata)
print(adata.obs.columns.tolist())
print(adata.var_names[:10])
print(list(adata.obsm.keys()))
```

若机器内存有限，可只读打开：

```python
adata = sc.read_h5ad(
    "huARdb_v2_GEX.CD8.hvg4k.h5ad",
    backed="r"
)
```

全基因文件是 gzip 包装的 `.h5ad`，通常先用 `gzip -dk` 解压。10.82 GB 是下载体积，解压和载入内存后的占用会更大；普通笔记本优先使用 4k HVG 对象或预计算 embedding，服务器再使用全基因对象。

## 4. 三类耗竭状态

论文将 Tex 细分为：

### 4.1 GZMK⁺ Tex

表现出 GZMK 和耗竭/激活相关程序，位于循环/效应记忆与更深组织耗竭状态之间。克隆共享提示其在部分肿瘤中与循环或非驻留 T 细胞关系更紧密。

### 4.2 ITGAE⁺ Tex

表达 ITGAE/CD103 以及 PDCD1、LAG3、TIGIT 等耗竭相关基因，具有更强组织驻留和细胞毒程序。其与 ITGAE⁺/ITGB2⁺ TRM 的克隆共享关系提示局部驻留谱系与耗竭状态相互连接。

### 4.3 XBP1⁺ Tex

具有 XBP1、内质网应激/蛋白稳态相关特征，提示耗竭异质性还包含细胞应激维度，而不只是 TCF7—终末耗竭轴。

三者都表达耗竭相关基因，但组织分布、克隆扩增和与其他状态的共享关系不同。因此，“Tex”不应作为单一终点标签。

## 5. TCR 数据与分析

### 5.1 这里的“配对 TCR”是什么意思

这里的“配对”是指在**同一个单细胞**中同时获得 TCRα 和 TCRβ，而不是“TCR 与抗原/HLA 已经配对”。作者使用以下六个字段定义受体，并加入 `individual_id` 避免把不同供者中偶然相同的序列直接合并：

- α链 CDR3 氨基酸序列：`IR_VJ_1_junction_aa`；
- β链 CDR3 氨基酸序列：`IR_VDJ_1_junction_aa`；
- α链 V/J 基因：`IR_VJ_1_v_call`、`IR_VJ_1_j_call`；
- β链 V/J 基因：`IR_VDJ_1_v_call`、`IR_VDJ_1_j_call`。

因此，本研究的 clonotype 可以近似理解为：

> 同一供者内，具有相同 TRA-CDR3、TRB-CDR3 及相同 TRA/TRB V–J 使用的细胞群。

### 5.2 到底有多少

Supplementary Table 1 中，主参考图谱的 **68/68 项研究均标记为 `scTCR = Yes`**。作者的复现 notebook 在完整图谱中报告 **498,679 个不同的配对 TCRαβ clonotypes**。

论文公开数据排除了受控访问部分。对 Zenodo v2 的 `clone_subtype.csv` 统计得到：

- 公开表覆盖约 **1,140,167 个具有 clonotype 归属的 CD8⁺ T 细胞**；
- 相当于论文 1,151,678 个细胞的约 **99.0%**；
- 包含 **494,974 条公开 clonotype 记录**；
- 其中约 **33,967 个 clonotype**达到作者用于主要共享分析的扩增阈值，即同一 clonotype 至少 3 个细胞。

公开数低于论文完整数，主要应按 Data availability 所述理解为受控数据被排除，而不是约 1% 的细胞必然“测序失败”。引用时最稳妥的表述是：

> 主图谱具有单细胞配对 TCRαβ 信息；完整分析定义了约 49.9 万个 clonotypes，公共 clone 表覆盖约 114 万个细胞。

### 5.3 哪些外部验证队列有配对 TCR

主图谱之外，论文用于迁移学习或验证的队列并不全部具有 TCR：

| 外部队列 | 疾病/场景 | TCR情况 |
|---|---|---|
| Bassez et al. | 乳腺癌 | 有，CDR3αβ |
| Borràs et al. | 结直肠癌 | 有，CDR3αβ |
| Caushi et al. | NSCLC | 有，TCRαβ |
| Liu et al. | NSCLC | 有，TCRαβ |
| Luoma et al. | 口腔鳞癌 | 有，CDR3αβ |
| Watson et al. | 转移性黑色素瘤 | 有，CDR3αβ |
| Garner et al. | MR1四聚体分选MAIT | 有，CDR3αβ |
| Bi et al. | RCC | 无 |
| Zhang et al. | TNBC | 无 |
| Vorkas et al. | MAIT | 无 |

另外，Zheng 2021 pan-cancer atlas 中只有部分来源队列有 TCR；Chu 2023 T-cell map 在本研究的外部比较表中标记为无 scTCR。因此，跨图谱表达映射的规模大于可以进行克隆分析的规模。

### 5.4 论文具体怎样分析 TCR

论文不只是“携带了TCR字段”，而是把TCR作为主要生物学证据：

1. **克隆多样性与扩增**：用 D50 和 Gini index 比较不同细胞亚型、个体和疾病条件的克隆集中程度（Extended Data Fig. 6g–i）。
2. **扩增分层**：按 clonotype size 分为 1、2–5、6–50、51–100 和 >100 个细胞；主要克隆共享分析通常将 ≥3 个细胞定义为 expanded。
3. **跨状态克隆共享**：判断同一 αβ clonotype 是否同时出现在两个或多个转录状态，从而连接 Tcm/Tem、Temra、TRM、Tpex 和三类 Tex（Fig. 3d,e）。
4. **三类 Tex 的克隆结构**：比较 GZMK⁺、ITGAE⁺、XBP1⁺ Tex 的克隆扩增程度，以及它们与 GZMK⁺ Tem、ITGAE⁺/ITGB2⁺ TRM 等状态的共享关系（Fig. 3、Extended Data Fig. 7）。
5. **癌症—炎症—irAE 比较**：比较健康、普通炎症、检查点抑制剂相关 irAE 和实体瘤中扩增细胞的比例、组成和共享路径（Fig. 4、Extended Data Fig. 8）。
6. **ILTCK-LC/MAIT 分析**：分析 ILTCK-LC 的克隆扩增、与其他亚型的共享，以及 TRAV1-2⁺ 细胞的 TRAJ 使用（Extended Data Fig. 9）。
7. **外部队列验证**：在具有 TCR 的 query 队列中检查标签迁移后是否仍能复现克隆扩增和状态共享，包括 CPI-colitis 与结直肠癌的比较（Fig. 5、Extended Data Fig. 10）。

### 5.5 TCR证据能说明什么、不能说明什么

相同 αβ TCR 的细胞跨越两个状态，强烈支持它们来自同一个克隆，可用来建立“状态之间存在克隆联系”的证据。但它不能单独证明：

- 哪个状态先出现、哪个状态后出现；
- 该 TCR 识别的具体抗原、肽段或 HLA；
- 该克隆一定是肿瘤特异，而不是病毒特异或旁观者 T 细胞；
- 转录状态相似就具有相同杀伤功能。

所以，本论文的严谨结论是“clonal sharing supports a relationship between states”，而不是“TCR证明了单向分化轨迹”。

## 6. 炎症与 irAE 发现

论文把癌症图谱与自身免疫、普通炎症和检查点抑制相关不良事件放在同一坐标中比较。GZMK⁺ 与 ITGAE⁺ Tex 可在炎症和 irAE 中富集，但其组织组成、克隆扩增及与 TRM/循环细胞的共享方式不同。

该结果说明：

> 相似的“耗竭样”转录标签可能由不同组织来源和克隆路径产生，治疗性抗肿瘤免疫与免疫毒性不能只靠同一组 marker 区分。

## 7. 迁移学习与新队列注释

scAtlasVAE 可把新数据映射到参考潜在空间并转移细胞亚型标签。作者在多个 query 数据集上展示：

- 自动注释；
- 参考与查询队列的状态组成比较；
- 在新队列中复现 GZMK⁺/ITGAE⁺ Tex 的克隆共享模式。

这使图谱从一次性描述资源变为可复用坐标。但部署时应加入未知状态拒绝、标签概率和供者留出验证；否则治疗诱导的新状态可能被强制归入最近的已知类别。

## 8. 推荐图版

- **Fig. 1**：数据规模、研究来源、转录组/TCR 信息与 scAtlasVAE 架构。适合章节开场。
- **Fig. 2**：18 个亚型的统一 CD8⁺ T 细胞图谱。适合讲参考坐标。
- **Fig. 3**：三类 Tex 的转录差异、克隆扩增与共享。最适合本章节的生物学主图。
- **Fig. 4**：普通炎症、irAE 与肿瘤中 CD8⁺ 状态的比较。适合讲跨疾病可迁移性与情境依赖。
- **Fig. 5**：迁移学习与新队列自动注释。适合方法/计算页。

如果只能选一张，选 Fig. 3；如果强调“大图谱如何建立”，选 Fig. 1 + Fig. 2。

## 9. 创新价值

1. 将跨疾病 CD8⁺ T 细胞整合推进到百万细胞尺度。
2. 联合表达与 TCR，把状态图谱扩展为克隆—状态关系图谱。
3. 将癌症、炎症和 irAE 放进同一参考框架。
4. 提供可下载图谱、模型和自动注释工具，具有较强复用性。
5. 将 Tex 从单一终末类别拆解为具有不同应激、驻留和克隆关系的亚型。

## 10. 局限性

1. 数据来自既有研究，疾病、平台、组织处理和临床注释不均衡。
2. 大规模整合可能删除疾病特异的真实信号，也可能保留未控制的技术偏差。
3. 18 类亚型仍是人为可解释离散化，真实状态可能连续且混合。
4. TCR 克隆共享不能确定方向、抗原或功能亲和力。
5. 图谱以 CD8⁺ T 细胞为主，不能替代 CD4、Treg、NK、髓系和空间生态分析。
6. 缺乏统一的蛋白、染色质、空间和直接功能 readout。
7. 自动映射可能对参考外状态产生过度自信标签。

## 11. 对本章节的作用

该文应承担“atlas infrastructure”角色：它证明跨研究、跨疾病的 CD8⁺ T 细胞状态可以放入统一坐标，并用 TCR 提供克隆联系。它不能单独回答状态在肿瘤中的位置、真实抗原、杀伤功能或未来命运。

## 12. 可直接用于综述的观点

> scAtlasVAE 将 68 项研究中的 1,151,678 个 CD8⁺ T 细胞整合为统一参考图谱，并以配对 TCR 揭示 GZMK⁺、ITGAE⁺ 和 XBP1⁺ Tex 具有不同克隆扩增和状态共享模式，说明耗竭不是可由单一标签概括的终末状态（Nature Methods 2025, Xue）。

## 13. 避免误读

- 不要把 18 个亚型视为自然界唯一正确的分类。
- 不要用 UMAP 邻近或 TCR 共享直接确定状态转换方向。
- 不要把克隆扩增等同于肿瘤特异性。
- 不要把自动注释当成新状态发现。
