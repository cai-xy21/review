# Algorithm Report 014

## Paper
Multiplexed droplet single-cell RNA-sequencing using natural genetic variation

## 基本信息
- 年份/期刊：2018, Nature Biotechnology
- DOI：https://doi.org/10.1038/nbt.4042
- PMID/PMCID：29227470 / PMC5784859
- 公开数据：GEO `GSE96583`
- 代码：demuxlet 原仓库 <https://github.com/statgen/demuxlet>；后续工具族 `popscle` <https://github.com/statgen/popscle>

## 算法视角定位
demuxlet 是 cohort-scale single-cell immunology 的底层算法之一。它让多个 donor 的细胞可以混池上机，再用自然遗传变异把每个 barcode 归回 donor，同时识别跨 donor doublets。对 T 细胞-人群免疫力研究来说，这解决了两个现实瓶颈：降低每个样本单独上机造成的 batch effect，并让大规模 donor 比较更经济。

## 数据与任务
- 物种/样本：`Homo sapiens` PBMC
- 器官/组织：外周血/PBMC
- 代表设计：8 个 lupus donors 混池后做 IFN-beta stimulation response；另扩展到 23 个 pooled samples 的 eQTL/人群应用
- 模态：droplet scRNA-seq + donor genotype/SNP reference
- 公开入口：GEO `GSE96583`
- 任务定义：对每个 droplet barcode 推断 `donor identity`、`singlet/doublet/ambiguous`，并给出双细胞来源 donor pair 的概率解释

## 核心算法结构
### 1. 观测模型
- 输入是每个 barcode 在 SNP 位点上的 allele-specific reads，以及每个候选 donor 的 genotype。
- 对于 singlet，barcode 的 allele reads 应与某一个 donor 的 genotype likelihood 匹配。
- 对于 doublet，barcode 的 reads 更像两个 donor 的混合基因型。
- 算法比较 singlet model、doublet model 与 ambiguous/unassigned model 的 likelihood。

### 2. Donor assignment
- 对每个 barcode 计算来自每个 donor 的 posterior/likelihood。
- 选择最大支持的 donor 作为 singlet assignment。
- 如果两个 donor 的混合模型显著更好，则标记为 doublet 并给出 donor pair。

### 3. Doublet detection
- demuxlet 对跨 donor doublets 很强，因为两名 donor 的 SNP 组合会产生明显混合信号。
- 对同 donor homotypic doublets 无法直接识别，这是 genotype-based demultiplexing 的固有限制。
- 这也是后续需要把 genotype、hashing、expression、TCR/BCR evidence 联合建模的原因。

## 输入、输出与模型意义
- 输入：aligned scRNA-seq BAM/BCF/VCF、barcode list、donor genotype reference、SNP annotation
- 输出：每个 barcode 的 donor assignment、singlet/doublet/ambiguous label、best doublet pair、likelihood/posterior score
- 模型结构：基于 SNP allele read likelihood 的概率 demultiplexing model
- 方法意义：把 donor identity inference 从实验标签问题转成统计推断问题，使 pooled cohort scRNA-seq 成为常规设计

## 对新算法开发的贡献程度
- 直接算法创新：非常高
- 数据/实验设计贡献：非常高
- 对 T 细胞人群队列贡献：非常高
- 综合评估：**P0 级 cohort-scale 基础算法**

## 新算法空间
1. Genotype-free 或 weak-genotype demultiplexing：适合没有全基因型数据的人群队列。
2. Joint demux-doublet-batch model：把 donor assignment、doublet probability 与 batch correction 统一估计。
3. TCR-aware demultiplexing：利用 TCR/BCR clonotype 与 donor SNP 信息共同判别跨样本混合。
4. Uncertainty propagation：将 donor assignment posterior 传递到 differential abundance、DE 和 immune-response association。

## 可纳入 method report 的一句话
demuxlet 的关键贡献是用自然遗传变异把 pooled scRNA-seq 中的 donor identity 和跨 donor doublet 变成可推断变量，从而支撑了后续人群免疫单细胞队列设计。
