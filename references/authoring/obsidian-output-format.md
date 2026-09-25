---
title: Obsidian 输出格式规范（v2.7.0 新增）
description: 当 deep-reading 产出精读落到 Obsidian vault 时，必须严格遵守的 Markdown 语法、YAML frontmatter 格式、命名约定。沉淀依据：用户 2026-09-12 反馈 Obsidian 不识别 `###**xxx**`、YAML frontmatter 报错、命名与 vault 实际样张不一致三类问题。
domain: [写作规范, Obsidian]
source: 用户 2026-09-12 精读谢林《先验唯心论体系》过程中沉淀
状态: 已沉淀
精读日期: 2026-09-12
related: "[[agent-role §7.8 vault 适配与 Obsidian 输出格式]], [[reading-format-system §4.5 命名规范]]"
---

# Obsidian 输出格式规范

> 本文件是 deep-reading 产出精读落到 Obsidian vault 时的**强制格式规范**。Obsidian 严格遵守 CommonMark（Markdown）+ YAML 1.1+ 标准，不能依赖宽容渲染（GitHub/Typora 容错但 Obsidian 不容错）。

## 1. Markdown 语法硬约束

### 1.1 标题格式

**正确**：
```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
```

**错误**（Obsidian 不识别）：
```markdown
###**节点 1：xxx**      ← 标题标记与加粗号连写，Obsidian 无法识别为标题
#标题                   ← 标题标记后无空格
```

**规则**：
- 标题标记 `#`、`##`、`###` 等后**必须有空格**，再跟标题内容。
- 标题内容**不能被 `**加粗号**` 整行包裹**——这会破坏标题识别。
- 标题中可以有局部加粗：`### 节点 1：**先验时间**与**先验发生学**`（加粗只包裹部分词）。
- 多级标题（# / ## / ### 等）可按需使用，但同一精读文档的标题层级应保持一致（如 §3 用 `###`，§4 用 `####`）。

### 1.2 列表格式

**正确**：
```markdown
- 项目一
- 项目二
  - 子项目
- 项目三
```

**错误**：
```markdown
-项目一           ← `-` 后无空格
** 项目一 **       ← 用 `**` 代替 `-`
```

**规则**：
- 列表使用 `-`（无序）或 `1.` `2.`（有序），后**必须有空格**。
- 列表项内容可以包含加粗、链接、代码块等，但项目符号后必须空格。

### 1.3 引用块

**正确**：
```markdown
> 这是一个引用块。
> 可以多行。
>
> **底本**：某某某某
```

**规则**：
- 引用使用 `>` 开头（可后接空格）。
- 引用块内可以嵌套其他 markdown（加粗、链接、代码等）。

### 1.4 代码块

**正确**：
```python
def hello():
    print("Hello")
```

**规则**：
- 用三个反引号 ` ``` ` 包裹代码，后接语言标识（如 `python`、`yaml`）。
- 行内代码用单反引号：`code`。

### 1.5 链接

**正确**：
```markdown
[[wikilink]]                                            # Obsidian wikilink
[普通链接](https://example.com)                          # Markdown 普通链接
[[wikilink#section]]                                    # Obsidian wikilink 到某节
[[wikilink|显示文本]]                                    # Obsidian wikilink 自定义显示
```

**规则**：
- vault 内引用必须用 wikilink `[[...]]`（Obsidian 才能解析）。
- 跨 vault 引用用 `[显示](vault://...)`。

### 1.6 加粗、斜体、删除线

**正确**：
```markdown
**加粗**
*斜体*
~~删除线~~
***加粗+斜体***
```

**规则**：
- 用 `**`（双星号）包裹加粗，不能用单星号或下划线（Obsidian 兼容但 Markdown 标准是双星号）。
- 标题内加粗见 §1.1。

## 2. YAML Frontmatter 硬约束

Obsidian 的 frontmatter 必须严格遵守 YAML 1.1+ 标准。常见错误：

### 2.1 `related` 必须用 YAML 列表

**正确**：
```yaml
related:
  - "[[先验唯心论体系]]"
  - "[[谢林]]"
  - "[[绝对自我]]"
```

**错误**：
```yaml
related: "[[先验唯心论体系]], [[谢林]], [[绝对自我]]"   ← 字符串不是列表，Dataview 等插件读不到
```

**规则**：
- `related`、`tags` 等需要多值的字段必须用 YAML 列表（每项 `- "value"`）。
- wikilink 必须用 `[[]]` 包裹。

### 2.2 含特殊字符的字段值必须用引号包裹

**正确**：
```yaml
原文路径: "raw/哲学/谢林著作集：先验唯心论体系 (谢林（Schelling）；先刚 译) (z-library.sk).pdf"
```

**错误**：
```yaml
原文路径: raw/哲学/谢林著作集：先验唯心论体系 (谢林（Schelling）；先刚 译) (z-library.sk).pdf   ← 含 `；`、嵌套括号，未引号包裹
```

**规则**：
- 含特殊字符（括号、分号、冒号、引号）的字段值必须用双引号 `"..."` 包裹整个字符串。
- 中文全角符号 `（）、；：""` 等都算特殊字符。

### 2.3 `summary` 等长字段用单引号包裹（避免与内部中文双引号冲突）

**正确**：
```yaml
summary: '第一部分（第三次修订）：...如"先验时间/先验逻辑/先验发生学"的逐一解释...'
```

**错误**：
```yaml
summary: "第一部分（第三次修订）：...如"先验时间/先验逻辑/先验发生学"的逐一解释..."   ← 内部中文双引号与外层半角双引号冲突
```

**规则**：
- 内部可能含中文双引号 `""` 的字段，外层用单引号 `'...'` 包裹。
- 内部不含中文双引号的可直接用双引号 `"..."` 或不用引号（裸字符串）。

### 2.4 时间戳用 ISO 8601 格式

**正确**：
```yaml
improve: 2026-09-12T23:50:00.000Z
精读日期: 2026-09-12
```

**规则**：
- `improve` 等时间戳字段用 ISO 8601 格式 `YYYY-MM-DDTHH:MM:SS.sssZ`。
- 仅日期字段用 `YYYY-MM-DD` 格式（YAML 会解析为 date 类型）。

### 2.5 完整 frontmatter 模板

```yaml
---
title: 谢林《先验唯心论体系》·序言精读（一）
domain: [哲学]
source: 《先验唯心论体系》〔德〕弗里德里希·威廉·约瑟夫·冯·谢林 著，先刚 译，北京大学出版社 2016 年（"谢林著作集"第 4 卷）
原文路径: "raw/哲学/谢林著作集：先验唯心论体系 (谢林（Schelling）；先刚 译) (z-library.sk, 1lib.sk, z-lib.sk).pdf"
状态: 精读中（分块推进）
精读日期: 2026-09-12
related:
  - "[[先验唯心论体系]]"
  - "[[谢林]]"
  - "[[绝对自我]]"
improve: 2026-09-12T23:50:00.000Z
summary: '第一部分（第三次修订）：...'
---
```

## 3. 命名约定硬约束

**严禁机械套用 `<书名>_精读_<定位>.md` 模板而不检查 vault 实际样张**。vault 命名约定是个例化变量，必须先读 `wiki/readings/<领域>/` 下已有文件，照搬该领域的实际命名方式。

### 3.1 已知 vault 命名差异（以用户 vault 为例）

| 领域 | vault 实际命名 | 是否符合 SKILL §4.5 模板 |
|---|---|---|
| 历史 | `<书名>_精读_<定位>.md`（如 `资治通鉴_精读_卷一_周纪一.md`） | ✓ 符合 |
| 古文 | `<书名>_<篇名>.md`（如 `文心雕龙_神思.md`） | ✗ 不符合 |
| 哲学 | `<书名>.md`（如 `拉康精神分析介绍性辞典.md`）或 `<书名>_<定位>_精读.md`（如 `拉康辞典_序言精读.md`） | ✗ 不符合 |
| 计算机 | `<书名>_<定位>.md`（如 `北大计算机基础能力手册_基础篇.md`） | 部分符合 |

### 3.2 命名规则

1. **先读 `wiki/readings/<领域>/` 下已有文件**，记录 2-3 个已有命名样张。
2. **照搬样张命名**——不要混用不同风格（如同一领域内既有 `<书名>.md` 又有 `<书名>_精读_<定位>.md`）。
3. **保持可排序**——多部分精读必须用书自身可排序的章节标识（卷/章/篇/词条）。
4. **文件名/title/链接三者一致**——改名后必须同步所有旧链接（index.md/log.md/概念页）。

## 4. 验证清单

完成一篇精读落库前，按以下清单验证：

- [ ] 所有标题 `#` 后有空格，标题内容不被 `**` 包裹
- [ ] 所有列表 `-` 后有空格
- [ ] `related` 字段用 YAML 列表（不是字符串）
- [ ] `原文路径` 等含特殊字符的字段值用双引号包裹
- [ ] `summary` 等内部含中文双引号的字段，外层用单引号
- [ ] 时间戳字段用 ISO 8601 格式
- [ ] 文件命名与 `wiki/readings/<领域>/` 已有样张一致
- [ ] frontmatter 的 title 与文件名一致
- [ ] 所有 wikilink 链接的目标页都已建立
- [ ] index.md 已同步更新（精读条目 + 状态升级）
- [ ] log.md 已记录 Ingest 操作

## 5. 自动验证脚本

可以在写入文件后用 PyYAML 验证 frontmatter：

```python
import yaml
import re

with open(path, 'r', encoding='utf-8') as f:
    content = f.read()

match = re.match(r'^---\n(.*?)\n---', content, re.DOTALL)
if not match:
    print('❌ frontmatter 格式错误')
else:
    try:
        fm = yaml.safe_load(match.group(1))
        print(f'✅ PyYAML 解析成功，{len(fm)} 字段')
    except yaml.YAMLError as e:
        print(f'❌ YAML 解析错误: {e}')
```

## 6. 参考样张

vault 中已有的成功样张：

- `wiki/readings/哲学/拉康精神分析介绍性辞典.md`
- `wiki/readings/哲学/拉康辞典_序言精读.md`
- `wiki/readings/历史/资治通鉴_精读_卷一_周纪一.md`
- `wiki/readings/古文/文心雕龙_神思.md`

新精读产出前必须先扫一遍这些样张，确认命名约定和 frontmatter 字段对齐。

## 7. 版本历史

- **v1.0.0** (2026-09-12) - 首版：用户 2026-09-12 反馈三类 vault 适配问题（Markdown 标题格式、YAML frontmatter、命名约定），按 kz-skill-creator 重构流程沉淀到 `references/obsidian-output-format.md`