---
title: Skill Evaluation Template
name: skill-evaluation-template
description: Skill 评估报告模板——给 kz-skill-creator 评测工作流使用。生成针对任意 Skill 的结构化评估文档，含主文档结构检查、references 分类检查、grilling + code-review + ask-matt 三轴评测、改进建议与版本号变更。
version: 1.0.0
---

# Skill 评估报告模板

> **版本**: v1.0.0
> Skill 评估报告模板——给 kz-skill-creator 评测工作流使用。生成针对任意 Skill 的结构化评估文档，含主文档结构检查、references 分类检查、grilling + code-review + ask-matt 三轴评测、改进建议与版本号变更。

## 1. 基础信息
- 目标 Skill：<name>
- 版本：<version>
- 评估时间：<date>
- 评估依据：kz-skill-creator v1.40+

## 2. 检查结果

### 2.1 主文档结构（SKILL.md）
- [ ] frontmatter 完整（name/description/version）
- [ ] 决策矩阵存在
- [ ] 强规则摘要存在
- [ ] workflow 步骤使用语义化标记
- [ ] 版本历史在文档末尾

### 2.2 references/ 目录
- [ ] 按角色分类（authoring/、templates/、scenarios/）
- [ ] 无散乱文件
- [ ] 长内容已下沉

### 2.3 评测（grilling + code-review + ask-matt）
- 6 条边界是否被遵守
- 6 条自检清单是否覆盖
- 主文档是否过厚

## 3. 改进建议
- 优先级：高/中/低
- 具体行动

## 4. 版本号变更
- vX.Y.Z → vX.Y.Z+1

---

## 版本历史

- **v1.0.0** (2026-08-22) - 初版评估模板，含 4 节标准结构