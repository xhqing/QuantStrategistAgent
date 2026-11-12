# pi-team 项目规则（quant-agent）

> **Runtime: pi-team。** 本文件只适用于 pi；如果你在 DSH 下运行，忽略本文件，见 `.dsh/APPEND_SYSTEM.md`。

本文件由 pi 在 cwd 位于本仓库（或其 worktree）时自动追加进系统提示。只写本仓库特有的 pi 约定；团队通用规则在全局 `~/.pi/agent/AGENTS.md`。

## 1. 运行时与规则

- 本仓库是开源项目，归 pi-team；用 pi 的 `/add`、`/commit`、`/bump`、`/release` 流程。
- 版本以仓库根 `VERSION` 为唯一权威；改动记仓库根 `CHANGELOG.md`。
- 通用工作规范（verify-before-report / file-operation-priority / tmp-dir-for-artifacts）由全局规则加载，不在这里重述。

## 2. 三个研究目录

- 动 `Intraday/` / `Swing/` / `gridtrader/` 前，显式读各自的 `AGENTS.md`。
- pi 只自动加载 cwd 与祖先目录的 `AGENTS.md`，**不会**自动加载嵌套目录的规则；所以必须显式去读。
- 数据与运行时产物不入库；tick / 订单簿数据在 macmini，需要时用 `macmini_run` 在那边跑。

## 3. Leader / Worker

- 本仓库是 **Agent 项目**：任何分支 / worktree 的根目录作为 cwd 的 pi 会话，都是 **Leader**。
- 本仓库没有独立子项目仓库（`Intraday/` / `Swing/` / `gridtrader/` 是同一仓库的三个目录），所以**不产生 Worker 会话**；任务 worktree 里的会话仍是 Leader，按 Leader 规则操作。
- Leader 的职责：同步 local main、按任务建 worktree、派任务、收尾；worktree 里不切 main、不 commit main。
- 用 pi 的 `agent_call` / `agent_wake` 派活；若要交给某个子项目 Worker，那个子项目必须是独立仓库并已登记。
- 详见全局 `~/.pi/agent/AGENTS.md`「Leader / Worker 的定义」。

## 4. 其他 pi 约定

- `.commit-cache.md` 是 pi 的提交缓存，按 pi 的 commit skill 维护。
- 不要混用 `.dsh/APPEND_SYSTEM.md` 的 dsh-team 规则；两套运行时规则各管各的。