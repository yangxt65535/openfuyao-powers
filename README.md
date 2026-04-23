# openfuyao-powers

openFuyao 社区 Superpowers 技能集合，提供从需求到代码的标准化研发工作流支持。

## 简介

本项目是 Claude Code 的插件化技能集合，覆盖 openFuyao 软件研发全生命周期，并提供若干 Gitcode 平台操作 Skill：

- **需求分析** → **方案设计** → **代码实现与开发者测试** → **集成测试**

## 技能清单

| 技能 | 描述 | 触发时机 |
|------|------|----------|
| `openfuyao-requirement-decomposition` | IR→SR→AR 三级需求拆解 | 收到原始需求文档、企业立项、社区提案 |
| `openfuyao-solution-design` | 方案设计与 4+1 架构视图 | 需求分析完成，进入方案设计阶段 |
| `openfuyao-story2code` | Story 编码实现（双 Agent 协作） | 实现具体 Story |
| `gitcode-pr-review` | GitCode PR 评审 | 需要评审 Pull Request |
| `gitcode-issue-fixer` | Issue 自动化修复 | 修复 GitCode Issue |

## Agent 清单

| Agent | 描述 |
|-------|------|
| `openfuyao-developer` | Story 编码实现与 PR 管理，按依赖顺序实现并创建 PR |

## 前置依赖

- Claude Code 或兼容的 AI 编程助手
- MCP 服务配置（`.mcp.json`），使用 `npx` 执行三方库，无需手动安装：
  - `gitcode-mcp`: GitCode 平台交互（需 `GITCODE_TOKEN`）
  - `plantuml`: PlantUML 图表渲染

## 使用方式

在 Claude Code 中直接调用技能：

```
请拆解需求：<需求描述>
```

或明确指定技能：

```
使用 openfuyao-solution-design 进行方案设计
```

## 目录结构

```
agents/          # Agent 定义文件
skills/          # Skill 技能定义文件
.claude-plugin/  # 插件配置
.mcp.json        # MCP 服务配置
```

## 文档

- [CLAUDE.md](CLAUDE.md) - 项目开发指南
