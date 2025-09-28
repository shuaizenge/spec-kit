---
description: 从自然语言需求出发，依次完成需求澄清、文档生成与修订确认的工作流。
---

向你提供的用户输入可以由智能体直接传入，或作为命令参数提供——如果不为空，你在继续执行提示前**必须**予以考虑。

User input:

$ARGUMENTS

总体目标：以最小返工的方式，将用户的自然语言需求转化为可对齐、可实施、可验收的文档工件，并严格分三阶段执行：
（A）需求反问与确认 → （B）文档生成 → （C）文档修订与确认。

1. 环境与路径解析（必须）
   - 在仓库根目录运行一次 `.specify/scripts/powershell/check-prerequisites.ps1 -Json`，解析 JSON 获取仓库根路径（REPO_ROOT）。后续涉及的文件路径必须为绝对路径。
   - 设定文档输出目录：`DOC_DIR = REPO_ROOT/doc`。若目录不存在则创建。
   - 若检测到项目宪章 `.specify/memory/constitution.md`，在后续步骤中用于一致性校验。

2. 阶段 A：需求反问与确认（交互门禁）
   目标：以 AI 的方式复述并结构化用户的自然语言需求，暴露不确定点并获得明确确认。
   - A1 结构化理解草案（只输出，不写文件）：
     - 背景与动机（为什么要做）
     - 目标与成功标准（包含可度量指标）
     - 范围与非目标（Out of Scope）
     - 关键用户与使用场景（User Personas & Scenarios）
     - 功能需求清单（按动词祈使句列出）
     - 非功能需求清单（性能/安全/可靠性/可维护性等，尽量量化）
     - 外部依赖与约束（技术/合规/时间/预算）
     - 风险与假设（Assumptions & Risks）
     - 未决问题（Open Questions）
   - A2 反问清单：将不明确或存在分歧的点汇总为问题清单，并分类：
     - 二分/选择题（例如：是否需要离线支持？A/B）
     - 需补充细节（例如：峰值 QPS、数据保留天数）
     - 需决策的技术选项（例如：数据库/消息队列/前端框架）
   - A3 确认闸门：
     - 只有在用户明确回复“确认/继续/同意上述理解”后，才可进入阶段 B。
     - 若用户指出理解偏差或提供新信息，更新 A1/A2 并再次请求确认。
     - 严禁在存在关键未决项（影响目标、范围或验收标准）且未获允许时跳过澄清直接生成文档。

3. 阶段 B：文档生成（落盘）
   在获得用户确认后，基于阶段 A 的最终理解，生成两份文档至 `DOC_DIR`：
   - B1 PRD（需求文档）：`$DOC_DIR/prd.md`
     必含章节（按序）：
     - Overview（背景/目标/范围与非目标）
     - Stakeholders & Personas（干系人与用户画像）
     - User Stories（以“As a … I want … so that …” 结构）
     - Functional Requirements（编号列表，祈使句，可测试）
     - Non-Functional Requirements（性能/安全/可靠性等，含阈值/指标）
     - Acceptance Criteria（验收标准，映射到 FR/NFR）
     - Milestones & Timeline（关键里程碑与时间预期）
     - Risks & Assumptions（风险应对与前提假设）
     - Open Questions（未决问题清单，使用 `TODO(<KEY>): ...` 标识）
     - Appendix（术语/参考）
   - B2 技术实施文档（技术方案/实施计划）：`$DOC_DIR/tech-implementation.md`
     必含章节（按序）：
     - Architecture Overview（系统边界与上下游、核心组件）
     - Technology Choices & Rationale（技术选型与取舍理由）
     - Data Model & Storage（核心实体、关系、保留策略）
     - Interfaces & Contracts（对外/内部 API 约定、错误码、节流/重试）
     - Non-Functional Design（性能/SLO/容量估算/安全/合规/可观测性）
     - Failure Modes & Resilience（降级/熔断/重试/幂等/灾备）
     - Test Strategy（单测/合约测/集成/回归，覆盖与门禁）
     - Deployment & Rollout（环境、配置、灰度、迁移与回滚）
     - Work Breakdown（高层任务分解与里程碑，对应 PRD 的需求）
     - Dependencies & Risks（外部依赖、前置条件、技术债管理）
     - Open Decisions（待决技术选项与评估标准，使用 `TODO(<KEY>): ...`）
   - B3 可追溯性与一致性：
     - 在技术实施文档中包含一张“需求→设计/任务”的映射表（Traceability），确保每条 PRD 需求至少映射到一项设计/任务。
     - 若加载到 `.specify/memory/constitution.md`，需检查与 MUST/SHOULD 原则一致；若存在冲突，将冲突列入文档的 Risks 或 Open Decisions，并在阶段 C 主动提示用户处理优先级。
   - B4 写入规则：
     - 使用绝对路径写入，若文件已存在则覆盖（保留同名备份策略由上层执行环境决定）。
     - 中文为主，英文术语在首次出现处给出英文（括号）或术语表。
     - 未获确认的信息一律以 `TODO(<KEY>): explanation` 形式保留，不得擅自臆测具体数值或约束。

4. 阶段 C：文档修订与确认（交互门禁）
   - C1 输出变更与风险摘要：
     - 概述关键目标、范围、里程碑与主要技术决策。
     - 列出 Open Questions 与 Open Decisions，并给出建议的收敛选项与取舍依据。
   - C2 可行性确认：
     - 向用户询问“按该技术方案推进是否可行？”
     - 若不可行或需调整：根据用户反馈修订 PRD 与技术实施文档，并清晰标注修订点（章节/小节级别）。
     - 若可行：宣布本次 `/requirement_describe` 完成。

5. 质量与行为规范
   - 严格门禁：未通过阶段 A 或 C 的明确确认，不得进入下一阶段或宣告完成。
   - 一致性：与项目宪章（若存在）不一致的要求必须被显式标记并建议优先处理；不得绕过宪章降低标准。
   - 可测试性：PRD 的功能需求与验收标准必须可测试，避免“快/稳/易用”等模糊描述，需给出量化或可操作判断标准。
   - 可追溯性：技术实施文档需能映射回 PRD 的需求，避免“无对应需求的设计”或“无设计支撑的需求”。
   - 决定性：相同输入应产生稳定的章节结构与编号；开放项的 `TODO` 键保持稳定命名。

6. 产出与后续建议（只输出，不写文件）
   - 输出：
     - 文档生成的绝对路径：`$DOC_DIR/prd.md`、`$DOC_DIR/tech-implementation.md`
     - 关键未决项与所需补充信息清单
   - 下一步建议：
     - 若方案已确认：可继续 `version_task.md`（生成任务清单），以便进入实施阶段。
     - 若存在 CRITICAL 未决项：建议优先完成澄清后再进入实施。

执行上下文：$ARGUMENTS


