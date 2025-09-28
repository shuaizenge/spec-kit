---
description: 执行实施规划工作流，使用计划模板生成设计工件。
---

向你提供的用户输入可以由智能体直接传入，或作为命令参数提供——如果不为空，你在继续执行提示前**必须**予以考虑。

User input:

$ARGUMENTS

基于提供的实施细节，执行以下步骤：

1. 在仓库根目录运行 `.specify/scripts/powershell/setup-plan.ps1 -Json` 并解析 JSON，获取 FEATURE_SPEC、IMPL_PLAN、SPECS_DIR、BRANCH。后续涉及的文件路径必须为绝对路径。
   - 在继续前，检查 FEATURE_SPEC 是否包含 `## Clarifications` 且至少一个 `Session` 子标题。若缺失或仍有明显歧义（模糊形容词、未决关键选择），请暂停并指示先运行 `/clarify` 以降低返工风险。仅在：（a）已存在澄清，或（b）用户明确允许“无需澄清继续”时才继续。切勿自行捏造澄清。
2. 阅读并分析功能规范，理解：
   - 功能需求与用户故事
   - 功能与非功能需求
   - 成功标准与验收标准
   - 提及的任何技术约束或依赖

3. 阅读 `.specify/memory/constitution.md` 以理解宪章要求。

4. 执行实施计划模板：
   - 加载 `.specify/templates/plan-template.md`（已复制到 IMPL_PLAN 路径）
   - 将输入路径设置为 FEATURE_SPEC
   - 运行主执行流步骤 1-9
   - 模板为自包含且可执行
   - 遵循其中的错误处理与闸门检查
   - 让模板在 $SPECS_DIR 引导生成工件：
     * Phase 0 生成 research.md
     * Phase 1 生成 data-model.md、contracts/、quickstart.md
     * Phase 2 生成 tasks.md
   - 将命令参数中的细节并入 Technical Context：$ARGUMENTS
   - 随阶段完成更新进度追踪

5. 核验执行完成：
   - 检查进度追踪显示所有阶段完成
   - 确保所有必需工件已生成
   - 确认执行中无 ERROR 状态

6. 报告结果：包含分支名、文件路径与生成的工件列表。

为避免路径问题，所有文件操作请使用仓库根目录起始的绝对路径。
