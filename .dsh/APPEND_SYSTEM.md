# dsh-team 项目规则（quant-agent）

> **Runtime: dsh-team。** 本文件只适用于 DSH；如果你在 pi 下运行，忽略本文件，见 `.pi/APPEND_SYSTEM.md`。

本文件由 DSH 插件在 cwd 位于本仓库（或其 worktree）时注入。只写本仓库特有的 dsh-team 约定；团队通用规则在全局 `~/.dsh/AGENTS.md`。

## 1. 运行时与边界

- 本仓库是开源的 pi-team 项目；DSH 在这里做闭源 / 私有量化研究时，**不要把闭源策略、客户数据、账户信息写进本仓库的跟踪文件**。闭源内容放独立私有子项目，或放本机被 `.gitignore` 忽略的目录。
- 不用 pi 的 `/commit`、`/add`、`agent_call`、`commit-gate`、`private-links` 等工具；用 DSH 的 shell / skill 流程。
- 凭证永远不入库；财务 / 账户数据即使闭源也只放本机被忽略的文件或私有子项目。
- 研究和实盘完全隔离：不下单、不接证券账户、不发实时信号。

## 2. 三个研究目录

- DSH 会从项目根到 cwd 逐级加载 `AGENTS.md`；动 `Intraday/` / `Swing/` / `gridtrader/` 前仍要读各自的 `AGENTS.md`。
- 数据与运行时产物不入库；tick / 订单簿数据在 macmini，用 `ssh mac-mini` 在那边跑，不往本机拉 25GB。

## 3. Leader / Worker（DSH）

- 本仓库是 **Agent 项目**：任何分支 / worktree 的根目录作为 cwd 的 DSH 会话，都是 **Leader**。
- 本仓库没有独立子项目仓库，所以不产生 Worker 会话；任务 worktree 里的会话仍是 Leader。
- Leader 用 DSH shell / tmux 建 worktree、派任务、收尾；worktree 里不切 main、不 commit main。
- 如果闭源工作放在独立子项目仓库，那个子项目 worktree 的 DSH 会话才是 Worker。

## 4. 交付与记录

- 回测结论只认同口径复现；开源仓库内的改动记仓库根 `CHANGELOG.md`，私有内容记私有项目自己的 CHANGELOG。
- 不要混用 `.pi/APPEND_SYSTEM.md` 的 pi-team 规则；两套运行时规则各管各的。