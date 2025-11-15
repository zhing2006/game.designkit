---
description: 从游戏规范生成游戏设计方案计划，包含设计决策、设计框架文档和设计产物清单
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## 执行流程概述

1. **设置**：从仓库根目录运行
  Windows: `.game.design/scripts/powershell/setup-plan.ps1 -Json`
  Linux or MacOS: `.game.design/scripts/bash/setup-plan.sh --json`
并解析 JSON 获取 FEATURE_SPEC、IMPL_PLAN、SPECS_DIR、BRANCH。For single quotes in args like "I'm Groot", use escape syntax: e.g 'I'\''m Groot' (or double-quote if possible: "I'm Groot").

2. **加载上下文**：读取 FEATURE_SPEC 和 `.game.design/memory/pillars.md`（设计支柱）。加载 IMPL_PLAN 模板（已复制）。
   - **检查 [NEEDS CLARIFICATION]**：如果 FEATURE_SPEC 包含 [NEEDS CLARIFICATION] 标记，警告用户并建议先运行 `/game.designkit.clarify`。
   - **错误处理**：如果找不到 FEATURE_SPEC，引导用户先运行 `/game.designkit.specify`。

3. **执行计划工作流**：按照 IMPL_PLAN 模板结构进行：
   - 填充技术上下文（将未知项标记为 "NEEDS CLARIFICATION"）
   - 填充章程检查部分（从设计支柱）
   - 评估质量闸门（如有违规且无正当理由则 ERROR）
   - Phase 0：生成 research.md（解决所有 NEEDS CLARIFICATION）
   - Phase 1：识别设计产物类型，生成 quickstart.md
   - Phase 1：生成设计支柱对齐验证
   - 重新评估章程检查（设计后）

4. **停止并报告**：命令在 Phase 2 计划后结束。报告分支、IMPL_PLAN 路径和生成的文件。

## 阶段

### Phase 0: 研究与设计决策

1. **从技术上下文中提取未知项**：
   - 每个 NEEDS CLARIFICATION → 研究任务
   - 每个依赖项 → 最佳实践任务
   - 每个集成点 → 模式研究任务

2. **生成并派发研究任务**：

   ```text
   对技术上下文中的每个未知项：
     任务："研究 {unknown} 的 {功能上下文}"
   对每个设计方法选择：
     任务："查找 {方法} 在 {领域} 的最佳实践"
   ```

3. **整合研究结果**到 `research.md`，使用格式：
   - 决策：[选择了什么]
   - 理由：[为什么选择]
   - 备选方案：[还评估了什么]

**输出**：research.md，所有 NEEDS CLARIFICATION 已解决

### Phase 1: 产物规划（在 plan.md 中定义，不实际生成）

**前置条件**：`research.md` 完成

**重要**：Phase 1 是**规划产物**，在 plan.md 中定义需要哪些文档，但**不实际生成这些文档**。实际文档编写将在 Phase 2（implement 命令）中完成。

1. **识别设计产物类型**（规划，不生成）：
   - 判断规范类型（从 BRANCH 或 SPECS_DIR 提取编号，000-* 为全局型，001+ 为功能型）
   - 分析 spec.md 所定义的内容和需求
   - **全局型规范（000-*）**：识别需要的全局设计文档
     - 如定义游戏愿景和目标 → 规划 game-vision.md（不生成）
     - 如定义核心玩法循环 → 规划 core-loop.md（不生成）
     - 如定义世界观和背景设定 → 规划 world-setting.md（不生成）
     - 如定义体验节奏 → 规划 emotional-curve.md（不生成）
   - **功能型规范（001+）**：识别需要的功能设计文档
     - 如定义玩法机制和操作 → 规划 gameplay-design.md（不生成）
     - 如定义数值系统和公式 → 规划 numerical-framework.md（不生成）
     - 如涉及多系统交互 → 规划 system-integration.md（不生成）
     - 如涉及难度和奖励设计 → 规划 balance-analysis.md（不生成）
   - **在 plan.md 中定义每个产物**：目的、结构、内容来源、输出路径（规划文档，不生成文件）

2. **生成设计支柱对齐验证**（在 plan.md 中）：
   - 读取 `.game.design/memory/pillars.md`（如果存在）
   - 在 plan.md 中生成"设计支柱对齐验证"章节
   - 包含对齐度表格：设计支柱 / 本设计的体现 / 对齐度 / 潜在冲突

3. **生成快速开始指南**：
   - 创建 `quickstart.md`
   - 说明如何使用本计划进行设计工作

**输出**：设计产物规划（在 plan.md 中定义）、设计支柱对齐验证（在 plan.md 中）、quickstart.md

**Phase 1 不输出**：gameplay-design.md、numerical-framework.md 等实际策划案文档（这些由 tasks 命令安排，implement 命令编写）

## 关键规则

- 使用绝对路径
- 质量闸门失败或未解决的澄清时 ERROR
- 读取设计支柱并验证对齐度
- 根据规范内容自动识别设计产物类型
