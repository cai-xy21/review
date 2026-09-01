# Algorithm Report 019

## Paper
DoubletFinder: Doublet detection in single-cell RNA sequencing data using artificial nearest neighbors

## 基本信息
- 年份/期刊：2019, Cell Systems
- DOI：https://doi.org/10.1016/j.cels.2019.03.003
- PMID/PMCID：30954475 / PMC6853612
- 代码：<https://github.com/chris-mcginnis-ucsf/DoubletFinder>
- Code Ocean：<https://doi.org/10.24433/CO.4902498.v1>

## 算法视角定位
DoubletFinder 是表达矩阵驱动的 doublet detection 基线方法。它不依赖 genotype 或 hashing，而是人工合成 doublets，并判断真实细胞在表达邻域中是否富集 artificial nearest neighbors。对 T-cell atlas 来说，它尤其重要，因为 T/NK、T/B、T/myeloid doublets 会制造假 marker 和错误过渡状态。

## 数据与任务
- 物种/样本：复用公开 human PBMC 与 mouse tissue scRNA-seq 数据
- 关键公开数据：
  - Cell Hashing ground truth：GEO `GSE108313`
  - demuxlet PBMC：GEO `GSE96583`
  - mouse kidney：GEO `GSE107585`
  - mouse pancreas：GEO `GSE101099`
- 模态：UMI-based scRNA-seq expression matrix
- 任务定义：在没有外部样本标签的情况下，为每个 cell barcode 输出 doublet score 与 singlet/doublet label

## 核心算法结构
### 1. Artificial doublet generation
- 从真实细胞中随机抽取两两配对。
- 将两细胞表达 profile 合成 artificial doublet。
- 人工 doublet 与真实细胞混合成 merged matrix。

### 2. 共同预处理与降维
- merged matrix 经过与真实数据相同的 Seurat-style normalization、variable gene selection、scaling、PCA。
- 这样 artificial doublets 和 real cells 被投影到同一个表达空间。

### 3. pANN score
- 在 PCA 空间中为每个真实细胞寻找 k nearest neighbors。
- 计算邻域中 artificial doublets 的比例，得到 `pANN` (proportion of artificial nearest neighbors)。
- pANN 越高，说明该真实细胞越像人工 doublet。

### 4. 参数选择与阈值
- `pN` 控制 artificial doublet 的比例，论文指出性能对 pN 相对不敏感。
- `pK` 控制邻域大小，需要 parameter sweep；BCmvn 用于辅助选择。
- `nExp` 是预期 doublet 数量，通常来自平台 loading rate，并可按 homotypic proportion 调整。

## 输入、输出与模型意义
- 输入：Seurat object/UMI matrix、PC 选择、`pN`、`pK`、expected doublet count、sample metadata
- 输出：每个细胞的 `pANN`、doublet/singlet classification、parameter sweep diagnostics
- 模型结构：simulation-based nearest-neighbor classifier；不是监督模型，也不需要 ground-truth labels
- 方法意义：将 doublet detection 转化为“真实细胞是否靠近模拟 doublet manifold”的问题

## 对新算法开发的贡献程度
- 直接算法创新：高
- QC 实用价值：高
- 对 T 细胞人群研究贡献：高
- 综合评估：**P1 级 QC 基础算法**

## 局限与风险
1. 对 heterotypic doublets 强，对 homotypic doublets 弱。
2. 不能直接区分真实 intermediate activation state 与 doublet-like expression。
3. 不建议直接对多个独立样本聚合后的对象运行，否则不同样本结构会影响 artificial doublet 模拟。
4. TCR/BCR 信息没有进入模型，可能漏掉 B/T 或 alpha/beta receptor 异常组合的 doublet evidence。

## 新算法空间
1. Evidence fusion：联合 genotype、hashing、expression、TCR/BCR、ADT marker 证据推断 doublet posterior。
2. Homotypic immune doublet model：针对 T-T、B-B、myeloid-myeloid doublets 建立更敏感的局部模型。
3. State-vs-artifact uncertainty：对像 activated/intermediate T cells 的 barcode 输出 calibrated uncertainty。
4. Downstream propagation：把 doublet probability 传给 clustering、DE、trajectory，而不是只做硬过滤。

## 可纳入 method report 的一句话
DoubletFinder 的贡献是用人工双细胞邻域构造表达型 doublet score，为没有 genotype/hash 标签的数据提供 QC baseline；但在 T-cell continuum 和 homotypic doublets 场景中仍需要联合多证据模型。
