# 更新日志

本文档记录 GameDesignKit 的所有重要变更。

格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.0.0/)，
版本号遵循 [语义化版本](https://semver.org/lang/zh-CN/)。

## [未发布]

### 计划功能
- 命令多语言支持（英文版本）
- 模板多语言支持（英文版本）

---

## [0.1.5] - 2025-01-19

### 新增功能

- **Gemini CLI, Kilo Code 支持** - 通过符号链接复用 Claude Code 命令文件
  - 添加 `.gemini/commands` 符号链接指向 `.claude/commands`
  - 添加 `.kilocode/workflows` 符号链接指向 `.claude/commands`
  - 所有 7 个命令文件自动在 Gemini CLI, Kilo Code 中可用
  - 零维护成本：修改 `.claude/commands/` 中的文件自动同步
  - 更新 `.gitignore` 规则以正确跟踪符号链接目录

---

## [0.1.4] - 2025-01-17

### 新增功能

- **命令工作流切换（Handoffs）** - 添加命令间无缝切换支持
  - `/game.designkit.pillars` 可切换到 `specify`（构造规范）
  - `/game.designkit.specify` 可切换到 `clarify`（澄清需求）或 `plan`（制定计划）
  - `/game.designkit.clarify` 可切换到 `plan`（制定计划）
  - `/game.designkit.plan` 可切换到 `tasks`（创建任务）
  - `/game.designkit.tasks` 可切换到 `implement`（编写文档）或 `analyze`（分析一致性）
  - 优化工作流体验，减少用户手动切换命令的步骤

### 改进

- **`create-new-feature` 脚本代码重构** - 同步 Speckit v0.0.85 改进
  - 新增 `get_highest_from_specs()` / `Get-HighestNumberFromSpecs` 函数：从 gamedesigns 目录提取最高编号（跳过 000）
  - 新增 `get_highest_from_branches()` / `Get-HighestNumberFromBranches` 函数：从所有 git 分支获取最高编号
  - 新增 `clean_branch_name()` / `ConvertTo-CleanBranchName` 函数：统一分支名称清理逻辑
  - 修复 `SCRIPT_DIR` 设置：添加 `CDPATH=""` 防止路径解析错误
  - 修改 `check_existing_branches()` / `Get-NextBranchNumber` 函数：添加 `gamedesigns_dir` 参数以支持目录路径传递
  - 重构内联逻辑为可复用函数，提高代码可维护性和可测试性
  - Bash 和 PowerShell 脚本保持一致的代码结构

---

## [0.1.3] - 2025-01-15

### 改进

- **`/game.designkit.plan` 命令优化** - 澄清 Phase 1 设计产物规划流程
  - 明确 Phase 1 是**规划产物**而非实际生成文档
  - 强调在 plan.md 中定义产物的目的、结构、内容来源和输出路径
  - 实际策划案文档（gameplay-design.md、numerical-framework.md 等）由 tasks 命令安排，implement 命令编写
  - 提升命令责任边界清晰度，避免规划与执行阶段混淆

### 修复

- **跨平台换行符规范化** - 确保所有文本文件使用一致的换行符
  - 添加 `.gitattributes` 文件配置 LF 换行符为标准格式
  - 禁用 Git autocrlf 配置，避免 Windows/Linux/MacOS 跨平台差异
  - 应用换行符规范化到所有命令文件（analyze/clarify/implement/plan/specify/tasks）
  - 确保 Shell 脚本（.sh）和 PowerShell 文件（.ps1）在所有平台正常执行

---

## [0.1.2] - 2025-01-14

### 改进

- **`/game.designkit.specify` 命令增强** - 多次迭代优化规范文件读取
  - 改进全局规范文件（000-*）的读取逻辑，优化对游戏愿景和核心循环文档的支持
  - 重新组织执行步骤顺序，提升命令执行的逻辑清晰度
  - 添加步骤 2.5：在生成规范前，先读取并分析所有相关的全局规范文档
  - 将步骤 4.a 和 4.b（依赖分析和术语映射）移至步骤 2，提前进行上下文分析
  - 优化 000-* 全局规范文件的识别和读取策略

### 修复

- **PowerShell 路径格式** - 修正 Windows PowerShell 脚本中的路径分隔符
  - 将 PowerShell 脚本中的反斜杠 `\` 改为正斜杠 `/`，提升跨平台兼容性

---

## [0.1.1] - 2025-01-14

### 新增功能

- **Windows PowerShell 支持** - 完整的跨平台脚本支持
  - 同步所有 bash 脚本改造：`common.ps1`, `check-prerequisites.ps1`, `create-new-feature.ps1`, `setup-plan.ps1`
  - 支持 `-Type <global|feature>` 参数区分规范类型
  - 支持 `--number` 参数手动指定分支编号
  - 目录结构完全对齐：`gamedesigns/`, `.game.design/`
  - 环境变量：`GAMEDESIGN_FEATURE`
  - 输出包含 `SPEC_TYPE` 字段

- **Codex CLI, Cursor 支持** - 通过符号链接复用 Claude Code 命令文件
  - 添加 `.codex/prompts`, `.cursor/commands` 符号链接指向 `.claude/commands`
  - 所有7个命令文件自动在 Codex CLI, Cursor 中可用
  - 零维护成本：修改 `.claude/commands/` 中的文件自动同步到 Codex CLI, Cursor
  - 修复 `.gitignore` 规则以正确跟踪符号链接（去掉尾部斜杠）

### 改进

- **脚本重构：SPEC_TYPE 提取统一化**
  - 将 spec type 判断逻辑移至 `common.sh` / `common.ps1` 的 `get_feature_paths()` 函数
  - 所有脚本通过 `SPEC_TYPE` 变量自动获取规范类型（global/feature）
  - 删除 `check-prerequisites` 中的重复代码
  - `setup-plan` 新增 `SPEC_TYPE` 输出字段
  - 提升代码可维护性和一致性

- **`create-new-feature.sh/.ps1`** - 增强分支编号管理，避免多人协作冲突
  - 添加 `check_existing_branches()` / `Get-NextBranchNumber` 函数
  - 检查三个源：远程分支、本地分支、gamedesigns 目录
  - 添加 `--number` / `-Number` 参数支持手动指定分支编号
  - 在创建新 feature spec 前自动 `git fetch` 获取最新远程分支信息
  - 基于相同 short-name 的最高编号自动递增，防止编号冲突
  - 同步 Speckit v0.0.79 改进

- **命令文件跨平台优化** - 所有命令支持 OS 检测
  - 6个命令文件（specify/plan/tasks/implement/analyze/clarify）添加操作系统检测
  - Windows 自动调用 PowerShell 脚本（PascalCase 参数风格）
  - Linux/MacOS 自动调用 Bash 脚本（kebab-case 参数风格）
  - 路径分隔符自动适配（Windows `\` vs Unix `/`）
  - AI 根据操作系统无缝选择正确的脚本执行

---

## [0.1.0] - 2025-01-13

### 新增功能

#### 核心命令（共7个）

- **`/game.designkit.pillars`** - 设计支柱管理
  - 从自然语言提取设计原则
  - 验证支柱质量（具体性、冲突检测、维度覆盖）
  - 根据游戏规模支持 1-7 个支柱
  - 存储位置：`.game.design/memory/pillars.md`

- **`/game.designkit.specify`** - 游戏/功能规范编写
  - 自动识别规范类型：全局（000-*）vs 功能（001+）
  - 从自然语言描述生成结构化规范
  - 创建需求验证质量清单
  - 验证与设计支柱的对齐度
  - 支持最多 3 个澄清标记

- **`/game.designkit.clarify`** - 模糊点澄清
  - 最多提出 5 个针对性问题
  - 结构化澄清工作流
  - 将答案直接记录到 spec.md
  - 根据规范类型调整提问粒度（全局 vs 功能）

- **`/game.designkit.plan`** - 设计方案生成
  - 创建设计交付物清单
  - 生成 `research.md`（设计决策与依据）
  - 生成 `quickstart.md`（设计使用指南）
  - 验证支柱对齐度
  - 识别需要的设计框架文档

- **`/game.designkit.tasks`** - 任务分解
  - 按用户故事组织，分优先级（P1/P2/P3）
  - 支持每个故事独立实现
  - 生成依赖关系图
  - 严格清单格式（任务ID、优先级、故事映射）

- **`/game.designkit.analyze`** - 一致性分析
  - 只读分析模式
  - 检测重复、模糊和遗漏
  - 验证所有文档的支柱对齐度
  - 生成结构化分析报告
  - 严重性分级（CRITICAL/HIGH/MEDIUM/LOW）

- **`/game.designkit.implement`** - 设计执行
  - 解析并执行 `tasks.md` 中的任务
  - 执行前验证清单状态
  - 遵守任务依赖和并行执行标记
  - 渐进式跟踪进度并更新状态

#### 设计框架

- **设计支柱系统**
  - 支柱变更的语义化版本管理
  - 验证规则（≥20字、无空洞短语、冲突检测）
  - 基于项目规模的数量建议（小型/独立/大型）

- **双类型规范**
  - 全局规范（000-*）：游戏愿景、世界观、核心玩法循环、情感曲线
  - 功能规范（001+）：具体系统（战斗、装备、对话等）
  - 基于关键词自动分类
  - 每种类型有不同的模板和验证规则

- **文件组织结构**
  - `.game.design/memory/` - 共享设计知识（支柱）
  - `.game.design/templates/` - 规范和方案模板
  - `gamedesigns/###-feature-name/` - 每个功能的工作目录
  - `docs/feature-name/` - 最终输出文档

#### 模板文件

- `spec-template-global.md` - 游戏愿景规范模板（000-*）
- `spec-template-feature.md` - 功能系统规范模板（001+）
- `plan-template.md` - 设计方案结构模板
- `tasks-template.md` - 任务分解模板
- `pillars.md` - 设计支柱记忆模板

#### 自动化脚本

- `check-prerequisites.sh` - 验证项目设置要求
- `common.sh` - Bash 脚本共享工具函数
- `create-new-feature.sh` - 创建新功能设计目录脚手架
- `setup-plan.sh` - 从模板初始化 plan.md

#### 文档

- 中文 README（`README.md`），包含完整工作流指南
- 英文 README（`README_EN.md`），结构与中文版对应
- 最佳实践章节，涵盖：
  - 设计支柱建议（好例子 vs 坏例子）
  - 规范编写技巧（体验目标 vs 实现细节）
  - MVP 策略（P1 → P2 → P3 渐进式交付）
  - 团队协作工作流
  - 常见陷阱及避免方法

#### 配置文件

- `.claude/settings.local.json` - Claude Code 集成设置
- `.mcp.json` - Model Context Protocol 配置
- `.gitignore` - Git 排除规则

### 设计理念

- **规范驱动工作流**：规范可执行，不会被丢弃
- **支柱驱动验证**：所有功能必须与设计支柱对齐
- **迭代式细化**：多轮改进，而非一次性生成
- **AI 辅助**：利用 Claude 进行规范解读和文档生成
- **文档优先**：活文档，而非过时的 Word 文件

### 与原版 Speckit 的差异

- **输出物**：设计文档（玩法、数值框架、平衡分析）而非源代码
- **原则系统**：设计支柱（创意原则）而非项目宪法（技术约束）
- **规范类型**：双类型（全局/功能）而非单一类型
- **验证方式**：文档完整性和支柱对齐，而非自动化测试
- **领域**：体验设计而非技术实现
- **语言**：中文（面向中国游戏策划）而非英文

---

## 版本历史

[未发布]: https://github.com/zhing2006/game.designkit/compare/v0.1.5...HEAD
[0.1.5]: https://github.com/zhing2006/game.designkit/compare/v0.1.4...v0.1.5
[0.1.4]: https://github.com/zhing2006/game.designkit/compare/v0.1.3...v0.1.4
[0.1.3]: https://github.com/zhing2006/game.designkit/compare/v0.1.2...v0.1.3
[0.1.2]: https://github.com/zhing2006/game.designkit/compare/v0.1.1...v0.1.2
[0.1.1]: https://github.com/zhing2006/game.designkit/compare/v0.1.0...v0.1.1
[0.1.0]: https://github.com/zhing2006/game.designkit/releases/tag/v0.1.0
