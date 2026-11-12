# Swing（日 K 趋势跟随策略）

> 原独立仓库 `xhqing/Swing`（远端仓库保留为存档，完整提交历史在那边），2026-11-08 并入 Mark 仓库成为 `Swing/` 目录。

日 K 趋势跟随策略工具集（美股大盘股，Python，完整文档 + 回测验证 + 可信度标定）。

## 定位与边界

- 仓库是**研究 / 工具**：不下单、不接实盘。
- 版本号与改动记录以 Mark 仓库根 `VERSION` / `CHANGELOG.md` 为准；本目录的 `CHANGELOG.md` 自并入之日起定格为历史记录，不再更新。

## 两个策略，两种证据等级（别混用）

- **主策略（交付级）**：`trend/` 日 K 趋势跟随，37 只美股 / ETF，2006–2026 回测 + walk-forward 零衰减，完整口径见 `trend/STRATEGY.md`。
- **候选策略（非交付级）**：`rotation/` A 股 ETF 动量轮动。参数检验含过拟合成分，**诚实预期年化 12%~16%**；文档、汇报、注释里不得把 19.8% 当预期收益，也不得称其为「稳健 / 交付级」。见 `rotation/ROTATION.md`。

## 数据面（回测可复现的前提）

- `engine/data_cache/*.parquet` 是回测的**唯一数据源**：同快照 → 同结果。只通过 `engine/data.py fetch ...` 增量更新，**不许手改 parquet、不许用实时数据覆盖历史**。
- 数据源分流：港美股走富途 OpenD（本机需先起 OpenD）；A 股（SH / SZ）走腾讯财经、免 OpenD。来源与坑见 `engine/data-sources.md`。
- 时区落盘即带市场时区：港股 `Asia/Hong_Kong (+08:00)`、美股 `America/New_York`（EDT / EST），不写裸时间戳。
- `engine/data_cache/`、`engine/output/`、`rotation/output/`、`tmp/`、`docs/`、`artifacts/` 已被忽略，**运行时产物不要入库**；`engine/confidence_table.json` 虽是回测产物但**要入库**（可信度查表随版本冻结）。

## 策略参数是验证出来的成果，不许「顺手优化」

- 主策略：收盘创 20 日新高（不含当日）+ 量 ≥ 1.5×20 日均量 → 次日开盘买；2×ATR 固定止损、3×ATR trailing、60 日超时。**刻意不加任何过滤**——`STRATEGY.md` §10 已用数据证伪 ADX 过滤、海龟主动离场、探底回升进场、多空对冲、加资产 / 加港股等「改进」，实测全部更差。
- 任何参数 / 逻辑改动：必须同口径跑改动前后回测（`trend/backtest.py --scheme 1`，需要时 `--all`），把数字写进 PR 与仓库根 `CHANGELOG.md`；纯文档改动不涉及。
- 标的池 37 只是验证过的边界：换池 / 加池要重跑四重泛化验证，不能只看总池化数字。

## 复现入口

首次拉数：`cd engine && python3 data.py fetch-all --ktype DAY`；主策略：`cd trend && python3 check_signal.py`、`python3 backtest.py --scheme 1`、`python3 analyze.py monthly|hold|yearly`；轮动：`cd rotation && python3 rotation.py fetch-pool && python3 rotation.py signals`。指标含义见 `trend/STRATEGY.md` §7。

## 更多

`trend/STRATEGY.md`（主策略全量）、`rotation/ROTATION.md`（轮动全量）、`engine/schema.md`（输出契约与回测设计）、`engine/data-sources.md`（数据源）；通用工作规则见仓库根 `AGENTS.md` 与各运行时的全局规则。
