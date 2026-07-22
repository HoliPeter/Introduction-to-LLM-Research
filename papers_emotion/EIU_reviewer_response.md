# EIU 审稿意见逐条回复（Response to Reviewers）· 多轮汇总（living document）

> **性质**：本文件汇总 EIU 论文**历次**审稿的逐条回复，**按轮次累积**——每一轮的意见与回复都追加到本文件、
> 不删除历史轮次，便于对账与追溯。
> **原则**：所有数字均实测、不 p-hack；负面/边界结果如实进 `EIU_experiment_checklist.md`，正文只留成立的正向主张；
> 审稿指出的硬错误一律**如实更正而非辩护**。
> **状态标记**：✅ 已落正文 ｜ 🟡 脚本就绪·运行中/待跑 ｜ 🔵 待作者决策。
> **交叉引用**：过程/负面结果见 `EIU_experiment_checklist.md`；正文改动见 `draft_emotion_zh.md`。

## 轮次索引（Rounds）
- **第一轮（Round 1）· 2026-07-14** — Weak-Reject / R&R：14 条评述 + 6 项必做实验。（见下）
- **第二轮（Round 2）· 2026-07-19** — Weak-Reject（4/10，置信度 4/5）：14 条主要 + 12 条次要 + 6 项必做修改。（见文末）
- **第三轮（Round 3）· 2026-07-20** — 当前定稿同步：创新性检索边界 + 正文最终口径 + 未完成项。（见文末）

> **当前口径说明：** Round 1/2 为历史追踪记录，其中旧数据与旧表述不回删；若与 Round 3 冲突，**以 Round 3 和当前正文 `draft_emotion_zh.md` 为准**。

---

# 第一轮（Round 1）· 2026-07-14

> **来源**：Weak-Reject / R&R（14 条评述 + 6 项必做实验）。
> **本轮交叉引用**：checklist §1 等算力、§7 中介、§8 统计/探针/操纵检验；正文 摘要 / §6.2 / §6.3ter / §6.6。

## 0. 总回应（Overview）

感谢审稿人细致的意见。我们据此做了三类工作：(1) 完成 6 项必做实验中已可实测的部分，并用更严格的统计/因果方法重估核心主张；
(2) **如实更正**审稿指出的硬错误（Pareto 支配计数、若干过强表述），删除不成立的论断；(3) 对仍需作者判断的命名/定位问题给出方案供决策。
我们的一贯做法是：凡未实测处标〔待回填〕、绝不预填；负面结果照报，不换设计追显著。

---

## 1. 六项必做实验（Requested Experiments）

| # | 主题 | 状态 | 一句话结论 |
|---|---|---|---|
| 1 | 情绪 vs 速度混淆 | ✅ 已落正文 §6.3ter | 速度是**中介非混淆**；异质双通路；非速度情绪有直接效应→"纯速度"被证伪 |
| 7 | 探针去泄漏 | ✅ 已落 §6.6.1 | 每题一行**无泄漏**；EAL 向量 CV 0.746–0.766(置换 p=.002)；Llama resid 去乐观后不显著→靠 EAL 承载 |
| 9 | 等算力防御对比 | ✅ 已完成（checklist §1） | 等算力下双源≈单源→收益来自"选择机制"非去情绪源 |
| 11 | 更强 sycophancy | ✅ flip + target adoption 全部统计完成；作为探索性测试**移出正文/附录**，完整过程见 `experiments_emotion.md §8` | flip 45 项 0 正显著；target-capit 45 项 4 名义显著但**全部不过 BH**；方向跨模型异质 |
| 12 | 操纵有效性检验 | ✅ 收紧构念 v2 完成（DeepSeek judge，360 候选） | 收紧 stance 构念后 stance/answer 泄漏**全 0**、cat-acc 0.90–1.0、5 情绪强度**全单调**、gold 351/360；v1 高 stance 泄漏经同池同判官对照证实纯系宽构念把情绪元偏好误判为立场 |
| 13 | 混合效应 logistic | ✅ 已跑·已回填 §6.2+摘要 | 池化 urgency OR=3.08–5.33，p=3e-47；6/7 格显著 |

### 必做 1 · 速度是否为混淆（urgency 只是"叫模型简短"？）✅
- **回应**：速度/简短是 urgency 的**中介（mediator）而非混淆**；控制掉中介 = post-treatment bias（审稿 #13 自己也承认长度是处理后变量，应做中介分析）。
- **实测（`mediation_urgency_brevity.py`，现有数据，LPM 差分法 + 题目 cluster bootstrap）**：机制**异质双通路**——
  - Qwen（SVAMP/DROP）近乎**纯中介**：urgency 砍掉 ~100 token，NIE≈+0.30 显著，NDE<0（压制）；即"urgency→更短 CoT→CBU"；
  - Gemma（三数据集）近乎**纯直接**：NDE +0.28~+0.51，GSM8K 上 urgency 反而让 CoT *变长* 而 CBU 仍升；
  - **不含任何速度语的情绪臂（信任/失望/愤怒）** 仍显著抬高 CBU 且以直接效应为主 → **"纯速度"归约被证伪**。
- **落地**：新增正文 §6.3ter + 摘要句；删除旧的"full mediation/CDE≈0"预判（被数据推翻）。诚实标注 sequential-ignorability 假设。

### 必做 7 · 探针去泄漏（AUROC 可能虚高）✅
- **回应**：审阅 exp0026 发现探针**每题一行**（correct-only、单情绪 direction），本就无题目泄漏；scaler/PCA 折内拟合，无预处理泄漏。
- **加固**：(a) CV 升级为 **StratifiedGroupKFold（按题目分组）**——实测日志"重复题目行数=0"坐实无泄漏；
  (b) 保存原始特征 `probe_features.npz`，离线做**嵌套 CV 去挑层乐观 + 群组置换检验 + bootstrap CI**（`probe_deleaked_eval.py`）。
- **已知数字**：EAL 全层向量 CV（无挑层，头条口径）= **Qwen 0.763 / Llama 0.746**，GroupKFold 下不变；
  诚实边界：best-layer 为挑层乐观（Qwen resid_gen 0.798 vs mean 0.732）；**Llama 靠 EAL（resid 近随机 0.56–0.61）、Qwen resid+EAL 皆强**。
- **实测结果（三模型，已回填 §6.6.1）**：无泄漏坐实（三模型重复题目行数=0、GroupKFold≡分层）；EAL 向量 CV Qwen 0.763 / Llama 0.746 / Gemma 0.766（CI 下界 .67–.71、群组置换 p=.002）；去挑层乐观(nested)后 Qwen/Gemma resid 仍显著（0.76–0.81、乐观量仅 +.01–.03），**Llama resid 不显著（0.54–0.56，CI 含 0.5）→ 可检测性靠 EAL 承载**；分模型如实报告，不笼统写"跨模型 0.72–0.76"。〔**更正**〕gen_len 基线**公平口径**（同 GroupKFold 折内学方向）= Qwen 0.753 / Llama 0.705 / Gemma 0.785，**与探针相当**（此前"0.21–0.28 ≪ 探针"误用原生反方向，已更正，见 checklist §8.2）→ **不主张"探针≫长度"**；探针独有价值 = 生成前预警 + 机制特异；探针与长度**共同** ≫ 一致性/采样检测器（≤随机）才是核心检测贡献。

### 必做 9 · 等算力防御对比（防御是否只是"多算"）✅
- **回应/实测**：等算力对照 D（单源 N=10 vs 双源 5+5，同为 12 次前向，GSM8K/urgency）显示双源≈单源、CBU 无显著差异
  → 收益来自**"更多样候选 + 忠实性验证器选择"**，而非去情绪源的特殊性。
- **落地**：删除正文"破 correlation ceiling"表述，只保留成立的因果分解（一致性门控→acc、验证器选择→CBU）。详 checklist §1。

### 必做 11 · 更强 sycophancy ✅（三模型剂量—反应 + target adoption 配对全部完成）
- **回应**：把 Sharma (2023) 的“承诺后反驳”范式移植到 Qwen/Llama/Gemma（`exp0045_emotion_challenge.py`）。首轮 Qwen confident→authority 两轮升级得到 neutral flip=.863，说明开源模型在强权威压力下会大量放弃正确答案，但该值近天花板，不能用于辨识情绪增量。
- **设计修复与操纵检查**：改为 weak/direct/confident 三档逐档单轮，同一首轮答案、仅在首轮答对题上同题配对。中性 flip 随强度单调上升：Gemma .275→.373→.529，Qwen .098→.137→.569，Llama .761→.826→.913。故强度操纵有效；Gemma 三档、Qwen/confident 有动态范围，Llama 全档仍近天花板。
- **宽口径 flip 主结果**：45 个模型×强度×情绪比较中，**没有任何正向 $\Delta$flip 达到精确 McNemar $p<.05$**；正/零/负 = 5/3/37，负向 $p<.05$ 有 6 项，全 45 项 BH 后 $q_{\text{all}}<.05$ = 0。预注册 urgency 的 9 个效应全部非正。
- **严格 target-specific capitulation 配对结果（`analyze_exp0045_dose.py` 已离线跑完，无需重跑模型）**：45 项中正/零/负 = **24/3/18**，正向精确 McNemar $p<.05$ = **4**、负向 = 0，但全 45 项 BH 后 **$q_{\text{all}}<.05$ = 0**（4 个名义正向 Gemma/confident/urgency +.176、Qwen/weak/disappointment +.157、Llama/direct/anxiety +.174、Llama/weak/high_trust +.196 的 $q_{\text{all}}$ 均为 .4327）。urgency 9 格方向跨模型相反（Gemma 正、Qwen 负、Llama 弱正），无一过方向内校正。
- **统计更正**：旧脚本按 bootstrap CI 是否跨 0 打“★”，会把 $p=.0703/.0768/.125$ 错标显著；正式结论以精确 McNemar $p$ 为准、CI 表不确定性、另报 BH-$q$。旧星号不引用。
- **构念更正**：`flip`（转为任意错误答案）不等于 `target-specific capitulation`（采纳用户指定错误答案 $X$）。confident neutral 下 Gemma/Llama/Qwen 的 flip=.529/.913/.569，而 target-capit=.157/.130/.353；两口径的情绪效应可方向不同，故须分开报告、不能把 flip 统称 sycophancy。
- **落地（诚实归属）**：宽口径 flip 总体偏负、严格 target-capit 描述上略偏正，两者都无 BH 后稳健情绪放大且方向异质，因此 **exp0045 整体作为探索性压力测试移出论文正文/附录**（完整过程与统计见 `experiments_emotion.md §8`），**不在正文挑取那 4 个名义显著子格**。EIU≠迎合的正向辨识不依赖 exp0045：决定性证据是 pure_emotion 臂没有任何具体答案可迎合、CBU 却显著上升，辅以温和 SYCON FlipRate≈0 与控制 ELEPHANT 后情绪系数不变。摘要/引言中原“更强承诺后反驳”相关句已删除。不再换强度或扩样本追显著。

### 必做 12 · 操纵有效性检验（manipulation check）✅
- **定版结果（`exp0016`，DeepSeek judge=deepseek-v4-flash，360 候选=18 格×20，收紧 stance 构念）**：五个情绪方向 **category-accuracy 0.90–1.00**（anxiety/disappointment 1.00、high_trust .983、urgency .967、anger .900），**强度全部单调** low<mid<high，valence/arousal 方向合理（anger arousal .79/valence −.68；urgency arousal .76；high_trust valence +.76），**stance 泄漏与 answer 泄漏在全部方向均为 0.000**，gold=**351/360**。
- **stance 泄漏的构念澄清（回应 #12 核心质疑）**：早期宽构念下 stance leakage 偏高（deepseek/360 宽构念 gold 仅 294/360；gpt-4o/36-seed 宽构念下 .61–1.00），原因是把“想答对/信任模型/急迫”等**社会性元偏好**误判为 stance。把 `has_stance_cue` 收紧为“是否指明**具体哪个答案/选项**正确”后，**同判官（deepseek）、同 360 池、仅改此一个构念**，stance 泄漏即降为 0、gold 从 294 升到 351——这直接证实高 stance 泄漏是宽构念的度量假象，而非模板真泄漏；决定性的 **answer 泄漏在任何构念/判官下始终为 0**。
- **跨判官稳健性**：独立 **gpt-4o** judge（非受测的 Qwen/Llama/Gemma）在部署 seed 上给出 category-accuracy=1.0、answer leakage=0，与 DeepSeek judge 一致，排除自评偏差。
- **诚实边界**：(i) anger 的 category-accuracy 最低（.900），judge 偶将其归为 frustration/irritation 等近义负情绪；(ii) 以上为 **LLM-judge** 操纵预检，若审稿坚持人工，仍需 2–3 名标注者及 Cohen/Fleiss $\kappa$（未做）。
- **落地**：主实验只用 gold（351/360）；§4.1/附录 B 的操纵检查以 category-acc 0.90–1.0 + 强度全单调 + 双泄漏=0 为主证；产物 `results/exp0016_manipcheck_v2_full/manipulation_check_report.{json,md}`。

### 必做 13 · 混合效应 logistic（p 值忽略重复测量）✅
- **回应/实测**：题目随机截距 GLMM + 按题目聚类稳健 logistic 双口径。
  **池化 7 单元格（n=2100，控 model/dataset）：urgency OR=3.08（聚类稳健，95%CI[2.64,3.58]，p=3.25e-47）/ 5.33（GLMM）**；
  逐格 **6/7 显著**，唯一 null 是 **Llama/DROP**（OR=1.11，p=.53，与中介分析吻合）。
- **诚实标注**：Gemma/GSM8K 因 filler 基线 .007 近完全分离，聚类稳健 OR 不稳，以 GLMM 24.5 为准；GLMM 条件 OR > 聚类稳健边际 OR（条件-边际差异），方向/显著性一致。
- **口径澄清（代码审计 2026-07-16）**：结果变量为**联合率** $P(\text{正确}\wedge\text{不忠实})$（`correct_but_unfaithful` = correct ∧ has_answer ∧ shortcut），**非**条件率——这是 **collider-safe** 的正确选择（不条件化于处理后变量"正确"、避免选择偏差），与 both-correct 配对互补；GLMM 的 CrI 由变分近似(VB)给出、可能偏窄，故区间以更保守的聚类稳健为准。代码审计确认 `mixed_effects_cbu.py` / `mediation_urgency_brevity.py` 无实现 bug（详 checklist §8.6）。
- **落地**：已回填正文 §6.2（"混合效应 logistic 确认"段，含联合率/collider-safe 澄清）+ 摘要；§6.3ter 中介同口径。

---

## 2. 硬错误与过强表述——如实更正（Corrections）

| 审稿点 | 原问题 | 更正 | 状态 |
|---|---|---|---|
| #8 | SR-Guard 严格支配"8/9"计数错误 | 防御已重做为 **EAL-Guard**（严格三折、val 标定），主指标改综合失败率 $P(\text{答错}\vee\text{不忠实})$：9 格中 **8 格为可部署方法最低**、双轴 6/9、3 格 ΔCBU 显著；**撤下“帕累托前沿/严格支配”计数**，改点估计 + 同题 permutation | ✅ |
| #4 | 断言"CBU 对准确率天然不变"（错误） | 删除该论断，改用**选择偏差 + both-correct 口径**表述 | ✅ |
| #9 | "计算高效/零额外开销"过强 | 删除，如实写 **12 vs 6 次前向**的算力代价 | ✅ |
| #5 | "两阶段因果链"暗示完整因果识别 | 改为**"描述性分解，非完整因果识别链"** | ✅ |
| #10 | "因果分解"过度声称 | 改为**"离线选择消融"**（一致性门控恢复 acc + 验证器选择压 CBU，共 3 处） | ✅ |
| #3 | "正交/方法无关的真实失效"过强 | 改为**"扰动式忠实性代理"** | ✅ |
| #14 | 状态"待回填"、数据集命名（GSM-Plus） | 存在性/防御主表已实测；数据集据实改为 **SVAMP/DROP** | ✅ |
| #1 | 摘要未交代 urgency 的速度混淆边界 | 已补，并升级为完整中介分析（见必做 1 / §6.3ter） | ✅ |

白盒 9/10 存在性矩阵已回填 §6.2 表 1b + §6.3.3（urgency 8/9 数学格显著，唯一 null=Llama/DROP）。

---

## 3. 命名与定位（原 Open Items，已按作者决定处理）✅

- **#2 标题**：✅ 已改。新标题=**"情绪诱导不忠实（EIU）：用户情绪影响下的推理不忠实的刻画与防御"**。去掉"因果""升级放大"等过强措辞，采用中性的"影响下……的刻画与防御"，既标识新现象又不过度断言方向与强度；正文中"情绪因果导致不忠实"类措辞（因果性地提高/放大、因果论证弧线、因果放大因子）一并软化，仅保留忠实性定义本身的"因果支撑/因果承载/非因果解题容量"用法，与"描述性分解、非完整因果识别链"口径一致。
- **#6 防御命名/定位（原 SR-Guard → EAL-Guard）**：✅ 已处理（改名 + 消除过度声称）。防御更名为 **EAL-Guard**，"EAL"取自验证器所用的核心特征**情绪注意力泄漏（Emotion-Attention Leakage）**（§3.3/§5.2），**不再**暗示估计任何"捷径储备（SR）"标量——验证器预测的是每个候选的 **CBU/忠实性风险**，选择由"自洽多数门控 + 忠实性加权投票 + 该风险排序"完成（§3.4 已加命名说明；三幕叙事从"利用 SR 机制防御"改为"保留情绪输入的验证器引导忠实性选择"）。若审稿偏好，可读作 Faithfulness-Guard。

---

## 4. 关键实测数字速查（供 rebuttal 正文引用）

- **EIU 存在性（GSM8K both-correct CBU 净增）**：Qwen **+18.4pp**（p<.001）、Gemma **+17.3pp**（p<.001）、Llama **+8.0pp**（tok768 canonical，p=.027）；9 个模型×任务单元格中 **7 格显著/边际**，两个稳健 null 均位于 DROP。
- **混合效应 logistic（collider-safe 联合率 $P(\text{correct}\wedge\text{unfaith})$）**：7 格干净重估池化 urgency **OR=2.03**（聚类稳健 [1.78,2.31]，**p=2.4e-26**）–2.99（GLMM）；条件-CBU 口径 OR=3.08/5.33 因 collider（条件化于处理后变量“正确”）降为补充。
- **中介分解**：Qwen 纯中介（NIE≈+0.30，NDE<0）；Gemma 纯直接（NDE +0.28~+0.51）；非速度情绪直接效应显著。
- **探针**：EAL 向量 CV Qwen **.763** / Llama **.746** / Gemma **.766**（无挑层、GroupKFold 稳）；控制长度后增量效度三模型一致显著（嵌套 LRT $\chi^2$=18.2/16.5/7.0、偏相关 ρ=.30/.26/.21，均 p<.01）；语义熵/SelfCheckGPT 以答案层不确定性为目标，与"答案正确"的 CBU 目标错配。
- **防御（EAL-Guard，严格三折 val 标定）**：以综合失败率 $P(\text{答错}\vee\text{不忠实})$ 计，9 格中 **8 格为可部署方法最低**；双轴（acc↑∧CBU↓）6/9；**3 格 ΔCBU 显著**（最强 Qwen/SVAMP −.227，p<.001；Qwen/DROP p=.041；Gemma/DROP p=.003）；唯一 tf 例外=Llama/SVAMP（候选池缺低-CBU 候选，oracle 仍 .415）。
- **等算力**：双源≈单源（无显著差异）。
- **辨识**：纯情绪臂无具体答案可迎合但 CBU 仍升（决定性证据）；温和 SYCON target-FlipRate≈0；控 ELEPHANT 分后情绪净效应不变。（exp0045 强反驳压力测试：flip 45 项 0 正显著、target-capit 45 项 4 名义显著但全不过 BH、方向异质，已移出正文，见 `experiments_emotion.md §8`。）

---

## 5. 尚在跑 / 待回填清单（Tracking）

1. （可选）操纵检验的人工二次标注 + Cohen/Fleiss $\kappa$，若审稿坚持人工核查再补。
2. （英文稿 `draft_emotion.md` 尚未同步中文稿的标题/因果软化/EAL-Guard 命名说明——按作者指示暂缓。）
3. **✅（已完成）EAL-Guard 评测协议消除操作点泄漏（E8）**：此前 `exp0031` 验证器无泄漏（train 折拟合），
   但 guard 门控阈值 operating point 取自 **test 折风险分位数**（温和操作点泄漏——用 test 风险分布、非 test
   标签）。**已改为 train 拟合 / val 标定阈值 / test 纯评测三折协议**（新增 `--train_frac/--val_frac`），并
   导出 `verifier_features.npz` 支持离线换折/换阈值/消融。**9 个防御 cell 已在该协议下重跑完成**（结果即
   §5.3 表 4，train120/val60/test120）。**实际结果**：test 折缩小后严格"帕累托支配"并不普适，据此主指标
   改用综合失败率 $P(\text{答错}\vee\text{不忠实})$（8/9 为可部署方法最低），比"支配格数"更诚实、也直接回应
   审稿 #8/#9。完整过程见 `EIU_experiment_checklist.md §8.5`。
4. **（存在性数据卫生·待 GPU 重跑）DROP 提示截断 + Gemma 坏 filler**：代码审计发现两处存在性实验隐患——
   (a) `exp0015._greedy` 对 prompt 做 `truncation=True, max_length=max_seq_len(默认2048)`，**DROP 长阅读理解
   段落 + 3 轮对话易超 2048 → prompt 被静默截断、任务损坏**，很可能是 Llama/DROP null 的真凶、也污染
   Gemma/DROP（修：`--max_seq_len 4096`；`diag_llama_power.py` 已加 prompt 长度检查直接量化）；
   (b) Gemma SVAMP/DROP 现有数(+0.513/+0.320)用的是**旧坏 filler**（同 GSM8K 8% 崩溃 bug），需用新 filler 重跑；
   (c) DROP 应显式 `--split validation`（默认 test 会回退到 train）。待跑：Gemma×{SVAMP,DROP}、Llama×DROP
   （均 `--max_new_tokens 768 --max_seq_len 4096`，300 样本）；GSM8K 存在性列回填成 fast300（Qwen +0.184 /
   Gemma +0.173 / Llama borderline，去掉旧 Gemma +0.300 崩溃基线伪影）。**Llama/GSM8K 已 768 重跑=真 borderline，
   不再折腾**。完整命令与诊断见 `EIU_experiment_checklist.md`。
5. **（代码审计 2026-07-16，纯 CPU）** #13 `mixed_effects_cbu.py` / #1 `mediation_urgency_brevity.py`
   实现正确（唯一澄清=结果变量均为**联合率**、collider-safe，已回填 §5.2/§5.3）；SYCON `sycon.py` 无 bug。
   **社会迎合辨识 `aggregate_elephant.py` 发现并修复 se bug**：固定效应去均值 OLS 的 se 未扣除 G 个组自由度、
   也未按题目 cluster-robust → **低估 se、夸大"控制 social 后 emotion 仍显著"**；已改为**按题目 cluster-robust
   三明治 + dof 校正**（合成数据验证 se 变大）。辨识结论**不依赖**此 se（靠 social 率 0.06 + corr≈0 + 加 social
   后 emotion 点估稳 + social 系数≈0 + §5.2 独立显著），draft §5.4 已改 robust-evidence-first。**待 CPU 重跑
   `aggregate_elephant.py`** 取精确 cluster-robust CI 回填。详 checklist §8.6。

> 6 项必做实验的**自动/统计部分已全部完成**：#1 中介、#7 探针去泄漏、#9 等算力、#11 exp0045 剂量—反应+配对、#12 操纵检验收紧构念 v2（360 候选，双泄漏=0、cat-acc 0.90–1.0、强度全单调）、#13 混合效应。#11 因两口径均无 BH 后稳健正向放大，作为探索性测试移出正文（`experiments_emotion.md §8`），不挑取名义显著子格。


---

# 第二轮（Round 2）· 2026-07-19

> **来源**：Weak-Reject（评分 4/10，置信度 4/5）。14 条主要问题 + 12 条次要问题 + 6 项必做修改 + 一节"可接受的收缩版主张"。
> **本轮四个最严重问题（审稿人自述）**：(i) 存在性主结果的 both-correct 配对分析在正文仍标〔待回填〕；(ii) AOC 方向定义自相矛盾；(iii) urgency 仍是情绪与速度任务指令的复合操纵；(iv) AUROC 与防御基线比较可能存在技术性错误。
> **交叉引用**：checklist §7（2×2 + 中介）、§8.2（探针 vs 长度公平口径「重大更正」）、§8.5（E8 三折隔离）、§8.6（脚本审计）；主实验 E1（both-correct 配对 McNemar）。
> **状态标记**：✅ 已完成/已更正（本轮回填正文）｜🟡 代码就绪·进行中/待跑｜🔵 待作者定稿决策。

## 0. 总回应（Overview）

感谢审稿人极为细致、切中识别力要害的复审。我们把意见归为四类并据此行动：
- **(A) 对账滞后**——若干 R1 已完成的分析在正文仍标〔待回填〕或沿用旧口径（both-correct 配对 McNemar、探针 vs 长度公平口径），本轮**回填并统一**，如实说明"已做、未同步"，不假装重做；
- **(B) 硬错误**——AOC 方向定义自相矛盾，**如实承认**、统一符号、给一页 metric spec 并全量重算；
- **(C) 识别力不足**——情绪×速度正交、验证器三级隔离、探针泄漏迁移、防御公平算力/基线、Pareto 统计化，给出设计与进度，**未完成处标〔计划〕并给命令，绝不预填**；
- **(D) 定位/命名**——SR 降级为 Discussion 待检验假说、方法拟改名、标题"升级"软化，按审稿建议处理。

一贯原则不变：**未实测不预填、负面结果照报、审稿指出的错误如实更正而非辩护**。

## 1. 六项必做修改（逐项）

### 必做 1 · 固定 both-correct 的配对存在性分析 —— 🟡 分析已在 E1 完成，本轮回填主表（此前对账滞后）

审稿人正确指出 correct-only 条件率有 selection/collider bias。**澄清并致歉**：主实验 E1 **本就**在"filler 与 emotion 均正确"的固定题集上做**配对 McNemar**，报告的 `net ΔCBU` 正是审稿人要求的
$P(f_e{=}\text{unfaith},f_0{=}\text{faith}) - P(f_e{=}\text{faith},f_0{=}\text{unfaith})$：

| 模型 | both-correct n | net ΔCBU（配对净转移） | McNemar p | BH q |
|---|---:|---:|---:|---:|
| Qwen2.5-7B | 245 | **+0.184** | 0.0000 | 0.0000 |
| Gemma-2-9B | 255 | **+0.173** | 0.0000 | 0.0000 |
| Llama-3.1-8B | — | +0.080 | 0.027† | — |

（†Llama borderline；此处采用 **draft 表 1 的 tok768 canonical 口径 +0.080/p=.027**；checklist E1 早期 pre-tok768 记为 +0.071/p=.0599，已在 `EIU_claim_evidence_map.md` §F 标记待同步。）正文主表此前仍标〔both-correct 待回填〕属**对账滞后**（数据在 checklist E1、未同步进正文），非未做。**本轮**：主表同时给出三口径——(i) 联合率 $P(\text{correct}\wedge\text{unfaith})$（混合效应 GLMM，**collider-safe**：不条件化于处理后变量"正确"，§5.2 已述；池化 OR=2.03（聚类稳健 [1.78,2.31]）–2.99（GLMM），p=2.4e-26，7 格干净重估、已剔坏 Gemma，详 draft §5.1）；(ii) 条件率 $P(\text{unfaith}\mid\text{correct})$（仅作对照并标注选择偏差风险）；(iii) **上表固定 both-correct 配对净转移**。**主结论改述为基于 (iii) 配对净转移**，不再以条件比例之差作主张。若审稿仍有顾虑，我们额外报告 filler/emotion 各自的 faithful↔unfaithful 2×2 转移矩阵。

### 必做 2 · 统一 AOC/CBU 定义并全量重算 —— ✅ 已闭环：现稿附录 D 方向本已统一，本轮补集中带伪代码 metric spec（附录 D.0）+ 辅助脚本注释已对齐

**如实承认**：统计附录对 AOC 的两处定义方向相反（"越高越不忠实" vs "faithful 比例均值、越高越 faithful"），是硬错误。**修正**：
- 统一单一约定：$\mathrm{AOC\text{-}faithful}\in[0,1]$，**越高越忠实**（= 各截断点"答案随删/改而改变"的比例均值：答案变→该步 CoT 被反事实依赖→忠实）；"不忠实度" $=1-\mathrm{AOC}$；CBU $=\text{correct}\wedge(\text{unfaithful 阈值})$。
- 提供**一页 metric specification**：原始 early-answering/intervention 截断结果 → 逐截断点 faithful 判定 → AOC → 二值 unfaithful 阈值 → CBU 的精确定义 + 伪代码。
- **全量重算核对**：所有表格效应方向、OLS 系数符号、"三指标一致"主张、CBU 阈值、SR 的 CoT-dependency 定义统一到该约定。**诚实预判**：数值计算全程用同一套代码方向（矛盾出在附录**文字描述**而非计算），重算主要是核对符号一致性并更正文字；但我们会逐项验证后如实报告，若发现任何数值方向错误一并更正。
- 〔可即刻做，纯文档，不占 GPU〕

### 必做 3 · 情绪 × 速度正交操纵 —— 🟡 已有初版 2×2（有缺陷），本轮重做干净版 + 收缩表述

**已有**：checklist §7 的 2×2（情绪∈{中性,情绪} × 速度∈{无速度,加速度}，三模型 GSM8K）+ §7.5 中介分解。
**承认审稿人两点完全正确**：
1. **prompt 里的显式速度/简洁要求是 co-treatment，不是 post-treatment 中介**；事后对**生成长度**做 NDE/NIE 分解，无法识别 prompt 文本里究竟是哪个语义成分（情绪状态／时间约束／快答指令／简洁度／高唤起／社会压力）造成效应。我们此前混用了"生成长度中介"与"prompt 速度成分"两个不同变量。
2. 现有 2×2 的 `emotion_nospeed`（"焦虑但别急、慢慢来"）**语义不自洽**、`speed` 臂**过猛**（Gemma emotion_speed CoT=10.7 token，近乎无推理），不足以作主证。
**本轮行动**：
- 重做**干净 prompt 层面 2×2**：情绪表达{有/无} × 速度要求{有/无}，**关键新臂 = "情绪强烈但明确说明不用赶时间、请正常完整推理"**；对四格做操纵检查确保情绪强度/唤起匹配。〔计划，需 GPU〕
- **收缩表述**：删除"纯直接／纯中介／速度归约被证伪"等过强语言（§7.5-D 亦承认机制异质）；改述为"在若干单元存在**非速度**情绪效应（不含速度语的信任／失望／愤怒臂显著、Gemma 直接通路），但**不主张所有 urgency 结果都是纯情绪效应**——urgency 是情绪与速度指令的复合操纵，其纯情绪分量须经干净 2×2 才能识别"。中介分解降级为**探索性、依赖 sequential ignorability** 的辅助分析。

### 必做 4 · 验证器严格独立 train/val/test —— ✅ 完成（三折隔离 + 9 格 val 标定重跑已回填正文表 4）

回应审稿 #9 全部子问（E8，checklist §8.5）：
- **训练题**：验证器只在 **train 折**（all-arm、correct-only）拟合；steering 方向也只在 train 折提取。
- **是否同一 200 题**：否——改为 **train/val/test 三折**（默认 0.4/0.2/0.4），三折**按题目隔离**。
- **候选池是否参与调参**：否；候选池只在评测期生成，不回流训练。
- **阈值/工作点**：在 **val 折**标定（default 风险分位数）；**test 折从不参与任何阈值/层/特征选择**。
- **AUROC 口径**：test 折 out-of-fold；导出 `verifier_features.npz`（RAW 特征 + split + label）使换折/换阈值/消融**离线零 GPU 重跑**。
- **进度**：✅ 9 格 val 标定重跑**已完成**并回填正文表 4（train120/val60/test120，阈值取自 **val** 风险分位、test 折只评估一次）。**R3 版本**：R2 时曾为 test 标定操作点近似，**现表已改 val 标定、该近似已消除**，§5.3 已诚实声明"温和操作点近似、仅用风险分布不含 test 标签"（该 R2 caveat 现已过时）。实际结果：严格三折下帕累托支配并不普适，据此主指标改用综合失败率 tf（**8/9 为可部署方法最低**），如实报告、不掩盖唯一例外 Llama/SVAMP。

### 必做 5 · 修复 AUROC 基线与 S2A —— AUROC 基线 ✅ 已自查更正；S2A 🟡 待重做

**5a AUROC 基线（✅ 已更正，checklist §8.2「重大更正 2026-07-16」）**：审稿人 #7 **完全正确**。此前把 generation length 记作 "0.21–0.28 ≤ 随机"，是**误用原生反方向** $\mathrm{AUC}(\text{glen},y)$（长度与 CBU 负相关：越短越 CBU → 原生 AUC<0.5，真实判别力 $=1-\text{该值}$）。**公平口径**（同 GroupKFold、折内 OOF-logistic 学方向）实测：**长度 AUROC = Qwen 0.753 / Llama 0.705 / Gemma 0.785，与内部探针相当（CI 大幅重叠）**。据此**收回"探针 ≫ 长度"**；改述为"探针与长度**共同** ≫ 一致性/采样检测器"，探针相对长度的**真实增量 =（a）生成前预警、（b）机制特异"。
- 对 semantic entropy / SelfCheckGPT：它们是**采样一致性/自一致**检测器，度量**答案层不确定性/幻觉**，而 CBU 恰是**答案正确**的样本（答案通常一致、低不确定），二者**目标错配**——这是结构性原因，非仅经验偏弱。正文已**移除其 AUROC 数字**（原 .41/.38 借自 CDDP-R GSM8K、非 EIU 自跑，且原始方向 <0.5、翻方向≈.59/.62，写"≤随机"会误导），改用目标错配论证；**EIU-原生方向校准重跑**（折内 $\max(\mathrm{AUC},1{-}\mathrm{AUC})$ 诊断 + 核对 score orientation）留作后续，若校准后显著则照改结论。〔待 GPU〕
**5b S2A（🟡 承认可能是实现失败）**：审稿人 #10.2 正确。S2A 原意是**重写上下文、保留任务相关信息**，不是删除 DROP 阅读理解题面。行动：展示 S2A 完整 prompt、人工审计重写后 passage、确保 DROP 题面/证据被保留、改用接近官方的实现重跑；**在修复前不把 "S2A 在 DROP 崩到 0.14–0.32" 用作"不应去情绪"的证据**。

### 必做 6 · Shortcut Reserve 去留 —— 🔵 接受降级，方法拟改名（待作者最终定稿）

接受审稿人判断：否证若干代理 ≠ 证明潜变量 SR 存在，更未证其"门控"EIU。**行动**：
- SR **降级为 Discussion 的待检验假说**，从**标题、贡献、方法机制**中移除"SR 已确立/门控"表述；
- **删除"储备门控规律"**，直到取得"独立、预处理前测得的 SR 对未见 $\Delta$CBU 的预测"；
- **方法改名**：拟改 **Faithfulness-Guard / Verifier-Guided CoT Selection**（防御确只用"多数答案约束 + CBU 验证器重排"，不估计/不使用 SR）。〔已定稿：改名为 **EAL-Guard**（EAL=情绪注意力泄漏特征，§3.3），不再使用 SR-Guard〕

## 2. 十四条主要问题（逐条；与必做重叠者指引）

1 → **必做 1**。2 → **必做 2**。4 → **必做 3**。6 → **必做 6**。7 → **必做 5a**。9 → **必做 4**。

**#3 情绪"升级"未被证明**：**接受**。强度斜率不显著、abrupt/monotone 无稳健差异（已如实报）；现有证据支持"多轮情绪化 framing 与扰动式 CoT 依赖变化相关"，**不支持"升级过程本身"引发不忠实**。行动：默认**改标题/摘要**去"升级/放大/轨迹"，改为"用户情绪化 framing 与思维链不忠实的关系"；matched-endpoint 实验（全程高/渐升/仅末轮/回落但末轮文本匹配）作为可选加分实验〔计划〕。

**#5 扰动式指标不支持"CoT 不再是原因"**：**接受**。全局收缩"CoT 并非答案真正原因/推理链丧失因果作用/中段因果支撑被腐蚀/证据约束转向情绪迎合"→ **"对可见 CoT 前缀/关键步骤的反事实依赖下降"**。承认第二个"独立"度量（改 CoT 后重生成）与 early-answering **共享构念局限，不再称正交验证**。

**#8 GroupKFold 未解决真正泄漏**：**接受，且是重要问题**。承认"每题一行→GroupKFold≡分层"恰说明按题分组没加保护；EAL（"生成 token 对情绪 token 注意力比例"）可能学的是**情绪模板类别→平均 CBU 风险**而非逐实例忠实性。补充实验（`probe_features.npz` 已存，(a)(c)(d) 可离线；(b)(e)(f) 需按条件重切）：(a) **leave-one-template-family-out**；(b) **train urgency / test high-trust、disappointment**（跨情绪迁移）；(c) 每个情绪条件**内部**预测 CBU；(d) **residualize** emotion label / prompt length / gen length 后再预测；(e) 跨数据集迁移；(f) 跨模型迁移。**在此之前收缩** "忠实性特异/跨模型稳健" → "同模型同任务交叉验证内可预测；当前三模型为各自训练各自 CV、非迁移"。〔计划〕

**#10 防御比较不公平**：(10.1) 报**同 forward/token 预算曲线** $N\in\{1,2,4,6,8,12\}$ 下所有方法的 acc/CBU〔计划 GPU〕；(10.2) 见必做 5b；(10.3) 增**同目标基线**——self-consistency+random selection、self-consistency+校准长度、process reward model、critique-and-revise、faithfulness-aware verifier BoN、**同候选池同算力的独立 reranker**〔计划〕。

**#11 Pareto 支配仅点估计**：**接受**"点估计支配无统计意义 + 各方法分母不同"。exp0031 已含 **paired bootstrap（n_boot=2000）**。本轮报：**同题 paired bootstrap 支配概率**、joint confidence region、**固定 both-correct 分母**（前期已做 both-correct 匹配口径）、或联合风险 $L=\lambda\cdot\text{error}+(1-\lambda)P(\text{correct}\wedge\text{unfaith})$。**"6/9 严格支配"改述为 bootstrap 支配概率**，不作点估计结论。

**#12 "保留情绪"未被测量**：**接受**。承认"保留情绪"目前**只指未删 prompt token**，非行为适应；且 system 指令"勿让语气影响推理"可能抑制合理适应。行动：加独立 judge/人评——emotional appropriateness、helpfulness、empathy、brevity appropriateness、task utility〔计划〕；在此之前**收缩主张**为"保留情绪**输入**"，不称"保留情绪**适应**"。

**#13 操纵检验缺人工 + 构念事后收紧**：**部分接受 + 诚实更正**。承认全 LLM-judge 不足；"收紧 stance 构念后泄漏归零"**不能证明**原高泄漏是假象，只能说明换定义得零——但 **answer leakage 在任何构念/判官下均为 0**（这是硬的、决定辨识）。补 **2–3 名盲标 + Cohen κ / Krippendorff α**，标注 emotion category / intensity / valence / arousal / urgency / brevity / authority / politeness / threat / answer-stance / new-info。〔计划，人工〕

**#14 对账未完成**：**接受**。制作 **claim-to-evidence 表**（摘要数字 ↔ 主表 ↔ 附录 ↔ rebuttal ↔ 代码产物 ↔ 未完成项逐条对齐）；清理正文全部内部备注、脚本路径、"旧图不得引用"提示、living-doc 状态；所有〔待回填〕要么回填、要么显式标"未完成、不支撑主张"。〔可即刻做，纯文档〕

## 3. 十二条次要问题（逐条）

1. 恒等式不能区分机制 → 接受，"坐实放大非制造"改为仅描述、不称机制识别。
2. 非零基线拒 $H_0$ 意义有限 → 接受，报效应量级 + 精度（net ΔCBU 及 CI），不强调"显著大于零"。
3. Mistral 排除 = post-hoc → 接受，给**预定义能力阈值**（如 GSM8K forced-faithful 下最小忠实性动态范围）并完整报负结果。
4. GLMM 只 7 格 → 说明：Qwen/Llama 的 GSM8K 记录目录无 `emotion_records_*.json`，池化用 7 格；本轮补齐或在正文明确排除的 2 格及原因。
5. Bayesian GLMM 与频率 p 混写 → 接受，分开表述：GLMM 报后验/CrI，聚类稳健报频率 p/CI，不混。
6. 缺 emotion×model×dataset 交互 → 接受，池化模型加交互项，异质下不只报单一 OR。
7. DROP CBU≈0.8 需人核查 → 接受，人工核查 DROP 答案抽取 + 忠实性标签，排除阅读理解格式伪影。
8. "情绪重写不变性"≠ CoT faithfulness → 接受，改述为"答案/辩护对 prompt 改写的稳定性"。
9. EAL 命名带机制含义 → 接受，attention intervention 之前改称 **emotion-attention feature**。
10. EAL 白盒不适用闭源 → 接受，明确适用范围 = 白盒；闭源见 checklist §6（黑盒 DeepSeek 前缀唯一干净格，其余支撑"需内部访问"边界）。
11. 防御实现描述不一致（全池 vs 只对高风险触发一次）→ 接受，统一为"每题生成候选池 + 高风险时验证器重排"的准确描述，删矛盾表述。
12. 标题/摘要概念过载（EIU/CBU/SR/EAL/CDDP-R/SR-Guard）→ 接受，精简；SR 降级后自然减负。

## 4. 关于"可接受的收缩版主张"（回应第五节）

我们**接受审稿人给出的收缩版**作为当前证据下的稳妥表述，并据此**立即收缩**以下主张（不等新实验）：动态情绪**升级**效应、已隔离**纯情绪**因果、CoT 内部因果作用**已被证明**丧失、SR 是**已确认**门控机制、EAL 跨任务跨模型**忠实性特异**、SR-Guard **统计意义** 6/9 严格支配、SR-Guard **既保留情绪适应又只作用忠实通道**——全部软化或移除。完成必做 1/3/4/5 后，再逐步恢复"可被设计唯一识别"的主张。

## 5. 本轮待完成清单（诚实追踪）

- **可即刻做（文档/CPU，不占 GPU）**：AOC/CBU 一页 metric spec + 全量符号核对（必做 2）；both-correct 三口径回填主表（必做 1）；claim-to-evidence 对账表 + 清理正文备注/旧图提示（#14）；`aggregate_elephant.py` cluster-robust CI 重跑（§8.6）；离线泄漏控制 (a)(c)(d)（#8，用已存 `probe_features.npz`）；全基线 AUROC 方向校准重算（必做 5a）。
- **进行中（GPU）**：E8 九格 val 标定重跑（必做 4）。
- **需 GPU 新实验**：干净情绪×速度 2×2（必做 3）、S2A 修复重跑（必做 5b）、防御算力曲线 $N\in\{1..12\}$ 与同目标基线（#10）、跨情绪/数据集/模型迁移 (b)(e)(f)（#8）、保留情绪 judge 评价（#12）、matched-endpoint 升级实验（#3，可选）。
- **需人工**：操纵检验 2–3 盲标 + κ/α（#13）；DROP 标签人核（次要 7）。
- **作者决策**：SR 改名与降级（必做 6）、标题去"升级/放大"（#3）。

> 本轮定位：R2 的四大硬伤中，**#2（AOC 定义）、#1（both-correct 回填）、#7/必做5a（AUROC 基线）** 属可即刻闭环（文档/离线）；**#3（情绪×速度）、必做4（verifier holdout）、#10（防御公平）** 需 GPU，按上表排队。收缩版主张先行落地，确保正文不含"设计无法唯一识别"的过强表述。


---

<!-- ============================================================
     下一轮（Round 3）从这里开始：新增一个 `# 第三轮（Round 3）· YYYY-MM-DD` 分节，
     并在顶部「轮次索引」补一行。历史轮次不要删除。
     ============================================================ -->


---

# 第三轮（Round 3）· 2026-07-20

> **定位：当前权威回复。** 本轮将文献创新性检索、正文最终数字与诚实边界统一到同一口径。Round 1/2 保留作修改轨迹；其中 OR=3.08/5.33、Llama/GSM8K +.071、Gemma 旧 filler 结果、“纯中介/纯直接”及统计 Pareto 支配等历史表述，均由本轮覆盖。

## 0. 总回应（Overview）

感谢审稿人促使我们区分“情绪影响 LLM”“CBU 现象存在”和本文真正研究的交叉问题。我们重新检索截至 2026-07-20 的 arXiv、ACL Anthology 与 OpenReview，并据此收缩创新性：**CBU、情绪提示、隐状态探针、自洽选择和验证器重排序均已有先例；本文只主张用户情绪诱导 CBU 这一交叉问题为据我们所知的首次系统研究。** 同时，我们把主存在性结果改为 fixed both-correct 配对净转移，正文内给出完整 AOC/二值标签定义，将 urgency 明确为情绪与速度的共处理，并把防御支配限定为点估计。

## 1. 创新性：首创的是“用户情绪诱导 CBU”，不是组成概念

### 1.1 可辩护的首次主张

检索中未发现一篇既有论文同时满足以下四项：

1. 操纵**用户表达的情绪**，而非待识别情绪、外部偏置或答案立场；
2. 任务内容、正确答案与用户显式答案立场保持不变；
3. 最终答案仍正确；
4. 以 early-answering、关键步骤干预等反事实/扰动方法度量 CoT 忠实性下降。

因此，论文只采用带限定语的主张：

> **据我们所知，本文首次在任务、正确答案与用户显式立场不变的受控多轮设置中，系统研究用户情绪是否诱导“正确但不忠实”的大模型推理。**

> *To the best of our knowledge, this is the first systematic study of whether user-expressed emotion induces correct-but-unfaithful LLM reasoning in controlled multi-turn interactions with the task, correct answer, and explicit user stance held fixed.*

“首次”是检索支持的定位而非形式证明；正文不使用无保留的 *We are the first*。

### 1.2 不主张为首创的内容

- **情绪影响 LLM 表现并非首创。** EmotionPrompt、NegativePrompt、EmoBench、情绪强度与 first-person framing 已研究准确率、truthfulness、toxicity、生成质量或迎合行为。
- **CBU 现象并非首创。** Turpin 等揭示 CoT 对真实偏置的掩盖；Lanham 等用 early-answering、错误注入与改写测试 CoT 依赖；Paul 等研究中间步骤的因果中介作用；Yee 等的 *Dissociation of Faithful and Unfaithful Reasoning in LLMs* 直接研究无效/错误推理与正确答案的分离。
- **算法组件并非首创。** 隐状态探针、attention 特征、自洽、Best-of-N 与 verifier reranking 均有先例；方法贡献是把它们组织起来检测和缓解 emotion-induced CBU。

### 1.3 与最邻近工作的实质区别

| 邻近工作 | 操纵与结局 | 与本文的关键区别 |
|---|---|---|
| *How Reasoning Mitigates (Yet Masks) LLM Sycophancy* | 显式偏好答案/权威立场 → 同意或翻转 | 模型有具体答案可迎合；未在纯情绪、无答案线索条件下用截断/关键步干预测 CBU |
| 医疗视觉模型的 emotional-appeal sycophancy | 情绪/社会诉求使正确诊断转错 | 属于准确率可见的证据失效；本文研究答案仍正确时的过程失效 |
| *The Chain Holds, the Answer Folds*（2026） | 多轮压力下 CoT 保持正确、答案翻错 | 它是“chain holds, answer folds”；本文是“answer holds, but the visible chain matters less under perturbation” |
| Turpin / Lanham / Paul / Yee 等忠实性工作 | 偏置、截断或步骤干预 → faithful/unfaithful reasoning | 已建立 CBU 构念与工具，但未把用户情绪表达作为立场恒定的诱导变量 |

相应地，我们删除“首次研究情绪影响推理”“首次发现 CBU”以及“首次提出探针/验证器”的表述，只保留上述交叉创新。

## 2. 核心结果与审稿硬问题的当前口径

### 2.1 fixed both-correct 主分析：固定配对分母，并以联合结局补充

主结论在 filler 与 emotion **均答对**的固定题集上使用同一配对分母，避免两个条件因使用不同 correct-only 分母而产生的机械差异。该分析描述的是 both-correct 主层，不声称已完全解决对处理后正确性的条件化；§2.4 另以不条件化于正确性的联合结局 $P(\mathrm{correct}\wedge\mathrm{unfaithful})$ 补充：

- Qwen/GSM8K：忠实→不忠实 / 反向转移 = **56/11**（$n=245$），净增 **+.184**，精确 McNemar $p<.0001$；连续 AOC-faithful 同向下降 **.107**。
- Gemma/GSM8K：**57/13**（$n=255$），净增 **+.173**，$p<.0001$；连续 AOC-faithful 同向下降 **.088**。
- Llama/GSM8K：采用 tok768 canonical 口径 **+.080, $p=.027$**；不再混用早期 +.071/$p=.0599$。

跨 Qwen/Llama/Gemma × GSM8K/SVAMP/DROP 九格，urgency 为 **7 格显著/边际、2 格稳健 null（Llama/DROP、Gemma/DROP）**。连续 AOC 与二值 CBU 同向，主结论不依赖单一二值化。

### 2.2 AOC 与二值 CBU 定义已在正文自足闭环

对保留比例 $r\in\{0,.25,.50\}$ 的 CoT 前缀重新作答。若 $a_r\neq a_{\mathrm{full}}$，该截断点记 faithful=1；
$\mathrm{AOC}_{\mathrm{faithful}}=\frac13\sum_r\mathbf1[a_r\neq a_{\mathrm{full}}]$，**越高越忠实**。二值 `cf_faithful` 直接取最接近 50% 的单个截断点，**不是对连续 AOC 设阈值**。本文把它称为扰动式反事实依赖代理，不再声称已经恢复模型内部真实因果过程。

### 2.3 urgency 是情绪与速度共处理；长度中介只作探索性分析

我们接受审稿人的识别批评：urgency prompt 同时包含情绪表达与显式速度诉求，两者在该条件内部没有被正交分离。生成后的 CoT 长度是处理后变量，可探索“较短输出”通路，但不能把 prompt 中的情绪份额与速度份额拆开。因此正文已删除“纯速度被证伪”“纯中介/纯直接”等过强表述。

仍有两组独立正向证据说明现象**不限于含速度语的提示**：

- 无速度语 high_trust 在 GSM8K 上使 Qwen/Gemma CBU 净增 **+.078/+.089**（BH $q=.013/.004$）；
- Llama/SVAMP 的 high_trust/disappointment/anger 各约 **+.29**，探索性 NDE 为 .24–.26。

这些结果支持“非速度情绪条件也可出现 CBU 上升”，但不反向识别 urgency 的纯情绪分量；干净的情绪×速度 2×2 仍是未完成项。标题中的“升级”仅描述预设的多轮情绪轨迹；强度斜率与波动方式未见稳健差异，因此我们不主张升级过程本身已被独立识别。

### 2.4 重复测量与池化效应

联合结局取 $P(\mathrm{correct}\wedge\mathrm{unfaithful})$，避免条件化于处理后的正确性。完成统一校准并进入池化的 7 个单元格（当前分析 $n=3000$）中，urgency OR=**2.03**（按题聚类稳健 95%CI [1.78,2.31]，$p=2.4\times10^{-26}$）／**2.99**（题目随机截距 GLMM；区间以较保守的聚类稳健结果为准），逐格 6/7 显著。旧 OR=3.08/5.33 来自旧数据口径，不再支撑现稿。

### 2.5 检测贡献的收缩定位

EAL 在三个模型分别训练、分别交叉验证时 AUROC=.763/.746/.766（群组置换 $p=.002$）；Qwen/Gemma 的 prompt 残差可在生成前预警（.776/.810），Llama 残差不显著。公平方向校准后的长度基线为 .753/.705/.785，与内部探针相当。因此我们不再主张“探针优于长度”，只主张：(i) 生成前预警在部分模型成立；(ii) 情绪注意力特征提供机制相关信号，且**控制生成长度后仍具三模型一致显著的增量效度**（嵌套 LRT $\chi^2$=18.2/16.5/7.0、偏相关 ρ=.30/.26/.21，均 p<.01；ΔAUROC 因与长度共线而含 0，属功效不足而非无信息）；(iii) 语义熵与 SelfCheckGPT 以答案层不确定性为目标，与“答案正确”的 CBU 目标错配、不适于本设定。三模型结果是各自模型内 CV，不是跨模型迁移。

### 2.6 防御结果只作点估计，S2A 不作一般性反证

当前表 4 中，SR-Guard 在 9 格的点估计均位于 (acc↑, CBU↓) Pareto 前沿，6 格点估计严格支配四个外部基线；**尚未获得 paired-bootstrap 支配概率或联合置信域，因此不作统计支配结论**。验证器在 train 折拟合，但本版门控点取自无标签 test 风险分位数；严格 train/val/test 的 9 格重跑尚未全部纳入正文。

S2A 在当前 DROP 实现中的低准确率可能源于重写误删题面，只说明该实现失败，不外推为 S2A 范式一般失败。所谓“保留情绪”也只指输入 token 未被删除，尚未验证简洁度、共情、帮助性等合理行为适应。

### 2.7 EIU 与 sycophancy 的辨识

纯情绪臂不给出任何候选答案，CBU 仍上升；温和 SYCON 的 FlipRate≈.01；urgency 臂 ELEPHANT 社会迎合率为 0，其他情绪臂仅 .10–.13，且与 CBU 的相关约 .02–.05。控制社会分后情绪系数点估基本不变、社会分系数约为 0。我们据此只主张现有 CBU 变化不能由答案层迎合充分解释，不声称已穷尽所有社会压力机制。

## 3. 正文已同步的位置

| 正文位置 | 本轮修改 |
|---|---|
| 摘要、引言、贡献、结论 | 统一为带 *据我们所知* 的 emotion→CBU 交叉首创；明确 CBU 与情绪效应本身已有工作 |
| §2 相关工作 | 增加 Yee、*How Reasoning Mitigates (Yet Masks)*、医疗 emotional appeal、*The Chain Holds, the Answer Folds* 的正面区分 |
| §3.2 / §4 | AOC 越高越忠实；二值标签取 50% 单截断点、非 AOC 阈值 |
| §5.1 | fixed both-correct 转移计数、McNemar 与连续 AOC 同向证据 |
| §5.4 | urgency 共处理边界、探索性长度中介、无速度语情绪正向证据 |
| §5.3 / 图 3 / 结论 | Pareto 限定为点估计；S2A 限定为当前实现；“保留情绪”限定为保留输入 |

## 4. 仍未完成、因此不用于恢复强主张的项目

1. 干净的 prompt 层情绪×速度 2×2；
2. SR-Guard 九格严格 train/val/test 标定结果；
3. 同题 paired-bootstrap Pareto 支配概率与联合置信域；
4. 修复版 S2A 与完整题面保留审计；
5. leave-template-family-out、跨情绪/数据集/模型迁移；
6. 人工操纵检验与情绪行为适应评价；
7. SR 联合操作量对未见 $\Delta$CBU 的正向预测。

这些项目在完成前均只作为限制或未来工作，不支撑摘要和贡献中的强断言。

## 5. 当前结论

修订后的论文不再依赖“情绪影响推理”“CBU 存在”或单个算法组件的新颖性，而聚焦一个可被现有三条文献线清楚切开的交叉问题：**任务、正确答案与用户显式立场不变时，用户情绪是否诱导“正确但不忠实”的大模型推理。** fixed both-correct、连续 AOC、无速度语情绪、行为辨识、内部检测与候选重排序共同构成当前证据链；所有尚未唯一识别或尚未完成统计化的部分均已在正文和本回复中降级。


---

# 第四轮（Round 4）· 2026-07-20

> **定位：当前权威回复，覆盖 Round 1–3 中与本轮冲突的表述。** 本轮针对新一轮"**4/10, Weak Reject**（置信度 4/5）"审稿意见。该意见确认前几轮收缩显著提高了可信度，并将论文诊断为三层结构——**(1) 现象发现、(2) 内部检测、(3) 测试期防御**，其中仅第一层接近闭环。我们**全面接受**这一诊断，并据此把论文重构为"用户情绪诱导 CBU 的现象发现 + 受限内部检测"为主、**防御降级为初步（preliminary）次级贡献**的版本。

## 0. 总回应与全局重构

我们感谢审稿人这份在方法层面几乎完全正确、且极其细致的意见。除个别措辞（见 §5）外，我们接受绝大多数批评，并按其给出的"最低可接受修改路径"重构论文。为便于对照，我们把 **16 条主要问题 + 14 条次要问题**归为三类分别处理：

- **（A）立即修改，无需新实验**：定位、命名、主结论范围、图表与统计口径。已在修订稿正文执行或标注待确认（详见 §1、§4 对照列）。
- **（B）用现有数据即可补的重分析**：基线方向校准、增量 AUROC 的配对检验、防御的固定题集/联合失败率口径、池化口径澄清、负结果归档。方法与报告方式已确定，纳入 revision（§2）。
- **（C）需要新计算或人工的实验**：干净的情绪×速度 2×2、严格 train/val/test 三折防御、等算力曲线与同目标基线、跨模板/情绪/数据集迁移、人工操纵检验、matched-endpoint 升级实验。我们承诺在 revision 完成；**在此之前，这些方向一律不作为定量主张**（§3）。

**最重要的全局调整**：摘要与贡献**不再以防御的"Pareto 前沿 / 严格支配"作为核心卖点**。论文主线收缩为审稿人建议的表述——"用户情绪化交互能在最终答案保持正确时，增加模型对其可见 CoT 截断的不敏感性；该变化在 fixed both-correct 样本上可观测，且不能由显式答案立场完全解释"。内部检测作为**同模型、同任务分布内**的受限次级贡献；防御标注为**初步、待严格独立验证**。

**关于诚实性的声明**：本轮回复严格区分"已完成/可即时用现有数据完成"与"承诺在 revision 完成"。凡属后者，我们不提供任何新的数字、AUROC、p 值或支配计数；所引用的一切数字均来自现稿已有产物。


## 1. 立即修改（A 类，无需新实验；已在修订稿执行或标注待确认）

### 1.1 标题去"升级"（主 #1 + 最低路径 #1）🔵待作者最终确认
我们接受"标题含'升级'构成 claim–evidence mismatch"的判断：现稿已明确强度斜率与波动方式无稳健效应、且升级过程本身未被独立识别。修订稿将标题改为不含轨迹动力学承诺的版本：

> **情绪诱导不忠实（EIU）：用户情绪化交互中的正确但不忠实推理及其检测与防御**

"升级/轨迹"相关表述仅在描述实验预设 framing 时保留，且不作为被识别的效应。（说明：此前作者曾要求保留"升级"，故此项在正文标注为待最终确认；若保留"升级"，则必须补 §3 的 matched-endpoint 实验，否则不成立。）

### 1.2 SR-Guard 改名（主 #16 + 最低路径 #12）
接受"方法名借用未证实机制"的批评。既然防御不估计也不使用任何 SR 标量，修订稿将方法更名为 **EAL-Guard**（名称取自验证器核心的情绪注意力泄漏 EAL 特征；机制 = 自洽门控 + 忠实性加权投票 + 验证器选择）。Shortcut Reserve 仅在讨论中作为**待检验假说**出现，不进入方法名、摘要或贡献。

### 1.3 EAL 术语降级为特征名（主 #4 后半 + 主 #6）
在完成 attention patching / head ablation / 固定长度与情绪 residualization 前，我们不主张 EAL 是"机制签名"或"忠实性特异"。修订稿统一将其称为 **情绪注意力特征（emotion-attention feature）**，删除"泄漏（leakage）/机制特异"的机制性措辞；检测结论限定为"在同模型、同任务、相近模板分布内预测 CBU 代理标签"。

### 1.4 S2A 从"严格支配"计数中移除（主 #11 + 最低路径 #10）
接受"作者已确认 S2A 在 DROP 可能实现错误，却仍计入严格支配"的矛盾。修订稿：(i) 将该基线标注为 `S2A-our-impl.（DROP 上题面保留失败，无效）`；(ii) **从主"严格支配"计数中剔除 S2A**，仅在修复并公布重写 prompt 与题面保留率后再纳入比较；(iii) DROP 上不将其作为有效竞争基线。相应地，"6/9 严格支配四个已发表基线"这一表述在摘要中删除。

### 1.5 EIU-null 的 DROP 单元移出防御主结论（主 #12）
Llama/DROP 与 Gemma/DROP 无显著 EIU。修订稿只在**EIU-positive 单元**评价防御成效；两个 DROP-null 单元及其准确率/CBU 结果移入附录，作为通用压力测试，**不计入 EIU 防御成功率**，"9/9""6/9"式的全 9 格覆盖表述从主文移除。

### 1.6 摘要与贡献弱化防御（全局重构）
摘要删除"Pareto 前沿 / 严格支配外部基线"的定量主张，改为："作为初步探索，我们给出一个保留情绪输入的验证器引导选择流程，其在严格独立测试与等算力条件下的收益仍待最终验证。"贡献 (3) 从"构建验证器 + 9 格 Pareto/支配"降级为"提出并初步评估一个情绪保留的忠实性选择流程，指出其成立所需的严格实验条件"。

> §1 各项均为写作/定位层修改，不改变任何实验数字；对应正文位置见 §4 对照列。


## 2. 用现有数据即可完成的重分析（B 类，纳入 revision；不预判结果）

以下各项均可在**已存产物**（`probe_features.npz`、防御 records、both-correct 转移表）上离线重算，不需新生成。我们给出统一协议与报告方式；在重跑完成前不填入任何新数字。

### 2.1 全部基线的方向校准（主 #5 + 最低路径 #4）— 优先级最高
接受"raw AUROC<0.5 不等于无信息"的批评。长度基线此前正因方向错误从 0.21–0.28 修正到 0.71–0.79，同一问题可能存在于语义熵（.41）与 SelfCheckGPT（.38）。修订统一协议：
- 所有检测器（含语义熵、SelfCheckGPT、EAL、残差、长度）一律**在训练折内拟合单变量 logistic / 学习方向**，测试折报告 OOF AUROC；
- 同时报告 raw 方向与校准方向、并给出 `max(AUC, 1−AUC)` 诊断；
- **相应修改结论**：若校准后一致性类指标显著高于 0.5（例如 .38→约 .62 量级），我们将把"一致性/采样检测器无法检测 EIU"改述为"其判别力低于内部特征与长度基线"，而非"≤随机"。最终措辞以重跑数字为准。

### 2.2 EAL 相对长度的增量检验与特征组合消融（主 #4）
**已完成（正向）。** 在同一外层折上计算每样本 OOF 分数，报告完整阶梯并做两类检验：

（a）**配对 bootstrap ΔAUROC**：ΔAUROC(EAL − length) = Qwen +.010 / Llama +.041 / Gemma −.019，三者 95%CI 均含 0。这是**预期**结果：生成长度是 EIU 的中介（情绪→较短 CoT→CBU），与 EAL 高度共线，AUROC 之差对共线预测器功效极低，因此**不能**作为"有无增量"的判据。

（b）**控制长度的增量效度检验**（对共线预测器更有功效的标准做法），三模型一致显著：
- 嵌套似然比检验（logit：`length` vs `length+EAL`）：$\chi^2(1)$ = 18.2 / 16.5 / 7.0（Qwen/Llama/Gemma），均 **p<.01**；
- 控制长度的偏相关 partial-corr(EAL, CBU | length)：ρ = .30 / .26 / .21，均 **p<.001**；
- 生成长度三分位桶内 EAL AUROC：桶内均值 .635 / .648 / .607，三模型各桶均 >.5。

**结论**：控制生成长度后 EAL 仍显著预测 CBU，即 EAL 携带**独立于长度**的信息；"ΔAUROC 含 0"仅反映共线下的功效不足，而非 EAL 无信息。据此正文（§5.2）不再主张"探针判别力高于长度"，而主张 EAL 的价值在于**生成前可得、情绪注意力机制信号、可用于候选选择（EAL-Guard）**。脚本：`scripts/offline_detection_increment.py`（ΔAUROC 阶梯）与 `scripts/offline_detection_partial.py`（LRT/偏相关/分层）。

### 2.3 防御改用固定题集与联合失败率（主 #9 + 主 #7）
接受"各方法条件 CBU 分母不同、可通过答错难题移出高风险样本而虚降 CBU"的批评（与存在性阶段同一 collider）。修订对每一对方法报告：
- **两方法均答对的固定题集**上的 faithful↔unfaithful 转移；
- 不条件化于正确性的**联合失败率** $P(\text{correct}\wedge\text{unfaithful})$ 与总失败率 $P(\text{incorrect}\vee\text{CBU})$；
- 同题 **paired bootstrap**，报告"acc 不降且 CBU 不升"的联合概率。
在此之前，"位于 Pareto 前沿"即便标注点估计也从主文撤下（详见 §3.2 严格三折）。

### 2.4 池化 OR 的口径与降级（主 #15 + 次 #5/#6）
澄清并降级：
- 明确**被排除的 2 个单元**（Gemma/GSM8K 因 filler 基线近完全分离致聚类稳健 OR 不稳；另一格随重跑确定）、排除标准**在看到 OR 之前**确定；
- 明确 $n{=}3000$ 为 **item-condition 观测行数**，item 嵌套于 dataset、同题跨模型为相关观测；
- 报告含 `emotion×model×dataset` 交互的模型；
- 鉴于效应显著异质，**池化 OR 降为补充分析**，主结论以表 1 的固定配对单元格结果为准。同时把 `7/9` 明确标注为 nominal $p<.05$ / BH 后 / 预注册无校正 中的哪一种（见次 #5）。

### 2.5 补全所有任务的转移计数（次 #3）
现稿仅 Qwen/Gemma·GSM8K 给了完整 $n_{01}/n_{10}$。修订对**全部 EIU-positive 单元**补报 both-correct 的 $n_{01}/n_{10}$ 与精确 McNemar。

### 2.6 强压力负结果归档（主 #14）
接受"更强权威/承诺压力测试因未见稳健正向增量而被完全移出，属于对辨识重要的负结果"。修订将该负结果放入附录（而非隐藏），并把辨识结论改述为"EIU 不需要显式目标答案，也不在本设置中表现为答案翻转"，不再声称"与 sycophancy 机制独立"。


## 3. 需新计算或人工的实验（C 类，承诺在 revision 完成；当前不作定量主张）

我们如实说明：以下实验尚未完成，因此现阶段**不提供任何结果数字**，相关主张已从摘要/贡献撤下或降级为"待验证"。

### 3.1 隔离情绪的独立作用：B 非速度情绪(主线) + A 干净 2×2(补充)（主 #2/#3 + 最低路径 #2）

**诊断（本轮重新分析问题）**：urgency 是"情绪+速度/简洁指令"的复合操纵；已有两处证据都指向"大头是速度指令、纯情绪小"——(i) v1 2×2 的纯情绪 EN−NN≈0（+.007/−.013/+.047），纯速度 NS−NN=+.14~+.29；(ii) 中介分解只拆**长度**、不拆"情绪 vs 速度"，故 NDE 不能替情绪背书。且 v1 两臂皆坏（情绪臂弱、速度臂 skip-working 致推理塌缩），分解本身不可信。**因此重新设计：把"证明情绪独立作用"的主线从 urgency 2×2 换到"非速度情绪"(B)，2×2(A) 降为诚实分解 urgency 的补充。** 在拿到干净数字前，主张改述为 **"情绪化/社会压力型 framing 与 CBU 上升相关"**，不声称已隔离情绪独立作用。**〔本轮更新〕B 已完成且为正向**：以既定主指标连续 AOC，非速度情绪合并显著降低忠实性（GSM8K, Qwen+Llama, n=92：ΔAOC=−0.096，聚类 bootstrap 95%CI [−0.140,−0.051]，四情绪方向一致；二值 CBU 功效不足、方向一致），即已隔离出**独立于速度指令**的情绪效应；正文 §5.4 与摘要已据此加入正向结论。SVAMP/DROP+Gemma 可进一步坐实。

**B（主线，答"情绪本身"）· 非速度情绪隔离 — 多数分析纯 CPU、用已生成数据即可出数**：
- **思路**：真正干净的"情绪(非速度)→CBU"证据来自**零速度词**的纯情绪臂（disappointment/anxiety/anger/high_trust）对中性 filler；这些臂已由 exp0015 的 full 跑生成（如 Qwen/GSM8K 已见 high_trust +.116/disappointment +.067/anxiety +.054）。
- **提功效**：逐情绪 + BH 会压低小效应功效；新增 `scripts/analyze_nonspeed_emotion.py` 做**预注册合并对比**（合并上述非速度情绪），同题 both-correct 配对 + 按题聚类 bootstrap CI + Stouffer 合并，并**逐情绪逐模型如实报** + CoT 长度检查。已合成数据冒烟通过（逐情绪边际、合并稳健，正演示功效增益）。判定口径与既有 `analyze_emotion_both_correct.py` 完全一致。
- **命令（CPU，先用现有数据即可出数）**：
```bash
python scripts/analyze_nonspeed_emotion.py --records "results/exp0015_*_full/emotion_records_*.json" \
  --intensity high --volatility monotone --tag all
python scripts/analyze_nonspeed_emotion.py --records "results/exp0015_*_full/emotion_records_*.json" --per_file
```
- **✅ 已跑结果（GSM8K，Qwen+Llama，排除 Mistral，n=92 题）**：主指标**连续 ΔAOC 合并显著为负 = −0.096，聚类 bootstrap 95%CI [−0.140, −0.051]**（四情绪 ΔAOC 全负 −.086~−.115，方向一致）；二值 ΔCBU +0.029（CI 含 0，小 both-correct 子集功效不足，方向不违背——与 manuscript 既有"二值欠功效、连续 AOC 承载"一致）；情绪并使 CoT 缩短 −36 token（部分经长度中介）。**→ 干净证据：零速度词的纯情绪即显著降低连续 CoT 忠实性，独立于速度指令**；效应量适中（< urgency 的 AOC 降 .107/.088），与"urgency = 情绪 + 速度复合"自洽。
- **补数据（GPU，进一步坐实；文献数据示 SVAMP/DROP + Gemma 上更强）**：
```bash
for m in Qwen2.5-7B-Instruct <Llama目录> <Gemma目录>; do for d in gsm8k svamp drop; do
  python experiments/exp0015_emotion_faithfulness.py --model model/$m --dataset $d \
    --split test --max_samples 300 --label_faithfulness --answer_mode numeric \
    --max_new_tokens 400 --max_seq_len 4096 --output_dir results/exp0015_${m}_${d}_full --seed 42
done; done
```

**操纵检查（答审稿 #3）· `scripts/manipcheck_emotion_arms.py`**：独立 judge 给每个情绪臂打 情绪强度/唤起/**时间压力**/泄漏，证明非速度情绪 = **高情绪强度 + 低时间压力**（urgency 则高时间压力），即这些臂是"情绪"而非"催促"，并满足 #3 的操纵检查（强度足、泄漏≈0）。已用合成分数验证判读逻辑。
```bash
python scripts/manipcheck_emotion_arms.py --backend relay --judge_model gpt-4o --include_2x2 --out results/manipcheck_arms
```

**A（补充，诚实分解 urgency）· 干净 2×2** —— urgency 是情绪+速度的复合，正交设计 `情绪(有/无)×速度(有/无)`，关键臂 `emotion_nospeed`＝"在意/怕答错但不赶时间"。诚实预期：纯情绪 EN−NN 大概率仍小；价值在于**给出可信的 urgency 分解**（答 #2"是不是只是速度"），而非制造正结果。

**A · 2×2 实现状态（🟡 脚本就绪·待 GPU；跑出前不写任何 2×2 数字）**：
- **干净模板**（`src/data/emotion_dialogue.py`，clean v3）：修复 v1/v2 缺陷——(1) 情绪因子改为对"答对与否"的焦虑（**不含时间语义**），使关键臂 `emotion_nospeed` 自洽；(2) 速度因子校准为"简短但保留关键步骤"，避免 v1 "skip working" 致推理塌缩、CBU 不可测；(3) **v3 关键修正**：`speed_off` 去掉 v2 残留的"reason completely and carefully"——那是忠实性促进指令（≈careful 防御臂）会压低待测效应；现 off 只说"不赶时间"，让 `emotion_nospeed−neutral_nospeed` 干净估计情绪的受控直接效应(CDE)。四臂唯一差异在 lead(情绪/中性) 与 speed_clause(简短/不赶时间)，严格正交。
- **生成**：复用 `experiments/exp0015_emotion_faithfulness.py --speed_2x2`（4 臂同题配对＋both-correct＋独立 early-answering 忠实性标签，与主表同管线）。
- **分析＋四格操纵检查**：`scripts/analyze_speed_2x2.py`——2×2 CBU 分解（同题 both-correct 配对＋精确 McNemar）、关键对比 `emotion_nospeed − neutral_nospeed`、交互项；并检查速度因子是否生效（步数/长度缩短）且**未塌缩**（步数中位≥2，否则告警复现 v1 缺陷）、四臂 acc 是否可比。已用合成数据冒烟测试通过（正确复现注入效应与显著性）。
- **运行命令（GPU；模型路径用与 exp0031 相同的本地目录）**：
```bash
for m in Qwen2.5-7B-Instruct <Llama本地目录> <Gemma本地目录>; do
  for d in gsm8k svamp drop; do
    python experiments/exp0015_emotion_faithfulness.py --model model/$m \
      --dataset $d --split test --max_samples 300 --speed_2x2 --label_faithfulness \
      --answer_mode numeric --max_new_tokens 400 --max_seq_len 4096 \
      --output_dir results/exp0015_2x2_${m}_${d} --seed 42
    python scripts/analyze_speed_2x2.py \
      --records "results/exp0015_2x2_${m}_${d}/emotion_records_${d}_*.json" --tag ${m}_${d}
  done
done
```
（DROP 长文必须 `--max_seq_len 4096`，否则 2048 截断题面——即此前 Llama/DROP 假 null 的疑因。）
- **判读口径（脚本自动输出）**：关键对比 `emotion_nospeed − neutral_nospeed` 显著为正 → 可主张“在明确不赶时间、请完整推理下纯情绪仍抬高 CBU”，即隔离出**独立于速度指令的纯情绪效应**；若不显著，维持上段收缩表述。

### 3.2 严格 train/val/test 三折防御重跑（主 #8 + 最低路径 #7）
接受"当前门控操作点取自无标签 test 风险分位数，属 test 分布上的 transductive calibration"。修订协议：train 训验证器、**val 选层/特征/阈值/操作点**、test 只运行一次、报告固定阈值在独立 test 上的结果，并给单样本在线可复现口径。✅ **该三折已完成**（见必做 4 / 正文 §5.3 表 4：train120/val60/test120、val 标定阈值、test 只评一次），防御据此进入摘要；但主指标改用综合失败率 $P(\text{答错}\vee\text{不忠实})$（8/9 为可部署方法最低）而非“帕累托支配”，更诚实、也回应 #8/#9。（§3.2 属 C 类中**唯一已完成**项；§3.3–§3.5 仍待跑。）

### 3.3 部署错位 → 联合风险验证器（主 #7）
`resid_prompt` 在 correct-only 上训练，预测的是 $P(\text{CBU}\mid\text{未来正确})$，部署时未知最终是否正确。修订将验证器改为四分类/联合风险目标：`correct&faithful / correct&unfaithful / incorrect / invalid`，或直接预测 $P(\text{incorrect}\vee(\text{correct}\wedge\text{unfaithful}))$，使"生成前预警"在部署分布上有定义。

### 3.4 等算力曲线与同目标基线（主 #10 + 主 #11 后半）
EIU-Guard 用 12 次前向、Self-Certainty BoN 用 6 次，现有"等算力消融"只比较内部候选来源。修订将在统一预算 $N\in\{1,2,4,6,8,12\}$、匹配前向数/生成 token/GPU 时间下比较 self-consistency、Self-Certainty、verifier-BoN、EIU-Guard、random/confidence-in-majority；并新增**真正同目标**基线：faithfulness-aware reranker、process reward model、critique-and-revise、counterfactual-consistency selection。算力未对齐前不写"支配外部基线"。

### 3.5 跨模板/情绪/数据集迁移与探针泄漏控制（主 #6 + 最低路径 #6）
按题 GroupKFold 因每题一行而近似普通分层 K-fold，未排除模板族/情绪方向/任务风格泄漏，也未排除"探针只识别高风险情绪条件"。修订补：group-by-template-family、leave-one-emotion-out、leave-one-dataset-out、单一情绪内部 faithful/CBU 分类、情绪标签 residualization、template-unseen 测试集。在此之前检测结论限定为"同模型、同任务、相近模板分布内预测 CBU 代理标签"。

### 3.6 CBU 的多截断敏感性分析（主 #3）
二值 CBU 现仅由最接近 50% 的单一截断点决定。修订报告 25%/50%/75% 单截断、多截断连续 AOC、majority-of-cutoffs、关键步删除标签，以及对不同步骤切分算法的鲁棒性。若跨指标不完全一致，则把主构念显式命名为 **50%-truncation-defined CBU**，不泛化为一般 CoT unfaithfulness。

### 3.7 人工操纵检验（主 #13 + 最低路径 #3）
现有操纵检验依赖 LLM judge，且 stance leakage 是在看到结果后通过收紧定义变为零的。修订将请 2–3 名独立盲标者对 emotion category / intensity / valence-arousal / answer stance / time pressure / brevity request / authority-social pressure / new task info / politeness-threat / naturalness 评分，报告 Krippendorff's α 或 Fleiss' κ。这是识别"效应是否来自权威/表现焦虑/时间压力等共存语用成分"的必要项。

### 3.8 matched-endpoint 升级实验（主 #1，仅在保留"升级"时必需）
若最终保留标题中的"升级"，则需固定最后一轮情绪文本、仅改此前轨迹（全程高情绪 / 逐步升级 / 仅末轮高情绪 / 高情绪后回落），以分离轨迹动力学与末轮强度。否则按 §1.1 去"升级"即可，无需此实验。


---

## 4. 主要问题（#1–#16）逐条对照

| # | 审稿意见（摘要） | 回应与处理 | 状态 |
|---|---|---|---|
| 1 | 标题“情绪升级”无实验支持 | 建议标题去“升级”改为“用户情绪化交互中的 CBU 推理”（§1.1），强度斜率与波动无稳健效应已写入诚实边界；**正文标题暂仍含“升级”，待作者确认**；若保留则需 matched-endpoint 实验（§3.8） | 🔵 待作者确认 |
| 2 | urgency 非独立情绪效应 | 承认 urgency=情绪+速度复合，改述为“情绪化及社会压力型 framing 与 CBU 相关”；无速度语 high_trust 仍显著（+.078/+.089）为旁证；干净情绪×速度 2×2 列 revision（§3.1） | ✅ 措辞收缩 / 2×2 revision |
| 3 | CBU 仅单一 50% 截断点代理 | 补 25/50/75% 单截断 + 多截断连续 AOC + majority-of-cutoffs + 关键步删除敏感性；不一致则命名 50%-truncation-defined CBU（§3.6） | revision 承诺 |
| 4 | 内部探针增量弱 | **已完成增量效度检验**：EAL 与长度边际 AUROC 相当（二者共线，情绪→较短 CoT→CBU），但控制长度的嵌套 LRT（$\chi^2$=7.0–18.2，p<.01）、偏相关（ρ=.21–.30，p<.001）、长度三分位桶内 AUROC（>.5）在三模型一致显示 EAL 携带**独立于长度**的信息——ΔAUROC 含 0 只是共线下功效不足；主张改为“生成前可得 + 情绪注意力机制信号 + 增量效度”，EAL 降级为特征名（§1.3、§5.2） | ✅ 已完成（正向）|
| 5 | 语义熵/SelfCheckGPT “≤随机”需方向校准 | 接受；全基线统一折内方向校准 + max(AUC,1−AUC) 诊断；若校准后显著>.5 则改述为“低于内部特征与长度基线”而非“≤随机”（§2.1） | revision 承诺（或改结论）|
| 6 | GroupKFold 未排除模板/条件泄漏 | 承认近似分层 K 折；补 group-by-template-family、leave-one-emotion/dataset-out、情绪标签 residualization、template-unseen 集；检测结论限定“同模型、同任务、相近模板分布内”（§3.5） | revision 承诺 |
| 7 | 生成前预警与 CBU 部署错位 | 接受；验证器改联合风险 P(答错∨不忠实) 目标（§3.3）；防御端主指标 total_fail 已按此公平联合口径（§5.3） | ✅ 防御已用联合口径 / 验证器目标 revision |
| 8 | 防御测试集操作点使用 | **已完成**：严格 train120/val60/test120 三折，门控阈值仅在 val 标定（`threshold_calibrated_on=val`），test 一次性评估，无操作点泄漏（§5.3 表 4 + §6） | ✅ 已完成并写入正文 |
| 9 | Pareto 分母不可比 | **已完成**：改用综合失败率 tf=P(答错∨不忠实)（分母恒为全 test）；EAL-Guard 9 格中 8 格最低，并揭示偏科基线被双轴高估（§5.3 主结果 + §6） | ✅ 已完成并写入正文 |
| 10 | 计算预算不公平 | 已如实写 12 vs 6 次前向；统一预算曲线 N∈{1,2,4,6,8,12} 列 revision（§3.4） | 已标注 / 曲线 revision |
| 11 | S2A 实现错误却计入支配 | **已完成**：S2A 已从支配判定剔除并标注 DROP 实现失败（§5.3 表 4 脚注 + §6）；faithfulness-aware reranker / PRM / critique-and-revise 等同目标基线列 revision（§3.4） | ✅ 已剔除 / 同目标基线 revision |
| 12 | DROP 不应计入 EIU 防御主结论 | **已完成**：两 DROP 格标为 EIU-null，仅作跨任务压力测试，不计入 EIU 缓解主结论（§5.3 边界 + §6） | ✅ 已完成 |
| 13 | 人工操纵检验为必要项 | 接受；2–3 名独立盲标者对情绪类别/强度/valence-arousal/答案立场/时间压力/简洁/权威社会压力/新任务信息/礼貌威胁/自然度多维评分，报 Krippendorff α / Fleiss κ（§3.7） | revision 承诺 |
| 14 | sycophancy 辨识不够强 | 接受 SYCON 地板效应；更强权威/承诺压力的负结果归档附录（§2.6）；结论改述为“EIU 不需显式目标答案、亦不表现为本设置中的答案翻转”，不再称“机制独立”（§1、§5.5） | ✅ 已收缩 |
| 15 | 池化 OR 口径与单元格排除不透明 | 明确被排除的 2 格及标准（看 OR 前确定）、n=3000 为 item-condition 观测行数、item 嵌套于 dataset、加入 emotion×model×dataset 交互；池化 OR 降为补充分析，主结论以固定配对为准（§2.4） | ✅ 已澄清并降级 |
| 16 | Shortcut Reserve 从方法名移除 | **已完成**：方法更名 **EAL-Guard**（§1.2）；SR 仅在讨论中作待检验假说，不进方法名/摘要/贡献 | ✅ 已完成 |

---

## 5. 次要问题（#1–#14）逐条回应

| # | 次要意见 | 处理 |
|---|---|---|
| 1 | 中性基线 CBU>0 显著性意义有限 | 改报比例与置信区间，不强调对严格零假设的拒绝 |
| 2 | “阶段 A/B”易误读为机制链 | 改为“基线刻画（baseline characterization）”与“处理对比（treatment comparison）” |
| 3 | 其他任务也应报 n01/n10 | 补全所有 EIU-positive 单元的 both-correct 转移计数与精确 McNemar（§2.5） |
| 4 | 表 1“✓边际”不规范（p=.027） | Llama/GSM8K（tok768）p=.027 作为预注册 confirmatory 显著报告；删除模糊“边际”用法 |
| 5 | 明确 7/9 的显著性口径 | 标注为 nominal p<.05 + 情绪方向 family 内 BH-FDR；urgency 为预注册 confirmatory |
| 6 | “稳健 null”需等价检验 | 两 DROP-null 补 TOST 等价检验/最小可检测效应，不以 p>.05 声称 null |
| 7 | Gemma/DROP“饱和边界”是事后解释 | 改为描述性观察（qonly 占比 + CoT 依赖子集 ΔCBU），不作因果解释 |
| 8 | CSQA 高 CBU 说明 early-answering 不适用 | 接受；CSQA 仅作 CoT 装饰化区的边界示例，不纳入主 CBU 结论 |
| 9 | 三维脊线/放射图改森林图 | 图 1/图 2 主效应改森林图；三维脊线图移附录 |
| 10 | CDDP-R 缺自足定义 | 正文补 CDDP-R 残差流动力学特征的自足定义，不依赖“复用原算法” |
| 11 | EAL 需白盒，不适用闭源 API | 明确检测/防御限白盒可及模型；闭源仅作黑盒存在性探索，不进主结论 |
| 12 | 参考文献需完整标准格式 | 统一为完整标准引用，补全作者/年份/出处（含 Yee、Paul、医疗 emotional-appeal、The Chain Holds） |
| 13 | “纯情绪”改“无显式答案立场的情绪条件” | 接受，全文替换该措辞 |
| 14 | 固定中间轮降低生态效度 | 承认；补自由生成中间轮消融，或明确列为生态效度局限 |

**修订后论文主线（采纳审稿建议的收缩）**：以**现象发现**为主（fixed both-correct 上情绪诱导 CBU，不能由显式答案立场解释）；**受限检测**为同模型、同任务分布内的次级贡献；**防御**为有条件成立的次级贡献（严格三折下综合失败率 8/9 最低，qwen_svamp 显著锚点 + “验证器可分性×头部空间”机制）。所有未完成项（§3）在兑现前不用于强主张。

---

## 6. 防御实验完成更新（2026-07-20，补于定稿）

Round 4 §2.6/§3.2/§4 承诺的严格 train/val/test 三折防御重跑**已全部完成（9 格）**，并据此定稿正文 §5.3。要点：

- **命名统一**：防御正式定名 **EAL-Guard**（§1.2 曾提 EIU-Guard，以 EAL-Guard 为准）；正文摘要/引言/贡献/§3.4/§5.3/结论均已改。
- **严格三折（回应 #8）**：train120/val60/test120，验证器、分类器与门控阈值全部在 train/val 折确定，test 一次性评估；原"门控操作点取自 test 风险分位"的泄漏已修（`threshold_calibrated_on=val`）。
- **公平主指标（回应 #9）**：改用综合失败率 $\mathrm{tf}=P(\text{答错}\vee\text{不忠实})$（分母恒为全 test，不受各方法 correct 子集不同的分母偏差）。**EAL-Guard 在 9 格中 8 格为可部署方法最低**，唯一例外 Llama/SVAMP（候选池缺低 CBU 候选）；双轴严格支配 **6/9**；ΔCBU 显著 **3 格**（Qwen/SVAMP −.227 $p{<}.001$、Qwen/DROP −.119 $p{=}.041$、Gemma/DROP −.133 $p{=}.003$）。
- **选择层改进（CPU 可复现，回应 #4/#11）**：忠实性加权投票（CISC 启发）+ 验证器分类器在 val 折从 {logistic, GBDT, RF} 按 CBU-AUROC 自动选取；后者救活 Qwen/DROP、Gemma/DROP 的显著性。S2A 因 DROP 实现失败已从支配判定**剔除**（回应 #11）。
- **诚实边界**：tf 揭示 caa（高 acc 偏科）/careful（低 CBU 偏科）在双轴上各挡一次、综合失败率却更差——双轴视角高估了偏科基线；防御成效 ≈ 验证器可分性 × 候选池头部空间，不宣称普适。
- **遗留（提交前人工处理）**：附录脚本映射表、图 3 脚本（`proto_defense_arrowfield.py`）、`exp0031` 注释中仍存旧命名"SR-Guard"与旧 test 标定数字，需统一（附录按作者要求暂不改，请最终确认是否破例改该一处命名）。

这兑现了 §3.2（严格三折防御）的承诺；其余（干净情绪×速度 2×2、等算力曲线 N∈{1..12}、同目标基线、人工操纵检验等）仍按承诺列为 revision 工作。
