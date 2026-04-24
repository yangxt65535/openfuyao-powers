# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

本仓库是 openFuyao 社区的 Superpowers 技能集合，提供标准化的研发工作流支持，涵盖需求拆解、方案设计、代码实现、PR 管理等环节。

## 仓库结构

```
agents/          # Agent 定义文件
skills/          # Skill 技能定义文件
.mcp.json        # MCP 服务配置（gitcode-mcp、plantuml）
```

## 核心工作流

本仓库支持的需求到代码的完整工作流：

1. **需求拆解** (`requirement-decomposition`): IR→SR→AR 三级拆解
2. **方案设计** (`solution-design`): 4+1 架构视图 + DFX 设计
3. **Story 实现** (`story2code`): 双 Agent 协作编码
4. **PR 管理** (`openfuyao-developer`): 顺序依赖管理 + 自动创建 PR
5. **代码评审** (`gitcode-pr-review`): PR 差异分析与评论
6. **Issue 修复** (`gitcode-issue-fixer`): 自动化问题定位与修复

## MCP 配置

项目依赖两个 MCP 服务（配置见 `.mcp.json`）：

- **gitcode-mcp**: GitCode 平台交互（PR、Issue 管理）
  - 需要环境变量 `GITCODE_TOKEN`
- **plantuml**: PlantUML 图表渲染
  - 使用 `plantuml__generate_plantuml_diagram` 工具生成 SVG

## 文件命名规范

- Agent 文件：`{name}.md`，存放于 `agents/`
- Skill 文件：`SKILL.md`，存放于 `skills/{skill-name}/`
- 方案设计中的 PlantUML 模板：`plantuml-templates.md`

## 通用约定

### 技能与 Agent 的前置声明

所有技能/Agent 文件必须以 YAML frontmatter 开头：

```yaml
---
name: {名称}
description: >-
  单行或多行描述，说明触发条件和用途
---
```

### 工作流中的临时文件

部分技能会创建临时跟踪文件（已配置 `.gitignore` 排除）：

- `.dev-plan.md`: Story 开发计划跟踪（openfuyao-developer）
- `.story-plan.md`: 单个 Story 编码计划（story2code）

### 代码评审要点

- PR 评审前必须先执行 `git fetch upstream`
- 评审后需删除本地临时分支 `pr_<IID>`
- 跨仓 PR 需传入 `fork_path` 参数

### PlantUML 渲染规范

- 优先使用 MCP 生成 SVG 文件保存到 `figures/` 目录
- MCP 不可用时保留源码，禁止用其他工具强行生成
- 文档中同时保留 SVG 引用和源码（用 `<details>` 折叠）
