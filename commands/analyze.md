---
description: 在生成任务清单后，对 `spec.md`、`plan.md`、`tasks.md` 执行无破坏性的跨工件一致性与质量分析。
---

向你提供的用户输入可以由智能体直接传入，或作为命令参数提供——如果不为空，你在继续执行提示前**必须**予以考虑。

User input:

$ARGUMENTS

目标：在实施前，识别三大核心工件（`spec.md`、`plan.md`、`tasks.md`）中的不一致、重复、歧义以及欠规范项。此命令仅能在 `/tasks` 成功生成完整的 `tasks.md` 之后运行。

严格只读：**不得**修改任何文件。输出结构化的分析报告。可以附带可选的修复建议方案（需要用户明确批准，后续任何编辑命令都需手动调用）。

宪章权威：项目宪章（`.specify/memory/constitution.md`）在本分析范围内**不可协商**。与宪章冲突的内容自动定级为 CRITICAL，必须通过调整规范、计划或任务来解决——而不是稀释、曲解或默默忽略该原则。如果确需修改原则本身，必须在 `/analyze` 之外通过单独且明确的宪章更新流程进行。

执行步骤：

1. 从仓库根目录运行一次 `.specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks`，解析 JSON 获取 FEATURE_DIR 与 AVAILABLE_DOCS，并推导绝对路径：
   - SPEC = FEATURE_DIR/spec.md
   - PLAN = FEATURE_DIR/plan.md
   - TASKS = FEATURE_DIR/tasks.md
   若任一必需文件缺失则中止，并提示用户先运行缺失的前置命令。

2. 加载工件：
   - 解析 spec.md：Overview/Context、Functional Requirements、Non-Functional Requirements、User Stories、Edge Cases（如存在）。
   - 解析 plan.md：架构/技术栈选择、数据模型引用、阶段划分、技术约束。
   - 解析 tasks.md：任务 ID、描述、阶段分组、并行标记 [P]、引用的文件路径。
   - 加载宪章 `.specify/memory/constitution.md` 以进行原则校验。

3. 构建内部语义模型：
   - 需求清单：为每条功能/非功能需求生成稳定键（基于祈使句生成 slug，如 “User can upload file” → `user-can-upload-file`）。
   - 用户故事/动作清单。
   - 任务覆盖映射：将每个任务映射到一个或多个需求或故事（基于关键词/显式引用模式如 ID 或关键短语推断）。
   - 宪章规则集：提取原则名称及任何 MUST/SHOULD 的规范性陈述。

4. 检测流程：
   A. 重复检测：
      - 识别近似重复的需求，并标记质量较差的表述以便合并。
   B. 歧义检测：
      - 标记缺乏可度量标准的模糊形容词（如 fast、scalable、secure、intuitive、robust）。
      - 标记未解决的占位符（TODO、TKTK、???、<placeholder> 等）。
   C. 欠规范：
      - 仅含动词但缺少客体或可度量结果的需求。
      - 未与验收标准对齐的用户故事。
      - 引用规范/计划中未定义的文件或组件的任务。
   D. 宪章一致性：
      - 任一与 MUST 原则冲突的需求或计划元素。
      - 缺失宪章所要求的章节或质量门禁。
   E. 覆盖缺口：
      - 无任何关联任务的需求。
      - 无映射需求/故事的任务。
      - 未在任务中体现的非功能需求（如性能、安全）。
   F. 不一致：
      - 术语漂移（同一概念跨文件使用不同名称）。
      - 计划中引用而规范中缺失的数据实体（或反之）。
      - 任务排序矛盾（如在未注明依赖的情况下，将集成任务置于基础性设置任务之前）。
      - 相互冲突的要求（如一处要求使用 Next.js，另一处要求使用 Vue 作为框架）。

5. 严重性划分启发式：
   - CRITICAL：违反宪章 MUST、缺失核心规范工件、或阻断基础功能的零覆盖需求。
   - HIGH：重复或冲突的需求、模糊的安全/性能属性、不可测试的验收标准。
   - MEDIUM：术语漂移、非功能任务覆盖缺失、欠规范边界情形。
   - LOW：措辞/文风改进，不影响执行顺序的轻微冗余。

6. 生成 Markdown 报告（不写入文件），包含以下部分：

   ### 规范分析报告（Specification Analysis Report）
   | ID | Category | Severity | Location(s) | Summary | Recommendation |
   |----|----------|----------|-------------|---------|----------------|
   | A1 | Duplication | HIGH | spec.md:L120-134 | 两条相似需求… | 合并表述；保留更清晰版本 |
   （每条发现增加一行；ID 稳定且以类别首字母为前缀。）

   附加小节：
   - 覆盖情况汇总表：
     | Requirement Key | Has Task? | Task IDs | Notes |
   - 宪章一致性问题（如有）
   - 未映射任务（如有）
   - 指标：
     * Total Requirements
     * Total Tasks
     * Coverage %（有≥1个任务的需求占比）
     * Ambiguity Count
     * Duplication Count
     * Critical Issues Count

7. 报告末尾输出精炼的“下一步行动”（Next Actions）：
   - 若存在 CRITICAL：建议在执行 `/implement` 之前优先解决。
   - 若仅有 LOW/MEDIUM：可以继续推进，但提供改进建议。
   - 提供明确的命令建议：如 “运行 /specify 进行细化”、“运行 /plan 调整架构”、“手动编辑 tasks.md 为 ‘performance-metrics’ 增加覆盖”。

8. 向用户询问：“需要我为排名前 N 的问题给出具体修复编辑建议吗？”（请勿自动应用）。

行为规则：
- 切勿修改文件。
- 切勿臆测缺失章节——如缺失，原样报告。
- 保持结果确定性：不变的输入应产出一致的 ID 与计数。
- 主表最多列出 50 条发现；其余汇总于简要溢出说明。
- 若未发现问题，输出成功报告与覆盖统计，并给出继续建议。

Context: $ARGUMENTS
