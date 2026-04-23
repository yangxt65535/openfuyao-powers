---
title: oFEP Template
ofep-number: NNNN
authors:
  - "@jane.doe"
owning-sig: sig-xyz
participating-sigs:
  - sig-aaa
  - sig-bbb
status: provisional|implementable|implemented|deferred|rejected|withdrawn|replaced
creation-date: yyyy-mm-dd
reviewers:
  - TBD
  - "@alice.doe"
approvers:
  - TBD
  - "@oscar.doe"
see-also:
  - "/ofeps/sig-aaa/1234-we-heard-you-like-ofeps"
  - "/ofeps/sig-bbb/2345-everyone-gets-a-ofep"
replaces:
  - "/ofeps/sig-ccc/3456-replaced-ofep"
replaced-by:
  - "/ofeps/sig-ddd/4567-new-ofep"
stage: alpha|beta|stable
latest-milestone: "v1.19"
milestone:
  alpha: "v1.19"
  beta: "v1.20"
  stable: "v1.22"
feature-gates:
  - name: MyFeature
    components:
      - kube-apiserver
      - kube-controller-manager
disable-supported: true
metrics:
  - my_feature_metric
---
# oFEP-NNNN：你的简短描述性标题

<!-- toc -->
- [发布签核清单](#release-signoff-checklist)
- [摘要](#summary)
- [动机](#motivation)
  - [目标](#goals)
  - [非目标](#non-goals)
- [提案](#proposal)
  - [用户故事（可选）](#user-stories-optional)
    - [故事 1](#story-1)
    - [故事 2](#story-2)
  - [注释/约束/警告（可选）](#notesconstraintscaveats-optional)
  - [风险与缓解措施](#risks-and-mitigations)
- [设计细节](#design-details)
  - [测试计划](#test-plan)
      - [先决条件测试更新](#prerequisite-testing-updates)
      - [单元测试](#unit-tests)
      - [集成测试](#integration-tests)
      - [e2e 测试](#e2e-tests)
  - [毕业标准](#graduation-criteria)
  - [升级/降级策略](#upgrade--downgrade-strategy)
  - [版本倾斜策略](#version-skew-strategy)
- [生产准备情况评审问卷](#production-readiness-review-questionnaire)
  - [功能启用和回滚](#feature-enablement-and-rollback)
  - [推出、升级和回滚规划](#rollout-upgrade-and-rollback-planning)
  - [监控要求](#monitoring-requirements)
  - [依赖项](#dependencies)
  - [可扩展性](#scalability)
  - [故障排除](#troubleshooting)
- [实施历史](#implementation-history)
- [缺点](#drawbacks)
- [替代方案](#alternatives)
- [所需基础设施（可选）](#infrastructure-needed-optional)
<!-- /toc -->

## 发布签核清单

标记有（R）的项目*在达到里程碑/发布*之前是必需的。
- [ ]（R）发布里程碑中的增强问题，链接到 [openfuyao/ofep] 中的 oFEP 目录
- [ ] (R) oFEP 审批者已批准 oFEP 状态为"可实施"
- [ ] (R) 设计细节已适当记录
- [ ]（R）测试计划已到位，并考虑了 SIG 架构和 SIG 测试的输入（包括测试重构）
  - [ ] 针对所有 Beta API 操作（端点）进行 e2e 测试
  - [ ] (R) 确保 GA e2e 测试满足一致性测试的要求
  - [ ] (R) GA e2e 测试至少需要两周时间才能证明测试结果无 flake（不稳定或偶发失败）
- [ ] (R) 毕业标准已设定
  - [ ] (R) 所有 GA 端点必须通过[一致性测试]
- [ ] (R) 生产准备情况审查完成
- [ ] (R) 生产准备情况审查已获批准
- [ ] "实施历史"部分已更新里程碑
- [ ] 面向用户的文档已在 [openfuyao/docs] 创建，以便发布到 [openfuyao.cn]
- [ ] 支持文档 - 例如，额外的设计文档、邮件列表讨论/SIG 会议链接、相关 PR/问题、发行说明

- [openfuyao.cn](https://openfuyao.cn/)
- [openfuyao/ofep](https://gitcode.com/openfuyao/ofep)
- [openfuyao/docs](https://gitcode.com/openfuyao/docs)

## 摘要

本节应提供该 oFEP 的核心概述，帮助读者快速理解提案内容。
好的摘要通常包含：
- 提案要解决的问题
- 提议的解决方案概述
- 预期收益

## 动机

本节用于明确列出该 oFEP 的动机、目标和非目标。描述变更的重要性以及对用户的好处。

### 目标

- 列出 oFEP 的具体目标
- 定义成功标准

### 非目标

- 明确列出超出本 oFEP 范围的内容
- 帮助聚焦讨论并取得进展

## 提案

本节应包含提案的具体内容，阐明：
- 预期目标是什么？
- 我们如何衡量成功？

### 用户故事（可选）

详细说明如果该 oFEP 被实施，用户将能够做哪些事情。

#### 故事 1

#### 故事 2

### 注释/约束/警告（可选）

该提案有哪些注意事项或潜在限制？
有没有上文未能充分表达的重要细节？

### 风险与缓解措施

这个提案存在哪些风险？我们将如何加以缓解？请从广泛的角度思考，例如包括安全性问题，以及它可能对更大范围的 openfuyao 生态系统产生的影响。

## 设计细节

本节应包含足够的信息，以便读者能够清楚理解你所提出的变更具体是什么。这可能包括 API 规格说明（虽然并非必须）或代码片段。

### 测试计划

[ ] 我/我们理解，相关组件的所有者可能会要求更新已有的测试，以便在提交实现该增强功能所需的更改之前，使代码达到足够稳固的质量标准。

##### 先决条件测试更新

描述在实施此项增强功能之前需要补充哪些额外的测试。

##### 单元测试

- `<软件包>`: `<日期>` - `<测试覆盖率>`

##### 集成测试

- 测试名称

##### e2e 测试

- 测试名称

### 毕业标准

#### Alpha 阶段
- 功能已通过 Feature Gate 实现（默认关闭）
- 初步的端到端（e2e）测试已完成并启用

#### Beta 阶段
- 收集开发者反馈及用户调研结果
- 完成核心功能
- 额外的测试已加入 Testgrid
- 实现更严格的测试形式，例如降级测试和可扩展性测试
- 所有功能均已实现
- 所有安全相关机制均已完备
- 所有监控要求均已实现
- 所有测试要求均已满足
- 所有预发布阶段的问题与缺陷均已修复

#### GA 阶段
- 有 N 个真实生产环境中的使用示例
- 有 N 次实际安装部署记录
- 已留出反馈窗口期，收集充分用户反馈
- 所有在 Beta 阶段反馈的问题与缺陷都已解决

#### 弃用（Deprecation）
- 宣布弃用现有功能标志（flag）并说明支持策略
- 自引入替代功能以来，已过去两个版本
- 处理来自 gitcode Issues 等反馈渠道中的用法变更或行为差异问题
- 正式弃用原有标志（flag）

### 升级/降级策略

如果适用，该组件在升级和降级时将如何处理？

### 版本倾斜策略

如果适用，该组件在面对与其他组件的版本不一致（version skew）时将如何处理？

## 生产准备情况评审问卷

### 功能启用和回滚

###### 如何在实时集群中启用/禁用此功能？

- [ ] 功能开关（Feature gate）
  - 功能开关名称：
  - 依赖该功能开关的组件：

###### 启用该功能会改变任何默认行为吗？

###### 该功能一旦启用，是否可以禁用（即我们可以回滚启用）？

###### 如果该功能之前已回滚，现在我们重新启用它会发生什么情况？

###### 是否有任何针对功能启用/禁用的测试？

### 推出、升级和回滚规划

###### 部署或回滚为何会失败？这会影响正在运行的工作负载吗？

###### 哪些具体指标应该通知回滚？

###### 升级和回滚测试了吗？升级->降级->升级的路径测试了吗？

###### 推出时是否伴随任何功能、API、API 类型的字段、标志等的弃用和/或删除？

### 监控要求

###### 操作员如何确定该功能是否正在被工作负载使用？

###### 使用此功能的人如何知道它适用于他们的实例？

- [ ] 事件
  - 事件原因：
- [ ] API .状态
  - 条件名称：

###### 增强的合理 SLO（服务级别目标）是什么？

###### 运维人员可以使用哪些服务级别指标（SLIs）来判断服务的健康状况？

- [ ] 指标
  - 指标名称：
  - 暴露指标的组件：

###### 是否存在任何尚未覆盖的指标（metrics），可以用来进一步提升该功能的可观测性？

### 依赖项

###### 此功能是否依赖于集群中运行的任何特定服务？

### 可扩展性

###### 启用/使用此功能会导致任何新的 API 调用吗？

###### 启用/使用此功能是否会导致引入新的 API 类型？

###### 启用/使用此功能是否会导致对云提供商的任何新的 API 调用？

###### 启用/使用此功能是否会导致现有 API 对象的大小或数量增加？

###### 启用/使用此功能是否会导致现有 SLI/SLO 所涵盖操作的耗时增加？

###### 启用/使用此功能是否会导致任何组件的资源使用率（CPU、RAM、磁盘、IO 等）不可忽略的增加？

###### 启用/使用此功能是否会导致某些节点资源（PID、套接字、inode 等）耗尽？

### 故障排除

###### 如果 API 服务器和/或 etcd 不可用，此功能如何反应？

###### 其他已知故障模式有哪些？

###### 如果未满足 SLO，应采取哪些步骤来确定问题？

## 实施历史

- 记录 oFEP 生命周期中的主要里程碑

## 缺点

为什么不实施这个 oFEP？从反面角度分析该提案可能带来的负面影响、权衡成本、潜在风险或争议点。

## 替代方案

你还考虑过哪些其他方案？为什么将它们排除？

## 所需基础设施（可选）

如果你需要从项目或 SIG 获得资源支持，请在本节中列出。
