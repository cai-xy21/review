# 040. Single-cell analysis of Crohn's disease lesions identifies a pathogenic cellular module associated with resistance to anti-TNF therapy

## 基本信息
- 年份：2019
- 期刊：Cell
- DOI：https://doi.org/10.1016/j.cell.2019.08.008
- PMID/PMCID：31474370 / PMC7060942
- 主题：Crohn's disease lesion；tissue immune module；activated T cells；anti-TNF resistance

## 为什么重要
这篇非常适合放进“已有算法如何从 tissue single-cell data 走向临床免疫分层”章节。它没有提出通用深度学习模型，但它把 lesion single-cell profiles、cellular module、ligand-receptor network 与 anti-TNF outcome 联系起来，给出了比单纯 cluster atlas 更接近 precision immunology 的分析范式。

## 数据与研究设计
- 物种/器官：`Homo sapiens`；ileal Crohn's disease lesions、adjacent non-inflamed ileum and matched blood context
- 单细胞主体：Crohn tissue/blood single-cell expression profiles；GEO page for study data lists `31` samples
- 主资源：GEO `GSE134809`；BioProject `PRJNA556461`
- 交互资源：`scDissector` Martin dataset
- 外部验证：论文在四个 independent iCD cohorts 中测试 GIMATS module 与 anti-TNF response association，总体 `n=441`
- 文章中的关键免疫结构：IgG plasma cells、inflammatory mononuclear phagocytes、activated T cells、stromal cells 共同组成 GIMATS module

## 文章中的算法/分析流程
### 1. Tissue atlas and subtype programs
- 论文先从 lesion and matched context 中定义 cell populations and subtypes。
- 对 T-cell 部分，不只是给“activated T cell”标签，还把 tissue T-cell states 放进跨细胞群模块结构中。

### 2. GIMATS module discovery
- 作者识别一组在部分病人 lesion 中共同高表达/高丰度的细胞模块，命名为 GIMATS。
- 这一步算法意义很强：研究目标从单 cell cluster 变成 patient lesion module，输出开始接近 donor-level stratifier。

### 3. Cell-cell interaction and outcome association
- 论文通过 ligand-receptor interaction pairs 解释 module connectivity。
- 再把 module presence/signature 投射到独立 cohorts，关联 anti-TNF remission/resistance。
- 这不是 causal communication model，但确实给 tissue immune modules 与 clinical response之间搭了桥。

## 与 T 细胞—人群免疫力的关系
- 直接相关：activated T cells 是 pathogenic module 的组成之一。
- 更重要的是，它说明 T-cell phenotype 在 inflamed tissue 中常与 myeloid、plasma、stromal compartments协同出现；只做 T-cell-only clustering 很可能错过治疗反应层面的模块。
- 对新算法来说，值得建模的是 `T-cell state + tissue neighborhood/module + donor therapy outcome`。

## 数据可用性
- GEO scRNA resource：`GSE134809`
- BioProject：`PRJNA556461`
- 交互数据入口：`scDissector` Martin resource；原文说明 expression profiles 与 raw scRNA data可通过该在线应用交互探索
- 外部 cohort bulk/microarray references in paper include GEO `GSE83687` and `GSE112366`
- 本轮未定位到作者发布的独立 code repository
- 代码边界：
  - 输入：single-cell expression objects、cell subtype annotations、patient/sample labels、validation cohort expression/outcome data
  - 输出：cellular module/signature、ligand-receptor connectivity summaries、therapy-response association

## 算法贡献与不足
- 直接算法创新：**中**。不是新 package，但 patient-level cellular module discovery 与 cross-cohort validation 比单 atlas 更接近可推广算法任务。
- 数据资源价值：**高**，因为 GEO/BioProject/scDissector 多入口可定位。
- 局限：
  - ligand-receptor edges are inferred associations
  - public code不完整
  - module transfer across cohorts/platforms requires careful calibration

## 对新算法开发的启发
1. **Module-aware tissue representation**：以多 cell compartment module 代替单 cell-state score。
2. **Therapy-response predictor**：把 lesion T-cell/myeloid/stromal programs 与 baseline clinical covariates合并。
3. **Cross-cohort signature calibration**：解决 scRNA module到 bulk/microarray/clinical cohorts 的 domain shift。

## 可放入 method report 的表述
Martin et al. 把 Crohn lesion single-cell atlas推进到 patient-level pathogenic module and treatment-response association：其 GIMATS 分析说明组织 T-cell algorithms 需要建模跨 cell compartment 的 immune module，而不能把 activated T cells从 myeloid and stromal context中孤立出来。

## 一句话结论
这篇是 tissue immune module work 的高价值代表，尤其适合支撑“新算法应从 cell annotation 走向 donor-level module and therapy outcome modeling”。
