# Algorithm Report 067

## Paper
A cell atlas of human thymic development defines T cell repertoire formation

## 算法视角定位
这篇文章是 **human thymus development atlas + TCR repertoire formation** 的核心资源。它的算法贡献不是提出一个全新的通用单细胞算法，而是把 human thymic cell atlas、T-cell differentiation trajectory、TCR recombination/selection kinetics 和 cell-cell communication 组织成了一个可复用的发育免疫建模框架。

## 数据模态与样本设计
- 模态：10x scRNA-seq 3'/5'、Smart-seq2、scTCR-seq/repertoire analysis、RNA smFISH validation。
- 物种/器官：`Homo sapiens` thymus；包含 fetal, pediatric, adult life；并与 mouse thymus 数据比较。
- 样本：HCA 页面描述采样 15 个 embryonic/fetal thymi，7-17 post-conception weeks；9 个 postnatal thymi，来自 pediatric/adult samples。
- 组织与细胞：胸腺免疫细胞和非免疫微环境细胞，包含 DN/DP/CD4 SP/CD8 SP/Treg/CD8aa/gamma-delta T cells、TEC、fibroblast、endothelial、DC、monocyte/macrophage 等。

## 关键算法问题
1. 如何重建人类胸腺 T-cell differentiation trajectory。
2. 如何把 transcriptomic state 与 TCR recombination/selection event 对齐。
3. 如何识别支持 T-cell development 的 thymic microenvironment cell types。
4. 如何推断 thymocyte 与 TEC/DC/fibroblast 等细胞之间的 ligand-receptor communication。

## 论文中的算法贡献
### 1) Human thymus cell census and annotation
文章使用 Scanpy 加 custom code 进行 QC、batch alignment、clustering、annotation。最终定义 40 多个 cell types/states，覆盖 T-lineage differentiation 和 thymic stromal/antigen-presenting niches。

**算法意义**：它将 T-cell development 的 reference state space 建在真实 human thymus 上，而不是依赖 mouse 或外周血 T cells。

### 2) T-cell differentiation trajectory
文章重建 conventional 与 unconventional T-cell differentiation，包括 DN、DP、positive selection、CD4/CD8 single positive、Treg、CD8aa、gamma-delta 等阶段。

**方法学价值**：
- 给 T-cell state 模型提供 developmental prior。
- 外周 T-cell phenotype 不应只按 effector/memory/exhaustion 解释，还可追溯到 thymic selection and lineage commitment。

### 3) TCR recombination and repertoire formation analysis
文章把 TCR repertoire 信息与 differentiation trajectory 结合，分析 productive/nonproductive chain ratio、TCR alpha/beta recombination bias、mature conventional vs unconventional T-cell repertoire differences。

**算法意义**：
- 把 receptor formation 当作发育过程中的 dynamic readout。
- 提示 repertoire bias 可能来自 genomic position、lineage commitment kinetics 和 selection。
- 为未来的 `development-aware TCR model` 提供生物学约束。

### 4) Identity score and APC subtype comparison
文章对 activated DC subsets 与 canonical DCs 计算 identity score，通过 marker expression summary 判断 aDC1/aDC2 与 DC1/DC2 的关系。

**方法学价值**：这是简单但可解释的 reference scoring，用于把新发现状态映射到已知 lineage。

### 5) Cell-cell interaction analysis
文章使用 CellPhoneDB 推断 thymocytes 与 TEC/DC/stromal cells 的 ligand-receptor interactions，描绘支持 T-cell differentiation 的 microenvironment network。

**算法意义**：把 T-cell development 从单细胞内状态扩展为 niche-dependent process，但通信分析仍基于表达共现与 ligand-receptor database，不是因果模型。

## 不是它解决得很好的问题
1. 细胞互作推断主要是 ligand-receptor co-expression，缺少空间约束和功能因果验证。
2. TCR repertoire 被纳入发育解释，但没有形成端到端 joint model。
3. 不同年龄段、消化条件、技术平台带来的 domain shift 仍主要依赖常规 alignment。
4. 对 population-level immune baseline 的外推还需要更多 donor metadata。

## 数据可用性评估
- DOI：https://doi.org/10.1126/science.aay3224
- PubMed/PMC：PMID 32079746；PMCID PMC7611066
- HCA project：<https://explore.data.humancellatlas.org/projects/c1810dbc-16d2-45c3-b45e-3e675f88d87b>
- Raw sequencing：ArrayExpress `E-MTAB-8581`
- Web portal：<https://developmentcellatlas.ncl.ac.uk>
- Code：Zenodo DOI <https://doi.org/10.5281/zenodo.3572422>
- 可确认数据性质：human thymus across 7-17 PCW fetal stages and postnatal pediatric/adult samples；scRNA-seq plus TCR/repertoire and validation data。
- 复用性：高。HCA、ArrayExpress、portal 与 Zenodo code 都可定位。

## 代码/模型结构
- 输入：10x droplet count matrices、Smart-seq2 count matrices、TCR calls、sample age/stage/tissue processing metadata。
- 核心流程：`Cell Ranger/STAR/htseq-count -> QC -> Scanpy integration/clustering -> cell-state annotation -> trajectory analysis -> TCR repertoire statistics -> CellPhoneDB ligand-receptor analysis`
- 输出：human thymus cell atlas、T-cell developmental trajectories、TCR formation/recombination metrics、cell-cell interaction network。
- 模型意义：这是 developmental reference and repertoire formation workflow，适合被升级为 joint developmental-repertoire generative model。

## 对新算法贡献程度评估
- 定义任务价值：很高
- 数据资源价值：很高
- 直接算法创新：中
- 对后续方法启发：很高

综合评估：**高价值 human developmental immune atlas；算法创新主要在整合分析框架和 repertoire-development coupling，而非新基础模型。**

## 可发展的新算法空间
### A. Development-aware TCR generative model
联合建模 TCR recombination events、productive selection、cell-state trajectory 和 thymic niche signals。

### B. Spatially constrained thymus communication model
结合后续 spatial thymus atlas，把 CellPhoneDB 共表达网络升级为有组织位置约束的 niche interaction model。

### C. Age/stage-conditioned thymic output prediction
从 fetal/postnatal/adult thymus 数据学习 age-conditioned T-cell output and repertoire diversity。

### D. Human-mouse thymus alignment
建立跨物种发育轨迹和 receptor selection 的 alignment model，用于评估 mouse model 对人类 T-cell immunity 的可迁移性。

## 适合纳入 method report 的表述
这篇文章提供了人类 T 细胞发育和 TCR repertoire 形成的参考坐标系。它说明 T-cell population immunity 的建模不能只看外周状态，也需要把 thymic selection、repertoire formation 和 niche interaction 纳入方法学背景。
