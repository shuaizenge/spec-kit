
# 实施计划： [FEATURE]

**分支**: `[###-feature-name]` | **日期**: [DATE] | **规范**: [link]
**输入**: 来自 `/specs/[###-feature-name]/spec.md` 的功能规范

## 执行流（/plan 命令范围）
```
1. 从输入路径加载功能规范
   → 若未找到：ERROR "No feature spec at {path}"
2. 填写技术上下文（扫描 NEEDS CLARIFICATION）
   → 从文件结构或上下文检测项目类型（web=frontend+backend，mobile=app+api）
   → 根据项目类型确定结构决策
3. 基于宪章文档填写下方的宪章检查（Constitution Check）
4. 评估宪章检查
   → 若存在违规：在复杂度跟踪中记录
   → 若无法给出正当理由：ERROR "Simplify approach first"
   → 更新进度追踪：初始宪章检查
5. 执行 Phase 0 → 生成 research.md
   → 若仍存在 NEEDS CLARIFICATION：ERROR "Resolve unknowns"
6. 执行 Phase 1 → 生成 contracts、data-model.md、quickstart.md，以及代理特定模板文件（例如 `CLAUDE.md`、`.github/copilot-instructions.md`、`GEMINI.md`、`QWEN.md` 或 `AGENTS.md`）
7. 重新评估宪章检查
   → 若出现新的违规：重构设计并回到 Phase 1
   → 更新进度追踪：设计后宪章检查
8. 规划 Phase 2 → 描述任务生成方法（不要创建 tasks.md）
9. 停止 - 准备执行 /tasks 命令
```

**重要**：/plan 命令在第 7 步停止。第 2-4 阶段由其他命令执行：
- Phase 2：/tasks 命令创建 tasks.md
- Phase 3-4：实施执行（手动或通过工具）

## 摘要
[从功能规范中提取：主要需求 + 研究中的技术方案]

## 技术上下文
**语言/版本**: [如 Python 3.11、Swift 5.9、Rust 1.75 或 NEEDS CLARIFICATION]  
**核心依赖**: [如 FastAPI、UIKit、LLVM 或 NEEDS CLARIFICATION]  
**存储**: [如适用：PostgreSQL、CoreData、文件 或 N/A]  
**测试**: [如 pytest、XCTest、cargo test 或 NEEDS CLARIFICATION]  
**目标平台**: [如 Linux 服务器、iOS 15+、WASM 或 NEEDS CLARIFICATION]
**项目类型**: [single/web/mobile - 决定源码结构]  
**性能目标**: [领域相关，如 1000 req/s、10k lines/sec、60 fps 或 NEEDS CLARIFICATION]  
**约束**: [领域相关，如 p95 <200ms、内存 <100MB、离线可用 或 NEEDS CLARIFICATION]  
**规模/范围**: [领域相关，如 1 万用户、100 万行代码、50 个界面 或 NEEDS CLARIFICATION]

## 宪章检查（Constitution Check）
*闸门：Phase 0 研究前必须通过；Phase 1 设计后需再次检查。*

[根据宪章文件确定的闸门]

## 项目结构

### 文档（此功能）
```
specs/[###-feature]/
├── plan.md              # 本文件（/plan 输出）
├── research.md          # Phase 0 输出（/plan）
├── data-model.md        # Phase 1 输出（/plan）
├── quickstart.md        # Phase 1 输出（/plan）
├── contracts/           # Phase 1 输出（/plan）
└── tasks.md             # Phase 2 输出（/tasks 生成，非 /plan）
```

### 源码（仓库根）
<!--
  需要操作：以具体结构替换以下占位树；
  删除未用选项，并用真实路径展开所选结构（如 apps/admin、packages/something）。
  最终交付的计划不得保留 “Option/选项” 标签。
-->
```
# [未用请删除] 选项 1：单一工程（默认）
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# [未用请删除] 选项 2：Web 应用（检测到 "frontend" + "backend" 时）
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# [未用请删除] 选项 3：移动端 + API（检测到 "iOS/Android" 时）
api/
└── [同上 backend]

ios/ 或 android/
└── [平台特定结构：功能模块、UI 流程、平台测试]
```

**结构决策**： [记录所选结构并引用上面捕获的实际目录]

## Phase 0：纲要与研究
1. **从上方技术上下文中抽取未知项**：
   - 每个 NEEDS CLARIFICATION → 研究任务
   - 每个依赖 → 最佳实践任务
   - 每个集成点 → 模式任务

2. **生成并派发研究代理**：
```
For each unknown in Technical Context:
  Task: "Research {unknown} for {feature context}"
For each technology choice:
  Task: "Find best practices for {tech} in {domain}"
```

3. **在 `research.md` 汇总结论**：
   - 决策： [选择了什么]
   - 理由： [为何选择]
   - 备选： [评估过什么]

**输出**：research.md，所有 NEEDS CLARIFICATION 已解决

## Phase 1：设计与合约
*前置：research.md 完成*

1. **从功能规范中抽取实体** → `data-model.md`：
   - 实体名、字段、关系
   - 来自需求的校验规则
   - 如适用的状态变迁

2. **从功能需求生成 API 合约**：
   - 每个用户动作 → 一个端点
   - 使用标准 REST/GraphQL 模式
   - 将 OpenAPI/GraphQL Schema 输出到 `/contracts/`

3. **从合约生成合约测试**：
   - 每个端点一个测试文件
   - 断言请求/响应模式
   - 测试应先失败（尚未实现）

4. **从用户故事抽取测试场景**：
   - 每个故事 → 一个集成测试场景
   - Quickstart 测试 = 故事校验步骤

5. **增量更新代理文件**（O(1) 操作）：
   - 运行 `.specify/scripts/powershell/update-agent-context.ps1 -AgentType auggie`
     **重要**：严格按上面命令执行，不增不减参数。
   - 若文件已存在：仅添加当前计划中新出现的技术
   - 保留标记之间的手动补充
   - 更新最近变更（保留最近 3 条）
   - 控制在 150 行以内以节省 Token
   - 输出到仓库根目录

**输出**：data-model.md、/contracts/*、失败中的测试、quickstart.md、代理特定文件

## Phase 2：任务规划方法
*本节描述 /tasks 命令将做什么——/plan 中不要执行*

**任务生成策略**：
- 以 `.specify/templates/tasks-template.md` 为基底
- 基于 Phase 1 的设计文档（contracts、data model、quickstart）生成任务
- 每个合约 → 合约测试任务 [P]
- 每个实体 → 模型创建任务 [P]
- 每个用户故事 → 集成测试任务
- 为使测试通过而编写的实现任务

**排序策略**：
- TDD 顺序：测试在前、实现随后 
- 依赖顺序：模型 → 服务 → UI
- 独立文件的任务标记 [P] 以并行执行

**预计输出**：tasks.md 中 25–30 个编号且有序的任务

**重要**：该阶段由 /tasks 执行，非 /plan

## Phase 3+：后续实施
*超出 /plan 命令范围*

**Phase 3**：任务执行（/tasks 生成 tasks.md）  
**Phase 4**：实施（遵循宪章原则执行 tasks.md）  
**Phase 5**：验证（运行测试、执行 quickstart.md、性能验证）

## 复杂度跟踪
*仅在宪章检查存在必须论证的违规时填写*

| 违规 | 需要原因 | 更简单替代被否原因 |
|------|----------|--------------------|
| [例：第 4 个工程] | [当前需要] | [为何 3 个工程不足] |
| [例：仓储模式] | [具体问题] | [为何直接 DB 访问不足] |

## 进度追踪
*在执行过程中更新此清单*

**阶段状态**：
- [ ] Phase 0：研究完成（/plan）
- [ ] Phase 1：设计完成（/plan）
- [ ] Phase 2：任务规划完成（/plan，仅描述方法）
- [ ] Phase 3：任务已生成（/tasks）
- [ ] Phase 4：实施完成
- [ ] Phase 5：验证通过

**闸门状态**：
- [ ] 初始宪章检查：PASS
- [ ] 设计后宪章检查：PASS
- [ ] 所有 NEEDS CLARIFICATION 已解决
- [ ] 复杂度偏离已记录

---
*基于宪章 v2.1.1 - 参见 `/memory/constitution.md`*
