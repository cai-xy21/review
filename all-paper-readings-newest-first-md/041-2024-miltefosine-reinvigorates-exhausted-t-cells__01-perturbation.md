# 《Miltefosine reinvigorates exhausted T cells by targeting their bioenergetic state》精读

## 论文信息

- 作者：Xingying Zhang、Chenze Zhang、Shan Lu 等
- 期刊：*Cell Reports Medicine*
- 年份：2024；5: 101869
- DOI：10.1016/j.xcrm.2024.101869
- 原文：[Cell Reports Medicine](https://doi.org/10.1016/j.xcrm.2024.101869)
- 数据总入口：[GSA-Human HRA005964](https://ngdc.cncb.ac.cn/gsa-human/browse/HRA005964)
- BioProject：[PRJCA020945](https://ngdc.cncb.ac.cn/bioproject/browse/PRJCA020945)
- 直接下载：[HTTPS 目录](https://download.cncb.ac.cn/gsa-human/HRA005964)

## 一句话结论

作者利用反复肿瘤挑战建立低功能 CAR-T 模型并筛选已批准小分子，发现 miltefosine 可恢复 GLUT1、糖酵解和氧化磷酸化，使耗竭/低功能 T 细胞重新获得效应状态并增强实体瘤控制，而且作用不依赖 PD-1 阻断。

## 数据护照（先看这一表）

| 数据层 | 内容 | GSA-Human 记录 | 分析提醒 |
|---|---|---|---|
| exhaustion time course bulk RNA | Round 0、1、2、3 和 Activated，每组 3 个重复 | 15 个样本 | 用于反复挑战产生低功能状态的轨迹 |
| miltefosine bulk RNA | PBS 2 个、Miltefosine 2 个 | 4 个样本 | 小样本，适合通路方向而非过强单基因结论 |
| scRNA-seq | PBS-sc 与 Miltefosine-sc | 2 个样本 | 处理级无生物重复，细胞数不能替代重复数 |
| M28Z 数据 | fresh vs TIL，各 3 个 bulk RNA 样本 | 6 个样本 | 数据页后续组成的一部分；需确认与正文具体图/模型对应 |
| 合计 | 27 个样本/27 个 run accession | HRA005964；HRR1442074–HRR1442094、HRR1885301–HRR1885306 | paired-end 每个 run 有 R1/R2，文件表为 54 个文件，不是 54 个样本 |
| 访问状态 | Open access | 直接 HTTPS/FTP/Aspera | 不需要数据访问委员会申请；仍需遵守数据库使用条款 |

## 1. 研究要解决的问题

实体瘤中的 CAR-T/T 细胞经历反复抗原刺激后形成低功能、代谢不足状态。作者希望建立可筛药的体外模型，并从已有安全信息的药物中寻找能直接恢复低功能 T 细胞而不是只阻断某一抑制受体的候选物。

## 2. 实验与分析框架

1. 将 CAR-T 反复与肿瘤细胞共培养，形成 round 0–3 的低功能进程。
2. 以功能性 readout 筛选已批准小分子，锁定 miltefosine。
3. 用 bulk RNA-seq 和 scRNA-seq 分析处理前后状态。
4. 测量 GLUT1、葡萄糖摄取、糖酵解、氧化磷酸化和线粒体功能。
5. 在异基因/同基因及多种实体瘤模型验证疗效和 PD-1 独立性。

## 3. HRA005964 数据详解

数据页当前列出 27 个样本。样本组成可拆为：

1. 反复挑战时间过程 15 个：Round0、Round1、Round2、Round3、Activated，各 3 个重复；
2. miltefosine bulk RNA 4 个：PBS 2、Miltefosine 2；
3. miltefosine scRNA 2 个：PBS-sc、Miltefosine-sc；
4. M28Z fresh/TIL bulk RNA 6 个：两组各 3 个。

共有 27 个 run accession，但 paired-end 下载页按 R1/R2 分列为 54 个文件。主批次 accession 为 HRR1442074–HRR1442094，另有 HRR1885301–HRR1885306。页面标记 Open access，发布日期为 2024-09-25。

单个 bulk RNA read 文件约 1.3–2.3 GB；两个 scRNA 样本的 R1 约 5.3 GB、R2 约 12.8 GB/样本。整套数据超过 100 GB 量级，解压和比对需预留更大空间。

## 4. 数据下载方式

### 4.1 先下载元数据

在 HRA005964 页面先下载 metadata Excel（`HRA005964.xlsx`），核对 sample—experiment—run—file 对应关系，再决定下载范围。直接目录：

```text
https://download.cncb.ac.cn/gsa-human/HRA005964
ftp://download.big.ac.cn/gsa-human/HRA005964
```

### 4.2 推荐工具

- 少量文件：浏览器/HTTPS 逐文件下载；
- 批量：FileZilla 等 FTP 客户端；
- 大文件和断点续传：按 GSA-Human 页面说明使用 Aspera。

只复核 miltefosine 转录效应时，优先下载 4 个 bulk RNA 或 2 个 scRNA 样本；研究 exhaustion trajectory 再下载 15 个 round/activated 样本。不要一开始整包下载。

### 4.3 完整性检查

下载后按页面提供的大小/校验信息核对文件；paired-end 必须保证每个 HRR 的 R1 和 R2 成对。建议保存元数据 Excel、下载日期和实际 accession 清单。

## 5. 主要发现

1. 反复肿瘤挑战可稳定产生低功能 CAR-T，并形成可筛药模型。
2. miltefosine 提高低功能 CAR-T 的杀伤和细胞因子能力。
3. scRNA/bulk RNA 支持细胞由耗竭/低功能状态向效应样状态偏移。
4. miltefosine 恢复 GLUT1、葡萄糖摄取、糖酵解和 OXPHOS。
5. 多种体内模型显示疗效改善，且并不依赖 PD-1 轴。

## 6. 代谢机制解释

本文将低功能视为“生物能量供给不足且难以响应”的状态。miltefosine 不是简单提高瞬时激活，而是恢复葡萄糖输入和两条主要能量通路。其直接分子靶点和不同膜脂/信号效应仍可能复杂，因此“靶向 bioenergetic state”比宣称单一靶蛋白更准确。

## 7. 推荐图版

- 反复挑战 exhaustion 模型和药物筛选流程。
- miltefosine 对低功能 CAR-T 功能恢复图。
- scRNA 状态迁移/组成图。
- GLUT1、糖酵解/OXPHOS 代谢图。
- 实体瘤体内疗效与 PD-1 独立性图。

## 8. 创新价值

1. 以功能性 exhaustion 模型筛选已有临床使用经验的药物。
2. 把单细胞状态变化与直接代谢 readout 连接。
3. 数据在 GSA-Human 以开放方式提供，包括时间过程、处理和单细胞层。

## 9. 局限性

1. scRNA 只有 PBS 和 miltefosine 各一个样本，缺乏处理级生物重复。
2. miltefosine bulk RNA 仅 2 vs 2，统计功效有限。
3. 反复共培养模型不能完全复现人体实体瘤微环境。
4. miltefosine 的多效性、剂量窗口和细胞产品制造兼容性需进一步验证。
5. 无公开原创分析代码，细胞注释复现需自建流程。

## 10. 对综述的作用

适合“药物再利用逆转耗竭”和“恢复生物能量而非只解除受体抑制”部分。它可与 mannose、P4HA1 和 p38 论文共同构成代谢/制造期干预谱系。

## 11. 可直接用于综述的观点

> 反复肿瘤挑战和药物筛选发现，miltefosine 可通过恢复 GLUT1、糖酵解和氧化磷酸化重振低功能 CAR-T，使其转向效应样状态并增强实体瘤控制，且该作用不依赖 PD-1 阻断（Cell Reports Medicine 2024, Zhang）。

## 12. 数据复用建议

- 先用 metadata Excel 重建 27 样本设计，不要仅按 HRR 编号猜分组。
- exhaustion trajectory 用 15 个 3×5 时间/状态样本。
- miltefosine bulk 以通路/GSEA 为主；scRNA 作为描述性细胞状态证据。
- paired-end 54 文件对应 27 个 run，任何缺失 R1/R2 都应在比对前排除。

## 13. 避免误读

- 论文题名药物是 **Miltefosine**，源 PDF 文件名中的 “Mitefosine” 是拼写错误。
- HRA005964 当前是 Open access，不需要受控访问申请。
- 54 个下载文件来自 27 个 paired-end run，不是 54 个样本。
- 两个 scRNA 样本不能因细胞数多而声称有充分生物重复。
- M28Z fresh/TIL 六个样本需结合数据库元数据和正文图号再解释，不能自动并入 miltefosine 2 vs 2 比较。
