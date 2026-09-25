# deep-reading

面向 AI Agent 的全文精读技能（Agent Skill）。输入一本书或一篇文章，输出一份可独立阅读的精读文档，并将产物写入知识库。

支持 PDF / EPUB / TXT / DOCX / MD / HTML 等格式，覆盖历史、哲学、文学、政治、社会学、电工等领域。

## 功能

- **全文精读**：以「原文锚点 + 解读段落群」为最小合格单元，原文逐段在上、解读紧随其下，解读篇幅不低于原文的 5–10 倍。
- **多格式适配**：通用论述、历史文本、电工实操、哲学、小说叙事、批判性文本六种格式，各有独立的最小合格单元与验收标准。
- **跨域分析**：症候阅读（阿尔都塞）、辩证法、拉康精神分析三套框架，另有面向政治史与当代史的「力量分析」模块。相关视角融入解读正文，不单独成节。
- **知识库落库**：精读页、概念页、索引与操作日志四类产物同步更新，兼容 Obsidian。
- **质量约束**：6 条写作边界（不啰嗦、不列举、不分块标签、不引号泛滥、不堆元叙述、不堆学术名词）与 6 条内容自检（事实、范围、陷阱、术语、排查顺序、多视角）。

## 目录结构

```
deep-reading/
├── SKILL.md                      入口：决策矩阵 + 强规则摘要 + 主流程
├── CONTEXT.md                    领域术语与规则摘要
├── agents/Agent.md               执行体说明
├── references/
│   ├── authoring/                核心规则，22 份
│   │   ├── agent-role.md                      角色、边界、自检清单、维护纪律
│   │   ├── reading-format-system.md           六种格式的合格单元与选型
│   │   ├── historical-source-close-reading.md 历史文本协议
│   │   ├── symptomatic-reading.md             症候阅读
│   │   ├── psychoanalytic-rereading.md        拉康式重读
│   │   ├── integrated-philosophy-framework.md 三套理论的配合用法
│   │   ├── force-analysis.md                  力量分析
│   │   ├── writing-as-book.md                 行文要求
│   │   └── （另有 exegesis 协议、模板、OCR、Obsidian 输出格式、方向模块、维护流程）
│   └── templates/
│       └── skill-evaluation-template.md
├── docs/
│   ├── adr/                      7 份架构决策记录
│   ├── grilling-v2.5.0.md        自我审问记录
│   └── reader-audit-log.md       读者视角巡检日志
├── README.md
├── LICENSE
└── skill-evaluation.md
```

## 安装

将 `deep-reading/` 目录放入 Agent 的 skills 目录，重新加载后生效。

| 平台 | 路径 |
|---|---|
| HanaAgent | `<工作区>/.agents/skills/deep-reading/` |
| Claude Code | `~/.claude/skills/deep-reading/` |

未触发时只占用 `name` 与 `description` 两行上下文，触发后加载完整内容。

## 前置：知识库结构

本技能为执行器，产物需落盘至知识库。最小结构如下：

```
<知识库>/
├── SCHEMA.md            规则层：目录结构、命名约定、Ingest / Query / Lint 流程
├── raw/                 原文层（只读）
└── wiki/
    ├── index.md         内容索引：每页一行
    ├── log.md           操作日志
    ├── readings/        精读页，按领域分目录
    └── concepts/        概念页
```

推荐搭配 Obsidian 使用，精读页可直接阅读，页间以 `[[wikilink]]` 建立链接。

## 使用

| 输入 | 行为 |
|---|---|
| 「用 deep-reading 精读这本书」+ 文件 | 全篇精读：选格式、逐段解读、落库 |
| 「这段看不懂」/「解释这段」 | 难点细读 |
| 「我读到第三章没读懂」 | 答疑（先读索引，再定位页面） |
| 「Ingest 入库」/「体检知识库」 | 入库、更新索引、盘点矛盾与缺口 |
| 「继续往下写」 | 基于已有精读续写下一卷 |

## 维护

结构校验（需 kz-skill-creator 在同一工作区）：

```bash
python <kz-skill-creator>/scripts/skill_cli.py validate deep-reading
```

`kz-skill-creator` 是 K叔开源的 Skill 创建器：<https://gitee.com/kingzeus/skills>。本项目采用的渐进式披露结构、语义化标记与版本校验规则，均遵循其规范。

维护流程见 `references/authoring/skill-maintenance-workflow.md`：自审、规范检查、路由评估、落地改进。

## License

本项目采用 MIT 许可证，完整条款见 [LICENSE](LICENSE)。

```
Copyright (c) 2026 孤松 (gusong1729)
```

文档中引用的书刊、网站与第三方工具（anysearch、kz-skill-creator、Obsidian 等）版权归各自作者所有，此处仅作引用与方法来源说明。

## 版本

- `version-one`：首次发布（33 个文件）
- 随后补充 README 与 LICENSE
- 仓库：<https://github.com/gusong1729/deep-reading>
