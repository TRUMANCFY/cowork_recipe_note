# 20 分钟 Presentation Draft

*2026-09-29。14 页加 backup。第 2 到 5 页 motivation,第 6 页全局,第 7 页造数据,第 8 页训练,第 9 到 10 页主结果,第 11 到 13 页三个泛化,第 14 页收拢。方括号是待填的数。*

**总原则**:这是一个"我学到了什么"的报告,不是论文宣讲。每一页只讲一件事,页面上只放一个数字或一张图,其余靠讲。

---

## 1. Title(0.5 min)

**Synthesizing Training Data for Professional Agent Tasks Without Annotators**

副标题:Toward training data for Office-scale agent work

一句话开场:"Copilot already ships agents that build spreadsheets and documents. The question I spent twelve weeks on is where the training data for that kind of work can come from, when customer documents are off the table."

---

## 2. The workload already exists in the product(1.3 min)

**标题**:Copilot ships this workload today

页面:两三个产品事实,全部来自 M365 公开博客。Excel Agent Mode 在 SpreadsheetBench 上 [Excel Agent Mode 分数],人类基线 [人类基线];Office Agent 生成 Word 和 PowerPoint 交付物。一行任务形态:给一堆文件,产出 xlsx 和 docx。

讲:这一页只做一件事,让听众确认"我们说的任务是产品里正在跑的那种",不是学术上抽象出来的。什么都不推论,推论留给后面。

**takeaway**:任务是真实的,而且是这个组的。

---

## 3. Who does it today, and at what cost(1.5 min)

**标题**:Frontier models do this work. Models you can own don't. Yet.

页面:JobBench main 的一条横向刻度,标题小字 "as reported"。从左到右:"open models outside Claude/GPT: none above [开源上限] as of May"、Sonnet 4.6 + Claude Code [分数]、GPT-5.4 + Codex [分数]、GPT-5.5 + Codex [分数]、Opus 4.7 + Claude Code [分数]、GPT-5.6 [分数]、Claude Fable 5 [分数]。**在 GPT-5.5 旁边留一个空位(虚线圆,紧贴 GPT-5.5 的标记),不标,第 6 页再揭开。** 刻度下方一行成本:一次完整评测 Opus 4.7 加 Claude Code 约 [成本] 美元,GPT-5.5 加 Codex 约 [成本] 美元。

讲:今天做得好的全是闭源前沿模型,而且按 M365 的调用量,这个单任务成本不可持续。能自己训、自己部署、成本可控的模型,在这个任务上还没有成绩;开源里追上来的也是 [参数量] 的巨型模型。**差距不是几分,是能不能做。** 成本这件事只在这一页说,后面不再提。

**takeaway**:值得做,因为差距大且贵。

---

## 4. The training data does not exist(1.3 min)

**标题**:Three places it could come from. None of them work.

页面:三条,每条一行原因,最后一行结论。

- **专家写**:JobBench 一类的 benchmark 每题几小时,一百多题,只能评不能训
- **客户文档**:最像真实工作,但在合规边界之内,碰不得
- **现成的合成数据**:代码和数学的成熟,Office 形态的没有

最后一行:**只剩一条路,从公开数据合成。**

讲:要让自己的模型学会这个工作负载,需要任务形态对的训练数据。三个来源,两条走不通,一条不存在。合成不是偏好,是排除法之后唯一的选项。这一页要短,结论要落在最后一行上。

**takeaway**:为什么是合成,一页说完。

---


## 5. Why synthesis is feasible now(1.2 min)

**标题**:Professional tasks are more verifiable than they look

页面:三条。

- **任务形态刚被定义清楚。** JobBench 130 题、ALE 150 公开、Workspace-Bench 154,全是 2026 年 5 到 6 月的工作。一个工作区、多个交付物、链式 rubric,形态有共识了。
- **交付物里大部分东西可以对照数据核对。** rubric 锚在文件里的具体数上,不是印象分。JobBench 的 rubric 里"计算事件率为 [数值]"、"识别 CHK-1402 没有对应发票"这类占多数,不是主观判断。
- **RL 要的是任务,不是示范。** SFT 需要教师轨迹,RL 只需要有可评分交付物的任务。瓶颈是造任务不是造轨迹,而造任务便宜得多。

讲:上一页说只剩合成这条路,这一页说这条路走得通。以前走不通是因为形态不清、验证被当成主观;现在两个前提都变了。讲完直接接下一页的全局图。

**takeaway**:可验证性是 RL 在这个领域成立的前提,而它比预期充分。

---


## 6. Big picture, and the headline result(2.5 min)

**标题**:Synthesize tasks, train on them

页面:上半是一张横向流程图,分左右两半,中间一个共享的方框。

- 左半 **Synthesis**:public data → task and rubric proposal → sandbox feasibility check → filter
- 中间 **[N] feasibility-checked tasks with rubrics**
- 右半 **Training**:Qwen3.6-27B rolls out in the same sandbox → LLM judge scores deliverables against the rubric → DPPO update
- 最右 **JobBench main**(frozen)

**sandbox 这个框横跨两半,用颜色标出。**

下半一条横幅:**[ours] vs GPT-5.5 at [GPT-5.5 our judge], identical minimal scaffold and judge for every model.** 横幅右侧一条**同 judge 的小阶梯**:GPT-5.4、GPT-5.5、ours、GPT-5.6,全部是你测的数,ours 蓝色。小字:Qwen3.6-27B,trained on [N] synthesized tasks,[k] eval samples,base [base];on the reported scale of slide 3, ours sits next to GPT-5.5 ([GPT-5.5 reported])。

讲:先指着横幅说结果。一个 27B 的开源模型,用 [N] 个合成任务训出来,在同一个最简 scaffold 和同一个 judge 下略高于 GPT-5.5。然后回到第 3 页那个空位:按论文报告的口径,我们落在 GPT-5.5 旁边;按同条件测,略高于它。两句一起说,把口径和公平性两个问题提前关掉。然后让听众知道后面所有细节是为了解释这个数。然后回到流程图:整件事是一个循环的两半,左边造任务,右边在任务上做 RL。连接两半的是同一个沙箱:合成时 GPT-5.5 在里面把任务做一遍确认能做,训练时 27B 在里面跑 rollout。**注意 reward 完全是 rubric 的:没有 unit test,没有程序化 verifier,judge 看交付物按 rubric 打分。** 这和 coding agent 的训练不一样,后面第 12 页说它为什么还是成立。 接下来四页讲左半、右半各是怎么做的,第 10 页给它的对照。

**takeaway**:结果先给,细节后补。

---


## 7. Two choices that make the tasks trainable(1.5 min)

**标题**:What the synthesis side has to get right for RL to work

页面:左右两栏,每栏一个数或一张小图,底下一行写它对训练的意义。

**左:可行性由执行确认,不由模型判断。** 漏斗 [seeds] seed files → [candidates] candidates → [N]([存活率])。LLM 写任务和 rubric,GPT-5.5 在沙箱里真做一遍,做不出来的任务丢掉。
→ 对训练的意义:**训练集里每个任务都是可完成的。** 近 [淘汰率] 的候选在沙箱里做不出来,说明 LLM 看着数据提出的任务相当一部分不可完成;不过滤,RL 会在不可能的任务上烧 rollout。

**右:任务从目标来,不从轨迹来。** 从职业活动里采一个目标写死,再去做,而不是从执行轨迹反推任务。这一条来自同事的 feedback,改了整个管线的锚点。
→ 对训练的意义:**模型学不到泄题的捷径。** 从轨迹反推的 instruction 会贴着解法写,RL 学到的是复述路径。

**takeaway**:两条都是为了让右半边的 reward 有意义。

---

## 8. Training: a standard agentic RL recipe on a new environment(1.3 min)

**标题**:Keep the RL recipe standard, rebuild the environment and the reward

页面:左右两栏。

**标准的部分**:异步 rollout、group-based advantage、DPPO 一类的 clipped policy gradient、无 KL 惩罚、常数学习率。全部是现成的开源实现,一行没改。

**重建的部分**:
- 每条 rollout 一个独立的 Apptainer 工作区,挂载任务的全部参考文件
- agent 用最简的 bash scaffold 读文件、写代码、产出 xlsx 和 docx 交付物
- reward:task-level rubric score,LLM judge 看交付物,**无程序化 verifier**
- 训练规模:Qwen3.6-27B,64 H100,[x] 步

讲:RL 算法这一侧是有意不动的,用的是终端 agent 训练里已经验证过的标准配方,把风险集中在新的部分。新的部分是两样:环境从一个容器加单元测试变成一个工作区加多份交付物,reward 从测试退出码变成 rubric 分数。有人问用的是哪个配方再具体说。

**takeaway**:这个项目的训练贡献不在算法,在把成熟的 agentic RL 配方接到了 Office 形态的任务上。

---


## 9. Result: a 27B open model against frontier models, identical conditions(2 min)

**支撑 takeaway 4**

**标题**:Same scaffold, same judge, every model

页面:一条横向阶梯,五个点:Base [base]、GPT-5.4 [分数]、GPT-5.5 [GPT-5.5 our judge]、ours [ours]、GPT-5.6 [分数],全部在你的 judge 和最简 scaffold 下测,带误差条。ours 蓝色,其余橙色和灰色。右下角一个小插图:训练 reward 曲线,标出选用的 checkpoint。小字:paper reports GPT-5.5 at [GPT-5.5 reported] under its own harness and judge。

讲:第 6 页的横幅在这里展开成完整的阶梯。所有点同一个 scaffold、同一个 judge,前沿模型在自己 harness 下的报告数也放在小字里,两个口径都给。插图说明训练是稳定上升到这个 checkpoint 的,不是某一步的偶然。

**takeaway**:能自己拥有的模型,在这个工作负载上做到了。

---

## 10. Synthetic data against expert-written data(1.3 min)

**支撑 takeaway 2**

**标题**:No annotator, no loss

页面:三根柱子加参照虚线,误差条。Base(灰)、[n_expert] expert-written tasks(灰)**[+Δ_expert]**、[N] synthetic tasks(蓝)**[+Δ_public]**、[N_in] synthetic tasks with in-distribution seeds(深蓝)**[+Δ_in]**。橙色虚线 GPT-5.5 [GPT-5.5 our judge]。角落小字:JobBench main, our judge for all, [k] samples。

讲:合成的用公开数据当种子时追平专家,用分布内文件当种子时超过。**能说的是:不用标注员,效果不比标注差。** 两组合成之间的差异说明种子来源有影响,但没做控制实验,不归因。顺带一句:[N] 个任务就够了,而且训练日志显示只有其中一部分产生梯度,数据效率比想象高。

**takeaway**:合成替代标注,成立。

---

## 11. Generalization 1: across model scales(1.3 min)

**支撑 takeaway 1**

**标题**:The same synthetic data helps at every scale

页面:分组柱状图占满。三组 4B、9B、27B,每组 Base(灰)和 + synthetic(蓝),柱顶标分数和 +Δ,误差条。小字:JobBench main, our judge, [k] samples。

讲:同一批 [N] 条合成数据,不针对任何尺寸调整,在三个尺寸上都带来提升。数据的价值不绑定在某一个模型上。

**takeaway**:跨尺寸成立。

---

## 12. Generalization 2: across benchmarks(1.3 min)

**支撑 takeaway 1**

**标题**:It transfers to a benchmark it never saw

页面:两根柱占满,GDPval 上 27B Base [GDPval base](灰)和 ours [GDPval ours](蓝),误差条。小字:GDPval [子集,例如 gold 220], [grader 和分数定义], [k] samples, never seen in training。

讲:训练时从没见过 GDPval。它来源不同、任务作者不同、评分方式也不同,提升依然在。**不是只对 JobBench 过拟合。** base 已经很高,绝对提升不大,说"在一个接近饱和的 benchmark 上依然有提升",别夸大。

**takeaway**:跨 benchmark 成立。

---

## 13. Generalization 3: across judges(1.5 min)

**支撑 takeaway 1 和 takeaway 3**

**标题**:The reward is only a rubric, and the gain does not depend on the judge

页面:左 40%:两栏对比卡片。**Coding agents**:unit test → pass/fail,可验证。**This work**:rubric → LLM judge → score,无 verifier。右 60%:两根柱,训练用 judge A、评测换成 judge B,增益 [Δ_judgeB] 仍在。柱下一行小字:"if the model had learned judge A's preferences, this would collapse"。

讲:这项工作的 reward 完全是 rubric,judge 打分,没有 coding agent 那种 unit test。这类 reward 理论上可以被讨好。检验方法是换一个 judge 评:如果增益是 hack 训练 judge 得来的,换 judge 就消失;实际是 [Δ_judgeB] 仍在。所以增益是真实的,而且纯 rubric 的 reward 在这个工作负载上够用。

**takeaway**:跨 judge 成立,因此不需要可验证 reward。

---


## 14. Takeaways, and what this gives Microsoft(1.5 min)

**标题**:Five things to take away

页面:五条,每条一个数或一个可复述的原则。第一条带子证据。

- **泛化是稳健的**
  - 跨模型尺寸:4B / 9B / 27B 都提升
  - 跨 benchmark:训练没见过的 GDPval 上也提升
  - 跨 judge:换一个 judge 评,增益仍在
- **合成 instruction 和 rubric 可以替代人工标注**,效果不低于专家数据;而且 [N] 个任务就够,终端 agent 的同类工作用了 [参照环境数] 个环境
- **不需要像 coding agent 那样的可验证 reward。** 纯 rubric 就够,因为增益不依赖训练时的 judge
- **一个 27B 开源模型在同条件下超过 GPT-5.5**
- **配方不用重新发明,瓶颈在环境和 reward。** RL 算法一行没改;管线只用公有领域数据,内部可直接接手

**留下的**:pipeline code、[N] feasibility-checked tasks with rubrics、checkpoints。

**已经造好、还没训的**:同一条管线已经产出 **[M] 条任务**,是训练集的 [M/N] 倍。受算力限制这次只在 [N] 条上训练;在全量上训能不能继续提升,是最直接的下一步。

**开放的**:全量 [M] 条上的扩展曲线、种子来源为什么影响效果(需要控制实验)、难度校准、跨 scaffold 训练。

**投稿**:ACL 2027(ARR 12 月或 1 月轮),作者名单和内部审查流程待定。

讲这一页时点一句:"Everything you saw came from [N] tasks. The same pipeline has already produced [M]. We did not have the compute to train on them, and that is the first thing I would do next."

结束语一句:"The pipeline needs no annotator, no customer data, no verifier, and no new algorithm. What it needs is good seed files and a sandbox, and this org has both."

---



## 时间分配

| 段 | 页 | 分钟 |
|---|---|---|
| Motivation | 1 到 5 | 5.8 |
| Big picture + headline | 6 | 2.5 |
| Data | 7 | 1.5 |
| Training | 8 | 1.3 |
| Main results | 9 到 10 | 3.3 |
| Generalization | 11 到 13 | 4.1 |
| Close | 14 | 1.5 |

合计约 20 分钟。问答另算。

---

## Backup(不讲,被问到再翻)

- **B1 过程里学到的**:有效数据远小于名义数据([n] 个任务里 [m] 个贡献 [比例] 增益);早期 8B 在专家任务上不泛化而合成集在三个尺寸上都泛化;task-level reward 太稀。
- **B2 数据来源与许可**:用了的(SEC XBRL、USASpending、Socrata CC0 子集、Federal Register)和放弃的(Framingham 商业使用受限、PMC NC 子集、RSMeans 商业产品)。"公开"不等于"可商用"。
- **B3 没做成的**:operator-walk(单文件出不了调和题);难度校准(未做)。
- **B4 训练配置全表**:框架、算法、超参、和参照配方的差异(LR 3e-6 对 1e-6)。
- **B5 judge**:用的是什么,和官方 Grok judge 在同一批上的差值 [差值] 分。
- **B6 为什么不做 SFT**:GPT-5.5 的轨迹只用来确认可行性,不进训练;SFT 基线是 future work。
- **B8 [M] 条数据池**:生成成本(GPT 调用数、时间)、按源分布、与训练集 [N] 条的同分布抽样检查。
- **B7 漏斗按源拆**:[seeds] → [candidates] → [N],四个数据源各自的存活率。

## 讲之前要确认的九件事

1. 第 6 页横幅和第 9 页阶梯是核心。**先确认 easy 和 main 的文件无重叠**,否则 [+Δ_in] 和 [ours] 都要打折。
2. 第 9、10 页的误差条要真的画,[k] 次采样的标准差。
3. 第 7 页提到同事 feedback,如果那位同事在场,提前告诉他你会提这一点。
4. 第 2、3、14 页里关于产品和内部的说法只用公开博客的数字;涉及内部约束的表述讲之前和 Sihao 对一遍。
5. 第 8 页的 reward 定义、第 9 页的曲线插图、其余占位按实际填。
6. 第 3 页的前沿模型数字来自论文和公开 leaderboard,harness 口径未必一致,说"公开报告的数字"。
7. **用你的 judge 把 GPT-5.4 和 GPT-5.6 也跑一遍**,第 9 页那条阶梯才完整。
8. **checkpoint 是按什么选的。** 按 main 选的话 main 就是 dev set,第 9 页的插图要用 held-out 曲线,主结果靠 [k] 次采样撑。
9. **第 12 页 GDPval 两件事要讲清楚。** 一是评分口径:官方 GDPval 报的是对专家交付物的胜率,[GDPval base] 这个量级听众会以为是胜率,页面上必须写明你用的是什么 grader 和什么分数。二是去污染:确认合成数据的种子文件和 task 没有取自 GDPval,否则它就不是 held-out。
