# 《Sequential Single-Cell Transcriptional and Protein Marker Profiling Reveals TIGIT as a Marker of CD19 CAR-T Cell Dysfunction in Patients with Non-Hodgkin Lymphoma》精读

## 论文信息

- 作者：Zachary Jackson、Christina Hong、Rachel Schauner 等
- 期刊：*Cancer Discovery*
- 年份：2022；12: 2019–2037
- DOI：[10.1158/2159-8290.CD-21-1586](https://doi.org/10.1158/2159-8290.CD-21-1586)
- PubMed：[PMID 35554512](https://pubmed.ncbi.nlm.nih.gov/35554512/)；[PMC9357057](https://pmc.ncbi.nlm.nih.gov/articles/PMC9357057/)
- 原始数据：[EGA study EGAS00001005356](https://ega-archive.org/studies/EGAS00001005356)；[dataset EGAD00001007741](https://ega-archive.org/datasets/EGAD00001007741)
- 代码与处理后数据：[GitHub](https://github.com/hwanglab/hwanglab_2021_tigitCarT)；[Code Ocean](https://codeocean.com/capsule/0626385/tree/v1)

## 一句话结论

13 名 NHL 患者的输注产品与治疗后 CAR T 单细胞序列显示，疗效较差者在体内富集分化/耗竭程序，TIGIT 是与该功能障碍状态相关并可在模型中干预的检查点。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 患者 | 13 名 NHL | 纵向小队列 |
| 采集 | infusion product、day 14、day 30 | 共测 27 个样本，24 个通过 QC |
| 高质量细胞 | 约 94,000 | 平均约 3,917/样本 |
| 模态 | 10x 3′ GEX；部分样本 12-ADT CITE-seq | 无单细胞 TCR 主分析 |
| CITE-seq | 16/27 个送测样本 | ADT 覆盖并非全队列 |
| 原始数据 | EGA controlled access | EGA 页面“17 patient samples/109 samples”不是患者总数 |
| 处理后对象 | GitHub RDS、表格、分析脚本 | 可先于 EGA 授权复现图表 |

## 1. 研究要解决的问题

输注后的抗原刺激如何重塑 CAR T 状态，哪些变化与非持久反应相关？作者序列化采样，将制造产品与第 14、30 天外周 CAR T 比较，并将 TIGIT 从观察性 marker 推进到体外/小鼠阻断验证。

## 2. 方法框架

- 分选活的 CD3⁺CAR⁺ 细胞；
- 10x 3′ v3 feature barcoding；
- Cell Ranger 3.1 计数，Harmony 整合，SingleR/marker 注释；
- 比较治疗反应、时间点和表型状态；
- 体外 TIGIT 阻断与 NSG 模型作功能验证。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

图谱主体是患者来源 CAR⁺ T 细胞的纵向 scRNA-seq：

1. **输注产品**：抗原接触前/制造结束时的基线；
2. **第 14 天**：早期体内扩增和抗原驱动重塑；
3. **第 30 天**：后续持续、分化和功能障碍；
4. **ADT 子集**：用 12 个表面蛋白校准 RNA 状态；
5. **临床标签**：响应质量和持续性；
6. **功能验证**：TIGIT blockade 不属于单细胞图谱本身，但增强机制解释。

### 3.2 多大规模、覆盖哪些样本

| 层级 | 数量 | 说明 |
|---|---:|---|
| 患者 | 13 | 非霍奇金淋巴瘤 |
| 计划时间点 | 3 | 产品、day 14、day 30；并非每人每点均成功 |
| 送测样本 | 27 | 三个 sequencing runs |
| QC 后样本 | 24 | 论文主体分析口径 |
| 高质量细胞 | 约 94,000 | 每样本平均约 3,917 |
| 带 ADT 样本 | 16/27 | 蛋白层存在结构性缺失 |

12 个 TotalSeq-B 标记为 CD28、CD57、CD69、CD62L、CD197/CCR7、CD25、CD279/PD-1、CD45RA、CD127、CD4、CD45RO、CD8A。使用 ADT 比较时必须限制在这 16 个样本，不能把缺失当作蛋白阴性。

### 3.3 EGA 与 GitHub 各有什么

#### EGA：原始受控数据

- Study：`EGAS00001005356`；dataset：`EGAD00001007741`。
- 内容：未过滤原始 FASTQ 及其受控临床/样本映射。
- EGA dataset 页面写有“17 NHL patient samples”，并列出 **109 个 samples**。这与论文的 13 名患者、27 个采样样本并不矛盾：EGA 的表述和 sample/file 记录可按文库、模态或文件拆分，不能直接当作独立患者数。
- 获取：注册 EGA、向 Data Access Committee 提交研究计划，获批后用 EGA download client 获取。

#### GitHub：处理后数据与完整分析流程

仓库 `hwanglab_2021_tigitCarT` 提供从 `m00` 到 `m13` 的分析脚本、workflow 与 sample sheets。`data/` 中可直接看到：

- `03a_cluster_size_pcts.rds`（约 3.84 MB）；
- `STab2.xlsx`（约 9.4 KB）；
- `cell_counts_per_sample.tsv`；
- `samples1.csv`；
- marker gene sets 等。

Code Ocean capsule 保存可执行环境快照，更适合复现论文分析版本；GitHub 更适合检查代码与小型处理后对象。

### 3.4 如何获取：按目的选择

- **快速复现图和统计**：clone GitHub，先读取 sample sheet、cell count 和 RDS；对照 Code Ocean 环境版本。
- **重新做比对/QC**：申请 EGA dataset，按 sample sheet 重建 FASTQ—患者—时间点映射。
- **只研究 TIGIT 状态**：优先使用作者的处理后对象，按患者做 pseudobulk 或混合模型，避免把 94,000 个细胞当作独立重复。
- **研究蛋白与 RNA 一致性**：限定在 16 个 CITE-seq 样本，显式加入 sequencing run 和患者效应。

### 3.5 下载后先做什么

1. 先以 `patient_id × timepoint` 生成唯一生物样本表；
2. 对照 27 送测、24 QC、16 CITE 三套集合；
3. 检查 3 个 sequencing runs 的批次；
4. 统计检验以患者为重复单位；
5. 对 TIGIT 表达同时查看 RNA 与 CD279/PD-1 等 ADT，避免 drop-out 驱动结论。

## 4. 主要发现

抗原接触后 CAR T 转录景观发生明显重塑；非持久/较差响应者更倾向于分化、功能障碍和抑制性受体程序。TIGIT 在这些状态中突出，阻断可在体外和小鼠模型中改善功能。

## 5. 与“状态导航”最相关的分析

该文提供一个完整的小闭环：纵向单细胞发现候选状态标志物 → 选择 TIGIT → 功能扰动验证。但它仍未证明患者中 TIGIT 是唯一或首要因果驱动，最佳表述应是“可干预的伴随节点”。

## 6. 推荐图版

- 纵向 UMAP/状态组成图：展示产品到 day 14/30 的重塑。
- 响应者分层与 TIGIT 图：连接状态和临床。
- TIGIT 阻断实验：展示从图谱到扰动验证。

## 7. 创新价值

把产品和体内连续时间点放进同一单细胞坐标，并提供可运行代码与处理后对象，复用性较好。

## 8. 局限性

13 人且时间点不完整；ADT 只覆盖部分样本；无同细胞动态成像；临床相关性与阻断实验之间仍隔着模型系统。

## 9. 对本章节的作用

适合放在“link state/function transitions with molecular drivers”，作为从状态发现到检查点干预的范例。

## 10. 可直接用于综述的观点

> 13 名 NHL 患者的纵向 CAR T 单细胞图谱显示，治疗后非持久反应伴随分化/功能障碍程序富集，并将 TIGIT 提名为可干预的状态相关节点（Cancer Discovery 2022, Jackson）。

## 11. 避免误读

- 不要把 EGA 的 109 条记录写成 109 名患者。
- 不要把 16 个 CITE 样本的蛋白结果外推为所有 27 个样本均有 ADT。
- 不要把临床关联写成患者内 TIGIT 阻断已被证实有效。
