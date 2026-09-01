# Algorithm Report 032

## Paper
A single-cell and single-nucleus RNA-Seq toolbox for fresh and frozen human tumors

## 题录与数据
- 年份/期刊：2020, Nature Medicine
- DOI：https://doi.org/10.1038/s41591-020-0844-1
- 数据规模：`216,490` cells/nuclei，`40` samples，`23` specimens，八类 tumor settings
- Public data：GEO `GSE140819`；HCA project `a62dae2e-cd69-4d5c-b5f8-4f7e8abdbafa`

## 算法视角定位
这篇应归为 **measurement/protocol-aware computational workflow**。它不解决 TCR specificity 或 immune response prediction，而是解决 tumor tissue scRNA/snRNA 数据进入算法前最容易被忽视的层：fresh/frozen protocol choice、QC、公平比较和 immune composition recovery。

## 算法链
### 输入
- 10x scRNA/snRNA FASTQ 或 count-level data
- sample/protocol metadata：fresh/frozen、cell/nucleus、tumor/sample type

### 处理
1. `Cumulus` 上游处理，从 FASTQ 到 count matrix/clustering object
2. 用 DropletUtils downsample reads/UMIs，使协议比较不被测序深度主导
3. downstream QC：empty drops、doublets、cell/nucleus-specific thresholds
4. AnnData 到 Seurat 的后续 annotation 与 cell-type-specific QC
5. tumor characterization：含 CNA/inferCNV 相关输出
6. protocol-level metrics：quality、recovery、composition agreement/disagreement

### 输出
- 每 sample 的 processed expression object 与 clustering summaries
- scRNA/snRNA cell-type annotations
- protocol comparison metrics and decision support
- tumor toolbox summaries

## 详细算法贡献
### 1. 把 protocol comparison 变成 benchmark task
论文不只给实验配方，而是把 protocol performance 写成可计算指标集合：quality、recovery、cellular composition 与 matched fresh/frozen consistency。

### 2. 把 QC 做成 modality-aware layer
whole cells 与 nuclei 的 RNA content、ambient contamination 和 detected genes 不同。本文专门区分 QC 逻辑，避免一种 threshold 覆盖全部 clinical tissue samples。

### 3. 提供 pipeline artifacts
- `Cumulus` 是可执行上游 workflow。
- `HTAPP-Pipelines` 中 `HTAPP_methods_toolbox/` 保存论文分析脚本与 scRNA/snRNA 示例。
- 这类 artifact 对后续方法论文的意义是：能复用输入对象、QC decisions 和 protocol labels，而不只是下载 final UMAP。

## 模型结构与意义
- 结构：`workflow + evaluation metrics`，不是单一神经网络/统计模型。
- 意义：为 cell/nucleus harmonization、protocol bias estimation 与 clinical tissue atlas QC 提供 benchmark substrate。

## 对新算法贡献程度
- 任务定义：高
- 数据开放：高
- 直接新模型：低
- 对 tissue T-cell algorithm 的基础价值：高

## 可开发空间
1. **Fresh/frozen harmonization model**：显式建模 protocol covariate，不把 absent cell populations 误校正回来。
2. **Protocol shift detector**：预测某 tumor/tissue 在给定 protocol 下哪些 immune states 会系统性流失。
3. **Uncertainty-aware atlas transfer**：对 nuclei-only T-cell annotation 与 malignant/immune boundary 给出置信度。

## 数据与代码
- `Cumulus`：<https://github.com/klarman-cell-observatory/Cumulus>
- `HTAPP-Pipelines`：<https://github.com/klarman-cell-observatory/HTAPP-Pipelines>
- 输入：FASTQ/count matrices、sample metadata、Cumulus AnnData objects
- 输出：processed objects、QC/annotation artifacts、protocol comparison summaries
- 可复用性：**高**，尤其适合 protocol-bias methods；不适合作为 repertoire model benchmark

## 可纳入 method report 的一句话
For tissue immune atlases, protocol-aware QC and cell-versus-nucleus evaluation are algorithmic prerequisites: the Slyper toolbox makes those measurement biases explicit and reusable before cross-sample T-cell modeling begins.
