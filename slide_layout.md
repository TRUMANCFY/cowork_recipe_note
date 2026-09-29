# 每页版式

*配合 presentation_draft.md 用。这里只管"长什么样",讲什么看那份。*

## 全局规则

- **每页一个视觉主体**,其余是标注。不要两张图挤一页。
- **正文字数上限 40 词**,第 8、12 页放宽到 60。超了就砍,讲的内容在嘴上不在页面上。
- **三种颜色贯穿全场**:expert 灰、synthetic 蓝、reference(前沿模型、基线)橙。第 3、6、9、10、13 页都用同一套,听众看颜色就知道在比什么。
- **字号**:标题 32 以上,正文 20 以上,图内标注 16 以上。低于 16 的字不放。
- **动画只用两处**:第 6 页横幅在流程图之后出现;第 13 页结束语最后出现。其他一律静态。
- **每页右下角一个小页码**,方便提问时定位。

---

## 1. Title

全屏深色底。**Mini Cowork** 一行大字,下面一行 "An Open Recipe for Training Agents on Professional Work",再下面小一号副标题 "Synthetic tasks, rubric-only rewards, no annotators"。左下:姓名、UKP Lab / OAR、日期。没有图。

## 2. The workload already exists in the product

左 60%:一个大数字 **[Excel Agent Mode 分数]**,下面一行小字 "Excel Agent Mode on SpreadsheetBench, human [人类基线]";再下一行 "Office Agent: Word and PowerPoint deliverables"。
右 40%:一个横向小图,三个文件图标(csv、pdf、xlsx)→ 箭头 → 两个交付物图标(xlsx、docx)。图下一行 "the task shape"。

不放产品截图,省时间也省 IP 麻烦。

## 3. Who does it today, and at what cost

全宽一条横向数轴,0 到 60。标记点从左到右:"open, non Claude/GPT: <[开源上限]"(灰区)、Sonnet 4.6 + CC [分数]、GPT-5.4 + Codex [分数]、GPT-5.5 + Codex [分数] 紧贴一个**空位(虚线圆)**、Opus 4.7 + CC [分数]、GPT-5.6 [分数]、Fable 5 [分数]。前沿模型全部橙色。数轴标题小字 "JobBench main, as reported"。
数轴下方两个 chip:"$[成本] / run(Opus 4.7 + CC)"、"$[成本] / run(GPT-5.5 + Codex)"。

页面上只有数轴和两个 chip,没有正文。

## 4. The training data does not exist

三行,每行:左侧来源名(粗体)+ 中间一句原因 + 右侧一个 ✗。
- Expert-written  |  hours per task, ~100 tasks  |  ✗
- Customer documents  |  inside the compliance boundary  |  ✗
- Existing synthetic data  |  code and math only  |  ✗

底部一条横幅(蓝底):**→ synthesize from public data**

## 5. Why synthesis is feasible now

三张并排卡片,每张一个标题加一个证据:
- **Task shape is defined**:三个 benchmark 名和日期(JobBench 05/26、ALE 06/26、Workspace-Bench 05/26)
- **Deliverables are verifiable**:直接引一条真实 rubric,"identify CHK-1402 ($1,847.50) has no matching AP invoice",下面小字 "not a judgment call"
- **RL needs tasks, not demonstrations**:一个极简对比,SFT: task + trajectory;RL: task + verifier

## 6. Big picture, and the headline result

上 65%:横向流程图。左半三个框(public data → propose task + rubric → sandbox feasibility check),中间一个竖长框 **sandbox**(蓝色高亮,上下贯通),右半三个框(rollouts → reward → DPPO),最右一个框 JobBench main。左半框标 "Synthesis",右半框标 "Training"。箭头从左到右。
下 35%:横幅(第二步出现)。左边大字 **[ours]**,中字 "vs GPT-5.5 at [GPT-5.5 our judge], identical scaffold and judge";右边一条同 judge 的小阶梯(GPT-5.4、GPT-5.5、ours、GPT-5.6,全是自测数),ours 蓝点大一号;不要把第 3 页那条报告口径的数轴缩小放这里,两种口径不混在一条线上。横幅底部小字:Qwen3.6-27B,[N] synthesized tasks,[k] eval samples。

## 7. Two choices that make the tasks trainable

两列。
左列:一个竖向漏斗图,三段,[seeds] seed files → [candidates] candidates → [N] kept,右侧标 [存活率]。漏斗下一行:"feasibility is confirmed by execution"。
右列:两个上下叠的小流程,上面一个划掉:trajectory → task(红色斜线);下面一个:O\*NET goal → agent → execution → values。右列下一行:"instructions from goals, not from trajectories"。

两列底部各一行小字说它对训练的意义(每个任务都可完成 / 学不到泄题路径)。

## 8. Training: a standard agentic RL recipe on a new environment

两列等宽,标题分别 **Standard** 和 **Rebuilt**。
Standard:async rollouts、group advantage、clipped policy gradient (DPPO)、no KL、constant LR。
Rebuilt:Apptainer workspace per rollout、bash-only scaffold、rubric-only reward (LLM judge, no verifier)、Qwen3.6-27B on 64 H100。
两列中间一个竖线。右下角一个小图标:容器里有几个文件图标。

页面上不出现 TMax 或 open-instruct 的名字,口头被问到再说。字号可以到 18,1.3 分钟带过。

## 9. Result: a 27B open model against frontier models

全宽一条横向阶梯图,五个点带误差条,从低到高:Base(灰)、GPT-5.4(橙)、GPT-5.5(橙)、**ours(蓝,大一号)**、GPT-5.6(橙)。每个点上方标分数。图标题小字 "JobBench main, identical scaffold and judge"。
右下角一个小插图(占 25%):训练 reward 曲线,竖线标 checkpoint。
底部一行小字:paper reports GPT-5.5 at [GPT-5.5 reported] under its own harness。

## 10. Synthetic data against expert-written data

一张柱状图占满。四根柱子:Base(灰)、Expert [n_expert](灰)、Synthetic [N](蓝)、Synthetic in-dist [N_in](深蓝)。柱顶标 +Δ,误差条。橙色虚线 GPT-5.5。
柱状图下一行小字:our judge for all models, [k] samples。

## 11. Generalization 1: across model scales

分组柱状图占满。三组 4B / 9B / 27B,每组 Base(灰)、+synthetic(蓝),柱顶标 +Δ,误差条,小字 "JobBench main"。

## 12. Generalization 2: across benchmarks

两根柱占满:GDPval 27B Base(灰)、ours(蓝),柱顶标分数,误差条。小字 "GDPval, never seen in training, [grader]"。纵轴不要从 0 画到 100 就把差异压扁,也不要截断到看起来夸张;从 base 往下留一段即可,并在轴上标清起点。

## 13. Generalization 3: across judges

左 40%:两栏对比卡片。**Coding agents**:unit test → pass/fail(打勾图标)。**This work**:rubric → LLM judge → score(无打勾)。
右 60%:两根柱,judge A(training)、judge B(evaluation),都涨,标 +Δ。柱下一行小字 "gains survive a judge swap"。
底部横幅一行:**no verifier needed**

## 14. Takeaways, and what this gives Microsoft

五行大字,每行一个数或一个原则加粗,第一行下面缩进三个子证据(小字)。
底部一条分栏:**The recipe**(pipeline、training config、[N] tasks、checkpoints)| **Built, not yet trained**(一个大数字 **[M] tasks**,小字 "same pipeline, [M/N]× the training set")| **Open**(scale-up curve、seed-source control、calibration、cross-scaffold)。中间那栏用蓝色,是这一条分栏的视觉重心。
最后一步出现结束语,一行,居中,加粗。

不放投稿信息在页面上,口头一句带过。

## Backup

每页一张表或一个列表,不做设计。B1 到 B6 对应讲稿里的 backup 清单。
