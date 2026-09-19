# Changelog

## [Unreleased]

### 新增（`.pi/skills/` 软链接指向 `.claude/skills/`）

- **为什么改**（2026-09-19）：pi coding agent 的项目级 skill 查找路径在 `.pi/` 下，与 Claude Code 的 `.claude/skills/` 不同；用软链接把两边打通，避免维护两份副本、也避免分叉（`.claude/skills/` 仍是唯一权威源）。
- **改了什么**（2026-09-19）：新建 `.pi/` 目录，其下创建软链接 `skills -> ../.claude/skills`（相对路径，仓库整体移动后依然有效）。无文件复制，`quant` skill 真身仍在 `.claude/skills/quant/`。

### 变更（体系叙事修正：删去「交付 Victor 当加权投票员」叙事，确立独立开发定位）

- **为什么改**（2026-09-19 用户多次纠正后正式落地）：用户明确裁定——Markowitz 的量化策略是**独立开发**，与 DayTradingAgent 无关系；但 CLAUDE.md / README / workflow 图中长期写着「产出交给日内交易员 Victor 当加权投票员」，这套交付叙事与事实不符，且反复误导 AI 会话往 DTAgent 关联方向发挥。
- **改了什么**（2026-09-19）：① `CLAUDE.md`——定位句改为「量化策略独立开发，与 DayTradingAgent 等任何交易执行项目无依赖、无交付关系」；四步工作流第 4 步「交付加权投票员」改为「沉淀策略库」；「角色关系」块改为「独立边界」；其余 Victor 指代（留给 Victor 主观判断 / 由 Victor 侧触发 / 不干扰 Victor）改为中性表述；② `README.md` / `README_cn.md`——介绍段改独立开发口径，仓库版图删 DayTradingAgent 行，删「与 Victor 的关系」整节，workflow 图 alt 同步；③ `assets/markowitz_workflow.svg`——STEP 4「Deliver（One weighted vote for Victor）」改为「Library（策略库沉淀）」，底部 Victor 交付条改为「Independent by Design」独立边界条；④ 三个子项目 CLAUDE.md 嵌文同步刷新。保留项：schema.md / backtest.py 中 `g` 公式推导文档出处链接（指向 DayTradingAgent 仓库 notes）仅是参考文献引用、非交付关系，未动。

### 变更（quant skill 权威源回迁：本项目新建 `.claude/skills/quant/`；Intraday 可见性改公开口径）

- **为什么改**（2026-09-16 用户裁定）：① 超集规则要求 Agent 项目的 `.claude/` 为权威源，但本项目此前无 `.claude/` 目录，quant skill 真身悬在子项目 Intraday（权威源悬空）；② 实测 GitHub 上 Intraday 仓库为 PUBLIC，用户裁定文档统一改公开口径（不再保留「刻意保持私有」叙述）。
- **改了什么**（2026-09-16）：① 新建 `.claude/skills/quant/`——以 Intraday 副本（功能最新：含 A 股 ETF 股票池等 9 月演进）为权威基线整体迁入（11 个 git 跟踪文件：SKILL.md / README.md / schema.md / data-sources.md / backtest.py / data.py / confidence_table.json / strategies/×3+.gitkeep）；`data_cache/`（258MB K 线缓存）、`output/`、`__pycache__/` 等运行时数据不迁、留在 Intraday（`.gitignore` 既有规则天然覆盖）；② gridtrader 副本同步覆盖为权威版（diff 三处逐字节一致）；③ `CLAUDE.md` 中四处 skill 引用由「子项目 Intraday 的路径」改回本项目路径，子项目清单 Intraday 行同步改写；④ `README.md` / `README_cn.md`——自述段删「执行层 edge 公开等于自毁」保密叙述，仓库版图 Intraday 行改公开口径（可见性 public / 描述对齐 Intraday 实际研究内容）；⑤ 三个子项目 CLAUDE.md 嵌文同步刷新。

### 变更（体系概念修正：「公开门面 / 私有本体」作废，确立主仓库 + 三子项目结构）

- **为什么改**（2026-09-16 用户裁定）：只有以 Agent 结尾命名的仓库才是真 Agent 项目，「私有本体 / 公开门面」这套两分概念作废。原口径把 Intraday（原 QuantStrategistAgent 改名而来）当 Agent 项目、仅挂 gridtrader 一个子项目，Swing 与 Intraday 归属悬空，与实际（本仓库是 Markowitz 的 Agent 项目）不一致。
- **改了什么**（2026-09-16）：① `CLAUDE.md` 子项目清单更新为三个子项目（Intraday / Swing / gridtrader），并把正文中指向不存在路径的 `.claude/skills/quant/` 引用改为指向子项目 Intraday 的对应文件（本仓库无 `.claude/` 目录，quant skill 真身在 Intraday）；② `README.md` / `README_cn.md`——自述由「公开门面」改为「主仓库（agent 项目）」，仓库版图表格补子项目 / 协作方定位标注；③ 本文件全文同步进三个子项目的 CLAUDE.md（Swing 新建、gridtrader / Intraday 更新嵌文，diff 验证逐字节一致）。

### 变更（CLAUDE.md 删去「由 Claude Code 自动加载」说明句）

- **为什么改**：用户 2026-09-12 要求 CLAUDE.md 不再强调本文由 Claude Code 加载，团队全部项目的 CLAUDE.md 统一清理此类语句。
- **改了什么**（2026-09-12）：`CLAUDE.md` 开头角色定位行删去句尾「本文件由 Claude Code 在每次会话开头自动加载。」，角色描述本身保留。

### 变更（assets/logo.svg 副标题去中文）

- **为什么改**：全局规则新增「Logo / 图标资产文字一律用英文」（2026-09-12 用户立，起因 Swing 仓库 logo 副标题混入中文被指出）：logo 是面向全球读者的视觉标识，中文受众已有 README_cn.md 双语通道；且 SVG 中文依赖查看环境的字体回退，渲染不可控。本次为按新规批量清理存量。
- **改了什么**：`assets/logo.svg` 副标题「Quant Strategist · 量化策略师」→「Quant Strategist」。

## 0.1.0（2026-09-12）

### 新增

- **建仓（门面重组）**：Markowitz 项目体系重组——原私有 QuantStrategistAgent 仓库更名为 **Intraday**（私有，日内策略研究本体），本仓库以 **QuantStrategistAgent** 之名重建为**公开门面**。为什么重组：日内策略进入实盘化阶段须整体私有（执行层 edge 公开等于自毁），但 agent 的方法学、运行纪律与公开组件需要对外展示面（求职、能力证明、团队开源生态）——门面与本体分离，两边各得其所。旧链接（改名前收藏 / clone 过本仓的）经 GitHub 语义自然落到本门面。
- **初始内容**：Markowitz 角色纪律（CLAUDE.md，方法学层面、无策略参数）、双语门面 README（核心方法论 + 仓库版图 + 与 DayTradingAgent 的关系）、logo 与工作流图、MIT 许可证。
- **组件仓库**：[Swing](https://github.com/xhqing/Swing)（日 K 趋势策略完整文档）同日拆出独立公开；[gridtrader](https://github.com/xhqing/gridtrader) 原有公开。
