# Algorithm Report 016

## Paper
Comprehensive Integration of Single-Cell Data

## 基本信息
- 年份/期刊：2019, Cell
- DOI：https://doi.org/10.1016/j.cell.2019.05.031
- PMID/PMCID：31178118 / PMC6687398
- 代码：Seurat <https://github.com/satijalab/seurat>
- 教程：Seurat integration/reference mapping workflow <https://satijalab.org/seurat/>

## 算法视角定位
Seurat v3 anchor-based integration 是单细胞整合分析的主干方法之一。它把 integration 从简单 batch correction 推进到 reference mapping、label transfer 和跨模态信息投射。对人群 T 细胞研究而言，核心价值是让不同 donor、平台、组织和研究中的 T-cell states 可以被映射到共享参考框架中。

## 数据与任务
- 物种/样本：论文复用多类公开数据，包含 human/mouse、PBMC/bone marrow/brain 等示例
- 模态：scRNA-seq、scATAC-seq、CITE-seq/protein、spatial/in situ data
- 数据性质：方法论文复用公开 benchmark，不是单一新 cohort accession
- 任务定义：识别不同数据集之间的 mutual correspondence anchors，并基于 anchors 做 integrated embedding、reference mapping、label transfer 或跨模态 imputation

## 核心算法结构
### 1. Anchor 定义
- anchor 是两个数据集中处于相似生物状态的一对细胞。
- 算法先在降维空间中寻找 mutual nearest neighbors 或相关相似结构。
- anchor 不是简单最近邻，而是被打分和过滤后的跨数据集对应关系。

### 2. Anchor scoring 与 weighting
- 每个 anchor 被赋予质量分数，用于判断它是否代表真实共享生物状态。
- integration 时，每个 query cell 会根据附近 anchors 的权重进行校正或信息转移。
- 权重受 cell-anchor 距离和 anchor score 影响。

### 3. Integrated assay / shared embedding
- 对多个 scRNA-seq 数据集，anchors 用于估计 batch/dataset-specific expression shift，并生成 integrated representation。
- 对 reference mapping，anchors 用于把 query 投射到 reference 的 cell-type/state space。
- 对跨模态数据，anchors 可以把 protein、chromatin 或 spatial 信息投射到另一个模态。

### 4. Label transfer
- reference 中已有 labels 或 continuous features 时，可以通过 anchors 转移到 query。
- 输出通常包括 predicted label、prediction score 和 projected embedding。
- 这对免疫 atlas 复用非常关键：新 PBMC/T-cell 数据可以快速映射到既有 reference。

## 输入、输出与模型意义
- 输入：多个预处理 single-cell objects、shared features 或跨模态特征、PCA/CCA/RPCA embedding、reference labels
- 输出：anchors、integrated assay/embedding、transferred labels、prediction scores、projected modality values
- 模型结构：anchor-based nearest-neighbor correspondence + weighted correction/transfer；不是深度生成模型
- 方法意义：建立了 reference-centric integration 范式，并把 atlas reuse 变成常规工作流

## 对新算法开发的贡献程度
- 直接算法创新：非常高
- 实用生态贡献：极高
- 对 T 细胞人群研究贡献：极高
- 综合评估：**P0 级整合方法**

## 局限与风险
1. Anchor 依赖共享细胞状态；若 query 中存在 reference 没有的 rare T-cell state，可能被强行映射到最近已知状态。
2. Integrated assay 不应直接替代 raw counts 做所有 DE，需要区分 integration-for-clustering 与 expression-testing。
3. Donor-level variance 常被当作 batch/covariate 处理，缺少显式层级统计解释。

## 新算法空间
1. Probabilistic anchor model：给 anchor 本身建 posterior，而不是只给 heuristic score。
2. Donor-aware reference mapping：同时输出 cell label 与 donor-level immune-state deviation。
3. Novel-state detection：识别 reference 不覆盖的新 T-cell state，避免过度投射。
4. Multimodal missingness：在 RNA/ADT/TCR/ATAC 缺失不均衡的人群队列中做稳健映射。

## 可纳入 method report 的一句话
Seurat v3 的贡献是把跨数据集对应关系显式表示为 anchors，并由此支撑 integration、label transfer 和 reference mapping；它是当前人群免疫 atlas 构建中最重要的 reference-centric 方法之一。
