# 《Genome-wide CRISPR Screens in Primary Human T Cells Reveal Key Regulators of Immune Function》精读

## 论文信息

- **作者/期刊/年份**：Shifrut et al., *Cell*, 2018
- **DOI**：[10.1016/j.cell.2018.10.024](https://doi.org/10.1016/j.cell.2018.10.024)
- **PMID / PMCID**：30449619 / [PMC6689405](https://pmc.ncbi.nlm.nih.gov/articles/PMC6689405/)
- **全基因组/靶向screen原始数据**：[SRA SRP158611](https://www.ncbi.nlm.nih.gov/sra/?term=SRP158611)
- **CROP-seq数据**：[GSE119450](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE119450)，SRA SRP159632

## 一句话结论

SLICE首次把大规模CRISPR敲除筛选可靠地带入原代人T细胞，发现CBLB、RASA2、SOCS1等可增强增殖/杀伤的负调控因子，并用CROP-seq显示不同敲除如何把细胞导航到激活、细胞周期或低反应状态。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 技术 | sgRNA lentiviral infection + Cas9 protein electroporation（SLICE） |
| 主细胞 | 健康供者原代人CD8 T细胞；亦验证可用于CD4 |
| pilot library | 4,918条guide，1,211个基因 + 48个non-targeting controls |
| genome-wide library | Brunello 77,441条sgRNA，19,114个基因 |
| GW供者 | 初筛2位 + 独立复筛2位 = 4位健康供者 |
| 主读出 | TCR再刺激后CFSE高（不增殖）vs CFSE低（高增殖） |
| CROP-seq | 48条sgRNA；2位供者；刺激/未刺激；>15,000个可识别sgRNA的单细胞；13个cluster |
| 数据边界 | SRP158611是各screen；GSE119450只对应CROP-seq，不是全基因组screen计数矩阵 |

## 1. 研究要解决的问题

原代人T细胞难以同时高效递送大规模sgRNA和Cas9，限制了全基因组功能筛选。作者开发SLICE，并以增殖、免疫抑制抵抗和肿瘤杀伤为读出寻找可增强T细胞治疗功能的基因，同时用单细胞转录组解释命中基因改变了何种细胞状态。

## 2. 实验与分析框架

CD8 T细胞先激活并用低MOI慢病毒递送可追踪sgRNA，再电转Cas9蛋白。day 10标记CFSE并TCR再刺激4天，FACS分选不增殖与高增殖群体，测序guide并用MAGeCK计算富集。作者先做靶向pilot，再做两轮全基因组screen；另在20 μM A2A激动剂CGS-21680下筛选抗腺苷抑制基因。候选hit经arrayed编辑、NY-ESO-1 TCR/A375实时杀伤验证，并以48-guide CROP-seq读取状态。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

该文包含三个数据层，必须分开：① pooled CRISPR screen的guide丰度；②候选基因arrayed功能验证和36 h活细胞杀伤；③48-guide CROP-seq的单细胞表达。全基因组文库不是在每个细胞上做scRNA；GSE119450只存第三层。

### 3.2 多大规模、覆盖哪些生物情境

| 数据层 | 设计与规模 | 输出 |
|---|---|---|
| pilot screen | **4,918 guides / 1,211 genes + 48 NT controls**；2位供者 | 细胞表面/TCR通路靶向筛选 |
| primary GW screen | **77,441 sgRNA / 19,114 genes**；2位供者 | CFSE dividing vs non-dividing，MAGeCK gene/guide结果 |
| independent GW screen | 同一Brunello规模；另2位供者 | 与初筛整合后共4位供者，提高hit可信度 |
| adenosine GW screen | 20 μM CGS-21680 vs vehicle，刺激4天 | ADORA2A、FAM105A等选择性抗抑制hit |
| CROP-seq library | 20个GW命中基因×2 guides、checkpoint相关guides及8个NT controls，合计**48 guides** | perturbation—transcriptome连接 |
| CROP-seq样本 | 2位供者 × stimulated/no-stimulation；10x 3′ v2；另对guide做re-amplification | **>15,000个可识别sgRNA的细胞，13个表达cluster** |
| 肿瘤杀伤验证 | NY-ESO-1特异1G4 TCR T细胞 vs A375-RFP；4位供者、每基因2 guides、2技术重复 | 每4 h成像至36 h；TCEB2/SOCS1/CBLB/RASA2 KO增强清除 |

CROP-seq每个生物条件同时产生普通10x GEX库和guide re-amplification库。因此GEO有8个GSM：D1/D2 × Stim/NoStim × 10x/ReAmp。它们只对应4个供者-条件细胞悬液的两类测序读出，不是8个独立生物样本。

### 3.3 公共数据包有什么

#### pooled screens

- **SRA SRP158611**：论文所有screen的原始sgRNA测序文件，包括pilot、全基因组及腺苷条件。需要结合SRA RunInfo和补充表恢复condition、donor、sort bin与replicate。
- PMC补充表：S1为oligo/guide资源（约514.5 KB）；S2为pilot MAGeCK结果（约1.1 MB）；S3为初次GW结果（约12.2 MB）；S4为整合GW重复结果（约13.4 MB）；S5为CROP-seq guide（约11.1 KB）；S6为CGS-21680/vehicle GW结果（约24.1 MB）。

#### CROP-seq

- **GSE119450 / PRJNA489369**：8个GSM，HiSeq 4000；D1/D2、Stim/NoStim各有`10x`和`ReAmp`。
- **`GSE119450_RAW.tar`约328.0 MB**：CSV/TAR处理后文件，可获得表达和guide相关输出。
- **SRA SRP159632**：8个GSM的原始reads。
- 正文称所有分析代码“available by request”，没有给公开代码仓库；不能写成已公开可克隆。

### 3.4 如何获取：按你的目的选择

#### 路线A：直接使用screen命中结果

从PMC下载S2–S4和S6，无需重跑guide counting即可获得gene/sgRNA enrichment。建立统一字段：screen、donor、condition、bin、guide、gene、LFC、rank/FDR。

#### 路线B：从SRP158611重做screen

用SRA Run Selector导出run表并下载FASTQ。用Brunello 77,441-guide reference计数；论文MAGeCK对GW库需按`--trim-5 23,24,25,26,28,29,30`处理交错offset。先确认每run所属FACS bin再做high-vs-low比较。

#### 路线C：复用CROP-seq矩阵

下载GEO 328 MB TAR，并把每个`10x`对象与同供者/条件`ReAmp`的guide读数按cell barcode连接。直接下载S5核对48条guide序列及gene mapping。

#### 路线D：从CROP-seq FASTQ重建

由SRP159632下载reads。GEX用Cell Ranger v2.0/GRCh38；保留>500 genes的细胞；guide re-amplification reads依据U6邻接序列和20-bp guide匹配，论文允许总计4个mismatch。现代重分析可提高标准，但应先复现作者规则。

### 3.5 下载后先做什么

1. 明确SRP158611与SRP159632/GSE119450的任务边界。
2. 对GSE的8个GSM建立4对`10x-ReAmp`，核对供者和刺激标签。
3. 统计guide assignment率、多guide率、每guide细胞数及两个供者平衡。
4. screen分析检查文库覆盖、零计数、重复相关和CFSE分箱方向。
5. CROP-seq差异应考虑donor和activation cluster，避免把刺激主效应误归因于敲除。

## 4. 主要发现

- 必需TCR信号基因（如CD3D、LCP2）在高增殖群体中耗竭；CBLB、CD5、RASA2、SOCS1等负调控基因KO促进刺激依赖增殖。
- CROP-seq显示CBLB/CD5/RASA2/SOCS1等敲除推动IL2RA、GITR、MKI67、GZMB相关激活/细胞周期/效应程序，而CD3D/LCP2敲除推动低响应状态。
- TCEB2、SOCS1、CBLB、RASA2 KO提高NY-ESO-1 TCR T细胞对A375的36 h清除。
- 腺苷screen识别ADORA2A及FAM105A等，使T细胞选择性抵抗A2A介导的抑制。

## 5. “状态—功能—驱动”证据链

pooled screen提供基因—增殖因果关系，CROP-seq解析状态程序，实时肿瘤杀伤和抑制环境验证提供功能端点。这一层级最接近综述所需的“找到驱动—定量状态转移—验证功能”。

## 6. 推荐图版

SLICE流程、77,441 guides的GW volcano/排序图、48-guide CROP-seq UMAP及cluster富集、36 h实时杀伤曲线、CGS-21680抗抑制screen可组成完整方法链。

## 7. 创新价值

解决原代人T细胞全基因组筛选递送难题；把低维增殖筛选与高维单细胞状态读出结合；加入肿瘤杀伤和免疫抑制情境，体现多目标工程。

## 8. 局限性

需要预激活和体外扩增，命中更代表抗原经历T细胞；CFSE增殖不能覆盖所有效应/持久性；CROP-seq只含48 guides和两位供者；长期体内安全性与失控增殖风险未由本研究解决。

## 9. 对本综述架构的作用

是“perturb/manipulate cell states”的奠基性论文，也适合导向“real-time optimization”：用并行筛选找到控制输入，再以单细胞和动态杀伤构建多目标评价函数。

## 10. 可直接用于综述的观点

> 原代人T细胞状态可以通过全基因组扰动系统性导航：低维筛选确定增殖/抗抑制命中，高维CROP-seq解释状态去向，实时肿瘤杀伤则检验这些状态改变是否转化为治疗功能。

## 11. 避免误读

- 77,441条guide的全基因组screen不在GSE119450；其原始数据在SRP158611。
- GSE119450是48-guide CROP-seq，8个GSM对应4个生物条件的GEX/ReAmp双读出。
- >15,000是可识别sgRNA的单细胞数，不是全基因组筛选细胞数。
- 促进增殖的KO不必然改善持久性或临床安全性。
