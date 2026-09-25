# deep-reading

> 一个用于**全文精读**的 Agent Skill：把任何领域的书与文章，读成一份**可独立阅读**的精读文档，并沉淀进知识库。
>
> 触发方式：对 Agent 说「精读这本书」「解读这篇文章」「这段看不懂」，或直接把文件丢给它。

---

## 这是什么

`deep-reading` 是一份给 AI Agent 的工作手册（`SKILL.md` + `references/`）。它规定了四件事：

1. **怎么读**——原文逐段在上、解读紧随其下的「原文锚点」结构；**解读篇幅 ≥ 原文的 5–10 倍**，禁止骨架化、禁止只摘金句。
2. **按什么格式读**——通用型 / 历史风格 / 电工实操 / 哲学 / 小说叙事 / 批判性文本，六种格式各有最小合格单元。
3. **拿什么刀读**——三套跨域理论作手术刀：**症候阅读**（阿尔都塞）、**辩证法**、**拉康精神分析**，以及面向政治史/当代史的**力量分析**方向模块。要求融入解读正文，不另立小节。
4. **读到哪去**——产物落进知识库：精读页、概念页、索引与日志，三者联动。

它还带一套**质量边界**：6 条边界（不啰嗦、不列举、不分块标签、不引号泛滥、不堆元叙述、不学术名词堆砌）+ 6 条自检清单（事实 / 范围 / 陷阱 / 术语 / 排查顺序 / 多视角）。

---

## 目录结构

```
deep-reading/
├── SKILL.md                      入口：决策矩阵（路由）+ 强规则摘要 + 主流程
├── CONTEXT.md                    领域术语与规则摘要（Schema 层）
├── agents/Agent.md               执行体说明
├── references/
│   ├── authoring/                核心规则（22 份）
│   │   ├── agent-role.md                    角色、边界、自检清单、维护纪律
│   │   ├── reading-format-system.md         六种格式的最小合格单元与选型
│   │   ├── historical-source-close-reading.md  历史文本协议（卷次/锚点/书写者视角）
│   │   ├── symptomatic-reading.md           症候阅读（文本没说什么）
│   │   ├── psychoanalytic-rereading.md      拉康精神分析重读（主体位置）
│   │   ├── integrated-philosophy-framework.md  三套理论的整合用法
│   │   ├── force-analysis.md                力量分析（把历史读成力量场，不读成路线史）
│   │   ├── writing-as-book.md               写书式精读
│   │   ├── exegesis-protocol.md / fulltext-exegesis-protocol.md / fulltext-reading-workflow.md
│   │   ├── guwen-template.md / structured-template.md / concept-page-template.md
│   │   ├── obsidian-output-format.md / ocr-vision-guide.md
│   │   ├── direction-module-guide.md        新增学科方向模块的注册方式
│   │   └── skill-maintenance-workflow.md    skill 自身的检测与迭代
│   └── templates/
│       └── skill-evaluation-template.md     结构评估报告模板
├── docs/
│   ├── adr/                      7 份架构决策记录（0001–0007）
│   ├── grilling-v2.5.0.md        自我审问记录
│   └── reader-audit-log.md       读者视角巡检日志
├── skill-evaluation.md           结构评估
└── validate_v2.9.24.txt          某轮校验输出留档
```

---

## 安装

把整个 `deep-reading/` 目录放进 Agent 的 skills 目录，重启或重新加载后即可被触发：

| 平台 | 放置位置 |
|---|---|
| HanaAgent | `<工作区>/.agents/skills/deep-reading/` |
| Claude Code | `~/.claude/skills/deep-reading/` |

> 放入后，Agent 在你说「精读 / 解读 / 读这本书」时才会加载它——平时只加载 `name` 与 `description` 两行。

---

## 它还需要一个知识库（重要）

**这个 skill 只是执行器，产物要落在知识库里。** 只放 skill、不建库，等于读完就散。最小结构：

```
<你的知识库>/
├── SCHEMA.md            规则层：目录结构、命名约定、Ingest / Query / Lint 流程
├── raw/                 原文层（只读，存放原始 PDF/EPUB/网页剪藏）
└── wiki/                知识层（由 Agent 维护）
    ├── index.md         内容索引：每页一行（链接 + 一句话摘要 + 标签）
    ├── log.md           操作日志：## [日期] ingest|query|lint | 对象
    ├── readings/        精读专库（按领域分子目录：历史/哲学/古文/…）
    └── concepts/        概念页（跨域扁平存放，用 domain 标签区分）
```

搭配 Obsidian 使用效果最好：精读页直接可读，`[[wikilink]]` 自动连成网。

---

## 怎么用

| 你说 | 它做什么 |
|---|---|
| 「用 deep-reading 精读这本书」+ 文件 | 全篇精读：选格式 → 原文锚点 + 大段解读 → 落库 |
| 「这段看不懂 / 解释这段」 | 难点细读（文本驱动） |
| 「我读到第三章没读懂」 | 答疑（Query 模式：先读索引，再定位页面） |
| 「Ingest / 体检知识库」 | 入库、索引更新、矛盾与缺口盘点 |
| 「继续往下写」 | 基于已有精读续写下一卷/下一章 |

---

## 维护与校验

- 版本记录：见 `SKILL.md` 的版本历史与 `docs/adr/`。
- 改动后跑一次结构校验（需要 `kz-skill-creator` 在旁边）：

```bash
python <kz-skill-creator>/scripts/skill_cli.py validate deep-reading
```

- 维护流程本身写在 `references/authoring/skill-maintenance-workflow.md`：grilling 自检 → 规范合规检查 → 路由评估 → 落地改进。

---

## 版本

首次发布：标签 **`version-one`**

## 许可

本项目采用 **MIT License**——可自由使用、修改、分发，包括商业用途，只需保留版权声明与许可声明。

```
Copyright (c) 2026 孤松 (GitHub: gusong1729)
```

完整条文见仓库根目录的 [`LICENSE`](LICENSE)。

### 关于本文档包里的第三方内容

本包为方法与实践的整理，文中引用的书刊、网站与工具（如 anysearch、kz-skill-creator、Obsidian）归各自权利人所有，仅作引用与方法来源说明；这些引用不改变上述 MIT 许可。
