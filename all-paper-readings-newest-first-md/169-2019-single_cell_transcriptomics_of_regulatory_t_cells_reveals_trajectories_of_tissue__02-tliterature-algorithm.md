# Algorithm Report 063

## Paper
Single-Cell Transcriptomics of Regulatory T Cells Reveals Trajectories of Tissue Adaptation

## 算法视角定位
这篇文章是 **Treg tissue adaptation trajectory** 的代表性研究。它不是纯算法论文，但使用了较强的计算建模组合：单细胞聚类、BGPLVM latent trajectory、gene kinetic ordering、RNA velocity、TCR reconstruction 和 mouse-human conserved signature 比较。对 T 细胞-人群免疫力方向，核心价值是把 Treg 从“固定 subtype”改写为“从 lymphoid tissue 到 non-lymphoid tissue 的连续适应过程”。

## 数据模态与样本设计
- 模态：Smart-seq2 scRNA-seq；部分 10x/scRNA；TCR 从 scRNA-seq 中用 TraCeR 重建；体内 melanoma challenge 验证。
- 物种/器官：主要为 `Mus musculus`；spleen、skin、colon、skin-draining lymph node、mesenteric lymph node；包含 human blood 与 non-lymphoid tissue Treg/Tmem comparison。
- 细胞：约 35,000 个 CD4+ Treg 与 memory T cells。
- 研究对象：Treg/Tmem 在 lymphoid tissue 与 non-lymphoid tissue 之间的迁移、预激活和终末组织适应。

## 关键算法问题
1. 如何从 cross-sectional scRNA-seq 推断 Treg 从 LT 到 NLT 的连续适应轨迹。
2. 如何比较 skin 与 colon 两条组织适应路径是否共享 gene kinetic order。
3. 如何把 tissue-specific homing、activation、metabolic adaptation 放到同一伪时间轴上。
4. 如何比较 mouse 与 human tissue Treg signatures 的保守性。

## 论文中的算法贡献
### 1) Treg tissue adaptation latent trajectory
文章使用 Bayesian Gaussian Process Latent Variable Model (BGPLVM) 对 mLN-colon 与 bLN-skin Treg cells 建立 latent variables。最重要的 latent variable 被解释为 lymphoid-to-non-lymphoid adaptation axis。

**算法意义**：
- 不是简单 cluster ordering，而是用连续 latent variable 表示 tissue adaptation。
- 把 Treg NLT-like cells、Treg LT-like cells、fully adapted NLT Treg 放到同一过渡轴上。
- 给后续 tissue adaptation modeling 提供了结构：`source lymphoid state -> primed intermediate -> tissue-adapted endpoint`。

### 2) Gene kinetic ordering
沿 latent variable 对基因表达拟合 sigmoidal curve，并估计每个 gene 的 activation point。再把 GO biological processes 的基因激活时间聚合，比较 skin 与 colon 的过程顺序。

文章报告 skin 和 colon 的 gene expression kinetics 具有一致性，迁移和 glycolytic process 较早，随后是 proliferation，cytokine production 和 fatty acid homeostasis 更晚。

**方法学价值**：
- 从“哪些基因差异表达”推进到“哪些程序先后启动”。
- 对免疫适应、组织驻留和治疗干预窗口的建模更有意义。

### 3) RNA velocity directionality
文章用 velocyto 推断 Treg adaptation 的方向性，支持部分 NLT-like/eTreg cells 正在朝更明显的 NLT phenotype 适应。

**算法意义**：用 spliced/unspliced signal 为伪时间方向提供独立证据，但仍是 cross-sectional inference，不能等价于真实 lineage tracing。

### 4) TCR reconstruction and clonality
文章从 scRNA-seq 数据中用 TraCeR 重建 TCR 序列，并用于推断 clonality。

**意义**：把 Treg tissue adaptation 与 clonal context 连接起来，但主要仍是辅助注释/解释，没有构成 clone-aware trajectory model。

### 5) Cross-species conserved tissue module
文章比较 human 与 mouse non-lymphoid tissue CD4+ T cell signatures，识别保守 tissue programs，同时指出 paralog usage 可能存在物种差异。

**方法学价值**：这为 human translation 提供桥梁，也提示跨物种模型不能只依赖一对一 ortholog matching。

## 不是它解决得很好的问题
1. 主要是 mouse 数据，human 部分更像保守性验证，不是大规模人群建模。
2. pseudotime/velocity 无法完全替代真实纵向迁移追踪。
3. TCR 信息未深度进入 latent model。
4. donor/sample 层级结构和批次效应不是本文核心。
5. tissue adaptation 与抗原特异性之间的因果关系仍未建模。

## 数据可用性评估
- DOI：https://doi.org/10.1016/j.immuni.2019.01.001
- PubMed：PMID 30737144
- Raw scRNA-seq accessions：ArrayExpress `E-MTAB-6072`、`E-MTAB-7311`
- Processed data：Figshare project `Treg_scRNA-seq`：<https://figshare.com/projects/Treg_scRNA-seq/38864>
- 作者数据门户：<http://www.teichlab.org/data/>
- 代码仓库：<https://github.com/tomasgomes/Treg_analysis>
- 可确认数据性质：mouse CD4+ Treg/Tmem from spleen, lymph nodes, skin, colon；human blood/NLT CD4+ T-cell comparison；约 35,000 cells。
- 复用性：高。raw accession、processed data 和 analysis notebooks 均可定位。

## 代码/模型结构
- 输入：Smart-seq2/10x expression matrix；cell metadata；tissue labels；Treg/Tmem annotations；可选 TCR reconstruction。
- 核心流程：`QC -> Seurat clustering/DE -> BGPLVM latent trajectory -> sigmoidal gene kinetics -> GO timing -> velocyto directionality -> TraCeR clonality -> mouse-human signature comparison`
- 输出：Treg tissue-adaptation axis、subpopulation labels、gene activation order、conserved tissue signatures、TCR clonality summaries。
- 模型意义：将 tissue adaptation 形式化为 continuous latent process，为 tissue-conditioned T-cell trajectory learning 提供模板。

## 对新算法贡献程度评估
- 定义任务价值：高
- 数据资源价值：高
- 直接算法创新：中
- 对后续方法启发：高

综合评估：**应用型算法贡献较强；它没有发明全新通用模型，但把 latent trajectory、gene kinetics、velocity 与 TCR context 组合成了一个值得复用的 Treg adaptation workflow。**

## 可发展的新算法空间
### A. Tissue adaptation VAE
用 condition-aware generative model 同时学习 tissue-invariant Treg identity 和 tissue-specific adaptation program。

### B. Clone-aware Treg migration model
把 TCR clonotype 作为 graph/hyperedge 纳入 trajectory，使同克隆细胞的跨组织分布约束 latent transition。

### C. Cross-species immune state alignment
从简单 ortholog mapping 升级为 many-to-many paralog-aware alignment，尤其适合 mouse-to-human Treg translation。

### D. Perturbation-aware adaptation kinetics
结合 tumor challenge、infection、autoimmunity 或 therapy perturbation，学习 gene programs 的启动顺序和可逆性。

## 适合纳入 method report 的表述
这篇文章展示了单细胞 Treg 数据可以从 subtype catalog 进一步发展为 tissue adaptation trajectory。现有方法依赖 BGPLVM、gene kinetic fitting 和 velocity 等分散工具，下一步空间在于把 tissue、clone、species 和 perturbation 合并进统一可解释模型。
