# 056. Defining T cell states associated with response to checkpoint immunotherapy in melanoma

## 基本信息
- 年份：2018
- 期刊：Cell
- DOI：https://doi.org/10.1016/j.cell.2018.10.038
- PMID：30388456
- 主题：melanoma；checkpoint response；CD8 state balance；TCF7

## 为什么重要
这篇是 checkpoint immunotherapy response-associated T-cell state discovery 的基础文献。它的价值不在于提出新软件包，而在于把 melanoma TIL scRNA-seq 和临床 response 直接挂钩：哪些 CD8 T-cell states 在 responders 中富集，哪些在 non-responders/progressors 中富集，以及能否把高维单细胞状态压缩为可解释 marker，例如 `TCF7`。对我们的方法报告来说，它是“从 cell-level clusters 到 donor/patient-level immune response readout”的早期代表。

## 数据与研究设计
- 物种/组织：Homo sapiens；melanoma tumour immune infiltrates under checkpoint inhibitors
- 样本：48 tumour samples，来自接受 checkpoint blockade therapy 的 melanoma patients；包含治疗前/治疗后和 responder/non-responder context
- 单细胞规模：16,291 CD45+ immune cells
- 技术：Smart-seq2 scRNA-seq；raw human reads 通过 dbGaP；文章结合 clonality、epigenetic/regulatory interpretation 和 validation evidence 解释 states
- 研究目标：发现与 checkpoint immunotherapy response 相关的 T-cell states，并将高维 state 转换为临床可验证的 marker/readout

## 算法贡献
- 以 therapy outcome 为主轴发现 CD8 T-cell states，而不是只做无监督 atlas。
- 将 CD8 state abundance/balance 提升为 patient-level feature，用于解释 response/progression。
- 把 high-dimensional CD8 responder-associated state 压缩为 `TCF7`-linked interpretable readout。
- 提供 response-state transfer、marker distillation 与 patient-level prediction 的早期问题定义。
- 通过外部 cohort/组织验证思路说明单细胞状态必须能迁移到可部署 readout。

## 文章中的算法/分析流程
### 1. Outcome-conditioned single-cell state discovery
- 输入是 tumour CD45+ immune-cell Smart-seq2 expression profiles，以及患者/样本的 checkpoint therapy response metadata。
- scRNA 聚类和 marker analysis 识别 immune cell states，随后聚焦 CD8 T cells。
- 关键不是单纯命名 cluster，而是比较 CD8 states 在 responder 与 non-responder/progressor lesions 中的分布。

### 2. CD8 state balance
- 文章识别 responder-associated 与 non-responder-associated CD8 programs。
- 这些 programs 被解释为患者层 immune state balance：某些 memory/progenitor-like CD8 features 与 response 相关，dysfunction/exhaustion-like features 与 resistance/progression 相关。
- 算法意义：response 预测不应只用单个 marker 或 bulk average，而应建模 state abundance、state intensity 和 patient-level aggregation。

### 3. Marker distillation：从高维 state 到 `TCF7`
- `TCF7` 被用作 favourable CD8 state 的可解释 marker/readout。
- 这对应一个重要算法问题：如何把 single-cell latent state 压缩成可在临床样本、固定组织、flow/IHC 或外部 cohort 中验证的少量特征。
- 后续算法可以把这一过程形式化为 supervised feature distillation with calibration。

### 4. 多证据解释
- 文章不只停留在 transcriptome clusters，还结合 clonality/regulatory/epigenetic evidence 与功能通路解释。
- 对算法开发的启发是：response-associated state discovery 需要整合 transcriptome、TCR clonality、regulatory landscape 和 treatment metadata，而不是孤立看 scRNA embedding。

## 数据可用性
- 文章 DOI：https://doi.org/10.1016/j.cell.2018.10.038
- GEO processed data：`GSE120575`，链接 <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE120575>
- raw dbGaP：`phs001680.v1.p1`
- BioProject：`PRJNA493623`
- 数据性质：human melanoma tumour CD45+ immune cells；48 tumour samples；16,291 cells；Smart-seq2 scRNA-seq；checkpoint therapy response metadata
- GEO 文件：TPM expression matrix、cell/patient metadata 等 processed files；GEO 记录注明 raw sequencing files 因 human data 提交 dbGaP
- 论文网页：<https://www.cell.com/cell/fulltext/S0092-8674(18)31494-1>
- DOI 备注：`https://doi.org/10.1016/j.cell.2018.12.034` 是后续 correction，不应替代主文 DOI。

## 代码可用性
- 作者公开 repository：本轮未定位到与本文配套的公开 analysis repository。
- 可复现输入：
  - GEO TPM matrix
  - cell metadata/patient IDs
  - pre/post and responder/non-responder labels
  - dbGaP controlled raw reads
  - 文章补充表中的 marker/state information
- 预期输出：
  - immune/T-cell clusters
  - CD8 responder-associated and non-responder-associated state scores
  - patient-level state abundance/balance
  - `TCF7` marker/readout comparisons
  - validation-ready marker lists
- 模型结构与意义：
  - 本文是 clinically framed analysis workflow，不是 public model package。
  - 可概括为 `tumour scRNA states + response labels -> response-associated CD8 states -> interpretable marker/readout`。

## 新算法空间
1. calibrated donor-level response model
2. state-to-marker distillation with spatial validation
3. clonality/regulatory evidence joint response representation
4. cross-cohort transfer learning for response-associated CD8 states
5. uncertainty-aware aggregation from cell-level clusters to patient-level prediction

## 对新算法贡献程度
- 直接算法创新：**中等偏低**。没有新通用算法包。
- 任务定义贡献：**高**。把 checkpoint response 与 CD8 state balance 建立明确联系。
- 数据贡献：**高**。processed GEO 与 dbGaP raw accessions 清楚，适合做 response-state benchmark。
- 新算法空间：**高**。特别适合 donor-level response prediction、marker distillation 和 cross-cohort calibration。

## 可作为我们 method 报告里的位置
建议放在“clinical response-aware T-cell state modeling”。它能说明现有工作已经从 atlas 走向 response readout，但仍缺少可泛化、校准良好、能整合 clonality/regulatory/spatial evidence 的患者层模型。

## 一句话结论
`056` 是 melanoma checkpoint response state discovery 的基础条目，代表现有算法从 cell clusters 向 clinical T-cell readout 迈出的关键一步。
