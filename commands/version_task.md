---
description: 基于版本目标生成版本计划，确认后按阶段实施、测试与提交的工作流。
---

向你提供的用户输入可以由智能体直接传入，或作为命令参数提供——如果不为空，你在继续执行提示前**必须**予以考虑。

User input:

$ARGUMENTS

本命令对应用户输入示例：`/version_task 设计0.1版本，完成基本功能`。

总体目标：围绕一个明确的版本号（如 0.1），（P）生成并评审版本计划 →（I）实施并提交 →（T）测试与修复 →（Q）询问是否推进下个版本（如 0.2），以最小返工闭环推进。

1. 环境与路径解析（必须）
   - 在仓库根目录运行 `.specify/scripts/powershell/check-prerequisites.ps1 -Json -PathsOnly`，解析 JSON 获取：`REPO_ROOT`、`FEATURE_DIR`、`TASKS`、`BRANCH` 等。所有路径必须为绝对路径。
   - 设定计划输出目录：`PLAN_DIR = REPO_ROOT/doc/plan`，若不存在则创建。
   - 从 $ARGUMENTS 中解析版本号（接受 `0.1`/`v0.1` 等形式），标准化为：
     - `VERSION = 0.1`（纯数字点分）
     - `VERSION_TAG = ver_0.1`（用于文件名）
     - `VERSION_BRANCH = ver/0.1`（用于 Git 分支）
   - 设定计划文件：`PLAN_FILE = $PLAN_DIR/plan_$VERSION_TAG.md`。

2. 阶段 P：版本计划生成与评审（落盘 + 交互门禁）
   - P1 生成版本计划至 `$PLAN_FILE`，建议包含以下章节（按序）：
     - Version Overview（版本号/日期/动机/目标摘要/不在范围）
     - Deliverables（可交付项清单）
     - Milestones & Timeline（里程碑与时间预期）
     - Work Breakdown（工作分解：模块/文件/任务，若存在 `tasks.md` 则建立映射）
     - Risks & Mitigations（主要风险与应对）
     - Test Strategy（单测/合约/集成/回归，门禁）
     - Acceptance & Exit Criteria（验收与退出标准）
     - Rollback Strategy（失败回滚方案）
     - Dependencies（外部依赖与前置条件）
     - Open Questions（未决问题，使用 `TODO(<KEY>): ...`）
   - P2 输出计划摘要，询问用户“该计划是否可行/是否需要修改？”
   - P3 确认闸门：仅在用户确认后进入实施阶段；若提出修改，修订 `$PLAN_FILE` 并再次确认。

3. 阶段 I：实施与提交（按计划执行）
   - I1 Git 分支：
     - 若不存在 `VERSION_BRANCH`：`git checkout -b ver/0.1`
     - 若已存在：`git checkout ver/0.1` 并同步更新（按需 `git pull`）。
   - I2 任务执行准备：
     - 若检测到 `tasks.md`：
       - 依据版本计划的 Work Breakdown 构建当次执行子集（映射到具体任务 ID/文件路径），遵循 `tasks.md` 的依赖与并行 [P] 规则。
       - 建议先执行测试类任务（TDD），再执行实现类任务。
     - 若未检测到 `tasks.md`：
       - 可先运行 `/tasks` 生成任务清单；或直接依据 `$PLAN_FILE` 的 Work Breakdown 执行实现与测试。
   - I3 实施执行：按阶段推进，阶段完成后进行本地提交：
     - `git add -A`
     - `git commit -m "feat(ver $VERSION): <简述本阶段完成内容>"`

4. 阶段 T：测试与缺陷修复（循环至通过）
   - T1 运行测试：若存在自动化测试则执行（单测/合约/集成/回归）；否则进行可行的自我测试。
   - T2 若失败：定位与修复缺陷 → 重复 I3 的提交流程（可使用 `fix(ver $VERSION): <问题简述>` 提交信息）。
   - T3 若条件不允许自测：请用户提供协助测试路径或手工测试输入/期望；修复后提交。

5. 阶段 Q：版本收尾与续迭询问
   - Q1 版本完成检查：对照 `$PLAN_FILE` 的 Acceptance & Exit Criteria 逐条确认达成。
   - Q2 向用户询问：“是否继续推进下一个版本（例如 0.2）？”
     - 若是：将 `VERSION` 递增（或使用用户指定版本），回到 阶段 2（P）并生成对应 `plan_ver_<new>.md`，重复闭环。
     - 若否：输出最终结果并结束。

6. 质量与行为规范
   - 门禁严格：未经 P3 确认不得进入实施；未经 Q1 完成检查不得宣告完成。
   - 一致性：若检测到 `.specify/memory/constitution.md`，计划与实施需与其 MUST/SHOULD 原则一致；冲突项需在计划与风险中显式列出并优先处理。
   - 可追溯：`$PLAN_FILE` 的 Work Breakdown 应能映射到具体任务/文件；若使用 `tasks.md`，需建立任务 ID 映射清单。
   - 决定性：相同输入应产生稳定的章节结构与命名；开放项以稳定键 `TODO(<KEY>)` 标注。

7. 输出与后续建议（只输出，不写文件）
   - 输出：计划文件路径 `$PLAN_FILE`、使用的 Git 分支 `ver/$VERSION`、测试结论摘要、未决事项清单。
   - 建议：
     - 存在 `tasks.md` 时，建议使用 `/implement` 执行当次任务子集。
     - 若测试覆盖不足，建议回到 `/tasks` 增补测试任务，再次执行本命令的阶段 T。

执行上下文：$ARGUMENTS


