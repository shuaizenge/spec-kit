<div align="center">
    <img src="./media/logo_small.webp"/>
    <h1>🌱 Spec Kit</h1>
    <h3><em>更快构建高质量软件。</em></h3>
</div>

<p align="center">
    <strong>借助规范驱动开发（Spec-Driven Development），让团队把精力集中在产品场景，而非重复的“无差异化”代码。</strong>
</p>

[![Release](https://github.com/github/spec-kit/actions/workflows/release.yml/badge.svg)](https://github.com/github/spec-kit/actions/workflows/release.yml)

---

## 目录

- 🤔 什么是规范驱动开发？
- ⚡ 快速开始
- 📽️ 视频概览
- 🤖 支持的 AI 代理
- 🔧 Specify CLI 参考
- 📚 核心理念
- 🌟 开发阶段
- 🎯 实验性目标
- 🔧 先决条件
- 📖 进一步了解
- 📋 详细流程
- 🔍 疑难排查
- 👥 维护者
- 💬 支持
- 🙏 致谢
- 📄 许可证

## 🤔 什么是规范驱动开发？

规范驱动开发（Spec-Driven Development，SDD）在传统软件开发方式上“翻转剧本”。几十年来，代码一直是王——规范只是搭起而后丢弃的脚手架。SDD 改变了这一切：**规范变为可执行**，直接生成可工作的实现，而不是仅仅指导实现。

## ⚡ 快速开始

### 1. 安装 Specify

选择你偏好的安装方式：

#### 方式一：持久安装（推荐）

一次安装，到处可用：

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

随后可直接使用：

```bash
specify init <PROJECT_NAME>
specify check
```

#### 方式二：一次性使用

无需安装，直接运行：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>
```

**持久安装的优势：**

- 工具常驻 PATH，可随时调用
- 无需创建 Shell 别名
- 通过 `uv tool list`、`uv tool upgrade`、`uv tool uninstall` 更好地管理工具
- 更干净的 Shell 配置

### 2. 建立项目原则

使用 **`/constitution`** 命令为你的项目创建治理原则与开发指南，以指导后续所有开发阶段。

```bash
/constitution 创建以代码质量、测试标准、用户体验一致性和性能要求为核心的项目原则
```

### 3. 创建规范（Spec）

使用 **`/specify`** 命令描述你要构建的内容。聚焦 **做什么（what）** 与 **为何（why）**，暂不涉及技术栈。

```bash
/specify 构建一个应用，帮助我将照片整理到不同的相册中。相册按日期分组，并可在主页面通过拖拽重新排序。相册之间不允许嵌套。每个相册内，照片以平铺方式预览。
```

### 4. 创建技术实现计划

使用 **`/plan`** 命令提供技术栈与架构选择。

```bash
/plan 该应用使用 Vite，尽量减少依赖库数量。尽可能采用原生 HTML、CSS 和 JavaScript。图片不会被上传到任何地方，元数据存储在本地的 SQLite 数据库中。
```

### 5. 拆解为任务

使用 **`/tasks`** 根据实现计划生成可执行的任务清单。

```bash
/tasks
```

### 6. 执行实现

使用 **`/implement`** 按计划执行所有任务，构建功能。

```bash
/implement
```

更详细的分步说明，参见我们的[完整指南](./spec-driven.md)。

## 📽️ 视频概览

想看看 Spec Kit 的实际效果？观看我们的[视频概览](https://www.youtube.com/watch?v=a9eR1xsfvHg&pp=0gcJCckJAYcqIYzv)！

[![Spec Kit video header](/media/spec-kit-video-header.jpg)](https://www.youtube.com/watch?v=a9eR1xsfvHg&pp=0gcJCckJAYcqIYzv)

## 🤖 支持的 AI 代理

| Agent                                                     | 支持 | 说明                                                         |
|-----------------------------------------------------------|------|--------------------------------------------------------------|
| [Claude Code](https://www.anthropic.com/claude-code)      | ✅   |                                                              |
| [GitHub Copilot](https://code.visualstudio.com/)          | ✅   |                                                              |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli) | ✅   |                                                              |
| [Cursor](https://cursor.sh/)                              | ✅   |                                                              |
| [Qwen Code](https://github.com/QwenLM/qwen-code)          | ✅   |                                                              |
| [opencode](https://opencode.ai/)                          | ✅   |                                                              |
| [Windsurf](https://windsurf.com/)                         | ✅   |                                                              |
| [Kilo Code](https://github.com/Kilo-Org/kilocode)         | ✅   |                                                              |
| [Auggie CLI](https://docs.augmentcode.com/cli/overview)   | ✅   |                                                              |
| [Roo Code](https://roocode.com/)                          | ✅   |                                                              |
| [Codex CLI](https://github.com/openai/codex)              | ⚠️   | Codex [不支持](https://github.com/openai/codex/issues/2890)自定义斜杠命令参数 |

## 🔧 Specify CLI 参考

`specify` 命令支持以下选项：

### Commands

| 命令        | 描述                                                                 |
|-------------|----------------------------------------------------------------------|
| `init`      | 使用最新模板初始化一个新的 Specify 项目                              |
| `check`     | 检查已安装的工具（`git`、`claude`、`gemini`、`code`/`code-insiders`、`cursor-agent`、`windsurf`、`qwen`、`opencode`、`codex`） |

### `specify init` 参数与选项

| 参数/选项              | 类型     | 描述                                                                 |
|------------------------|----------|----------------------------------------------------------------------|
| `<project-name>`       | 参数     | 新项目目录名称（使用 `--here` 时可选）                               |
| `--ai`                 | 选项     | 使用的 AI 助手：`claude`、`gemini`、`copilot`、`cursor`、`qwen`、`opencode`、`codex`、`windsurf`、`kilocode`、`auggie` 或 `roo` |
| `--script`             | 选项     | 脚本类型：`sh`（bash/zsh）或 `ps`（PowerShell）                       |
| `--ignore-agent-tools` | 标志     | 跳过对 AI 代理工具（如 Claude Code）的安装检查                        |
| `--no-git`             | 标志     | 跳过 Git 仓库初始化                                                   |
| `--here`               | 标志     | 在当前目录初始化项目，而非创建新目录                                 |
| `--force`              | 标志     | 在非空目录结合 `--here` 时强制合并/覆盖（跳过确认）                   |
| `--skip-tls`           | 标志     | 跳过 SSL/TLS 校验（不推荐）                                           |
| `--debug`              | 标志     | 启用详细调试输出，便于排障                                            |
| `--github-token`       | 选项     | GitHub Token（或设置 GH_TOKEN/GITHUB_TOKEN 环境变量）               |

### 示例

```bash
# 初始化基础项目
specify init my-project

# 指定 AI 助手
specify init my-project --ai claude

# 启用 Cursor 支持
specify init my-project --ai cursor

# 启用 Windsurf 支持
specify init my-project --ai windsurf

# 使用 PowerShell 脚本（Windows/跨平台）
specify init my-project --ai copilot --script ps

# 在当前目录初始化
specify init --here --ai copilot

# 在当前（非空）目录强制合并，无需确认
specify init --here --force --ai copilot

# 跳过 Git 初始化
specify init my-project --ai gemini --no-git

# 启用调试输出
specify init my-project --ai claude --debug

# 为 API 请求使用 GitHub Token（企业环境常用）
specify init my-project --ai claude --github-token ghp_your_token_here

# 检查系统要求
specify check
```

### 可用的斜杠命令

运行 `specify init` 后，你的 AI 编码代理将可使用以下斜杠命令来进行结构化开发：

| 命令            | 描述                                                   |
|-----------------|--------------------------------------------------------|
| `/constitution` | 创建或更新项目治理原则与开发指南                      |
| `/specify`      | 定义要构建的内容（需求与用户故事）                    |
| `/plan`         | 基于所选技术栈创建技术实现计划                        |
| `/tasks`        | 生成可执行的实现任务清单                              |
| `/implement`    | 按计划执行所有任务，构建该功能                        |

### 环境变量

| 变量              | 描述                                                                                                    |
|-------------------|---------------------------------------------------------------------------------------------------------|
| `SPECIFY_FEATURE` | 在非 Git 仓库中覆盖特性检测。设置为特性目录名（如 `001-photo-albums`），用于在不使用 Git 分支时选择具体特性。<br/>在使用 `/plan` 或后续命令前，必须在所用代理的上下文中设置。 |

## 📚 核心理念

规范驱动开发强调：

- **意图驱动开发**：在谈“如何实现”之前，先用规范明确“做什么”
- **有护栏的规范编写**：结合组织级原则与质量标准
- **多步打磨**：拒绝从一句 Prompt 直接“一把梭”生成代码
- **深度依赖**先进 AI 模型对规范的理解与执行能力

## 🌟 开发阶段

| 阶段 | 关注点 | 关键活动 |
|------|--------|----------|
| **0→1 开发**（Greenfield） | 从零开始生成 | <ul><li>从高层需求出发</li><li>生成规范</li><li>规划实现步骤</li><li>构建生产级应用</li></ul> |
| **创意探索** | 并行实现 | <ul><li>探索多样解法</li><li>支持多技术栈与架构</li><li>尝试不同 UX 模式</li></ul> |
| **迭代增强**（Brownfield） | 存量现代化 | <ul><li>迭代加功能</li><li>现代化遗留系统</li><li>适配现有流程</li></ul> |

## 🎯 实验性目标

我们的研究与实验聚焦：

### 技术独立性

- 使用多样技术栈创建应用
- 验证“规范驱动开发是一种与具体技术/语言/框架无关的过程”的假设

### 企业级约束

- 演示关键任务型应用的开发
- 纳入组织约束（云平台、技术栈、工程实践）
- 支持企业设计系统与合规要求

### 以用户为中心的开发

- 面向不同用户群体与偏好构建应用
- 支持多种开发方式（从“氛围编码”到 AI 原生开发）

### 创意与迭代流程

- 验证“并行实现探索”的概念
- 提供稳健的迭代式功能开发工作流
- 扩展流程以处理升级与现代化任务

## 🔧 先决条件

- **Linux/macOS**（或 Windows 上的 WSL2）
- AI 编码代理：[Claude Code](https://www.anthropic.com/claude-code)、[GitHub Copilot](https://code.visualstudio.com/)、[Gemini CLI](https://github.com/google-gemini/gemini-cli)、[Cursor](https://cursor.sh/)、[Qwen CLI](https://github.com/QwenLM/qwen-code)、[opencode](https://opencode.ai/)、[Codex CLI](https://github.com/openai/codex)、或 [Windsurf](https://windsurf.com/)
- 用于包管理的 [uv](https://docs.astral.sh/uv/)
- [Python 3.11+](https://www.python.org/downloads/)
- [Git](https://git-scm.com/downloads)

如果你在使用某个代理时遇到问题，请提交 issue，便于我们改进集成体验。

## 📖 进一步了解

- **[规范驱动开发完整方法论](./spec-driven.md)** —— 深入了解完整流程
- **[详细演练](#-详细流程)** —— 分步实施指南

---

## 📋 详细流程

<details>
<summary>点击展开分步演练</summary>

你可以使用 Specify CLI 引导项目，在你的环境中拉起所需工件。运行：

```bash
specify init <project_name>
```

或在当前目录初始化：

```bash
specify init --here
# 当目录已存在文件时跳过确认
specify init --here --force
```

![Specify CLI 在终端中引导新项目](./media/specify_cli.gif)

你将被提示选择正在使用的 AI 代理。也可以在终端中直接指定：

```bash
specify init <project_name> --ai claude
specify init <project_name> --ai gemini
specify init <project_name> --ai copilot
specify init <project_name> --ai cursor
specify init <project_name> --ai qwen
specify init <project_name> --ai opencode
specify init <project_name> --ai codex
specify init <project_name> --ai windsurf
# 或在当前目录：
specify init --here --ai claude
specify init --here --ai codex
# 在非空当前目录中强制合并
specify init --here --force --ai claude
```

CLI 会检查你是否安装了 Claude Code、Gemini CLI、Cursor CLI、Qwen CLI、opencode 或 Codex CLI。如果没有，或你希望不检查工具也能获取模板，可在命令中加入 `--ignore-agent-tools`：

```bash
specify init <project_name> --ai claude --ignore-agent-tools
```

### **步骤 1：** 建立项目原则

进入项目文件夹并运行你的 AI 代理。以下示例使用 `claude`。

![引导 Claude Code 环境](./media/bootstrap-claude-code.gif)

若看到 `/constitution`、`/specify`、`/plan`、`/tasks` 与 `/implement` 命令可用，说明已配置就绪。

第一步是使用 `/constitution` 命令建立项目治理原则。这有助于在后续所有开发阶段维持一致的决策：

```text
/constitution 创建以代码质量、测试标准、用户体验一致性和性能要求为核心的项目原则。请包含治理机制，说明这些原则如何指导技术决策与实现选择。
```

该步骤会创建或更新 `/memory/constitution.md`，其中的项目基线指南将在规范、规划与实现阶段被引用。

### **步骤 2：** 创建项目规范（Spec）

建立项目原则后，就可以创建功能规范了。使用 `/specify` 命令，并提供项目的具体需求。

> [!IMPORTANT]
> 明确写出你要构建的“是什么（what）”与“为什么（why）”。**此时不要关注技术栈。**

示例提示：

```text
开发 Taskify —— 一个团队生产力平台。该平台应允许用户创建项目、添加团队成员、分配任务、评论任务，并以看板（Kanban）方式在不同任务板之间移动任务。在本功能的初始阶段（我们称之为“创建 Taskify”），需要支持多个用户，但这些用户需预先定义。需要五个用户，分为两类：一名产品经理和四名工程师。请创建三个不同的示例项目。每个任务应有标准的看板列来表示状态，如“待办（To Do）”、“进行中（In Progress）”、“评审中（In Review）”和“已完成（Done）”。本应用无需登录功能，因为这是最初的测试阶段，目的是确保基础功能已搭建。

在任务卡片的 UI 中，用户应能在看板的不同列之间变更任务的当前状态。每个任务卡片应支持添加无限条评论。用户还应能在任务卡片中为任务分配任意一个有效用户。

首次启动 Taskify 时，系统会展示五个用户供选择，无需输入密码。点击某个用户后，将进入主界面，显示所有项目列表。点击某个项目后，将打开该项目的看板视图，展示各个状态列。用户可以在不同列之间拖拽任务卡片。分配给当前登录用户的任务卡片应以不同颜色显示，便于快速识别。用户可以编辑和删除自己发表的评论，但不能编辑或删除他人评论。
```

输入该提示后，你将看到 Claude Code 开始生成计划与规范草稿，并触发内置脚本来设置仓库。

完成后，你应该会得到一个新的分支（如 `001-create-taskify`），以及位于 `specs/001-create-taskify` 目录下的一份新规范。

产出的规范应包含用户故事与功能性需求，如模板所定义。

此时你的项目目录结构应类似如下：

```text
├── memory
│	 ├── constitution.md
│	 └── constitution_update_checklist.md
├── scripts
│	 ├── check-prerequisites.sh
│	 ├── common.sh
│	 ├── create-new-feature.sh
│	 ├── setup-plan.sh
│	 └── update-claude-md.sh
├── specs
│	 └── 001-create-taskify
│	     └── spec.md
└── templates
    ├── plan-template.md
    ├── spec-template.md
    └── tasks-template.md
```

### **步骤 3：** 澄清功能规范

在基线规范创建后，你可以进一步澄清首轮尝试中未准确捕捉的需求。例如，在同一 Claude Code 会话中使用如下提示：

```text
每个示例项目或你创建的项目都应包含 5 到 15 个不等的任务，这些任务应随机分布在不同的完成状态中。请确保每个阶段至少有一个任务。
```

你还应让 Claude Code 验证**“评审与验收清单”**，勾选满足条件的条目，保留不满足的条目为空。可使用如下提示：

```text
阅读评审与验收清单，如果功能规范满足某项条目，则在该项前打勾；如不满足则留空。
```

把与 Claude Code 的交互当作澄清规范与提出问题的机会——**不要把它的第一次输出当作最终版**。

### **步骤 4：** 生成计划

现在可以具体化技术栈与其他技术要求。使用 `/plan` 命令，并输入类似如下的提示：

```text
我们将使用 .NET Aspire 进行生成，数据库采用 Postgres。前端应使用 Blazor Server，支持拖拽式任务看板和实时更新。需要创建 REST API，包括项目 API、任务 API 和通知 API。
```

该步骤的输出将包含多份实现细节文档，目录结构类似：

```text
.
├── CLAUDE.md
├── memory
│	 ├── constitution.md
│	 └── constitution_update_checklist.md
├── scripts
│	 ├── check-prerequisites.sh
│	 ├── common.sh
│	 ├── create-new-feature.sh
│	 ├── setup-plan.sh
│	 └── update-claude-md.sh
├── specs
│	 └── 001-create-taskify
│	     ├── contracts
│	     │	 ├── api-spec.json
│	     │	 └── signalr-spec.md
│	     ├── data-model.md
│	     ├── plan.md
│	     ├── quickstart.md
│	     ├── research.md
│	     └── spec.md
└── templates
    ├── CLAUDE-template.md
    ├── plan-template.md
    ├── spec-template.md
    └── tasks-template.md
```

检查 `research.md`，确保所选技术栈与指令一致。若有不妥，可让 Claude Code 细化，甚至检查本机的框架/平台版本（如 .NET）。

此外，如果所选技术栈变化较快（如 .NET Aspire、JS 框架），可以让 Claude Code 针对实现计划与细节中需要进一步研究的部分做增补：

```text
请你仔细阅读实现计划和实现细节，找出哪些部分由于 .NET Aspire 作为一个快速演进的库而需要进一步研究。对于你识别出的需要补充研究的领域，请在 research 文档中补充关于本 Taskify 应用将要使用的具体版本的详细信息，并为每个细节发起并行的研究任务，利用网络资源澄清所有相关问题。
```

在此过程中，如果 Claude Code 研究方向跑偏，你可以用如下提示纠偏：

```text
我认为我们需要把这个过程拆解为一系列步骤。首先，列出在实现过程中你不确定、或者需要进一步研究的任务。请把这些任务逐条写出来。然后针对每一项任务，分别创建一个独立的研究任务，这样我们就能并行地对所有这些具体问题进行研究。我注意到你之前是在泛泛地研究 .NET Aspire，但这对我们当前的需求帮助不大，这种研究太过宽泛。研究应该聚焦于帮助你解决具体、明确的问题。
```

> [!NOTE]
> Claude Code 可能过于“积极”，添加你未要求的组件。请让它说明理由与变更来源。

### **步骤 5：** 让 Claude Code 校验计划

计划到位后，应让 Claude Code 通读以确保无遗漏。你可以使用如下提示：

```text
现在请你审查实现计划和实现细节文件。
请通读这些内容，判断是否存在一系列你需要执行的任务，这些任务是否在文档中已经明确列出。因为我不确定这里的信息是否足够。例如，在查看核心实现部分时，最好能在实现细节中引用相应的位置，以便在执行核心实现或细化步骤时能够查找到所需信息。
```

这有助于完善实现计划，避免规划周期中遗漏的盲点。完成初步打磨后，让 Claude Code 再按照清单过一遍再进入实现阶段。

你也可以让 Claude Code（若已安装 [GitHub CLI](https://docs.github.com/en/github-cli/github-cli)）从当前分支向 `main` 创建 PR，并附详细描述，以确保工作被妥善跟踪。

> [!NOTE]
> 在开始实现前，值得让 Claude Code 自查是否存在“过度工程”——它确实可能过于积极。若存在此类组件或决策，可让它进行精简。请确保它遵循[宪章](base/memory/constitution.md)作为基础约束来制定计划。

### 步骤 6：实施（Implementation）

准备就绪后，使用 `/implement` 执行你的实现计划：

```text
/implement
```

`/implement` 将会：
- 验证所有先决条件是否就绪（宪章、规范、计划与任务）
- 解析 `tasks.md` 中的任务分解
- 按正确顺序执行任务，并尊重依赖关系与并行执行标记
- 遵循任务计划中定义的 TDD 流程
- 提供进度更新，并妥善处理错误

> [!IMPORTANT]
> AI 代理会执行本地 CLI 命令（如 `dotnet`、`npm` 等）——请确保你机器上已安装所需工具。

实现完成后，请实际运行与测试应用，并解决可能不在 CLI 日志中显现的运行时错误（例如浏览器控制台错误）。你可以将此类错误复制给 AI 代理以便定位与修复。

</details>

---

## 🔍 疑难排查

### Linux 上的 Git Credential Manager

若在 Linux 上遇到 Git 认证问题，可安装 Git Credential Manager：

```bash
#!/usr/bin/env bash
set -e
echo "Downloading Git Credential Manager v2.6.1..."
wget https://github.com/git-ecosystem/git-credential-manager/releases/download/v2.6.1/gcm-linux_amd64.2.6.1.deb
echo "Installing Git Credential Manager..."
sudo dpkg -i gcm-linux_amd64.2.6.1.deb
echo "Configuring Git to use GCM..."
git config --global credential.helper manager
echo "Cleaning up..."
rm gcm-linux_amd64.2.6.1.deb
```

## 👥 维护者

- Den Delimarsky ([@localden](https://github.com/localden))
- John Lam ([@jflam](https://github.com/jflam))

## 💬 支持

如需支持，请提交[GitHub Issue](https://github.com/github/spec-kit/issues/new)。欢迎错误报告、功能需求，以及与规范驱动开发相关的问题。

## 🙏 致谢

本项目深受并基于 [John Lam](https://github.com/jflam) 的研究与工作。

## 📄 许可证

本项目基于 MIT 开源许可证发布。完整条款请参见 [LICENSE](./LICENSE)。


