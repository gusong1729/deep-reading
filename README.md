# deep-reading

> 面向 AI 编码助手（Claude Code、Codex、HanaAgent 等）的开源 Skill：把一本书或一篇文章读成一份可独立阅读的精读文档，并写入你的知识库。

支持 PDF / EPUB / TXT / DOCX / MD / HTML，领域不限（历史、哲学、文学、政治、社会学、电工等）。

## 30 秒上手

```bash
# 1. 克隆到 skills 目录
git clone https://github.com/gusong1729/deep-reading.git ~/.claude/skills/deep-reading
```

2. 在知识库里准备好 `wiki/readings/`（见「知识库结构」）。
3. 重启 Agent 会话，然后说：**用 deep-reading 精读这本书**，并把文件交给它。

## 能力一览

| 能力 | 说明 |
|---|---|
| 全文精读 | 以「原文锚点 + 解读段落群」为最小合格单元，原文逐段在上、解读紧随其下，解读篇幅不低于原文的 5–10 倍 |
| 多格式适配 | 通用论述、历史文本、电工实操、哲学、小说叙事、批判性文本六种格式，各有最小合格单元与验收清单 |
| 跨域分析 | 症候阅读（阿尔都塞）、辩证法、拉康精神分析三套框架；另有面向政治史与当代史的「力量分析」模块。相关视角融入解读正文，不单独成节 |
| 落库联动 | 精读页、概念页、索引、操作日志四类产物同步更新，兼容 Obsidian 的 `[[wikilink]]` |
| 质量约束 | 6 条写作边界（不啰嗦、不列举、不分块标签、不引号泛滥、不堆元叙述、不堆学术名词）与 6 条内容自检（事实、范围、陷阱、术语、排查顺序、多视角） |

## 适用场景

**适用**：目标是把长篇材料读透、并留下可复查的笔记。史书、哲学原著、学术专著、政策文本、长篇报道都合适；需要按原文逐段推进，而不是只看结论。

**不适用**：只要摘要或速览；论文排版与出版（用 `docx` / `pdf` 一类 Skill）；单句解释。

## 产出长什么样

最小合格单元是「原文锚点 + 解读段落群」。下面是一段结构示意（非完整精读）：

> 原文：「知人者智，自知者明。胜人者有力，自胜者强。」——《道德经》第三十三章

老子把「智」与「明」、「力」与「强」分成两组，两组之间有高下。知人是朝外看，能看清别人的短长；自知是转身向内，把自己也当一个可被审视的对象。前者是能力，后者是位置的变化。同理，胜人靠的是对外较量的力气，自胜要处理的是自己内部的抵抗，后者没有旁观者，也没有终点。

这句话在《道德经》里不孤立。「明」字在全书反复出现，指向的都是同一种能力：不被表象牵走。放在第三十三章的上下文里看，它和「强行者有志」构成一组，整章谈的其实是持续性的问题，而不是一次性的胜负。

各格式的详细要求与验收标准见 `references/authoring/reading-format-system.md`。

## 安装

### 方式一：克隆到 skills 目录（推荐）

```bash
git clone https://github.com/gusong1729/deep-reading.git ~/.claude/skills/deep-reading
```

### 方式二：手动复制

将 `deep-reading/` 整个目录放入 Agent 的 skills 目录：

| 平台 | 路径 |
|---|---|
| HanaAgent | `<工作区>/.agents/skills/deep-reading/` |
| Claude Code | `~/.claude/skills/deep-reading/` |

未触发时只加载 `name` 与 `description` 两行，触发后才读入完整内容。

## 知识库结构

本 Skill 是执行器，产物需要落盘。最小结构：

```
<知识库>/
├── SCHEMA.md            规则层：目录结构、命名约定、Ingest / Query / Lint
├── raw/                 原文层（只读）
└── wiki/
    ├── index.md         内容索引：每页一行
    ├── log.md           操作日志
    ├── readings/        精读页，按领域分目录
    └── concepts/        概念页
```

不建库也能跑，但产出无处可存，等于一次性输出。

## 使用说明

### 目录结构

```
deep-reading/
├── SKILL.md              入口：决策矩阵 + 强规则摘要 + 主流程
├── CONTEXT.md            领域术语与规则摘要
├── agents/Agent.md       执行体说明
├── references/
│   ├── authoring/        核心规则 22 份（格式系统、历史协议、症候阅读、
│   │                     精神分析重读、力量分析、行文要求、维护流程等）
│   └── templates/        模板
├── docs/                 架构决策记录、自审与巡检日志
├── README.md
└── LICENSE
```

### 使用示例

1. **全篇精读**：给出文件并说「用 deep-reading 精读这本书」，它会选格式、逐段解读、落库。
2. **难点细读**：指出「这段看不懂」，它只对这一段加深。
3. **答疑**：说「我读到第三章没读懂」，它先读索引再定位页面。
4. **入库与体检**：说「Ingest 入库」或「体检知识库」，更新索引并盘点矛盾与缺口。
5. **续写**：说「继续往下写」，基于已有精读推进下一卷或下一章。

### 维护

结构校验（需 kz-skill-creator 在同一工作区）：

```bash
python <kz-skill-creator>/scripts/skill_cli.py validate deep-reading
```

`kz-skill-creator` 是 K叔开源的 Skill 创建器：<https://gitee.com/kingzeus/skills>。本项目的渐进式披露结构、语义化标记与版本校验规则，均遵循其规范。

维护流程见 `references/authoring/skill-maintenance-workflow.md`。

## 已知限制

- 依赖知识库落盘。只装 Skill 不建库，产出不会留存。
- 长文本开销大，整本书通常需要分批推进。
- 精读质量取决于 Agent 对 `references/` 规则的执行程度，关键结论建议回原文核验。
- 不替代人工校对与判断。

## 参与贡献

1. Fork 本仓库
2. 创建特性分支：`git checkout -b feat/your-change`
3. 修改后同步升版本（`SKILL.md` frontmatter、头部版本块、版本历史三处一致），并跑一次结构校验
4. 提交：`git commit -m 'feat: your change'`
5. 推送并提交 Pull Request

新增学科方向的做法见 `references/authoring/direction-module-guide.md`。

## 许可证

[MIT License](LICENSE) © 2026 孤松 (gusong1729)

文档中引用的书刊、网站与第三方工具（anysearch、kz-skill-creator、Obsidian 等）版权归各自作者所有，此处仅作引用与方法来源说明。

## 版本

- `version-one`：首次发布（33 个文件）
- 随后补充 README 与 LICENSE
- 仓库：<https://github.com/gusong1729/deep-reading>
