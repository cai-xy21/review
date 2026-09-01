# Algorithm Report 020

## Paper
SoupX removes ambient RNA contamination from droplet-based single-cell RNA sequencing data

## 基本信息
- 年份/期刊：2020, GigaScience
- DOI：https://doi.org/10.1093/gigascience/giaa151
- PMID/PMCID：33367645 / PMC7763177
- 代码包：<https://github.com/constantAmateur/SoupX>
- 复现脚本：<https://github.com/constantAmateur/ambientRNA_paper>
- Docker：<https://hub.docker.com/r/constantamateur/soupxpaper>

## 算法视角定位
SoupX 是 droplet scRNA-seq ambient RNA correction 的代表工具。它处理的不是细胞内真实表达，而是 cell-free RNA 被 droplet 捕获后造成的背景污染。对 PBMC、炎症组织和 T-cell atlas 来说，ambient immunoglobulin、hemoglobin、cytokine 或 tissue lysis transcripts 会把 T-cell marker 解释推偏，因此 SoupX 属于上游可信度控制工具。

## 数据与任务
- 物种/样本：论文复用 human/mouse droplet scRNA-seq datasets
- 代表数据：fetal liver data 可从 ArrayExpress `E-MTAB-7407` 获取；另有多种 droplet technology examples
- 器官/组织：fetal liver 等，重点是 ambient contamination 明显的 droplet 数据场景
- 模态：droplet scRNA-seq raw and filtered count matrices
- 任务定义：估计每个实验/细胞群的 ambient soup profile 与 contamination fraction，并输出污染校正后的 count matrix

## 核心算法结构
### 1. Soup profile estimation
- 使用 raw/unfiltered matrix 中 low-count/empty droplets 估计环境 RNA 的 gene expression profile。
- 这个 profile 代表细胞裂解或样本处理过程中释放到悬液中的 cell-free mRNA。

### 2. Contamination fraction estimation
- 对每个细胞或 cluster 估计 contamination fraction。
- 关键思想：某些 marker genes 在目标细胞群中理论上不应真实表达，如果观察到这些基因，可能来自 ambient soup。
- 自动模式通常依赖 clustering 与 marker enrichment；用户也可以提供先验 marker 信息。

### 3. Count adjustment
- 对 observed counts 中可归因于 soup 的部分进行扣除。
- 输出 adjusted counts，可被 Seurat/Scanpy 等下游工具继续使用。
- 方法不会简单按比例删除所有基因，而是依据 soup profile 与细胞表达结构做校正。

## 输入、输出与模型意义
- 输入：raw droplet matrix、filtered cell matrix、cell clusters、marker genes 或自动估计参数
- 输出：estimated soup profile、global/per-cell contamination fraction、adjusted count matrix、diagnostic plots
- 模型结构：ambient background profile + contamination fraction + count correction
- 方法意义：把 droplet background contamination 从隐含噪声变成可估计、可校正的显式变量

## 对新算法开发的贡献程度
- 直接算法创新：中等偏高
- QC 实用价值：高
- 对 T 细胞人群研究贡献：高
- 综合评估：**P1 级 ambient RNA QC 方法**

## 局限与风险
1. 空滴 soup profile 未必完全代表每个细胞附近的局部污染。
2. 高炎症样本中，真实 paracrine/activation signal 与 ambient cytokine RNA 可能难以区分。
3. sample-specific soup 差异很大，跨样本合并后校正可能引入偏差。
4. 对 ADT ambient signal、TCR/BCR library contamination 不是完整解决方案。

## 新算法空间
1. Donor/sample-aware soup model：每个 donor/sample 独立建背景，再在 cohort 层做层级共享。
2. Multimodal contamination correction：联合 RNA、ADT、V(D)J reads 判断背景污染。
3. Spatial/tissue-aware ambient model：对组织 dissociation 造成的局部环境 RNA 做空间或组织来源建模。
4. Uncertainty-aware correction：输出 correction posterior，并传递到 marker testing、cell-type annotation 和 trajectory。

## 可纳入 method report 的一句话
SoupX 的关键贡献是把 droplet ambient RNA 显式建模为可估计的 background soup；在人群 T-cell 数据中，它是防止假 marker、假活化状态和组织污染解释偏差的重要上游算法。
