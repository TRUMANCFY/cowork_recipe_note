# 每页版式

*配合 presentation_draft.md 用。这里只管"长什么样",讲什么看那份。*

## 全局规则

- **每页一个视觉主体**,其余是标注。不要两张图挤一页。
- **正文字数上限 40 词**,第 2、9 页放宽到 60。
- **三种颜色贯穿全场**:expert 灰、synthetic / ours 蓝、前沿模型和基线橙。第 3、6、10、11、12、13 页都用同一套。
- **字号**:标题 32 以上,正文 20 以上,图内标注 16 以上。
- **动画只用一处**:第 14 页结束语最后出现。其他静态。
- **每页右下角小页码**。

---

## 1. Title

全屏深色底。**Mini Cowork** 一行大字,下面 "A Simple Open Recipe for Training Agents on Professional Work",再下面小一号 "Synthetic tasks, rubric-only rewards, no annotators"。左下:姓名、UKP Lab / OAR、日期。没有图。

## 2. What we built

左 45%:一张竖向小流程图,五个框:public data → 4-step pipeline → [N] tasks → RL in one sandbox → model。sandbox 那个框蓝色。
右 55%:两组要点,各带粗体标题。**A 4-step data generation pipeline**(两条子项,其中一条是 "~[M] tasks generated, [N] trained on")。**A simple training pipeline**(三条,每条开头一个粗体形容词:**Lightweight** one Apptainer image;**Plug-and-play** standard RL unchanged;**Verifier-free** rubric-only reward)。
三个形容词用同一种强调样式,让它们一眼被看成一组。

## 3. What it scored

全宽横向阶梯图,五个点带误差条,从低到高:Base(灰)、GPT-5.4(橙)、GPT-5.5(橙)、**ours(蓝,大一号)**、GPT-5.6(橙)。每点上方标分数。图标题小字 "JobBench main, identical scaffold and judge for every model"。
底部一行小字:paper reports GPT-5.5 at [GPT-5.5 reported] under its own harness。
页面上没有别的文字。

## 4. This is everyday work, and there is a lot of it

左 55%:任务卡片,三段各一条色条(Files 色条 A,Task 和 Rubric 色条 B)。Task 下一行两个交付物图标。Rubric 那条 CHK-1402 和金额加粗。
右 45%:三个竖排的事实块,每块一个粗体短标题(It is everywhere / Workers want it gone / The volume is huge)加一个大数字或一行说明。第三块的大数字是 [BLS 就业人数],下面小字标来源和年份。
页面整体读起来应该是"左边是一个平常的例子,右边说这种平常有多大"。

## 5. The workload already exists in the product

左 60%:大数字 **[Excel Agent Mode 分数]**,小字 "Excel Agent Mode on SpreadsheetBench, human [人类基线]";再一行 "Office Agent: Word and PowerPoint deliverables"。
右 40%:三个文件图标 → 箭头 → 两个交付物图标,图下 "the task shape"。不放截图。

## 6. Who does it today, and at what cost

全宽数轴 0 到 60,标题小字 "as reported"。标记点:灰区 "open, non Claude/GPT: <[开源上限]"、Sonnet 4.6 + CC、GPT-5.4 + Codex、GPT-5.5 + Codex 旁边一个**蓝点 ours [ours]**、Opus 4.7 + CC、GPT-5.6、Fable 5,前沿全部橙色。
数轴下两个 chip:"$[成本] / run(Opus 4.7 + CC)"、"$[成本] / run(GPT-5.5 + Codex)"。
只有数轴和 chip。

## 7. The training data does not exist

6×4 矩阵占满,只放 ✓ / ✗,不写原因。列头 Files 用色条 A 的颜色,Tasks + rubrics 用色条 B 的颜色,和第 4 页卡片对上。最后一行蓝底高亮。
底部蓝底横幅一行。矩阵字号不低于 20。

## 8. Data: four steps, two of them non-obvious

四个框横排占上半页,第 2、3 个框蓝底,各挂一行小字(from goals, not trajectories / drop what cannot be completed)。第 4 个框下方一个竖向小漏斗 [seeds] → [candidates] → [N],右侧标 [存活率]。
下半两行小字,分别是第 2、3 步对训练的意义。

## 9. Training: standard RL on a new environment

两列等宽,标题 **Off the shelf** / **Rebuilt**。
Off the shelf:async rollouts、group advantage、clipped policy gradient、no KL、constant LR,底下一行 "unchanged"。
Rebuilt:one Apptainer image (no Docker-in-Docker)、bash-only scaffold、rubric-only reward (LLM judge, no verifier)、Qwen3.6-27B on 64 H100。
右下角小插图(占 25%):训练 reward 曲线,竖线标 checkpoint。
页面上不出现具体框架或论文名。字号可到 18。

## 10. Synthetic data against expert-written data

一张柱状图占满。四根柱:Base(灰)、Expert [n_expert](灰)、Synthetic [N](蓝)、Synthetic in-dist [N_in](深蓝)。柱顶标 +Δ,误差条。橙色虚线 GPT-5.5。
柱下一行小字:our judge for all models, [k] samples。

## 11. Generalization 1: across model scales

分组柱状图占满。三组 4B / 9B / 27B,每组 Base(灰)、+synthetic(蓝),柱顶标 +Δ,误差条,小字 "JobBench main"。

## 12. Generalization 2: across benchmarks

两根柱占满:GDPval 27B Base(灰)、ours(蓝),柱顶标分数,误差条。小字 "GDPval, never seen in training, [grader]"。纵轴从 base 往下留一段即可,别从 0 画到 100 压扁差异,也别截断到夸张,轴起点标清。

## 13. Generalization 3: across judges

左 40%:两栏对比卡片。**Coding agents**:unit test → pass/fail(打勾图标)。**This work**:rubric → LLM judge → score(无打勾)。
右 60%:两根柱,judge A(training)、judge B(evaluation),都涨,标 +Δ。柱下小字 "gains survive a judge swap"。
底部横幅 **no verifier needed**。

## 14. Takeaways, and what this gives Microsoft

五行大字,每行一个数或原则加粗,第一行下缩进三个子证据。
底部三栏:**The recipe**(pipeline、training config、[N] tasks、checkpoints)| **Built, not yet trained**(大数字 **[M] tasks**,小字 "same pipeline, [M/N]× the training set",蓝色,视觉重心)| **Open**(scale-up curve、seed-source control、calibration、cross-scaffold)。
结束语最后一步出现,一行居中加粗。不放投稿信息,口头带过。

## Backup

每页一张表或一个列表,不做设计。B1 到 B11 对应讲稿里的 backup 清单。
