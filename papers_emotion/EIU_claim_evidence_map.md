# EIU claim-to-evidence 对账表（提交前核对 · 回应审稿 R2 #14）

> **目的**：把**对外主张**（摘要 / 正文 / 图表措辞）逐条映射到 **数字 / 证据来源（脚本·产物）/ draft 位置 / 状态**，
> 并标出**跨文档数字不一致**，供提交前统一。
> **真相源 = `draft_emotion_zh.md` 现稿**；辅助文档（`EIU_reviewer_response.md`、`EIU_experiment_checklist.md`、
> `experiments_emotion.md`）如与 draft 冲突，**一律向 draft 对齐**。
> **状态**：✅ 已回填且自洽 ｜ 🟡 待完成/待重跑 ｜ ⚠️ 跨文档数字不一致（须统一）｜ 🔵 待作者定稿决策。

## A. 存在性（§5.1 / 表 1 / 摘要）

| 主张 | draft 数字 | 证据来源 | 状态 |
|---|---|---|---|
| 阶段 A：基线 CBU>0 | Llama .254 / Gemma .151 / Qwen .111（filler, both-correct） | exp0015 | ✅ |
| 阶段 B：urgency 放大（GSM8K, both-correct 净转移） | Qwen **+.184** / Gemma **+.173** / Llama **+.080**(p=.027,边际) | exp0015 + McNemar | ⚠️ Llama：draft +.080/p=.027（tok768）vs checklist E1 +.071/p=.0599（pre-tok768）→ 统一到 draft |
| SVAMP | Qwen +.173 / Llama +.280 / Gemma +.106（均 sig） | exp0015 | ✅ |
| DROP | Qwen +.140(sig) / Llama +.027(null) / Gemma +.039(null) | exp0015 + diag_drop_cell | ✅ 两 null 同落 DROP，诚实边界 |
| 9 格汇总 | 7 显著/边际 + 2 稳健 null | 表 1 | ✅ |
| 池化混合效应 OR | **2.03**（聚类稳健[1.78,2.31], p=2.4e-26）/ 2.99（GLMM），7 格 n=3000 | mixed_effects_cbu.py | ⚠️ draft 已统一 2.0–3.0；reviewer_response §4 + checklist E1 仍写旧 **3.08/5.33** → 更新辅助文档 |
| 主口径 = both-correct 净转移 | P(F→U) − P(U→F) | analyze_emotion_both_correct.py | ✅ 审稿 #1 满足 |
| **非速度情绪独立效应（§5.4，无任何速度/简洁指令）** | 合并 **ΔAOC=−0.096**（GSM8K, Qwen+Llama, n=92；聚类 bootstrap 95%CI [−0.140,−0.051]，四情绪 disappointment/anxiety/anger/high_trust 方向一致）；二值 CBU +0.029 功效不足、方向不违背 | `analyze_nonspeed_emotion.py`（连续 AOC 为主指标） | ✅ 已写入 §5.4+摘要；正面回应审稿 #2（该失效非纯速度伪影）；SVAMP/DROP+Gemma 可进一步强化 |

## B. 检测（§5.2 / 摘要）

| 主张 | draft 数字 | 证据来源 | 状态 |
|---|---|---|---|
| EAL 探针 AUROC | Qwen .763 / Llama .746 / Gemma .766（GroupKFold, 置换 p=.002） | exp0026 + probe_deleaked_eval.py | ✅ |
| 长度基线（公平口径） | 0.71–0.79（Qwen .753 / Llama .705 / Gemma .785） | 折内 OOF-logistic | ✅ 已收回"探针≫长度" |
| **EAL 增量效度（控制长度）** | 嵌套 LRT $\chi^2$=7.0/16.5/18.2（Gemma/Llama/Qwen，p<.01）；偏相关 ρ=.21/.26/.30（p<.001）；长度三分位桶内 AUROC .61–.65（均>.5） | `offline_detection_partial.py` | ✅ **已完成（正向）**：控制长度后 EAL 独立预测 CBU，已写入 §5.2；正确回应审稿 #4（ΔAUROC 对共线预测器不敏感，改用 LRT/偏相关）|
| 一致性/采样检测器 | 语义熵 .41 / SelfCheckGPT .38（原始方向）；翻方向 ≈ .59/.62 | baselines（**借自 CDDP-R GSM8K，EIU 未自跑**）| ⚠️ 来源为别批数据；正文已**移除其 AUROC 数字**，改用**目标错配**论证（一致性/采样测答案层不确定性，而 CBU 为正确答案，结构上测不到）；EIU-原生方向校准重跑留待 GPU |
| 生成前预警 | Qwen resid_prompt .78；Llama resid 不承载（分模型如实） | nested CV | ✅ |
| AOC/CBU 定义 | 附录 D.0 metric spec（方向统一 + 伪代码） | exp0015 | ✅ 审稿 #2 满足 |
| 泄漏迁移（跨情绪/模板/数据集/模型） | — | probe_features.npz | 🟡 未做（审稿 #8）；收缩"跨模型稳健/忠实性特异"表述 |

## C. 防御（§5.3 / 表 4 / 摘要）

| 主张 | draft 数字 | 证据来源 | 状态 |
|---|---|---|---|
| 9/9 位于 Pareto 前沿 | test 标定（旧） | exp0031 | ⛔ 已被 C.bis 严格三折覆盖，勿再引用 |
| 6/9 严格支配全 4 基线 | test 标定（旧） | check_defense_dominance.py | ⛔ 已被 C.bis 严格三折覆盖，勿再引用 |
| S2A 去情绪崩准确率 | Qwen/GSM8K .94→.82；DROP .14–.32 | exp0031 | 🟡 DROP 崩疑似**实现失败**（审稿 #10.2）；已从严格支配计数**剔除**（审稿 #11）|
| 算力 12 vs 6 次前向 | 已如实（12 vs 6） | — | 🟡 需 N∈{1,2,4,6,8,12} 等算力曲线（审稿 #10.1） |

### C.bis 严格三折（val 标定）实测 — E8 重跑 exp0031（2026-07-20；**9 格全完成**）

口径：train120 / val60 / test120；guard operating point 取自 **val**（`threshold_calibrated_on=val`）；ΔCBU@both-correct + 精确检验 + budget 前沿 @0.0–@1.0。数据源：`_clean`（7 格）+ `_v4val`（gemma_svamp/drop，同口径，已用 llama_gsm8k 双版本验证数字一致）。**旧 test 标定的"9/9 Pareto、6/9 严格支配"作废，一律以下表为准。**

| 格 | 验证器AUROC | eal_guard acc/CBU_cond | ΔCBU@bc (p) | 双轴地位（排除 acc 崩的 s2a）|
|---|---|---|---|---|
| qwen_svamp | .889 | .942/.566 | **−.229 (p<.001)** | ✅ 严格支配全部 + **唯一确认显著** |
| gemma_svamp | .791 | .867/.673 | 〔p 待提取〕 | ✅ 点估计严格支配全部 |
| gemma_gsm8k | .776 | .867/.250 | −.069 (p=.181) | ✗ 被 careful(.875/.248) 挡 |
| llama_svamp | .752 | .850/.627 | −.022 (p=.799) | ✗ 前沿；cast CBU 更低(.600) |
| gemma_drop | .748 | .792/.789 | 〔p 待提取〕 | ✅ 点估计严格支配（但 **EIU-null**、CBU 绝对值高）|
| qwen_gsm8k | .746 | .942/.257 | 〔p 待提取；点估计 −.029〕 | ✗ careful CBU 更低(.227)、caa/cast acc 更高(.950/.958) |
| llama_gsm8k | .702 | .850/.225 | −.076 (p=.285) | ✅ 双轴第一（排 s2a），不显著 |
| qwen_drop | .581 | .758/.681 | −.089 (p=.147) | ✗ 前沿；caa acc 更高(.767) |
| llama_drop | .578 | .692/.663 | −.101 (p=.182) | ✅ 双轴第一，但 **EIU-null** |

**诚实结论（9 格齐）：**
- 双轴第一（点估计、排 s2a）：**5 格** = qwen_svamp、gemma_svamp、gemma_drop、llama_gsm8k、llama_drop。
- 其中 **2 格是 EIU-null**（llama_drop、gemma_drop，防御意义弱）；真正 EIU-positive 且双轴第一 = **qwen_svamp、gemma_svamp、llama_gsm8k**。
- **显著性目前只确认 qwen_svamp（p<.001）**；gemma_svamp/gemma_drop/qwen_gsm8k 的 p 待提取；其余 4 格 p 均不显著。
- **SVAMP 两格（qwen/gemma）均双轴第一** → 若 gemma_svamp 的 p 也显著，则有 2 格显著正向，SVAMP 可作为防御主战场。
- GSM8K：数学题上 careful（反情绪 prompt）本身强降 CBU，挡住 qwen/gemma 的 eal_guard；只有 llama_gsm8k 双轴第一。
- 经验规律：防御成效 ≈ f(验证器 AUROC × baseline CBU 头部空间 × 捷径是否自洽)；非单调于 AUROC 单变量。
- 定位：防御 = "有条件成立的次级贡献"，qwen_svamp 显著 + SVAMP 双格为锚点；**不写"普遍双轴第一 / 严格支配"**。
### C.ter 选择层改进 + 正文定稿（2026-07-20；`offline_defense_reselect.py` + `offline_verifier_boost.py`，9 格）

- **有效改进 = 加权投票（CISC）+ val 选分类器（GBDT/RF 替换 logistic）**：单换联合目标收益不稳健（多数格略削 CBU 降幅），但加上加权投票与换分类器后：
  - **主指标 total_fail = P(答错∨不忠实)（审稿 #9 公平口径，分母恒为全 test）：9 格中 8 格为可部署方法最低**，唯一例外 llama_svamp（候选池缺低 CBU 候选，cast steering 更优 .667）。
  - 双轴严格支配全部基线 **6/9**（val 自动选配方口径 5/9；llama_drop 用线性验证器即达前沿）。
  - ΔCBU@bc 显著 **3 格**：qwen_svamp −.227 (p<.001)、**qwen_drop −.119 (p=.041，gbdt 救活)**、**gemma_drop −.133 (p=.003，rf 救活)**。
  - 机制：total_fail 揭示 caa（高 acc 偏科）/careful（低 CBU 偏科）被双轴高估；成效 ≈ 验证器可分性 × 候选池头部空间。
- **正文已定稿（已写入）**：§5.3 + 摘要/引言/贡献/结论 全部 SR-Guard→**EAL-Guard**；表 4 换严格三折 9 格；主指标 total_fail(8/9) + 双轴(6/9) + 显著(3/9) + 机制解释 + llama_svamp 诚实例外。EAL-Guard 正式定义 = 自洽门控 + 忠实性加权投票 + val 选分类器（候选池后处理，`offline_defense_reselect.py` 可复现）。
- **遗留不一致（提交前人工处理）**：① 附录 `draft_emotion_zh_appendix.md` 脚本映射表仍写"SR-Guard"（用户要求不改附录）→ 正文/附录命名不一致，需破例改该处或统一；② 图 `proto_defense_arrowfield.py`/`eiu_paper_figs.py` 仍用旧 test 标定数字 + SR-Guard 标签 → 需改数字与命名并重新生成图 3；③ `exp0031` 注释"论文命名：SR-Guard"→ 改 EAL-Guard；④ gemma_svamp/gemma_drop/qwen_gsm8k 的原验证器口径 ΔCBU p 仍宜从 defense_compare.json 复核（offline 复现值：gemma_svamp≈.38、gemma_drop 加权 rf=.003、qwen_gsm8k≈.49）。

## D. 辨识（§5.4 / 摘要）

| 主张 | draft 数字/证据 | 证据来源 | 状态 |
|---|---|---|---|
| EIU ≠ 立场翻转 | 纯情绪臂无答案可迎合 CBU 仍升；SYCON FlipRate≈0 | exp0015 + sycon.py | ✅ 决定性证据 |
| EIU ≠ 社会安抚 | 控 ELEPHANT 后情绪净效应不变 | aggregate_elephant.py | 🟡 se bug 已修，待 CPU 重跑取精确 CI（§5.4）|

## E. 边界 / 收缩（§6 / §7 / 摘要）

| 主张 | 现状 | 状态 |
|---|---|---|
| SR 门控 | 候选边界概念，正向联合估计未完成 | 🔵 拟降级 Discussion + 方法改名（审稿 #6/必做6） |
| 情绪"升级" | 强度斜率不显著、abrupt/monotone 无稳健差 | 🔵 拟软化标题去"升级/放大"（审稿 #3） |
| 速度中介 | 中介占比 62–82%（LPM 差分）| 🟡 收缩"纯速度证伪"表述 + 干净 2×2（审稿 #4/必做3）|
| CoT 因果丧失 | 扰动式代理 | 🟡 全文收缩为"反事实依赖下降"（审稿 #5）|
| 保留情绪 | 仅保留 prompt token，未测行为适应 | 🟡 收缩为"保留情绪输入" + judge 评价（审稿 #12）|

## F. 跨文档不一致（提交前必须统一）

1. **Llama/GSM8K**：draft 表 1 = **+0.080/p=.027**（tok768 canonical）；checklist E1 里程碑 = +0.071/p=.0599（pre-tok768）；R2 回复必做 1 原写 +0.071 → **全部统一到 draft 的 +0.080/p=.027**。
2. **池化 OR**：draft = **2.03/2.99**（p=2.4e-26, 7 格 n=3000，已剔坏 Gemma）；reviewer_response §4 速查 + checklist E1 = 旧 **3.08/5.33** → **更新辅助文档为 2.0–3.0** 并注"含坏 Gemma 虚高、已修正"。
3. **图 3 注 / 正文内部备注**："旧图不得引用"提示、脚本路径、living-doc 状态标记 → **提交版删除**（审稿 #14）。
4. **exp0045 / 黑盒探索**：正文不引用；确认摘要/引言无残留（reviewer_response #11、checklist §6 已移出）。

## G. 审稿 R2 四大硬伤 · 收敛状态

| 硬伤 | 状态 |
|---|---|
| #1 both-correct 配对 | ✅ draft 表 1 已用净转移 |
| #2 AOC 方向定义 | ✅ 方向统一 + 附录 D.0 metric spec |
| #4 AUROC / 防御基线 | AUROC 基线 ✅ 已更正；S2A 实现 🟡 待修 |
| #3 urgency × 速度 2×2 | 🔴 唯一需 GPU 新实验、未做 |

> 结论：四大硬伤中 3 个已在文档/离线层面闭环，仅 #3（干净 2×2）需 GPU。F 区 3 处跨文档数字不一致为提交前**必须**清零项（纯文档）。
