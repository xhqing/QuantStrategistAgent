<p align="center">
  <img src="assets/logo.svg" width="320" alt="Markowitz logo" />
</p>

<h1 align="center">Markowitz — 量化策略 Agent（港股 / 美股）</h1>

<p align="center">
  <a href="LICENSE.md"><img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License: MIT" /></a>
  <img src="https://img.shields.io/badge/Version-0.1.0-4F46E5.svg" alt="Version" />
  <img src="https://img.shields.io/badge/Type-AI%20Agent-FF1493.svg" alt="Type: AI Agent" />
  <img src="https://img.shields.io/badge/Markets-HK%20%2F%20US-16C784.svg" alt="Markets: HK / US" />
  <img src="https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/xhqing/xhqing/main/traffic/badges/QuantStrategistAgent.json" alt="Visits/day (14d)" />
</p>

<p align="center">🌐 <a href="README.md">English</a></p>

**Markowitz** 是专职**量化策略开发**的拟人化 AI agent（港股 / 美股），基于 [Claude Code](https://claude.com/claude-code) 构建。它把交易思路写成可回测的确定性代码，用多年历史数据回测标定可信度，产出交给日内交易员 **Victor**（[DayTradingAgent](https://github.com/xhqing/DayTradingAgent)），作为 Victor 综合判断中**一个经过历史验证的加权投票员**。

> 本仓库是 Markowitz 项目的**公开门面**：agent 运行纪律（CLAUDE.md）、方法学与公开组件。进行中的日内策略研究放在**私有仓库**（见下方[仓库版图](#仓库版图)）——执行层的 edge 公开等于自毁，**知道什么该保密，本身也是专业能力的一部分**。

---

## 核心方法论

edge 来自可复现的数学严谨：

- **输出全量化**：每个字段要么是数值、要么是有限枚举——禁止「动能减弱」这类定性文字。这是量化区别于主观交易的本质。
- **回测验证的可信度**：策略不给自己打分；它的信心来自历史实测的胜率 / avg_R 与样本数。
- **纯函数、无前视**：同数据 + 同参数 → 永远同结果。未来数据不泄漏进任何决策。
- **诚实的负结果**：已证伪的想法留档记录、不掩埋——见各策略文档的「已证伪方案」节（如 [Swing 的 STRATEGY.md](https://github.com/xhqing/Swing/blob/main/swing/STRATEGY.md)）。
- **walk-forward 跨年验证**：样本内说明不了什么；样本外零衰减才是及格线。

<p align="center">
  <img src="assets/markowitz_workflow.svg" width="100%" alt="Markowitz 工作流：策略设计 → 回测 → 可信度查表 → 加权投票给 Victor" />
</p>

## 仓库版图

| 仓库 | 可见性 | 是什么 |
|---|---|---|
| **[Swing](https://github.com/xhqing/Swing)** | 公开 | 日 K 趋势跟随策略——**文档完整**：逻辑、参数、20 年 × 37 只回测、诚实标注局限、已证伪方案清单 |
| **[gridtrader](https://github.com/xhqing/gridtrader)** | 公开 | 网格交易策略开发与回测工具（Python / backtrader） |
| **[DayTradingAgent](https://github.com/xhqing/DayTradingAgent)** | 公开 | Victor——消费 Markowitz 加权投票的日内交易 agent |
| Intraday | 私有 | 进行中的日内策略研究（执行层 edge）。刻意保持私有。 |

## 与 Victor（DayTradingAgent）的关系

| | Markowitz（本仓库） | Victor（[DayTradingAgent](https://github.com/xhqing/DayTradingAgent)） |
|---|---|---|
| **职责** | 设计并回测量化策略 | 盯盘并发出交易信号 |
| **模式** | 离线、确定性代码 | 实时、LLM 判断 |
| **输出** | 策略代码 + 可信度查表 | 🟢/🔴/🟡/🟠 信号（人工执行） |
| **碰账户？** | 从不 | 从不（信号模式） |

Markowitz 的产物是 Victor 的**一个加权输入**——一个「历史验证过的可量化投票员」。资金流、盘口、新闻、宏观等主观判断因素仍归 Victor。

## xhqing Agent 团队成员

Markowitz 是 [xhqing AI agent 团队](https://github.com/xhqing)的成员——一支覆盖选品、数字生产、增长、交易、法务、测试等职能的拟人化 agent 舰队。完整名册见[团队总览](https://github.com/xhqing)。

## 免责声明

策略研究仅供学习。全部内容不构成投资建议；回测结果不代表实盘与未来表现。

## 版权与署名

- **版权**：Copyright (c) 2026 All Contributors，基于 [MIT 许可证](LICENSE.md)发布。
- **署名方式**：引用或基于本项目二次开发时，请保留版权声明并注明来源仓库。
- **项目地址引用**：<https://github.com/xhqing/QuantStrategistAgent>
