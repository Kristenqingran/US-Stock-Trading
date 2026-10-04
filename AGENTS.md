## Development and Release Workflow

本项目采用 `test → review → main` 的开发与发布流程。

### Branch 定义

- `test`：开发、实验和验证分支
- `main`：稳定版本分支

`main` 只保存已经经过测试和人工 Review 的稳定版本。

### 标准开发流程

所有开发变更必须遵循：

`Development → Local Test → Commit → Push origin/test → Validation → Owner Review → Merge main`

1. 所有代码修改从 `test` 分支开始，禁止直接在 `main` 上进行日常开发。
2. 开发完成后必须先执行与修改相关的本地测试，测试失败不得进入 Release 流程。
3. 本地测试通过后才能 Commit，并只 Push 到 `origin/test`。
4. Push 后进行功能验证、Agent 输出 Review、Test Design Review，以及必要的实验 / Evaluation。
5. 只有 Owner 明确确认当前版本通过后，才允许将 `test` 合并到 `main`。
6. Codex 不得自行 Merge 到 `main`，不得直接 Push 开发代码到 `origin/main`，不得 Force Push。

### 开发前检查

开始任何开发任务前必须执行：

`git branch --show-current`

`git status`

当前开发分支必须为 `test`，并确认工作区符合预期、没有非预期修改或敏感文件。

### Commit 与 Push

Commit 前必须执行 `git status` 并检查实际准备提交的文件。Commit 应保持单一职责，并使用清晰的 Conventional Commit message。

本地验证和 Commit 完成后，只允许执行 `git push origin test`。

### Push / Git Authentication Failure

如果 Push 因 Authentication、Credential、Network、Permission 或 Execution Environment 失败：

1. 立即停止 Push 流程。
2. 保留本地 Commit。
3. 输出完整错误信息并等待用户处理。

不得自行修改 Git Credential、Remote URL、SSH Key 或用户全局 Git 配置，也不得 Force Push。

### Repository Data Boundary

以下目录和文件禁止 Commit / Push：

- `raw/`
- `private/`
- `docs/`
- `experiments/`
- `requirements/`
- `.env`
- `.env.*`
- `.DS_Store`
- API Key、Token、Password、Credential 及其他敏感数据

始终遵守 `.gitignore`。

### Evaluation Data Isolation

Human Baseline 和未来的 Evaluation Dataset 不等于 Agent Runtime Context。运行 Test Design Agent 时禁止读取 Human Baseline、将其注入 Prompt，或根据其补充 Rule / Risk / Scenario。

`Agent Generation Context ≠ Evaluation Ground Truth`
