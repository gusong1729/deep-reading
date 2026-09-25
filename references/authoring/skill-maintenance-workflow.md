---
name: skill-maintenance-workflow
description: deep-reading skill 的检测与迭代工作流——skill 维护相关的四个步骤（grilling 自检 / code-review 合规检查 / ask-matt 路由评估 / 落地改进）。本工作流从 SKILL.md 主文档下沉而来（v2.6.0），让主文档保持路由层职责（<100 行）。
version: 1.0.0
---

# skill 检测与迭代工作流

> **本工作流的角色**：deep-reading skill 自身的质量维护流程。从 v2.6.0 起从 SKILL.md 主文档下沉到本文件——主文档只保留路由层职责（决策矩阵 + 强规则摘要 + 主工作流），维护相关的细节都放到本文件。
>
> **触发**：用户要求"再检测 skill 质量"、"不断改进"、"grilling"、"评估 skill"、"评审 skill"、"看结构有无问题"等。

## @工作流: 检测与迭代

<!-- @类型: 主工作流 -->
<!-- @目的: 用现有检测 skill 验证本 skill 质量 -->
<!-- @场景: 用户要求"再检测 skill 质量"、"不断改进"、"grilling" -->
<!-- @前置条件: 当前 skill 存在 -->
<!-- @后置验证: 生成 skill-evaluation.md 报告 + 改进项落地 -->
<!-- @ID: wf-detect-and-iterate -->

### @步骤1: 调 grilling 自检

<!-- @类型: 操作步骤 -->
<!-- @优先级: 必须 -->
<!-- @产物: 主文档/边界/自检清单的检查报告 -->
<!-- @验证点: 已问自己 8+ 关键问题并能回答 -->
<!-- @验证方式: grilling 风格的 frontier 问题逐项检查 -->
<!-- @ID: step-grilling-self -->

- @动作: 主文档是否仍只做路由？（理想 <100 行；当前实际约 256 行时考虑下沉内容到 references/）
- @动作: 6 条边界是否仍被违反？（自检最新一次精读——卷三锚点 6 等）
- @动作: 6 条自检清单是否覆盖 Obsidian AI 反馈？（agent-role §6）
- @动作: 决策矩阵是否覆盖所有用户意图？（用 ask-matt 模拟 5+ 用户原话评估）
- @动作: 强规则摘要是否足够简洁（<10 条）？
- @动作：是否有新发现的失败模式需要入档？

**Grilling 自检报告模板**：`docs/grilling-v{version}.md`（参见 docs/grilling-v2.5.0.md 作为示例）

### @步骤2: 调 code-review 跑 Standards 轴

<!-- @类型: 操作步骤 -->
<!-- @优先级: 必须 -->
<!-- @产物: kz-skill-creator 合规报告 -->
<!-- @验证点: SKILL.md 通过 kz-skill-creator 规范校验 -->
<!-- @验证方式: 对照 [kz-skill-creator/SKILL.md](../../../kz-skill-creator/SKILL.md) 检查 -->
<!-- @ID: step-review-standards -->

- @动作: 主文档含 frontmatter（name/description/version）
- @动作: 含决策矩阵（如适用）和强规则摘要
- @动作: 使用语义化标记（`## @工作流:` / `### @步骤N:` / `@类型` `@优先级` `@验证点` `@验证方式` `- @动作:`）
- @动作: references/ 按角色分类（authoring/、templates/、scenarios/）
- @动作: 包含 `skill-evaluation.md` 评测文档

### @步骤3: 调 ask-matt 评估路由清晰度

<!-- @类型: 操作步骤 -->
<!-- @优先级: 可选 -->
<!-- @产物: 路由问题清单 -->
<!-- @验证点: 用户各类输入能命中正确 workflow -->
<!-- @验证方式: 模拟 5+ 用户原话查表 -->
<!-- @ID: step-route-clarity -->

- @动作: 用决策矩阵模拟用户原话——"精读这本书"、"这段什么意思"、"扫描这本古籍"、"体检知识库"等
- @动作: 识别矩阵未覆盖的意图

### @步骤4: 落地改进

<!-- @类型: 操作步骤 -->
<!-- @优先级: 必须 -->
<!-- @产物: 更新后的 SKILL.md + references/ + 版本号 -->
<!-- @验证点: 改进项全部落档 -->
<!-- @验证方式: 同步更新 version（frontmatter + 头部 + 版本历史首条） -->
<!-- @ID: step-iterate -->

- @动作: 把 GRILLING + code-review + ask-matt 三项的发现合并去重
- @动作: 优先级：高=直接影响输出质量；中=影响可维护性；低=风格统一
- @动作: 落档后跑 validate（`python scripts/skill_cli.py validate <path_to_skill_folder>`）
- @动作: 若主文档超过 100 行，下沉内容到 references/

---

## 与 deep-reading skill 的协作

- **触发**：本工作流由 SKILL.md 主工作流之外的"检测"类用户意图触发（如"再检测 skill 质量"、"grilling"、"评估 skill"等）。
- **决策矩阵**：对应的路由是"评估 / 评审 skill 本身 / 看结构有无问题"——跳转到 references/templates/skill-evaluation-template.md 或本工作流。
- **Grilling 自检报告**：每次自检后输出 `docs/grilling-v{version}.md`，记录发现的问题与建议。

---

## 版本历史

- **v1.0.0** (2026-08-31) - 从 SKILL.md @步骤 10-13 下沉而来；保留四个步骤（grilling 自检 / code-review 合规检查 / ask-matt 路由评估 / 落地改进）；让 SKILL.md 主文档回到路由层（<100 行）。