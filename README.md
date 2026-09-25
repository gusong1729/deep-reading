# deep-reading

一个给 AI Agent 用的精读技能。把一本书或一篇文章丢给它，它按一套固定规矩读一遍，产出一份能独立阅读的精读文档，并写进你的知识库。

对 Agent 说「精读这本书」「解读这篇文章」「这段看不懂」，或者直接把文件递过去，就能触发。

开源项目，MIT 许可，拿去用、改、再分发都行。

## 它做什么

规矩写在 `SKILL.md` 和 `references/` 里，主要四条。

**怎么读。** 原文一段段放在上面，解读紧跟着写在下面，解读的长度至少是原文的五到十倍。不给骨架，也不摘几句金句了事，每个锚点都得配一大段分析。

**按什么格式读。** 通用论述、史书、电工手册、哲学原著、小说、带立场的评论，各有一套最小合格单元。格式选错，整篇都会走形。

**拿什么刀读。** 症候阅读（阿尔都塞）、辩证法、拉康的精神分析各一把；另有一个力量分析模块，专门应付政治史和当代史，把历史读成几股力量在时间里的相互推挤，而不是某条路线的展开。这些视角要求融进解读正文，不单独开小节。

**读到哪去。** 精读页、概念页、索引、日志，四样都落进知识库（推荐 Obsidian）。

另有一套质量边界：6 条边界（不啰嗦、不列举、不分块标签、不引号泛滥、不堆元叙述、不堆学术名词）和 6 条自检（事实、范围、陷阱、术语、排查顺序、多视角）。这些是被反复挑刺之后攒下来的，不是装饰。

## 目录结构

```
deep-reading/
├── SKILL.md                      入口：决策矩阵 + 强规则摘要 + 主流程
├── CONTEXT.md                    领域术语与规则摘要
├── agents/Agent.md               执行体说明
├── references/
│   ├── authoring/                核心规则，22 份
│   │   ├── agent-role.md                    角色、边界、自检清单、维护纪律
│   │   ├── reading-format-system.md         六种格式的合格单元与选型
│   │   ├── historical-source-close-reading.md  历史文本协议
│   │   ├── symptomatic-reading.md           症候阅读
│   │   ├── psychoanalytic-rereading.md      拉康式重读
│   │   ├── integrated-philosophy-framework.md  三套理论的配合用法
│   │   ├── force-analysis.md                力量分析
│   │   ├── writing-as-book.md               怎样写得让人读得下去
│   │   └── （另有 exegesis / 模板 / OCR / Obsidian 格式 / 方向模块 / 维护流程等）
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

把整个 `deep-reading/` 塞进 Agent 的 skills 目录，重载后生效。

| 平台 | 放在哪 |
|---|---|
| HanaAgent | `<工作区>/.agents/skills/deep-reading/` |
| Claude Code | `~/.claude/skills/deep-reading/` |

平时它只占两行上下文（名字和描述），你说「精读」的时候才整个加载。

## 它还需要一个知识库

技能本身只是执行器，产物得有地方落。只放技能、不建库，读完就散。

最小结构长这样：

```
<你的知识库>/
├── SCHEMA.md            规则层：目录结构、命名、Ingest / Query / Lint
├── raw/                 原文层，只读
└── wiki/
    ├── index.md         索引：每页一行
    ├── log.md           操作日志
    ├── readings/        精读页，按领域分目录
    └── concepts/        概念页
```

搭 Obsidian 用最顺手，精读页能直接读，`[[wikilink]]` 自动连成网。

## 怎么用

| 你说 | 它做 |
|---|---|
| 「用 deep-reading 精读这本书」加一个文件 | 全篇精读，选格式、逐段解读、落库 |
| 「这段看不懂」「解释这段」 | 难点细读 |
| 「我读到第三章没读懂」 | 答疑（先读索引，再定位页面） |
| 「Ingest 入库」「体检知识库」 | 入库、更新索引、盘点矛盾与缺口 |
| 「继续往下写」 | 接着已有的精读写下一卷 |

## 维护

改完之后跑一次结构校验（需要 kz-skill-creator 在旁边）：

```bash
python <kz-skill-creator>/scripts/skill_cli.py validate deep-reading
```

维护流程本身写在 `references/authoring/skill-maintenance-workflow.md`：自审、规范检查、路由评估、落地改进。

## 许可

MIT。随便用，商用也行，唯一的要求是保留版权声明这一行：

```
Copyright (c) 2026 孤松 (GitHub: gusong1729)
```

完整条文见 [LICENSE](LICENSE)。

包里引用的书刊、网站和工具（anysearch、kz-skill-creator、Obsidian 等）版权归各自作者，这里只是引用和说明，不影响上面的 MIT。

## 版本

- `version-one`：首次发布，33 个文件
- 之后补了 README 与 LICENSE（MIT），仓库转为公开
- 仓库地址：https://github.com/gusong1729/deep-reading
