---
description: 根据游戏设计方案生成可执行的、按依赖顺序的设计任务分解
---

## User Input

```text
$ARGUMENTS
```

在继续之前，**必须**考虑用户输入（如果非空）。

## 执行流程概述

1. **设置**：从仓库根目录运行
  Windows: `.game.design\scripts\powershell\check-prerequisites.ps1 -Json`
  Linux or MacOS: `.game.design/scripts/bash/check-prerequisites.sh --json`
并解析 FEATURE_DIR 和 AVAILABLE_DOCS。所有路径必须是绝对路径。对于参数中的单引号（如 "I'm Groot"），使用转义语法：'I'\''m Groot'（或使用双引号："I'm Groot"）。

2. **加载设计文档**：从 FEATURE_DIR 读取：
   - **必需**：plan.md（设计产物清单）、spec.md（用户故事和优先级）
   - **可选**：research.md（设计决策）、quickstart.md（快速开始指南）
   - 注意：GameDesignKit 读取所有 plan 阶段文档以获取更丰富的上下文。根据可用文档生成任务。
   - **重要**：虽然 research.md 和 quickstart.md 标记为"可选"，但在生成 Setup 阶段任务时，应该验证这些文档的存在（如果它们存在于 AVAILABLE_DOCS 中）。

3. **执行任务生成工作流**：
   - 加载 plan.md 并提取设计产物清单、设计方法
   - 加载 spec.md 并提取用户故事及其优先级（P1、P2、P3 等）
   - 如 research.md 存在：提取设计决策用于任务上下文
   - 如 quickstart.md 存在：提取快速入门信息
   - 生成按用户故事组织的任务（参见下方任务生成规则）
   - 生成依赖关系图，显示用户故事完成顺序
   - 创建每个故事的并行执行示例
   - 验证任务完整性（每个用户故事有所需的所有任务，独立可测）

4. **生成 tasks.md**：使用 `.game.design/templates/tasks-template.md` 作为结构，填充：
   - 从 plan.md 获取正确的功能名称
   - Phase 1：Setup 任务（验证文件、创建 docs/ 目录）
   - Phase 2：基础任务（全局依赖 - 游戏设计通常为空）
   - Phase 3+：每个用户故事一个阶段（按 spec.md 优先级顺序）
   - 每个阶段包含：故事目标、独立测试标准、设计文档编写任务
   - 最终阶段：润色和跨领域关注点（一致性审查、对齐验证）
   - 所有任务必须遵循严格的检查清单格式（参见下方任务生成规则）
   - 明确的文件路径
   - 依赖关系部分，显示故事完成顺序
   - 每个故事的并行执行示例
   - 实施策略部分（MVP 优先、增量交付）

5. **报告**：输出生成的 tasks.md 路径和摘要：
   - 总任务数
   - 每个用户故事的任务数
   - 识别的并行机会
   - 每个故事的独立测试标准
   - 建议的 MVP 范围（通常只包含用户故事 1）
   - 格式验证：确认所有任务遵循检查清单格式（复选框、ID、标签、文件路径）

任务生成上下文：$ARGUMENTS

tasks.md 应该可直接执行 - 每个任务必须足够具体，AI 可以在没有额外上下文的情况下完成。

## 任务生成规则

**关键**：任务必须按用户故事组织，以实现独立实施和测试。

**测试任务**：游戏策划工作通过实际使用验证，不生成测试代码任务。

### 检查清单格式（必需）

每个任务必须严格遵循此格式：

```text
- [ ] [TaskID] [P?] [Story?] 描述（含文件路径）
```

**格式组成**：

1. **Checkbox**：始终以 `- [ ]` 开头（markdown 复选框）
2. **Task ID**：顺序编号（T001、T002、T003...）按执行顺序
3. **[P] 标记**：仅当任务可并行时包含（不同文件，无依赖未完成任务）
4. **[Story] 标签**：仅用户故事阶段任务需要
   - 格式：[US1]、[US2]、[US3] 等（对应 spec.md 中的用户故事）
   - Setup 阶段：无故事标签
   - Foundational 阶段：无故事标签
   - User Story 阶段：必须有故事标签
   - Polish 阶段：无故事标签
5. **Description**：清晰的操作，含确切文件路径

**示例**：

- ✅ 正确：`- [ ] T001 创建项目结构`
- ✅ 正确：`- [ ] T005 [P] 编写设计文档 docs/xxx/gameplay-design.md`
- ✅ 正确：`- [ ] T012 [P] [US1] 创建玩法设计文档 docs/feature/gameplay-design.md`
- ✅ 正确：`- [ ] T014 [US1] 设计数值公式 docs/feature/numerical-framework.md`
- ❌ 错误：`- [ ] 创建设计文档`（缺少 ID 和路径）
- ❌ 错误：`T001 [US1] 创建文档`（缺少 checkbox）
- ❌ 错误：`- [ ] [US1] 创建设计文档`（缺少 Task ID）
- ❌ 错误：`- [ ] T001 [US1] 创建文档`（缺少文件路径）

### 任务组织

1. **从用户故事（spec.md）** - 主要组织方式：
   - 每个用户故事（P1、P2、P3...）获得自己的阶段
   - 将所有相关设计产物映射到其故事：
     - 该故事需要的设计文档
     - 该故事需要的数值设计
     - 该故事需要填充的框架
   - 标记故事依赖关系（大多数故事应独立）

2. **从设计产物（plan.md）**：
   - 将每个产物（gameplay-design.md、numerical-framework.md 等）→ 映射到其服务的用户故事
   - 每个产物 → 该故事阶段中的编写/填充任务
   - 输出路径：docs/[feature-name]/[deliverable-name].md

3. **从设计决策（research.md）**：
   - 提取设计方法和理由
   - 用于丰富任务描述的上下文
   - 在任务描述中引用决策

4. **从设置/基础设施**：
   - 验证前置文件 → Setup 阶段（Phase 1）
   - 创建 docs/ 目录 → Setup 阶段（Phase 1）
   - 基础阶段通常为空（游戏设计）

### 阶段结构

- **Phase 1**：Setup（验证文件、创建目录）
- **Phase 2**：Foundational（全局依赖 - 游戏设计通常为空）
- **Phase 3+**：用户故事按优先级顺序（P1、P2、P3...）
  - 每个故事内：编写设计文档 → 填充数值框架 → 完成相关设计
  - 每个阶段应该是完整的、独立可审查的设计增量
- **Final Phase**：润色和跨领域关注点（一致性、对齐）
