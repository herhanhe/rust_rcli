# Copilot / AI Agent 指南

此文件为代码生成与修改时，AI 编码代理（如 Copilot、GPT 代理）在本仓库中应遵循的具体指南与示例命令。目标是让代理能快速进入本项目并执行常见开发任务。

**项目概览**

- **用途**: 这是一个由课程模板生成的最小 Rust CLI 模板项目（参考 [Cargo.toml](../Cargo.toml) 和 [src/main.rs](../src/main.rs)）。
- **重要位置**: 项目根有 `Cargo.toml`、`README.md`、`cliff.toml`（变更日志配置）、`deny.toml`（依赖检查），CI 在 [workflows/build.yml](workflows/build.yml) 定义。

**构建 / 测试 / CI 流程（可直接运行的命令）**

- **格式检查**: `cargo fmt -- --check`（CI 使用）
- **类型/依赖检查**: `cargo check --all`
- **静态分析**: `cargo clippy --all-targets --all-features --tests --benches -- -D warnings`
- **运行测试（项目使用 nextest）**: `cargo nextest run --all-features`（CI 使用 `cargo nextest run --all-features --no-tests warn`，请参照 [workflows/build.yml](workflows/build.yml)）
- **生成 changelog（发布时）**: CI 使用 `git-cliff` 配置文件 `cliff.toml`。

在本仓库进行代码修改时，代理应先确保本地验证：

1. 运行 `cargo fmt`（修复格式）或 `cargo fmt -- --check`（检查）。
2. 运行 `cargo clippy ...` 并解决所有警告（CI 将把警告视为错误）。
3. 运行 `cargo nextest run` 以验证测试（如果添加了测试）。

**项目约定与模式**

- 项目来自课程模板，`Cargo.toml` 的 `package.name` 目前为 `template`，不要随意重命名除非知道影响。参见 [Cargo.toml](../Cargo.toml)。
- 主入口位于 [src/main.rs](../src/main.rs)，当前为简单示例打印。新增 CLI 功能应遵守小而明确的改动，优先添加模块到 `src/` 并更新 `Cargo.toml`。
- CI 假设在 `main` 分支和以 `v*` 标签发布：不要更改工作流中触发规则，除非同步更新 `.github/workflows/build.yml`。

**依赖与工具链集成**

- 本仓库依赖一系列本地工具（参见 `README.md`）: `cargo-deny`, `typos-cli`, `git-cliff`, `cargo-nextest` 等。CI 也通过 action 安装 `nextest` 与 `cargo-llvm-cov`。
- 当修改依赖或升级工具链时，请检查 `deny.toml` / `typos.toml` 等配置文件对 CI 的影响。

**修改提议与 PR 要点**

- 小修复（格式、拼写、日志）: 在本地运行 `pre-commit`（如果已安装）并保证 `cargo fmt` 与 `cargo clippy` 通过。将这些变更合并为单个主题提交。
- 功能变更: 在 PR 描述中列出影响的主要文件和变更测试覆盖点，CI 会自动运行完整检查和 `git-cliff` 生成发布日志（仅在 tag 时）。

**示例：添加新依赖并验证**

1. 编辑 `Cargo.toml` 添加依赖。
2. 运行 `cargo check --all`。
3. 运行 `cargo clippy --all-targets --all-features --tests --benches -- -D warnings` 并修复警告。
4. 运行 `cargo nextest run --all-features`（如添加测试请确保通过）。

**参考文件**

- 根 README: [README.md](../README.md)
- CI workflow: [workflows/build.yml](workflows/build.yml)
- 入口: [src/main.rs](../src/main.rs)

请审阅此指南并指出是否需要把更多仓库特定例子加入（例如：特有代码风格、模块位置或常见 PR 模板）。
