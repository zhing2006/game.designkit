---
description: 从自然语言描述创建或更新游戏规范文档（游戏愿景或功能系统）
---

## 用户输入

```text
$ARGUMENTS
```

必须考虑用户输入（如果非空）。

## 执行流程

用户在 `/game.designkit.specify` 后输入的文本是游戏或功能描述。假设它始终可用，即使下面出现 `$ARGUMENTS` 字面值。除非用户提供空命令，否则不要要求用户重复。

执行以下步骤：

1. **生成简洁的短名称**（2-4个单词）：
   - 分析功能描述，提取最有意义的关键词
   - 创建 2-4 个单词的短名称，捕捉功能本质
   - 游戏策划常用格式：
     - 全局型："game-vision", "world-setting"
     - 功能型："combat-system", "equipment-upgrade", "dialogue-tree", "talent-system"
   - 示例：
     - "创建游戏愿景：一个生存建造游戏" → "game-vision"
     - "设计装备强化系统：玩家通过材料提升装备" → "equipment-upgrade"
     - "战斗系统设计，包含连招和技能释放" → "combat-system"

1.5. **判断 Spec 类型**（全局型 vs 功能型）：

分析用户描述中的关键词，判断 spec 类型：

**判断规则**（按优先级）：

a) 关键词匹配（最高优先级）：
   - 描述包含以下关键词之一 → **全局型（000）**：
     - "游戏愿景"、"整个游戏"、"世界观"、"全局设定"
     - "游戏类型"、"核心玩法"、"情感曲线"
     - "整体"、"全局"、"游戏整体"

   - 描述包含具体系统/功能名称 → **功能型（001+）**：
     - "战斗系统"、"装备强化"、"对话系统"、"建造系统"
     - "天赋树"、"技能系统"、"经济系统"、"任务系统"
     - "XX系统"、"XX功能"、"XX模块"

b) 默认行为：
   - 如果无法明确判断 → 默认为**功能型**
   - 生成规范后在 Dependencies 章节标记：
     [NEEDS CLARIFICATION: 该功能是否为全局型规范？如果是，应该使用 000 编号]

输出变量：
- spec_type = "global" | "feature"

2. **检查现有分支并确定编号**（仅功能型 spec）：

   如果 spec_type == "feature"：

   a. 首先，获取所有远程分支以确保拥有最新信息：
      ```bash
      git fetch --all --prune
      ```

   b. 查找该 short-name 的最高功能编号，检查所有三个源：
      - 远程分支：`git ls-remote --heads origin | grep -E 'refs/heads/[0-9]+-<short-name>$'`
      - 本地分支：`git branch | grep -E '^[* ]*[0-9]+-<short-name>$'`
      - gamedesigns 目录：检查匹配 `gamedesigns/[0-9]+-<short-name>` 的目录

   c. 确定下一个可用编号：
      - 从所有三个源提取所有编号
      - 找到最高编号 N
      - 使用 N+1 作为新分支编号

   d. 将计算好的编号保存到变量 BRANCH_NUMBER

   **编号检查规则**：
   - 检查所有三个源（远程分支、本地分支、gamedesigns 目录）以找到最高编号
   - 只匹配精确 short-name 模式的分支/目录
   - 如果未找到该 short-name 的现有分支/目录，从编号 1 开始
   - 跳过编号 000（保留给全局 spec）

   如果 spec_type == "global"：
   - 跳过此步骤（全局 spec 始终使用 000）

2.5. **前置文档读取** （仅功能型 spec）

   如果 spec_type == "feature"：

   a. 读取 `.game.design/memory/pillars.md` 文件：
      - 如果文件不存在：
        ERROR "未找到 pillars.md，建议先运行 /game.designkit.pillars 创建设计支柱"
        结束，不再执行后续步骤
      - 如果文件存在：
        读取并理解其内容

   b. 查看 000 global 型规范 是否已经存在
      - 如果存在：则读取其中的`spec.md`, `plan.md`, `tasks.md`文件内容，确认 000 global 型规范的产出文件有哪些
         - 如果产出文件存在：读取并理解其内容
         - 如果产出文件不存再：通过 000 global 型规范内容推断世界观和设计愿景
      - 如果不存在：
        ERROR "未找到 global 型规范，建议先运行 /game.designkit.specify 创建世界观和设计愿景规范"
        结束，不再执行后续步骤

3. 运行脚本创建 spec 目录和文件：

   **功能型 spec**：
   Windows:
   ```powershell
   .game.design/scripts/powershell/create-new-feature.ps1 -Json -Type feature -Number <BRANCH_NUMBER> -ShortName "<生成的短名称>" "$ARGUMENTS"
   ```
   Linux or MacOS:
   ```bash
   .game.design/scripts/bash/create-new-feature.sh --json --type feature --number <BRANCH_NUMBER> --short-name "<生成的短名称>" "$ARGUMENTS"
   ```

   **全局型 spec**：
   Windows:
   ```powershell
   .game.design/scripts/powershell/create-new-feature.ps1 -Json -Type global -ShortName "<生成的短名称>" "$ARGUMENTS"
   ```
   Linux or MacOS:
   ```bash
   .game.design/scripts/bash/create-new-feature.sh --json --type global --short-name "<生成的短名称>" "$ARGUMENTS"
   ```

   解析 JSON 输出获取 BRANCH_NAME、SPEC_FILE、FEATURE_NUM、SPEC_TYPE。所有文件路径必须为绝对路径。

   **重要**：
   - 功能型必须传递 `--number` 参数（使用步骤 2 计算的 BRANCH_NUMBER）
   - 全局型不需要 `--number`（脚本强制使用 000）
   - 传递 `--type` 参数和 `--short-name` 参数
   - 脚本在 `gamedesigns/` 目录下创建 spec 目录
   - 只能运行此脚本一次
   - JSON 输出示例：
     - 全局型：`{"BRANCH_NAME":"000-game-vision","SPEC_FILE":"/path/to/gamedesigns/000-game-vision/spec.md","FEATURE_NUM":"000","SPEC_TYPE":"global"}`
     - 功能型：`{"BRANCH_NAME":"003-combat-system","SPEC_FILE":"/path/to/gamedesigns/003-combat-system/spec.md","FEATURE_NUM":"003","SPEC_TYPE":"feature"}`

4. 根据 SPEC_TYPE 加载对应的规范模板：
   - 如果 SPEC_TYPE == "global" → 加载 `.game.design/templates/spec-template-global.md`
   - 如果 SPEC_TYPE == "feature" → 加载 `.game.design/templates/spec-template-feature.md`

5. 执行以下 NLP 提取流程：

   a. 解析用户描述
      如果为空：ERROR "未提供游戏或功能描述"

   b. 根据 SPEC_TYPE 提取关键概念：

      **全局型（game-vision）提取内容**：
      - 游戏类型（ARPG、Roguelike、建造、策略、射击、RPG、模拟等）
      - 核心玩法循环（"A → B → C" 模式的动作链）
      - 情感曲线（玩家情感变化）
      - 世界设定（时间、地点、科技水平）
      - 核心冲突（主要矛盾）
      - 美术风格（视觉风格、参考作品）
      - 参考游戏（借鉴和避免）
      - 体验目标

      **功能型（feature-spec）提取内容**：
      - 功能类型（战斗、装备、对话、建造、天赋、经济等系统）
      - 功能目标（体验目标）
      - 依赖关系（依赖哪些其他 spec，如 000-game-vision）
      - 玩家操作（操作流程）
      - 系统接口（与其他系统的对接点）

   c. 对于不明确的方面：
      - 基于上下文和游戏行业标准做出合理推测
      - 仅当满足以下条件时标记 [NEEDS CLARIFICATION: 具体问题]：
        - 该选择显著影响功能范围或玩家体验
        - 存在多种合理解释且含义不同
        - 没有合理的默认值
      - **限制：最多 3 个 [NEEDS CLARIFICATION] 标记**
      - 优先级：范围 > 体验目标 > 系统设计 > 细节

   d. 填充用户故事章节：
      - 全局型：填充"高层用户故事"（玩家的整体游戏体验）
      - 功能型：填充"用户故事"（玩家如何使用该功能）
      如果无法确定用户流程：ERROR "无法确定玩家体验场景"

   e. 生成功能需求（仅功能型 spec）：
      每个需求必须可测试
      对未指定的细节使用合理默认值（在"依赖与假设"章节记录假设，如果模板包含此章节）

   f. 定义体验目标（全局型 spec）：
      创建可验证的、技术无关的体验结果

   g. 识别关键实体（如果涉及游戏实体）：
      如角色、道具、系统等

   h. 返回：SUCCESS（规范准备好进入 planning）

6. 将规范写入 SPEC_FILE，使用模板结构，用从功能描述提取的具体细节替换占位符，同时保持章节顺序和标题。

7. **规范质量验证**：写入初始规范后，根据质量标准进行验证：

   a. **创建规范质量检查清单**：在 `FEATURE_DIR/checklists/requirements.md` 生成检查清单文件，包含以下验证项：

      ```markdown
      # 规范质量检查清单：[功能名称]

      **目的**：在进入规划阶段前验证规范的完整性和质量
      **创建时间**：[日期]
      **功能**：[../spec.md](../spec.md)
      **Spec 类型**：全局型 / 功能型

      ## 内容质量

      - [ ] 无实现细节（游戏引擎、编程语言、数据库等）
      - [ ] 聚焦于玩家体验和游戏设计目标
      - [ ] 面向游戏设计师和非技术干系人编写
      - [ ] 所有必填章节已完成

      ## 需求完整性

      - [ ] 无 [NEEDS CLARIFICATION] 标记残留
      - [ ] 需求可测试且明确（功能型）或体验目标明确（全局型）
      - [ ] 所有验收场景已定义
      - [ ] 边缘情况已识别
      - [ ] 范围清晰界定
      - [ ] 依赖和假设已识别（如适用）

      ## 游戏规范特有检查

      ### 全局型规范 *(仅当类型为全局型时检查)*

      - [ ] Vision 章节包含核心玩法循环和情感曲线
      - [ ] World 章节世界设定内部一致，无逻辑矛盾
      - [ ] 参考游戏分析深入（不只是"类似XX游戏"）
      - [ ] 体验目标明确且可验证

      ### 功能型规范 *(仅当类型为功能型时检查)*

      - [ ] Dependencies 章节明确列出依赖的 spec（至少依赖 000-game-vision）
      - [ ] Feature Vision 描述了功能在核心循环中的位置
      - [ ] Feature World 与全局 World 一致
      - [ ] 功能需求不包含实现细节

      ## 功能就绪性

      - [ ] 用户故事覆盖主要玩家体验流程
      - [ ] 无实现细节泄露到规范中

      ## 备注

      - 未通过的检查项需要在 `/game.designkit.clarify` 或 `/game.designkit.plan` 前更新规范
      ```

   b. **运行验证检查**：对照每个检查项审查规范

   c. **处理验证结果**：
      - 如果全部通过：标记 checklist 完成，继续步骤 6.5
      - 如果有失败项（不包括 [NEEDS CLARIFICATION]）：
        1. 列出失败项和具体问题
        2. 更新规范解决每个问题
        3. 重新验证，最多 3 次迭代
        4. 如果 3 次后仍有失败，在 checklist 备注中记录问题并警告用户
      - 如果有 [NEEDS CLARIFICATION] 标记：
        1. 提取所有 [NEEDS CLARIFICATION: ...] 标记
        2. 限制检查：如果超过 3 个，只保留最关键的 3 个
        3. 对每个需要澄清的点（最多 3 个），向用户提供选项
        4. 等待用户回答
        5. 用用户的答案替换 [NEEDS CLARIFICATION] 标记
        6. 重新验证

   d. **更新 Checklist**：每次验证迭代后更新检查清单文件的通过/失败状态

7.5. **与设计支柱一致性检查**：

a) 读取 `.game.design/memory/pillars.md` 文件：
   - 如果文件不存在：
     WARNING "未找到 pillars.md，建议先运行 /game.designkit.pillars 创建设计支柱"
     跳过一致性检查，继续步骤 8
   - 如果文件存在：继续检查

b) 提取设计支柱列表：
   - 从 pillars.md 中提取所有设计支柱的名称和描述
   - 提取核心体验目标
   - 提取独特卖点（USP）

c) 检测冲突：
   - 对比生成的规范内容与设计支柱
   - 检测明显的语义冲突，使用关键词对立模式：
     - "硬核" ⟷ "休闲"、"轻度"、"简单"
     - "策略" ⟷ "快节奏"、"反应速度"、"动作"
     - "慢节奏" ⟷ "快节奏"、"高速"、"爽快"
     - "复杂" ⟷ "简单"、"易上手"
     - "写实" ⟷ "卡通"、"抽象"、"像素"
   - 检测体验目标不一致
   - 检测约束条件冲突

d) 记录冲突：
   - 如果发现冲突：记录到内存中的冲突列表（在步骤 8 输出）
   - 如果无冲突：记录"✅ 与设计支柱一致"

**注意**：只检测明显冲突，避免误报。不确定时不标记冲突。

8. 报告完成情况，包含：分支名称、spec 类型、feature number（包括如何计算的）、规范文件路径、checklist 结果、一致性检查结果、需要澄清的字段列表（如果有）、下一阶段建议（`/game.designkit.clarify` 或 `/game.designkit.plan`）。

**注意**：脚本会创建并切换到新分支，初始化规范文件。

## 通用指南

- 聚焦于**做什么**和**为什么**
- 避免**如何实现**（无游戏引擎、编程语言、数据库设计）
- 面向游戏设计师编写，非程序员
- 不要在规范中嵌入 checklist（由独立文件生成）

## AI 生成指南

创建规范时：

1. **做出合理推测**：基于上下文和游戏行业标准填补空白
2. **记录假设**：在"依赖与假设"章节记录合理默认值（如果模板包含此章节）
3. **限制澄清标记**：最多 3 个 [NEEDS CLARIFICATION] - 仅用于关键决策：
   - 显著影响功能范围或玩家体验
   - 存在多种合理解释且含义不同
   - 没有合理的默认值
4. **优先级排序**：范围 > 体验目标 > 系统设计 > 细节

**合理默认值示例**（不需要为这些提问）：

- 多人模式：如果没提及，默认单人游戏
- 目标受众：如果描述了游戏类型，默认该类型的典型玩家群体

## 章节要求

- **必填章节**：每个规范都必须完成
- **可选章节**：仅在相关时包含
- 当章节不适用时，完全删除它（不要留作"N/A"）
