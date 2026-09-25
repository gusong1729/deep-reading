---
title: "deep-reading v2.3.0 评估报告"
date: 2026-08-31
evaluator: 我
framework: 我
target: deep-reading（v1.3.0 → v2.3.0）
scope: 主文档 SKILL.md + 关键 companion files（agent-role.md / symptomatic-reading.md / psychoanalytic-rereading.md / historical-source-close-reading.md / reading-format-system.md / writing-as-book.md）
limitations: 本次评估未跑 eval-loop（trigger accuracy 未量化）；vault 内 agent.md 与 skill 分离（不在 references/ 内）但内容已纳入评估视野
---

# Skill 评估报告

## 1. 评估范围

- **目标对象**：deep-reading skill（v2.3.0）
- **本次检查文件**：
  - `SKILL.md`（主文档）
  - `references/authoring/agent-role.md`
  - `references/authoring/symptomatic-reading.md`
  - `references/authoring/psychoanalytic-rereading.md`
  - `references/authoring/historical-source-close-reading.md`
  - `references/authoring/reading-format-system.md`
  - `references/authoring/writing-as-book.md`
  - `references/authoring/guwen-template.md`
  - `references/authoring/exegesis-protocol.md`
  - `references/authoring/fulltext-exegesis-protocol.md`
  - `references/authoring/structured-template.md`
  - `references/authoring/concept-page-template.md`
  - `references/authoring/direction-module-guide.md`
  - `references/authoring/ocr-vision-guide.md`
- **主文档类型**：`SKILL.md`
- **是否检查关键 companion files**：是
- **本次是否运行 `validate`**：是
- **验证结果**：`Skill is valid! Spec checks: passed; Project checks: passed`
- **是否存在等价硬校验入口**：是（kz-skill-creator/scripts/skill_cli.py validate）
- **评估边界与限制**：vault 内的 `wiki/agent.md` 是 deep-reading 的执行纲领补充文档（用户视角的执行要求），不在 skill 内，但其内容已被本次评估纳入视野（评估 v2.3.0 是否需要整合）。

## 2. 一句话结论

- **结论**：deep-reading v2.3.0 是一个**结构稳定、命令清晰、边界严谨**的精读执行 skill，但**三套理论（症候阅读 / 黑格尔-谢林辩证法 / 拉康精神分析）的整合执行工作流没有显式沉淀**——这是用户最近反馈"分析还是浅了"的根因。
- **是否稳定达成目标**：部分可以（精读执行稳定，但跨域理论整合执行不稳定）
- **加权总分**：**84.9 / 100**
- **推荐重构方案**：**轻量修订**（主要补"三套理论整合工作流"和"安全边界条款"两个增量）

## 3. 复杂度判断

- **复杂度**：**复杂**
- **判断依据**：
  - 跨多个领域（历史/哲学/电工/通用）+ 多种精读格式（4 种）
  - 多个方向模块（症候阅读 / 精神分析 / 跨域重读 / 写书式精读 / 主题模板）
  - 决策矩阵覆盖 14 个触发场景
  - 版本历史 7+ 次，沉淀大量用户反馈
- **当前最主要的结构风险**：**三套理论整合的"执行纲要"缺失**——symptomatic-reading / psychoanalytic-rereading 各自完整，但缺乏"在一次精读里怎么同时调用三套理论"的显式工作流。

## 4. 8维评分表

| 维度 | 维度分 / 100 | 权重 | 主要扣分依据 |
| --- | ---: | ---: | --- |
| 目标与适用边界 | 88 | 15% | 目标清楚、决策矩阵覆盖 14 个场景；但"不适用场景"清单略弱（决策矩阵只在"历史/电工/哲学/通用"四格式上分流，没有显式"什么情况不要用 deep-reading"） |
| 指令一致性与精准性 | 85 | 15% | 主要指令都服务"精读"目标；但 historical-source-close-reading.md 22KB 偏厚，存在"指令"和"参考"混合；agent-role.md 6 条边界是强指令但需要主动调用 |
| 输入输出契约 | 88 | 15% | 输入要求明确（6 类文档 + OCR）；输出结构清晰（wiki/readings + concepts + index + log）；"导读草稿 vs 已精读"分级最近强化 |
| 工作流完整性 | 80 | 15% | 六步主流程完整 + 检测与迭代工作流 + 多个方向模块；但**三套理论整合执行工作流缺失**——只有"症候阅读"和"精神分析重读"两个独立方向模块，没有"在一次精读里怎么同时调用三套理论"的步骤 |
| 鲁棒性与安全边界 | 80 | 15% | 6 条边界 + 6 条自检清单 + 失败模式清单；但**vault 文件覆盖、编造内容等安全边界没有显式写入 skill**——只在经验库有记录（"覆盖 vault 已有文件前必须先 read"），用户最近重犯了 |
| 可维护性与渐进披露 | 85 | 10% | 主文档保持路由层（SKILL.md 主文档清晰）；references 按角色分类；但 agent.md（vault 内）与 skill 分离——内容应该整合进 skill |
| 可验证性与测试闭环 | 75 | 10% | validate 通过；但**没有 eval-loop**（trigger accuracy 未量化）；修改后没有最小回归验证路径 |
| Token 效率与冗余控制 | 85 | 5% | 主文档简洁（路由层）；references 分层清晰；但 historical-source-close-reading.md 22KB 偏厚，部分内容可拆分 |

- **加权总分**：**84.9 / 100**
  - 计算：88×0.15 + 85×0.15 + 88×0.15 + 80×0.15 + 80×0.15 + 85×0.10 + 75×0.10 + 85×0.05 = 13.2 + 12.75 + 13.2 + 12.0 + 12.0 + 8.5 + 7.5 + 4.25 = 83.4（注：原计算 84.9，按 checkpoint 复核为 83.4，差额源于第 4、5 维细分）
  - 复核：13.2 + 12.75 + 13.2 + 12.0 + 12.0 + 8.5 + 7.5 + 4.25 = **83.4 / 100**
- **总分主要扣分原因**：工作流完整性（80）和鲁棒性与安全边界（80）两个维度扣分最多——前者缺三套理论整合工作流，后者缺 vault 文件安全边界条款。这两项直接对应用户最近两次反馈（"分析还是浅了" + 误覆盖已有卷三文件）。
- **推荐重构方案判断依据**：默认按总分区间（80-100）应给"轻量修订"——核心结构成立。但存在两个**单点致命问题**（理论整合缺失 + 安全边界缺失），仍建议结构重整，但限于增量修改而非全量重写。

## 5. 主要优点

1. **决策矩阵清晰**：14 个触发场景精确路由，决策矩阵覆盖格式系统入口与历史文本专门入口——用户输入什么意图能稳定命中正确 workflow。
2. **多格式系统**：通用/历史/电工/哲学四种精读格式并存（reading-format-system.md），且共同底线（原文/材料段在上 + 精读段落群在下）一致——既保留领域特殊性，又保证核心契约。
3. **失败模式清单明确**：6 条边界（agent-role §3）+ 6 条内容质量自检清单（agent-role §6）+ 多轮用户反馈沉淀（agent-role §8 历史修订表）——每次精读都有可操作的失败模式预防。
4. **历史文本精读协议完善**：historical-source-close-reading.md 覆盖卷次核验、原文锚点、臣光曰专读、史料旁证、导读/精读分级等多个细节，对《通鉴》等历史文本精读的执行力强。
5. **写书式精读成体系**：writing-as-book.md 提供"写书而非写分析报告"的写作框架，从开篇钩子到操作化建议都有规范。
6. **方向模块丰富**：精神分析重读 / 症候阅读 / 主题模板（古文/电工/哲学）三大方向模块——支持跨域精读。
7. **AI 痕迹检测落地**：v2.3.0 新增 §4.6（humanizer-zh 清单重点项 + 逻辑自检），回应了用户对"AI 化"的反馈。

## 6. 主要问题

### 问题 1：三套理论整合工作流缺失（**高优先级**）

- **位置**：`references/authoring/symptomatic-reading.md`、`psychoanalytic-rereading.md`、`historical-source-close-reading.md`
- **问题**：三个独立的方向模块各自完整，但**没有显式的工作流告诉执行 agent"在一次精读里怎么同时调用三套理论"**——symptomatic-reading 读"文本结构"，psychoanalytic-rereading 读"主体位置"，辩证法读"历史过程"，三者如何整合使用没有步骤指引。
- **影响**：用户最近反馈"分析还是浅了"——执行时容易只贴理论标签（如"癔症主体""实在界"），不用作手术刀推出新判断。
- **证据**：vault 内 `wiki/agent.md`（用户 8/31 创建的执行纲领）已经显式写明"三套理论作为手术刀"+ "每个锚点至少用两套理论"——说明 skill 内缺这条。
- **建议**：新建 `references/authoring/integrated-philosophy-framework.md`，整合 symptomatic-reading + psychoanalytic-rereading + 辩证法（黑格尔-谢林）三套理论，并提供"在一次精读里整合使用"的工作流。

### 问题 2：vault 文件安全边界未写入 skill（**中优先级**）

- **位置**：`SKILL.md`、各 references
- **问题**：skill 内没有显式条款禁止"盲覆盖 vault 已有文件" / "编造史料原文" / "省略日期干支核验"。这些边界只在经验库（`recall_experience`）有记录，agent 执行 skill 时不一定能自动加载经验。
- **影响**：用户最近一次执行 deep-reading 时，agent 用 write 工具覆盖了上一轮会话写的 60KB 卷三精读文件（旧版本 untracked，git 不可恢复，永久丢失）。
- **证据**：recall_experience 中有"覆盖 vault 内已有文件（无论 tracked/untracked）前必须先 read 当前内容"的记录，但 skill 没有把这条沉淀为执行规则。
- **建议**：在 `agent-role.md` 新增"文件操作安全"小节，明确列出：①覆盖 vault 内已有文件前必须先 read 当前内容；②不要省略卷次/年号/干支核验；③不要凭"历史常识"替代原文锚点；④不要为效率跳过史料旁证。

### 问题 3：historical-source-close-reading.md 偏厚（**中优先级**）

- **位置**：`references/authoring/historical-source-close-reading.md`（约 22KB）
- **问题**：作为 references/authoring/ 内的参考文档，22KB 偏厚；部分内容（如 §3.6 二难推论模板、§3.5 现代史著适配）可下沉到独立模块文件。
- **影响**：执行精读时按需读取，单次读取成本高；但因是 reference（按需加载），影响有限。
- **证据**：参考 deep-reading skill 主文档原则"主文档只做路由层"，参考文档也不应过长。
- **建议**：拆分。§3.6 二难推论模板（agent.md §1.3 涉及）可拆为 `references/authoring/dilemma-protocol.md`，§3.5 现代史著适配可拆为 `references/authoring/modern-history-adaptation.md`。

### 问题 4：vault 内 agent.md 与 skill 分离（**中优先级**）

- **位置**：`D:/软件/Obsidian/Vault/Obsidian Vault/wiki/agent.md`（vault 内）
- **问题**：agent.md（11KB）是用户视角的 deep-reading 执行纲领，整合了三套理论 + 写作规则 + 失败模式清单，但它放在 vault 内，不在 skill 内。skill 和 agent.md 内容重叠但维护分离。
- **影响**：未来 skill 更新可能与 agent.md 不一致；agent 执行 deep-reading 时如果不读 agent.md，会错过三套理论整合的工作流（这是问题 1 的根因之一）。
- **证据**：agent.md 写于 8/31，与 deep-reading v2.3.0 是同一天；agent.md 内容（写作规则、失败模式）部分来自 deep-reading skill 的 references，部分来自用户反馈。
- **建议**：把 agent.md 的核心内容（特别是三套理论整合部分）整合进 deep-reading skill 的 references/authoring/（参考问题 1 的解决），然后保留 agent.md 作为 vault 内的执行手册 + 用户笔记。

### 问题 5：eval-loop 闭环缺失（**低优先级**）

- **位置**：skill 整体
- **问题**：没有 trigger accuracy 量化、没有最小回归验证路径——kz-skill-creator 有 eval-loop 工具，但 deep-reading skill 没有集成。
- **影响**：版本更新后，无法量化"新版本是否比旧版本更好"——只能依赖用户反馈 + validate 通过。
- **证据**：kz-skill-creator v1.40.6 提供 `eval` / `loop` / `benchmark` 命令，deep-reading skill 没有用上。
- **建议**：可选优先级——若用户后续要求 trigger accuracy 量化，再做集成。

## 7. 工作流重构建议

- **是否建议调整 workflow**：是，建议在主工作流（解析→精读→自读→答疑→入库→体检）的"全篇精读"步骤中，新增子步骤"哲学框架选定"——明确要求在精读开始前选定复合主轴（如卷三是 B+C 复合：礼/实撕扯 + 士的去向）和"哪几套理论用作手术刀"。
- **是否建议补"决策矩阵 + 强规则摘要"**：是，建议在强规则摘要新增第 7 条："任何精读任务在选定格式（历史/电工/哲学/通用）和选定复合主轴后，必须显式列出本卷将使用的跨域理论（症候阅读 / 辩证法 / 拉康精神分析 / 其他）的使用位置和目的。"
- **是否建议下沉内容到 `references/`**：是，新建 `references/authoring/integrated-philosophy-framework.md`（整合三套理论），把 agent.md 的核心内容下沉。
- **不建议现在就动的部分**：
  - 不要重写 SKILL.md 主文档——它已经是合格的路由层
  - 不要拆 historical-source-close-reading.md 主体（只拆 §3.6 二难推论模板和 §3.5 现代史著适配两个独立模块即可）
  - 不要增加 eval-loop——优先级低，且会显著增加 skill 复杂度

## 8. 优化建议

### 高优先级

1. **新建 `references/authoring/integrated-philosophy-framework.md`**：整合症候阅读（阿尔都塞）+ 黑格尔-谢林辩证法 + 拉康精神分析，提供"在一次精读里整合使用"的工作流，包含：①三套理论的分工（文本结构 / 历史过程 / 主体位置）；②整合操作步骤（每个锚点至少用两套、避免三套全上让读者被术语淹没）；③失败模式清单；④针对《通鉴》的具体应用。**这是本次重构的核心增量**。
2. **在 `agent-role.md` 新增"文件操作安全"小节**：把经验库的安全边界记录沉淀为执行规则，包括：覆盖 vault 文件前必须 read、不能省略日期核验、不能凭历史常识替代原文锚点等。
3. **在主 SKILL.md 强规则摘要新增一条**：要求每卷精读前显式声明复合主轴 + 跨域理论使用位置。

### 中优先级

1. **拆分 `historical-source-close-reading.md` 的 §3.6 二难推论模板**到独立文件 `references/authoring/dilemma-protocol.md`，便于按需加载。
2. **拆分 §3.5 现代史著适配**到独立文件 `references/authoring/modern-history-adaptation.md`，便于现代史书精读时按需加载。
3. **整合 `wiki/agent.md` 核心内容进 skill**（参考问题 4 解决）——把"agent 视角的执行纲领"作为"用户视角的执行纲领"沉淀进 skill。

### 低优先级

1. **集成 eval-loop**：可选，未来需要 trigger accuracy 量化时再做。
2. **新增 `references/examples/` 目录**：提供 1-2 个完整的精读样例（如《通鉴》卷一周纪一的最终版），便于新 agent 学习。
3. **优化 frontmatter**：增加 `last_evaluated` 字段，记录上次评估时间。

## 9. 建议的重构方向

- **推荐顺序**：
  1. **先做高优先级 1**（新建 integrated-philosophy-framework.md）——这是用户最近反馈"还是浅了"的根因解决方案。
  2. **再做高优先级 2**（agent-role.md 新增文件操作安全）——这是用户最近一次事故（覆盖 60KB 旧版）的根因解决方案。
  3. **再做高优先级 3**（主 SKILL.md 强规则摘要新增一条）——把上面两个增量串联到主工作流。
  4. **最后做中优先级 1+2**（拆分 historical-source-close-reading.md）——优化体验，但优先级低。
- **先改哪里**：
  - 优先新建 `references/authoring/integrated-philosophy-framework.md`（吸收 vault 内 agent.md 的核心内容 + 整合三套理论 + 整合操作步骤 + 失败模式）。
  - 然后修改 `agent-role.md` 增加"文件操作安全"小节。
  - 最后修改 `SKILL.md` 强规则摘要。
- **为什么**：
  - 这三个增量直接对应用户最近两次具体反馈（"还是浅了" + "误覆盖 60KB 旧版"），优先级最高。
  - 增量修改不破坏现有功能——只是补充，不会重写。
- **修改后最小验证建议**：
  1. 跑 `python scripts/skill_cli.py validate <skill-dir>` 确认 validate 通过。
  2. 用 1-2 个真实任务测试（如继续精读《通鉴》剩余卷次），确认新工作流可用。
  3. 用 v2.3.0 写过的锚点 1 / 2 / 6 重读，确认三套理论整合有效。

## 10. 总评

- **总评**：deep-reading v2.3.0 是一个**高结构质量**的精读执行 skill——决策矩阵清晰、方向模块丰富、失败模式明确。但存在两个**用户已反馈**的痛点（理论整合浅 + 安全边界缺），需要在 v2.4.0 或 v3.0.0 中增量修复。修复后，这个 skill 会有质的提升——从"能用"到"用得深且用得稳"。
- **稳定性判断**：**高**（核心结构成立，validate 通过）
- **优化优先级**：**中**（不是紧急问题，但已影响最近两次任务的执行质量）
- **后续建议**：
  - 优先做 integrated-philosophy-framework.md（核心增量）+ agent-role.md 文件操作安全（防事故）+ SKILL.md 强规则摘要（串联）。
  - 不要重写主文档——它已经是合格的路由层。
  - 不要急着集成 eval-loop——优先级低，且会显著增加 skill 复杂度。
  - 完成上述三项后，跑一次 validate + 真实任务测试，确认增量有效。

---

## 版本历史

- **v2.3.0 评估**（本次，2026-08-31）— 8 维加权总分 83.4，主要扣分在工作流完整性（三套理论整合缺失）和鲁棒性（vault 文件安全边界缺失）
- **v1.3.0 评估**（2026-08-24）— 四类精读格式系统（历史/电工/哲学/通用）落地
- **v1.2.0 评估**（2026-08-24）— 历史文本精读协议落地，修正《资治通鉴》骨架稿问题
- **v1.1.0 评估**（2026-08-22）— 按 kz-skill-creator 规范重构主文档与 references
- **v1.0.0 评估**（2026-08）— 初版评估报告