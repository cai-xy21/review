# 《Single-cell antigen-specific landscape of CAR T infusion product identifies determinants of CD19-positive relapse in patients with ALL》精读

## 论文信息

- 作者：Zhiliang Bai、Steven Woodhouse、Ziran Zhao 等
- 期刊：*Science Advances*
- 年份：2022；8(23): eabj2820
- DOI：10.1126/sciadv.abj2820
- 原文：[Science Advances](https://doi.org/10.1126/sciadv.abj2820)
- PubMed：[PMID 35675405](https://pubmed.ncbi.nlm.nih.gov/35675405/)
- 全文：[PMCID PMC9177075](https://pmc.ncbi.nlm.nih.gov/articles/PMC9177075/)
- 数据：[GEO GSE197215](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE197215)；BioProject `PRJNA809371`

## 一句话结论

对 12 名儿童 ALL 患者输注产品的 101,326 个细胞在四种刺激条件下做 scRNA/CITE-seq 表明，只有 CAR-specific CD19 stimulation 揭示的功能状态——尤其较弱 TH2 cytokine module、较强 CD8 exhaustion/terminal program——能区分长期缓解与 CD19 阳性复发。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 患者 | 12 名 pediatric B-ALL | 输注产品；临床分 CR、relapse、NR |
| 单细胞 | 101,326 transcriptomes + surface protein landscape | 四种功能挑战条件合计 |
| 条件 | unstimulated、CD19-3T3、mesothelin-3T3、anti-CD3/CD28 | CD19 为 CAR-specific；CD3/CD28 为 TCR/general activation |
| GEO records | 120 | 12 人 × 多条件 × GEX/ADT/重复；不是 120 名患者 |
| processed files | 4 个 integrated RDS.gz，总计约 3.11 GB | 分条件下载，CD19 文件 961.2 MB |
| 主要 baseline states | HMGB2+ cycling、LTB+ resting/CCL5 | 刺激后出现 CSF2+ active state |
| cytokine modules | type 1、type 2、β-chemokine、LTB、terminal CD8 等 | 模块活性比单一 cytokine 更稳健 |
| 扩展验证 | 另 49 例功能/流式队列 | 不等于 49 例都有 scRNA/CITE-seq |

## 1. 研究要解决的问题

静态 infusion-product phenotype 常不能解释为何患者发生 CD19 仍阳性的复发。能否对成品 CAR-T 做抗原特异“压力测试”，把真实 CAR 信号后的功能能力和临床持久缓解连接起来？

## 2. 方法框架

- 12 个 CTL019 infusion products 分为四种 ex vivo conditions；
- human CD19-expressing 3T3 APC 触发 CAR；mesothelin-3T3 控制非特异共培养；
- anti-CD3/CD28 beads 触发内源 TCR；另有 unstimulated baseline；
- scRNA-seq + CITE-seq 测表达和 surface ADT；
- 用 CAR transgene expression 推断 CAR+ cells，并构建 cytokine coexpression modules；
- 将条件特异状态与长期 CR、CD19+ relapse、NR 比较，并在更大队列做功能验证。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这是一套**带扰动的单细胞功能图谱**。每名患者的输注产品被拆分到四种刺激条件，因此同一产品可以比较“静态状态、一般 TCR 激活能力、非靶向共培养反应和 CAR 抗原特异反应”。它比无刺激 atlas 更接近产品 potency assay。

### 3.2 细胞与实验设计规模

| 层级 | 规模/组成 | 应如何理解 |
|---|---:|---|
| sc cohort | 12 patients | 临床独立重复只有 12 |
| cells | 101,326 | 四条件、所有患者合计 |
| functional conditions | 4 | 每个患者在条件间配对 |
| data modalities | GEX + ADT | RNA 与 surface protein 同细胞配对 |
| GEO records | 120 | GEX/ADT、条件和重复拆分后的仓库 records |
| validation cohort | 49 patients | 用功能/流式 readout 验证；不属于 101,326-cell atlas |

CAR transcript 的检测灵敏度估计约 47%；作者据此推断 infusion products 中约 30% T cells 为 CAR+。因此“检测到 CAR RNA 的细胞比例”不是直接的转导率真值。

### 3.3 状态图谱组成

未刺激时主要有 HMGB2+ cycling CAR-T 与 LTB+ / CCL5+ 非增殖状态。CAR 或 TCR 刺激后 CSF2+ active state 大幅增加，并伴 IFNG、IL2、IL13、CCL3/4、XCL1/2 等 cytokines。模块层面包括：

1. type 1-like modules（IFNG/IL2 等）；
2. type 2 module（IL4/IL5/IL9/IL13/IL31、GATA3）；
3. β-chemokine module（CCL3/CCL4 等）；
4. LTB/CCL5/IL16/IL32 module；
5. terminal CD8/ZEB2-associated module。

长期反应差异主要在 CD19-specific condition 被揭示：长期 CR 产品能诱导更充分 type 2 功能；CD19+ relapse 与 TH2 deficit、CD8 terminal/exhaustion 倾向相关。普通 TCR 刺激或未刺激状态的区分度较弱。

### 3.4 GEO 文件组成

`GSE197215` 页面列 120 records，提供四个按条件整合的 Seurat RDS：

| 文件 | 体积 | 条件/用途 |
|---|---:|---|
| `Integrated_object_of_CD19_3T3_condition.rds.gz` | 961.2 MB | CAR-specific 主分析 |
| `Integrated_object_of_CD3_CD28_beads_condition.rds.gz` | 680.3 MB | TCR/general activation 对照 |
| `Integrated_object_of_mesothelin_3T3_condition.rds.gz` | 492.7 MB | non-target APC 对照 |
| `Integrated_object_of_unstimulated_condition.rds.gz` | 979.2 MB | baseline state |

总下载量约 3.11 GB（gzip）。原始 reads 可从页面的 SRA Run Selector / BioProject PRJNA809371 获取。

### 3.5 如何获取

#### 路线 A：只分析临床相关 CAR-specific response

优先下载 961.2 MB 的 CD19-3T3 RDS 和 Supplementary clinical table；用 `readRDS()` 后检查 Seurat assays（RNA/ADT）、metadata、patient、condition 与 outcome。

#### 路线 B：完整比较四种刺激

下载四个 RDS；以 patient 为配对单位计算 module activation delta，例如 `CD19-stimulated − unstimulated`，避免比较不同患者的绝对比例时受基础状态混杂。

#### 路线 C：重跑原始数据

从 SRA 获取 FASTQ，分别处理 GEX 与 ADT；参考基因组需包含 CAR construct 序列。3T3 是小鼠 APC，应检查人/鼠混合比对和 ambient RNA 处理。

### 3.6 下载后先做什么

1. 检查 Seurat object 是否保留 raw counts、normalized data 和 ADT assay；
2. 按 patient 配对四条件，不能把 120 GSM 当成独立患者；
3. 报告 CAR RNA detection dropout；
4. 区分 CD19-3T3 和 mesothelin-3T3，验证 human/mouse read separation；
5. 结局模型使用 patient-level cross-validation。

## 4. 主要发现

刺激后 active CAR-T 的总比例本身不足以预测长期结局；真正有信息的是功能模块组成。长期 CR 产品在 CD19-specific challenge 后具有更完整 TH2 功能，复发产品呈 TH2 deficit 与 CD8 terminal/exhaustion 特征。

## 5. 与实时优化系统的关系

该研究给出“perturb-and-measure”范式：对制造产品施加标准化抗原挑战，实时读出多功能状态，而不是只做静态 marker 放行。若将 scRNA/CITE 降维成 targeted cytokine/protein panel，可成为制造闭环的 potency sensor。

## 6. 推荐图版

- Fig. 1：四条件设计与 baseline/active states；
- Fig. 2：cytokine modules；
- outcome comparison 图：TH2 deficit 与 CD19+ relapse；
- 验证队列图：从单细胞发现转为可测 assay。

## 7. 创新价值

1. 用抗原特异扰动而非静态 atlas 评估 CAR-T 产品。
2. 同一细胞联合 transcriptome 与 surface protein。
3. 将功能模块与 CD19+ relapse 直接关联。

## 8. 局限性

1. 单细胞发现队列仅 12 人。
2. 3T3 人工 APC 不完整重建免疫突触/TME。
3. CAR RNA detection sensitivity 约 47%。
4. 模块与结局仍是关联，制造过程和患者因素可能混杂。
5. type 2 功能的普适性需跨 CAR design/疾病验证。

## 9. 对本章节的作用

这是“techniques to perturb/manipulate states”和“build real-time optimization systems”的关键案例：标准化抗原刺激把潜在产品能力显现出来，并提供可压缩为快速 potency assay 的功能模块。

## 10. 可直接用于综述的观点

> 静态 infusion-product atlas 可能隐藏真正的功能缺陷；CD19-specific challenge 后的单细胞 cytokine modules，尤其 TH2 competence 与 CD8 exhaustion balance，才区分长期缓解和 CD19 阳性复发，为细胞制造的 perturb-and-measure 放行策略提供依据（Science Advances 2022, Bai）。

## 11. 避免误读

- 不要把 120 GEO records 写成 120 名患者。
- 不要把 49 人验证队列写成都有 scRNA/CITE-seq。
- 不要把检测到 CAR RNA 的 30% 直接当作产品真实 CAR+ 比例。
- 不要把人工 3T3 stimulation 等同于患者体内肿瘤环境。

