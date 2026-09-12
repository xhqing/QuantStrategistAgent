# Changelog

## [Unreleased]

### 变更（assets/logo.svg 副标题去中文）

- **为什么改**：全局规则新增「Logo / 图标资产文字一律用英文」（2026-09-12 用户立，起因 Swing 仓库 logo 副标题混入中文被指出）：logo 是面向全球读者的视觉标识，中文受众已有 README_cn.md 双语通道；且 SVG 中文依赖查看环境的字体回退，渲染不可控。本次为按新规批量清理存量。
- **改了什么**：`assets/logo.svg` 副标题「Quant Strategist · 量化策略师」→「Quant Strategist」。

## 0.1.0（2026-09-12）

### 新增

- **建仓（门面重组）**：Markowitz 项目体系重组——原私有 QuantStrategistAgent 仓库更名为 **Intraday**（私有，日内策略研究本体），本仓库以 **QuantStrategistAgent** 之名重建为**公开门面**。为什么重组：日内策略进入实盘化阶段须整体私有（执行层 edge 公开等于自毁），但 agent 的方法学、运行纪律与公开组件需要对外展示面（求职、能力证明、团队开源生态）——门面与本体分离，两边各得其所。旧链接（改名前收藏 / clone 过本仓的）经 GitHub 语义自然落到本门面。
- **初始内容**：Markowitz 角色纪律（CLAUDE.md，方法学层面、无策略参数）、双语门面 README（核心方法论 + 仓库版图 + 与 DayTradingAgent 的关系）、logo 与工作流图、MIT 许可证。
- **组件仓库**：[Swing](https://github.com/xhqing/Swing)（日 K 趋势策略完整文档）同日拆出独立公开；[gridtrader](https://github.com/xhqing/gridtrader) 原有公开。
