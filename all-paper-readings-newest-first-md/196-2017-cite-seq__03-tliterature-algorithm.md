# Algorithm Report 013

## Paper
Simultaneous epitope and transcriptome measurement in single cells

## 基本信息
- 年份/期刊：2017, Nature Methods
- DOI：https://doi.org/10.1038/nmeth.4380
- PMID/PMCID：28759029 / PMC5669064
- 公开数据：GEO `GSE100866`
- 代码：原文核心是实验方法与数据结构，没有独立算法包；后续标准分析主要进入 Seurat/Scanpy、totalVI、WNN 等多模态工具链

## 算法视角定位
这篇不是传统计算算法论文，但它定义了后续单细胞多模态算法的输入对象：同一个 cell barcode 下同时有 RNA UMI matrix 和 antibody-derived tag (ADT) UMI matrix。对 T 细胞和人群免疫研究来说，这一数据结构非常关键，因为很多 T cell state 不能只靠 RNA 稳定区分，必须把 CD45RA/CCR7/CD27/CD28/PD-1/TIGIT 等蛋白层信息纳入。

## 数据与任务
- 物种/样本：`Homo sapiens` cord blood mononuclear cells (CBMC/PBMC-like immune cells)；另有人/鼠 species-mixing 实验用于验证特异性
- 器官/组织：外周/脐带血免疫细胞
- 规模：代表性 CBMC CITE-seq 数据约 `8,005` 个 human CBMC，另含约 `600` 个 mouse control cells；GEO `GSE100866` 提供 RNA 与 ADT 数据
- 模态：3' scRNA-seq + oligo-tagged antibody counts
- 计算任务：joint cell-state annotation、RNA-protein correlation、ADT-based clustering、RNA+protein multimodal embedding、protein denoising/background correction

## 核心方法结构
### 1. CITE-seq 数据生成
- 抗体偶联 DNA oligo tag，抗体识别细胞表面 protein epitope。
- 单细胞反应中同时捕获 mRNA 与 antibody tag。
- 测序后形成两个矩阵：gene-by-cell RNA counts 与 antibody-by-cell ADT counts。
- 两个矩阵共享 cell barcode，因此可以在单细胞级别直接连接转录组与表面蛋白。

### 2. ADT 计数与归一化
- RNA 使用常规 UMI count 处理。
- ADT 是 compositional/high-count signal，原文使用 centered log-ratio (CLR) 一类变换做可视化和比较。
- 这一步后来成为 CITE-seq 分析的基础问题：ADT 背景噪声、抗体特异性、isotype/background、抗体间动态范围不一致，都不能简单用 RNA 的 log-normalization 解决。

### 3. 联合注释与聚类
- 原文展示了 RNA clustering 与 ADT clustering 可以互相验证。
- ADT 对免疫细胞亚群特别有用：同一转录簇内，表面蛋白可以分离更细的免疫表型；反过来 RNA 可以解释蛋白定义群体的功能状态。
- 对 T 细胞，CITE-seq 把 naive/memory/effector/exhaustion-like states 的标志物定义从 RNA-only 推进到 RNA+protein。

## 输入、输出与模型意义
- 输入：single-cell FASTQ，经 Cell Ranger/兼容流程得到 RNA count matrix、ADT count matrix、barcode metadata、antibody panel metadata
- 输出：joint metadata、ADT-normalized matrix、RNA/protein marker table、cell-type/state labels、可供 WNN/totalVI 等模型使用的多模态对象
- 模型结构：原文没有提出生成模型；它提出的是可规模化的 multimodal measurement design
- 方法意义：它让“单细胞多模态联合表示学习”成为真实任务，而不只是事后整合不同实验

## 对新算法开发的贡献程度
- 直接算法创新：中等偏低
- 数据结构创新：极高
- 对 T 细胞/人群免疫算法启发：极高
- 综合评估：**P0 级数据范式论文**

## 新算法空间
1. ADT background-aware normalization：显式建模 ambient antibody tag、非特异结合和抗体动态范围。
2. Donor-aware RNA-protein integration：在 cohort 中区分 donor effect、batch effect 与真实 immune-state variation。
3. T-cell state uncertainty：结合 RNA、ADT、TCR clonotype 给出 state label posterior，而不是硬标签。
4. Missing-modality transfer：在只有 RNA 的队列中预测 protein phenotype，并输出不确定性。

## 可纳入 method report 的一句话
CITE-seq 的算法意义不在于提出一个单独模型，而在于把 RNA 与表面蛋白变成同一细胞上的原生多模态观测，直接催生了 totalVI、WNN、ADT denoising 和 donor-aware multimodal immune modeling 等后续算法问题。
