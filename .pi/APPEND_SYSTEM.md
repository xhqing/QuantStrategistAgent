# Mark 仓库 · 项目追加规则

本文件由 pi 在 cwd 位于本仓库（或其 worktree）时自动追加进系统提示。**只写本仓库特有的约定**；团队通用规则在全局 `~/.pi/agent/AGENTS.md`，此处不重述。

## 三个研究目录，各有各的硬约束

本仓库根下有三个研究目录（2026-11-08 由三个独立仓库并入）：`Intraday/`、`Swing/`、`gridtrader/`。

**动某个目录之前，先读它自己的 `AGENTS.md`**（`Intraday/AGENTS.md` / `Swing/AGENTS.md` / `gridtrader/AGENTS.md`）。三份规则差异很大——数据源与 `data_cache` 口径、label 用成交价还是委托价、参数能不能"顺手优化"、依赖用 Pipenv 还是系统 Python——写错一边就是错的回测结论。嵌套的 `AGENTS.md` 不会被自动加载，只能靠显式去读。

## 三条跨目录的共同底线

1. **研究与实盘完全隔离**：不下单、不接证券账户、不发实时信号；本仓库只产出可回测的确定性代码与结论。
2. **数据与运行时产物不入库**：`Intraday/intraday/{data_cache,output,models,tick}/`、`Swing/engine/{data_cache,output}/`、`Swing/rotation/output/`、各目录 `tmp/`，需要时重新生成。tick / 订单簿数据在本机之外（macmini），要跑先说明、用 `macmini_run`。
3. **版本与记录单一口径**：仓库根 `VERSION` 是唯一版本权威；所有改动记仓库根 `CHANGELOG.md`。三个目录各自的 `CHANGELOG.md` 已定格为历史记录，不再更新。

## 入口

- 项目本体与协作规则：仓库根 `AGENTS.md`
- quant 策略技能：`.claude/skills/quant/`（`.pi/skills` 指向它）
- 研究结论层：`Intraday/intraday/CONCLUSIONS.md`、`Swing/trend/STRATEGY.md`、`Swing/rotation/ROTATION.md`
