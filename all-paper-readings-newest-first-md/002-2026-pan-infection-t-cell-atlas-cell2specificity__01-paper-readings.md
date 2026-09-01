# 《A pan-infection single-cell atlas of human T cells unlocks systematic antigen-specificity inference》资源精读

## 资源信息与状态说明

- 作者：Lisa M. Dratva、Yizhou Yu、Elizaveta K. Vlasova 等
- 年份：2026
- 项目主页：[tcellatlas.org](https://tcellatlas.org/)
- 工具：[cell2specificity GitHub](https://github.com/Teichlab/cell2specificity)
- 分析代码：[tcellatlas_analysis_code](https://github.com/Teichlab/tcellatlas_analysis_code)
- 交互浏览：[Atlas viewer](https://cxg.tcellatlas.org/)

截至 2026 年 8 月 12 日，项目网站和 reviewer-facing 代码已公开，但未在可核验页面上给出期刊、卷页或 DOI。正文引用应暂写“2026, Dratva，项目资源/待正式发表”，不要伪写成 Nature/Cell 论文。

## 一句话结论

该资源整合 25 个数据集、1,135 名供者、10 类组织和 14 种病原体情境中的 380 万以上人 T 细胞，以 paired scTCR + transcriptome 为核心，联合 TCR motif、HLA 推断、病原暴露推断与 TCR–peptide–HLA 结构建模。

## 1. 数据护照

| 维度 | 规模/内容 | 说明 |
|---|---:|---|
| T 细胞 | >3.8 million | 项目主页当前口径 |
| 供者 | 1,135 | 不同研究的临床和年龄信息不均衡 |
| 数据集 | 25 | 跨平台、跨实验室整合 |
| 病原体 | 14 | 感染/疫苗/暴露情境；精确列表以 metadata 为准 |
| 组织 | 10 | 不限于外周血 |
| 状态模型 | broad lineages；CD4 29 states；CD8 12 states | CellTypist 模型随工具发布 |
| 关键模态 | scRNA + paired TCR | 工具要求 TRA/ TRB V/J/CDR3 字段 |
| 抗原参考 | VDJdb、BEAM 等实验特异性数据 | 训练标签覆盖高度偏向常见病毒与 HLA |

## 2. cell2specificity 做什么

1. **状态注释**：用图谱训练的 CellTypist 模型标注 CD4、CD8、MAIT、iNKT、γδ/NKT 及细分状态。
2. **TCR motif discovery**：按序列距离将 clonotype 聚成可能共享特异性的 motif。
3. **已知特异性注释**：查询 VDJdb，并以 V/J 使用识别 MAIT/iNKT。
4. **HLA 推断**：利用 MHC 限制的 public TCR motif 推断供者 HLA。
5. **病原暴露推断**：根据 donor × motif 矩阵预测感染/暴露史。
6. **结构层**：用 TCRdock/结构预测查看 TCR–peptide–HLA 候选复合物。

## 3. 输入数据格式

项目网页的 motif matcher 要求至少包含：

```text
IR_VDJ_1_junction_aa, IR_VDJ_1_v_call, IR_VDJ_1_j_call,
IR_VJ_1_junction_aa,  IR_VJ_1_v_call,  IR_VJ_1_j_call,
donor_id
```

这相当于一条 productive TRB（VDJ）和一条 productive TRA（VJ）的首选链。多链细胞、缺失链和双细胞要在导入前定义处理规则。

## 4. 数据获取

- 项目主页提供 integrated atlas 与 raw BEAM antigen-specificity data 的 ArrayExpress 入口；若链接暂只跳到 ArrayExpress 首页，可在项目补充材料/仓库更新后按 accession 搜索。
- [CELLxGENE viewer](https://cxg.tcellatlas.org/) 适合查看状态、基因和供者 metadata。
- [cell2specificity](https://github.com/Teichlab/cell2specificity) 含 Python 包、三个 CellTypist 模型、motif–pathogen/HLA 表和测试数据。
- [analysis code](https://github.com/Teichlab/tcellatlas_analysis_code) 提供 reviewer-facing figure notebooks，但大型输入不在 Git 仓库中。

## 5. 如何评价“抗原特异性推断”

- **高置信**：实验测得的 TCR–peptide–HLA 结合/激活，且 HLA 与供者匹配。
- **中等置信**：与数据库中完整 paired TCR 高度匹配，并有相同 HLA/病原背景。
- **候选**：motif 邻近、暴露预测或结构模型支持，但未做功能验证。
- 不能把 motif 聚类、HLA 推断或 AlphaFold/TCRdock 结构直接写成“证明结合”。

## 6. 推荐图版

- **项目主页数字卡片**：3.8M T cells / 14 pathogens / 1,135 donors / 25 datasets / 10 tissues；适合前沿资源页。
- **cell2specificity workflow**：TCR motif → HLA → pathogen exposure；最能体现特色。
- **TCR–pMHC structure viewer**：只作概念图，图注写“结构预测”。
- 正式论文图号尚应待 version of record 核验，现阶段不要写未经确认的 Fig. 编号。

## 7. PPT 单页格式

**标题**：下一代 T 细胞图谱从“状态”走向“受体—HLA—抗原”

**正文**：>380 万 T 细胞；1,135 名供者；14 种病原体；paired TCR motif 用于 HLA 与暴露推断。

**配图**：项目数字卡片 + cell2specificity workflow。

**页脚引用**：2026, Dratva（项目资源；正式期刊信息待核验）。

## 8. 局限性

- 当前正式发表状态和 DOI 未在项目页确认。
- 数据库标签偏向常见病毒、HLA-A*02:01 等高频 HLA，泛化到稀有 HLA/病原有限。
- public TCR 可跨人出现，motif 与暴露存在群体频率和 HLA 混杂。
- 预测 HLA 与病原暴露不应替代基因分型、血清学或功能实验。
- 跨研究 atlas 的病原、组织和临床元数据不均衡。

## 9. 可直接用于综述

> Pan-infection T cell atlas 将 380 万级转录状态与 paired TCR motif 相连，展示图谱从“细胞是什么状态”走向“可能识别何种 peptide–HLA”的方向；但 motif、HLA 和结构预测仍需实验特异性验证（2026, Dratva，项目资源）。
