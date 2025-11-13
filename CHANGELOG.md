# 更新日志

本文档记录 GameDesignKit 的所有重要变更。

格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.0.0/)，
版本号遵循 [语义化版本](https://semver.org/lang/zh-CN/)。

## [未发布]

### 改进

- **`create-new-feature.sh`** - 增强分支编号管理，避免多人协作冲突
  - 添加 `check_existing_branches()` 函数，检查三个源（远程分支、本地分支、gamedesigns目录）
  - 添加 `--number` 参数支持手动指定分支编号
  - 在创建新 feature spec 前自动 `git fetch` 获取最新远程分支信息
  - 基于相同 short-name 的最高编号自动递增，防止编号冲突
  - 同步 Speckit v0.0.79 改进

### 计划功能
- 命令多语言支持（英文版本）
- 模板多语言支持（英文版本）

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

[未发布]: https://github.com/yourusername/game.designkit/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/yourusername/game.designkit/releases/tag/v0.1.0
