# 075. Comprehensive mapping of immune perturbations associated with severe COVID-19

## 基本信息
- 年份：2020
- 期刊：Science Immunology
- DOI：https://doi.org/10.1126/sciimmunol.abd7114
- PMID：32669287
- 主题：COVID-19 immunophenotyping；flow cytometry；severity module；antibody/IgH repertoire

## 为什么重要
这篇文章不是 scRNA-seq 论文，也不是算法工具论文，但它是 severe COVID-19 peripheral immune perturbation 的高维表型锚点。它用 fresh whole blood/PBMC flow cytometry、cytokines、serology 和临床指标系统比较 healthy、recovered、moderate 和 severe COVID-19 个体。对方法综述来说，它提醒我们：T-cell/RNA 算法最终需要能映射到 flow/CyTOF 等临床更可部署的表型空间。

## 数据与研究设计
- 样本对象：35 名 active hospitalized COVID-19 patients，其中 7 名 moderate、28 名 severe；7 名 recovered donors；12 名 healthy donors
- 物种/器官：Homo sapiens；peripheral blood / PBMC / plasma
- 模态：multiparametric flow cytometry、CBC/clinical labs、plasma cytokines/chemokines、SARS-CoV-2 antibody/serology、immunoglobulin heavy-chain sequencing
- 研究目标：识别 severe COVID-19 相关的 innate/adaptive immune perturbations，并评估 NLR 等 severity/organ failure biomarkers

## 核心亮点
1. **临床表型锚点**：以 flow cytometry 和 clinical labs 描述 severe disease，便于与 scRNA-derived states 对照。
2. **多免疫分支扰动**：同时覆盖 neutrophils、monocytes、NK、B/plasmablast、T cells 和 humoral response。
3. **NLR 与 organ failure 关联**：neutrophil-to-lymphocyte ratio 与 APACHE III 等临床严重程度指标相关。
4. **开放数据边界更清楚**：原文说明 paper/supplement 内含主要数据，IgH sequencing 到 SRA，flow files 通过 HPAP/PMACS 平台公开入口获取。

## 核心贡献
- 描述 severe COVID-19 中广泛 leukocyte perturbations，包括 neutrophil/eosinophil expansion、monocyte/NK receptor modulation、T-cell activation、plasmablast expansion。
- 将 flow-derived immune-cell frequencies and marker phenotypes 与 cytokine/serology/clinical severity 关联。
- 提出 NLR/NTR 等临床-免疫指标可反映 severe disease 和 organ failure 风险。
- 提供 high-dimensional immune phenotype modules，可作为 scRNA/CITE-seq severe-state 的外部验证层。

## 与 T 细胞-人群免疫力的关系
该文证明 severe disease 中 T-cell activation/exhaustion 需要放在 neutrophil、monocyte、B-cell/plasmablast 和 humoral response 的全局扰动中解释。若我们开发 T-cell population immunity 算法，不能只优化 RNA embedding，还要能与 flow marker panels、CBC 和 cytokine features 对齐。

## 文章中的算法/分析流程
### 1. High-dimensional flow immunophenotyping
作者使用多参数 flow cytometry 和 gating/t-SNE visualization 描绘 immune subset frequencies 和 phenotype shifts。它不是无监督新算法，但提供 marker-panel-level state definition。

### 2. Severity association
比较 healthy/recovered/moderate/severe groups 的 cell frequencies、activation markers、cytokines、antibody features 和 clinical labs，识别 severe-specific perturbation pattern。

### 3. Clinical immune biomarkers
将 NLR/NTR 等 CBC-flow features 与 APACHE III 等临床指标关联，形成 donor-level severity readout。

## 对算法工作的启发
1. **Flow/CITE/scRNA cross-platform alignment**：把 flow marker state 映射到 transcriptomic clusters 和 CITE-seq ADT space。
2. **Absolute-count aware composition model**：结合 CBC absolute counts 与 single-cell proportions，避免 compositional artifact。
3. **Panel-aware state inference**：不同 flow panels marker 缺失时仍能迁移 immune-state classifier。
4. **Clinical deployable severity score**：从 scRNA module 压缩到 flow/CBC/cytokine 可部署特征。

## 数据可用性
- DOI：https://doi.org/10.1126/sciimmunol.abd7114
- IgH sequencing BioProject/SRA：PRJNA630455
- Flow cytometry files：原文说明 compensated flow cytometry files 可通过 <https://hpap.pmacs.upenn.edu> 获取，并联系 MRB 获取下载说明
- Data Availability 原文边界：主要数据在 paper 或 Supplementary Materials；IgH data 按 AIRR-compliant manner 上传 SRA；flow files 通过 HPAP/PMACS
- 数据性质：human peripheral blood immune phenotyping；35 active hospitalized patients、7 recovered、12 healthy donors；moderate/severe severity labels
- 代码：未定位独立作者 GitHub；分析主要依赖 flow gating、frequency/marker statistics、t-SNE visualization 和 correlation
- 代码输入/输出（按论文流程抽象）：
  - 输入：FCS files、gating-derived frequency table、marker intensities、cytokine/serology/clinical tables
  - 输出：immune subset frequency changes、activation/exhaustion phenotype features、NLR/NTR severity associations、humoral response summaries
- 模型结构与意义：descriptive high-dimensional immunophenotyping workflow；不是 scRNA benchmark，但可作为 clinical phenotype anchor

## 可信度评估
- 期刊层面：Science Immunology，可信度高
- 可复现性：IgH SRA accession 和 flow file portal 明确，但 flow 下载需要平台/联系人流程；无独立代码仓库
- 局限：非 scRNA/CITE-seq；marker panel 限制状态发现；治疗、采样时间和 comorbidity 需要额外建模
- 综合判断：**直接算法创新低，临床 phenotype 与 cross-platform validation 价值中高**

## 一句话结论
这篇文章应作为 severe COVID-19 flow/CBC/serology phenotype anchor：它不提供新的单细胞算法，但对把 T-cell/RNA 模型转化到临床可测免疫表型非常有用。
