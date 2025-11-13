# 实施计划：[功能]

**分支**：`[###-feature-name]` | **日期**：[日期] | **规范**：[链接]
**输入**：来自 `/gamedesigns/[###-feature-name]/spec.md` 的游戏规范

**注意**：此模板由 `/game.designkit.plan` 命令填写。有关执行工作流程，请参见 `.game.design/templates/commands/plan.md`。

## 摘要

[从功能规范中提取：主要需求 + 来自研究的技术方法]

## 设计上下文

<!--
  ACTION REQUIRED: Replace the content in this section with the game design details
  for the project. The structure here is presented in advisory capacity to guide
  the iteration process.
-->

**Game Type/Genre**: [例如，RPG、策略、动作、模拟经营 或 NEEDS CLARIFICATION]
**Target Platform**: [例如，PC、移动端、主机、Web 或 NEEDS CLARIFICATION]
**Design Method**: [例如，关卡设计、数值设计、系统设计 或 NEEDS CLARIFICATION]
**Dependent Specs**: [例如，000-game-vision、001-combat-system 或 N/A]
**Reference Games**: [如适用，例如，《塞尔达》、《文明》、《黑暗之魂》 或 N/A]
**Spec Type**: [全局型(000-*) / 功能型(001+)]
**Experience Goals**: [特定领域，例如，策略深度、操作爽快感、叙事沉浸 或 NEEDS CLARIFICATION]
**Design Constraints**: [特定领域，例如，单局30分钟、学习曲线平缓、支持多人 或 NEEDS CLARIFICATION]
**Scale/Scope**: [特定领域，例如，10个关卡、50个技能、100小时内容 或 NEEDS CLARIFICATION]

## 章程检查

*门限：必须在第 0 阶段研究之前通过。在第 1 阶段设计后重新检查。*

[根据章程文件确定的门限]

## 项目结构

### 文档（此功能）

```
gamedesigns/[###-feature]/
├── plan.md              # 此文件（/game.designkit.plan 命令输出）
├── research.md          # 第 0 阶段输出（/game.designkit.plan 命令）
├── quickstart.md        # 第 1 阶段输出（/game.designkit.plan 命令）
├── checklists/          # 质量检查清单
│   └── plan-quality.md
└── tasks.md             # 第 2 阶段输出（/game.designkit.tasks 命令）
```

### 设计产物结构（仓库根目录）

```
gamedesigns/[###-feature]/
├── spec.md              # 游戏规范（输入）
├── plan.md              # 设计方案（本文件）
├── research.md          # 设计决策研究
├── quickstart.md        # 快速开始指南
├── frameworks/          # 设计框架文档（可选）
│   ├── gameplay-design.md       # 玩法设计框架
│   ├── numerical-framework.md   # 数值设计框架
│   ├── balance-analysis.md      # 平衡性分析框架
│   └── system-integration.md    # 系统集成框架
└── checklists/          # 质量检查清单
    └── plan-quality.md
```

**结构说明**：GameDesignKit 生成设计文档而非代码，frameworks/ 目录包含游戏设计特有的框架文档。

## 复杂度跟踪

> **仅当章程检查有必须证明的违规时才填写**

| 违规 | 需要原因 | 拒绝更简单替代方案的原因 |
|------|----------|-------------------------|
| [例如，第 4 个项目] | [当前需求] | [为什么 3 个项目不够] |
| [例如，Repository 模式] | [具体问题] | [为什么直接 DB 访问不够] |

---

## 设计产物清单

根据游戏规范类型和内容，本计划识别出以下需要产出的设计文档：

<!--
  NOTE: Global specs (000-*) and feature specs (001+) have different deliverable types.
  Command will auto-detect spec type from branch/directory number and identify needed documents
  based on defined content and requirements, not just keywords.
-->

### 全局型规范（000-*）产物示例

1. **game-vision.md**：游戏愿景文档
   - 目的：定义游戏的核心价值主张和设计目标
   - 结构：目标玩家、核心体验、差异化优势、商业目标

2. **core-loop.md**：核心玩法循环设计
   - 目的：描述游戏的核心玩法循环和反馈机制
   - 结构：玩法循环图、输入-反馈、短期/中期/长期循环

3. **world-setting.md**：世界观设定文档
   - 目的：定义游戏世界的规则和背景设定
   - 结构：世界规则、背景故事、派系/角色、时间线

4. **emotional-curve.md**：情感曲线设计
   - 目的：设计游戏的情感体验节奏和高低起伏
   - 结构：体验阶段、情感强度、节奏控制、高潮设计

### 功能型规范（001+）产物示例

**核心文档**：

1. **gameplay-design.md**：玩法设计文档
   - 目的：详细描述游戏玩法机制、玩家操作流程
   - 结构：玩法循环、操作输入、反馈系统、玩法变化

**可选文档（根据功能内容）**：

2. **numerical-framework.md**：数值框架文档（如涉及数值设计）
   - 目的：定义数值设计方法论和计算公式
   - 结构：数值分层、成长曲线、平衡公式、测试用例

3. **system-integration.md**：系统集成文档（如涉及多系统交互）
   - 目的：说明系统间依赖关系和交互流程
   - 结构：系统依赖图、交互时序、状态同步

4. **balance-analysis.md**：平衡性分析文档（如涉及难度平衡）
   - 目的：分析游戏难度曲线和奖励平衡
   - 结构：难度分级、奖励机制、进度控制

5. **narrative-design.md**：叙事设计文档（如涉及剧情故事）
   - 目的：设计游戏叙事结构和角色发展
   - 结构：故事线、角色弧光、对话系统、分支结构

---

## 设计支柱对齐验证

根据 `.game.design/memory/pillars.md` 定义的设计支柱，验证本设计方案的对齐情况：

| 设计支柱 | 本设计的体现 | 对齐度 | 潜在冲突 |
|---------|------------|-------|---------|
| [支柱1] | [说明如何体现] | ✅ 完全对齐 | 无 |
| [支柱2] | [说明如何体现] | ⚠️ 部分对齐 | [描述冲突] |

**对齐结论**：[总结设计是否符合设计支柱，是否需要调整]