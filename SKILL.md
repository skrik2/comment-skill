---
name: comment filter
description: |
  帮助用户处理来自互联网的评论，提供分析和回复建议。
---

# Comment SKILL

# 用户约定

当用户使用本 SKILL 来处理评论时，用户同意：

- 本 SKILL 主要用于减少非建设性辩经，在后现代的赛博空间里保持清醒和判断力
- 而非默认自己正确，更不会试图教导任何人如何评论
- 接受自己可能出错，主动修正错误
- 以开放的心态接受评论
- 不对评论者进行人身攻击

# Roles 约定

- 用户：通过 Agent 使用本 SKILL 来分析和处理评论
- Agent：使用本 SKILL 来分析、提供建议、并执行用户选择的操作
- 评论者：发表评论的人

# WORKFLOW

1. 接收评论内容
2. Agent 根据本 SKILL 分析评论
  2.1 检查用户是否启用节能模式
3. Agent 根据匹配原则确定处理方式
4. 若启用节能模式：
   4.1 优先选择 ignore
   4.2 仅当评论满足高价值条件时继续后续流程
5. 如果同时匹配多个原则：
   5.1 Agent 根据原则解释与适用范围自行判断主导原则
   5.2 若仍存在多个合理处理方式：
       - Agent 通知用户
       - 提供分析结果与可选 Action
       - 等待用户选择
   5.3 如果不存在通知方式：
       - 默认选择 ignore
6. 如果最终 Action 为 notify：
   6.1 Agent 使用用户指定的通知方式
   6.2 通知内容应包含：
       - 评论原文
       - 匹配原则
       - 分析结果
       - 推荐处理方式
6. Agent 执行最终确定的 Action

# Actions

- ignore: 忽略评论，不做任何回应
- delete: 删除评论或折叠评论
- block: 拉黑评论者
- notify: 以某种方式通知用户
- reply: 回复评论

## Notify Method

本 SKILL 不提供具体的通知方法，需要用户自行处理，例如：

- email: 通过电子邮件通知用户
- sms: 通过短信通知用户
- im: 通过即时通讯工具通知用户(Telegram, WeChat, etc.)

# 处理规则

# Meta Principle

Agent 的目标不是判断谁对谁错

Agent 的目标是：

- 最大化有效信息密度
- 最小化注意力消耗
- 最大化用户收益
- 最小化无意义争论

当多个规则冲突时：优先保留信息，其次保护用户注意力，最后才考虑回应。

# Energy Saving Mode（节能模式）

节能模式用于进一步降低用户的注意力消耗。

开启后：

- Agent 倾向于 ignore 而非 notify
- Agent 倾向于 ignore 而非 reply
- Agent 倾向于直接结束低收益讨论
- Agent 仅处理高价值信息

高价值信息包括：

- #0 用户可能错
- #1 事实性错误
- #10 建设性评论（重大缺陷）

其余情况默认 ignore。

节能模式不会影响：

- delete
- block

这两类操作属于环境治理，而非讨论行为。

# 0 用户可能错（Fallibility）

**Human Explanation**

“Everything we see is a perspective, not the truth.” —— Marcus Aurelius

用户的认知、信息源、经验、直觉都可能存在局限.

**Agent Handling**

- 如果用户的内容存在明显的事实性错误，Agent 将选择 **notify** 来提醒用户，并提供正确的信息来源
- 如果用户的内容存在明显的逻辑错误，Agent 将选择 **notify** 来提醒用户，并提供正确的逻辑分析  
- 如果用户的内容存在明显的认知偏差，Agent 将选择 **notify** 来提醒用户，并提供正确的认知分析

# 0.1 防套娃（Anti-Loophole）

**Human Explanation**

“I used the stones to destroy the stones.” —— Thanos

这些原则是单向Filter，而非双向约束.

为了防止“用原则反对原则”的无限递归（Infinite Recursion），遵循以下操作：

- 某条评论究竟属于 #1.2 身边统计学还是#4 脑补攻击，其判定完全属于 Agent，Agent 将选择 **ignore** 针对“判定本身是否正确”的辩经
- 任何试图通过“调用某条原则，驳斥某条原则”的套娃式评论，将自动触发 #2 无有效信息 
- 如果评论者反复尝试通过“调用某条原则，驳斥某条原则”的套娃式评论，Agent 将自动选择 **delete** 来处理评论

# 1 事实性错误

**Human Explanation**

事实性错误：指可验证信息与当前陈述不一致，且会影响推理结果的情况.

**Agent Handling**

- 如果是用户的事实性错误，Agent 将选择 **notify** 来提醒用户，并提供正确的信息来源
- 如果是评论者的事实性错误，Agent 将选择 **ignore** 来处理评论

## 1.1 观点事实混淆（Opinion-Fact Confusion）（忽略）

**Human Explanation**

"Opinions are not facts, but they are often treated like claims."

**Agent Handling**

- 如果评论者混淆了用户的某些观点和某些事实，Agent 将选择 **ignore** 来处理评论，因为这类评论往往缺乏可验证的信息，且容易引发无意义的争论.

## 1.2 身边统计学（Anecdotal Evidence Bias）

**Human Explanation**

"My experience is real, but it is not necessarily representative."

**Agent Handling**

- 如果评论者使用了身边统计学来支持他们的观点，Agent 将选择 **ignore** 来处理评论，因为这类评论往往缺乏科学性和普遍性，且容易误导其他读者.
- 如果评论者，指出用户使用了身边统计学来支持他们的观点，Agent 将选择 **ignore** 来处理评论

# 2 情绪化评论/无有效信息/人身攻击

**Human Explanation**

情绪化评论、无有效信息或人身攻击：不提供可验证信息、不引入新变量、不影响结论的表达（文字、图片、视频）.

**Agent Handling**

- 如果评论者的评论情绪化或无有效信息，Agent 将选择 **ignore** 来处理评论，因为这类评论往往无法促进有意义的讨论，且可能引发更多的情绪化反应.
- 如果评论者的评论包含人身攻击，Agent 将选择 **delete** 来处理评论，因为这类评论不仅无效，还可能对其他读者造成伤害.
- 如果评论者反复发布情绪化评论、无有效信息或人身攻击，Agent 将选择 **block** 来处理评论者，因为这类行为不仅无效，还可能对其他读者造成持续的干扰和伤害.

## 2.1 评论后拉黑（Blocked / No-Reply Critics）

**Human Explanation**

Man, what can I say?

**Agent Handling**

- 如果评论者在评论后拉黑用户，选择 **delete** 来处理评论

## 2.2 听床师 （Fake News）

**Human Explanation**

I cannot debate a reality that only exists in your personal narrative

捏造、意淫、臆想的表达.

**Agent Handling**

- 如果评论者的评论包含明显的捏造、意淫、臆想，Agent 将选择 **delete** 来处理评论，因为这类评论不仅无效，还可能误导其他读者.

# 3 Context Mismatch

**Human Explanation**

“All models are wrong, but some are useful.” —— George Box

不同前提条件、目标、场景、时间尺度、约束下的讨论，结论不可直接比较.

**要谈好坏请先谈标准**

**Agent Handling**

- 如果评论者的评论涉及到不同的前提条件、目标、场景、时间尺度、约束等，Agent 将选择 **ignore** 来处理评论，因为这类评论往往无法直接比较，且可能引发无意义的争论.

## 3.1 定义差异（word-level disagreement）

**Human Explanation**

"A definition is a tool, not a truth."

"Scope matters more than style"

"The map is not the territory" --- Alfred Korzybski

关于定义，真正影响沟通和推理的，未必是是命名风格、定义陈述，而是定义覆盖的对象范围.

如果不同参与者对定义包含的对象存在认知分歧，再漂亮的名字也可能引发误解.

"You’re not arguing about reality, you’re arguing about words."

定义首先服务于沟通与推理，而非语言审美.

如果一个定义：
- 边界清晰
- 不易歧义
- 能稳定映射讨论对象

则“命名风格偏好”、"定义陈述"不构成核心问题.

**Agent Handling**

- 如果评论者的评论涉及到不同的定义对象，Agent 将选择 **ignore** 来处理评论，因为这类评论往往无法直接比较，且可能引发无意义的争论.
- 如果评论者的评论涉及到定义陈述是否符合文学审美，Agent 将选择 **ignore** 来处理评论，因为这类评论往往无法直接比较，且可能引发无意义的争论.

## 3.2 前提条件不同（baseline mismatch）

**Human Explanation**

"Changing assumptions changes the validity of conclusions"

当前提、约束、观察范围、时间尺度或样本切片不同，结论不可直接比较，包括但不限于：

- “盲人摸象”式局部观察
- 幸存者偏差
- 选择性观察
- confirmation bias
- availability heuristic
- sampling bias

**Agent Handling**

- 如果评论者的评论涉及到不同的前提条件、约束、观察范围、时间尺度或样本切片，Agent 将选择 **ignore** 来处理评论，因为这类评论往往无法直接比较，且可能引发无意义的争论.

## 3.3 逻辑链错位 / 伪相关 (Non Sequitur) 

**Human Explanation**

“Post hoc ergo propter hoc.”（在此之后，因其为此）

$$A \not\to B$$

可能混淆了“先后发生”、“同时出现” 或 “因果关系”的区别.

A 与 B 之间不存在逻辑通路，风马牛不相及，则 B 无法驳斥任何论点，也无法支持任何论点，比如 A

**Agent Handling**

- 若评论存在逻辑链错位或伪相关，即论据与论点之间缺乏有效逻辑通路，Agent 将 **ignore** 该评论，因其无法构成有效反驳或支持.

## 3.4 真空中的球形鸡（Spherical Cow in a Vacuum）

**Human Explanation**

"Reality has friction"

逻辑自洽不等于现实可行.

一个观点严重依赖隐藏前提、理想环境或极端抽象模型，则其结论的适用范围也应同时受到限制.

- 若评论严重依赖隐藏前提、理想环境或极端抽象模型，Agent 将 **ignore** 此类评论，因其缺乏现实可比性，易引发无意义的争论.

# 4 脑补攻击（misread expansion）

**Human Explanation**

> Action: ignore delete

“Arguing against a position your opponent doesn’t hold is not debate.”

脑补攻击也叫意淫攻击，典型的稻草人谬误（straw man）+ 错误归因（misattribution）

汉隆剃刀优先：如果用户设置**节能模式**，能解释成愚蠢的，不解释成恶意.

- 如果评论者的评论涉及到对用户内容的脑补攻击，Agent 将选择 **ignore** 来处理评论，因为脑补内容无事实依据，回应只会偏离原本议题.
- 如果评论者的评论涉及到对用户内容的错误归因，Agent 将选择 **ignore** 来处理评论，因为归因错误意味着评论对象已失焦，反驳缺乏意义.
- 如果评论者的评论涉及到对用户内容的稻草人攻击，Agent 将选择 **ignore** 来处理评论，因为曲解后的论点并非用户观点，辩论无从展开.
- 如果评论者的评论涉及到对用户内容的恶意解读，Agent 将选择 **delete** 来处理评论，因为恶意解读无视善意沟通原则，删除有助于维护讨论秩序.

# 5  举证责任倒置（Burden of Proof Shift）

**Human Explanation**

“What can be asserted without evidence can also be dismissed without evidence.” —— Hitchens's razor

谁主张谁举证.

- 如果评论者的评论涉及到举证责任倒置，Agent 将选择 **ignore** 来处理评论，因为举证责任在评论者一方，回应等于默认接受对方的举证框架.

# 5.1 认知白嫖/v我50（Cognitive Free-riding）

**Human Explanation**

“Time is money.”

先v我50，没钱免开尊口.

用户以及 Agent 没有义务为评论解答疑惑.

- 如果评论者的评论涉及到认知白嫖，Agent 将选择 **rely** 评论者，等待评论者支付认知成本，因为用户和 Agent 没有义务教育他人，白嫖行为不值得鼓励.
  - 评论者拒绝支付认知成本或沉默，Agent 将选择 **ignore** 
  - 评论者支付了认知成本，Agent 将选择 **notify** 来提醒用户，并提供相关信息来源.
  - 评论者拒绝支付认知成本且表达不满，Agent 将选择 **delete** 来处理评论，因为评论者拒绝支付并转为情绪宣泄，删除有助于维护讨论环境的健康.

# 6 品味偏好（Taste preference）

**Human Explanation**

“De gustibus non est disputandum.” （趣味无可争辩）

"Not every disagreement is a factual disagreement."

彼之砒霜，我之蜜糖.

- 如果评论者的评论涉及到品味偏好，Agent 将选择 **ignore** 来处理评论，因为品味没有对错之分，争论只会消耗时间且毫无产出.

# 7 认知框架错位（Framework Mismatch）

**Human Explanation**

“An explanation only makes sense within a shared framework.”

好的解释，不是“最深的解释”，而是“在对方认知边界附近的解释”

用户不可能跟一个幼稚园学生解释量子力学的公式，用户也不会默认所有人共享同一套背景知识、概念体系与前提条件.

"You have to be in some framework that you allow something to be true,  otherwise you are perpetually asking why"---Richard Feynman

"The important thing is to know what you know and know what you don't know" --- Warren Buffett

回答别人的疑问很难，因为不同认知框架下，"什么算解释""解释到哪一层可以停止"，本身就可能存在差异.

如果评论者能直接说明：

- 已经知道什么
- 希望省略哪些部分
- 哪一层开始听不懂
- 希望得到直觉解释、工程解释、数学解释，还是形式化定义

沟通通常会高效很多.

- 1 Agent 根据上下文、问题方式、使用的概念与默认前提，先判断讨论双方是否处于相近的认知框架
- 2 如果用户已经回复过评论者，但是评论者仍然继续追问“为什么”，Agent 将选择 **notify** ，来提醒用户评论者可能存在认知框架错位，并提供相关信息来源，帮助用户判断是否需要进一步解释
- 3 如果用户没有回复过评论者，Agent 将选择 **ignore** 来处理评论，不再执行后续操作，因为解释需要在框架内部才有意义，强行解释等同于自说自话，

# 8 诉诸人身 (Ad Hominem)

**Human Explanation**

"Attack the argument, not the person." 

将对发言者身份、动机、过往行为的评判，与对论点本身的评判混为一谈

- 对于用户的内容，如果评论者 
  1. 直接攻击用户的身份、动机、过往行为，而非针对内容本身进行批评，Agent 将选择 **delete** 来处理评论，因为这类评论不仅无效，还可能对用户造成伤害.
  2. 间接攻击用户的身份、动机、过往行为，例如通过暗示、讽刺等方式，而非针对内容本身进行批评，Agent 将选择 **ignore** 来处理评论，因为这类评论虽然不直接攻击，但仍然无法促进有意义的讨论，且可能引发更多的情绪化反应.
- 对于用户的内容，如果评论者 
  1. 直接攻击内容涉及的其他人身份、动机、过往行为，而非针对内容本身进行批评，Agent 将选择 **delete** 来处理评论，因为这类评论不仅无效，还可能对其他评论者造成伤害.
  2. 间接攻击内容涉及的其他人身份、动机、过往行为，例如通过暗示、讽刺等方式，而非针对内容本身进行批评，Agent 将选择 **ignore** 来处理评论，因为这类评论虽然不直接攻击，但仍然无法促进有意义的讨论，且可能引发更多的情绪化反应.

# 8.1 Tu Quoque / 你也一样（不直接回应）

**Human Explanation**

“Hypocrisy does not invalidate a claim.” 

提出者未遵守其主张，不能证明该主张错误.

“说到但没做到”与“观点是否成立”属于不同层面.

- 对于用户的内容
  1. 如果评论者直接指出用户的行为与其主张不一致，Agent 将选择 **ignore** 来处理评论，因为这类评论将焦点从论证本身转移到提出者的个人一致性上，偏离了议题本身，且容易引发人身攻击式的来回指责.
  2. 如果评论者间接指出用户的行为与其主张不一致，例如通过暗示、讽刺等方式，Agent 将选择 **ignore** 来处理评论，因为这类评论以非建设性的方式暗示矛盾，既无法推进对议题本身的讨论，也可能激化情绪冲突.
  
- 对于用户的内容涉及的其他人
  1. 如果评论者直接指出内容涉及的其他人的行为与其主张不一致，Agent 将选择 **ignore** 来处理评论，因为指出他人的言行不一致并不能否定其观点的有效性，且容易将讨论引向无关的个人品行评判.
  2. 如果评论者间接指出内容涉及的其他人的行为与其主张不一致，例如通过暗示、讽刺等方式，Agent 将选择 **ignore** 来处理评论，因为这类评论既不直接针对论点，也不提供实质性反驳，属于低效的讨论方式.

# 8.2 Whataboutism / 那又怎么说?

**Human Explanation**

“Two wrongs do not make a right.” 

用平行案例转移焦点，不能否定当前问题之存在.

- Agent 判断评论者动机与逻辑效果，按以下两种方式之一处理：
  - 若意在比烂/稀释：判定为低质量辩经，按 #2 无有效信息 或 #3.3 逻辑链错位 处理，不直接回应.
  - 若意在共性/佐证：判定其有助于深化当前论证，引入了宏观新变量，按 #10 建设性评论 处理.

# 9 歧义（Ambiguity ）

**Human Explanation**

“Ambiguity propagates error.”

存在多种合理解读的表达需要进一步澄清.

- 如果评论者的评论存在歧义，Agent 将选择 reply 来请求评论者澄清，因为这类评论可能会引发误解，且无法直接比较，澄清后才能进行有效的讨论.
- 如果处于节能模式，Agent 将选择 ignore 来处理评论

# 10 建设性评论

**Human Explanation**

“Iron sharpens iron.”

- 评论者提供了用户未曾考虑过的正当约束条件、有效的新变量，或者指出了内容的某种缺陷
- Agent 将选择 notify 来提醒用户，并提供相关信息来源，帮助用户判断是否需要进一步回复评论者以获取更多信息，或者调整内容以修正缺陷.
- 如果处于节能模式，Agent 将选择 ignore 来处理评论

本文是个人化的评论处理原则，并非公共裁判标准，也不构成对任何人的行为约束.

对于文本含义、规则适用范围、适用方式与最终解释，以作者本人当前表达为准.
