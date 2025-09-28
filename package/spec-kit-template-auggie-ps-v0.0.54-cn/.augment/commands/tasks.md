---
description: 基于已有设计工件为该功能生成可执行、按依赖排序的 tasks.md。
---

向你提供的用户输入可以由智能体直接传入，或作为命令参数提供——如果不为空，你在继续执行提示前**必须**予以考虑。

User input:

$ARGUMENTS

1. 从仓库根目录运行 `.specify/scripts/powershell/check-prerequisites.ps1 -Json` 并解析 FEATURE_DIR 与 AVAILABLE_DOCS 列表。所有路径必须为绝对路径。
2. 加载并分析可用的设计文档：
   - 始终读取 plan.md 了解技术栈与依赖库
   - 如存在：读取 data-model.md 获取实体
   - 如存在：读取 contracts/ 获取 API 端点
   - 如存在：读取 research.md 获取技术决策
   - 如存在：读取 quickstart.md 获取测试场景

   说明：并非所有项目都具备全部文档。例如：
   - CLI 工具可能没有 contracts/
   - 简单类库可能不需要 data-model.md
   - 基于现有文档生成任务

3. 按模板生成任务：
   - 以 `.specify/templates/tasks-template.md` 为基础
   - 用实际任务替换示例任务，依据：
     * **Setup 任务**：项目初始化、依赖、lint
     * **测试任务 [P]**：每个合约一个、每个集成场景一个
     * **核心任务**：每个实体、服务、CLI 命令、端点一个
     * **集成任务**：数据库连接、中间件、日志
     * **打磨任务 [P]**：单测、性能、文档

4. 任务生成规则：
   - 每个合约文件 → 一个合约测试任务，标记 [P]
   - data-model 中每个实体 → 一个模型创建任务，标记 [P]
   - 每个端点 → 一个实现任务（若共享文件则不可并行）
   - 每个用户故事 → 一个集成测试任务，标记 [P]
   - 不同文件 = 可并行 [P]
   - 同一文件 = 串行（不加 [P]）

5. 按依赖排序任务：
   - 先 Setup
   - 测试先于实现（TDD）
   - 模型先于服务
   - 服务先于端点
   - 核心先于集成
   - 全部先于打磨

6. 包含并行执行示例：
   - 将可并行的 [P] 任务分组
   - 展示可执行的 Task agent 命令

7. 创建 FEATURE_DIR/tasks.md，需包含：
   - 来自实施计划的正确功能名称
   - 编号任务（T001、T002，…）
   - 每个任务明确的文件路径
   - 依赖说明
   - 并行执行指引

Context for task generation: $ARGUMENTS

生成的 tasks.md 应可直接被执行——每个任务必须足够具体，使得 LLM 无需额外上下文即可完成。
