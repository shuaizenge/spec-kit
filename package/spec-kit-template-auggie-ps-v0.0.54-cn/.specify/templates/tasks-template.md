# 任务： [FEATURE NAME]

**输入**: 来自 `/specs/[###-feature-name]/` 的设计文档
**前置**: plan.md（必需）、research.md、data-model.md、contracts/

## 执行流（main）
```
1. 从功能目录加载 plan.md
   → 若未找到：ERROR "No implementation plan found"
   → 提取：技术栈、依赖库、结构
2. 加载可选设计文档：
   → data-model.md：提取实体 → 模型任务
   → contracts/：每个文件 → 合约测试任务
   → research.md：提取决策 → Setup 任务
3. 按类别生成任务：
   → Setup：项目初始化、依赖、lint
   → Tests：合约测试、集成测试
   → Core：模型、服务、CLI 命令
   → Integration：数据库、中间件、日志
   → Polish：单测、性能、文档
4. 应用任务规则：
   → 不同文件 = 标记 [P] 可并行
   → 同一文件 = 串行（不加 [P]）
   → 测试先于实现（TDD）
5. 顺序编号（T001、T002…）
6. 生成依赖图
7. 创建并行执行示例
8. 校验任务完整性：
   → 所有合约均有测试？
   → 所有实体均有模型？
   → 所有端点均有实现？
9. 返回：SUCCESS（任务可用于执行）
```

## 格式：`[ID] [P?] Description`
- **[P]**：可并行（不同文件、无依赖）
- 描述中包含精确文件路径

## 路径约定
- **单一工程**: 仓库根的 `src/`、`tests/`
- **Web 应用**: `backend/src/`、`frontend/src/`
- **移动端**: `api/src/`、`ios/src/` 或 `android/src/`
- 下方路径默认按单一工程，需根据 plan.md 实际结构调整

## 第 3.1 阶段：Setup
- [ ] T001 根据实施计划创建项目结构
- [ ] T002 初始化 [language] 项目并添加 [framework] 依赖
- [ ] T003 [P] 配置 lint 与格式化工具

## 第 3.2 阶段：测试优先（TDD）⚠️ 必须先于 3.3 完成
**关键：以下测试必须先编写，且在任何实现前应当失败**
- [ ] T004 [P] 合约测试 POST /api/users → tests/contract/test_users_post.py
- [ ] T005 [P] 合约测试 GET /api/users/{id} → tests/contract/test_users_get.py
- [ ] T006 [P] 集成测试 用户注册 → tests/integration/test_registration.py
- [ ] T007 [P] 集成测试 认证流程 → tests/integration/test_auth.py

## 第 3.3 阶段：核心实现（仅在测试失败后进行）
- [ ] T008 [P] 用户模型 → src/models/user.py
- [ ] T009 [P] UserService CRUD → src/services/user_service.py
- [ ] T010 [P] CLI --create-user → src/cli/user_commands.py
- [ ] T011 实现 POST /api/users 端点
- [ ] T012 实现 GET /api/users/{id} 端点
- [ ] T013 输入校验
- [ ] T014 错误处理与日志

## 第 3.4 阶段：集成
- [ ] T015 将 UserService 接入数据库
- [ ] T016 认证中间件
- [ ] T017 请求/响应日志
- [ ] T018 CORS 与安全响应头

## 第 3.5 阶段：打磨
- [ ] T019 [P] 校验单元测试 → tests/unit/test_validation.py
- [ ] T020 性能测试（<200ms）
- [ ] T021 [P] 更新文档 → docs/api.md
- [ ] T022 移除重复
- [ ] T023 执行 manual-testing.md

## 依赖关系
- 测试（T004-T007）必须先于实现（T008-T014）
- T008 阻塞 T009、T015
- T016 阻塞 T018
- 实现在前，打磨（T019-T023）在后

## 并行示例
```
# 一并启动 T004-T007：
Task: "Contract test POST /api/users in tests/contract/test_users_post.py"
Task: "Contract test GET /api/users/{id} in tests/contract/test_users_get.py"
Task: "Integration test registration in tests/integration/test_registration.py"
Task: "Integration test auth in tests/integration/test_auth.py"
```

## 备注
- [P] 任务 = 不同文件、无依赖
- 必须在实现前验证测试失败
- 每个任务完成后提交一次
- 避免：模糊任务、同文件冲突

## 任务生成规则
*在 main() 执行期间应用*

1. **来源：合约**
   - 每个合约文件 → 合约测试任务 [P]
   - 每个端点 → 实现任务
   
2. **来源：数据模型**
   - 每个实体 → 模型创建任务 [P]
   - 关系 → 服务层任务
   
3. **来源：用户故事**
   - 每个故事 → 集成测试 [P]
   - Quickstart 场景 → 验证任务

4. **排序**
   - Setup → Tests → Models → Services → Endpoints → Polish
   - 依赖会阻止并行

## 校验清单
*在 main() 返回前由程序检查*

- [ ] 所有合约均有对应测试
- [ ] 所有实体均有模型任务
- [ ] 所有测试均先于实现
- [ ] 并行任务真正独立
- [ ] 每个任务指定精确文件路径
- [ ] 不存在与其他 [P] 任务修改同一文件的情况