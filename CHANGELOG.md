# Changelog

### 变更（同步子项目更名：TeamToolkit → agents-infra，2026-11-11）

- **为什么改**：用户 2026-11-11 决定把 Kit 名下的子项目 TeamToolkit 更名为 **agents-infra**（GitHub 仓库 `xhqing/TeamToolkit` → `xhqing/agents-infra`，全局 `~/.pi/agent` 里的引用与发布脚本目录同批改完）。本仓库 `AGENTS.md` 里还留着一处指向旧名的引用。
- **改了什么**：`AGENTS.md` 第 65 行（「工作规则」节末尾）那句「权威源见 TeamToolkit 仓库的 `pi/agent/AGENTS.md`」改为「权威源见 agents-infra 仓库的 `pi/agent/AGENTS.md`」；全仓库复查无其它 `TeamToolkit` / `team-toolkit` 残留引用。
- **回归风险**：无——纯文档改名引用，不涉及任何代码、数据或回测口径。

### 变更（项目更名为 quant-agent，2026-11-09）

- **为什么改**：用户 2026-11-09 正式规定 agent 项目命名统一为 `<职责>-agent`（全小写、连字符）。本项目（Mark）由 `QuantStrategistAgent` 更名为 `quant-agent`——名称最短且直接表明量化。
- **改了什么**：① 本地目录与 GitHub 仓库更名为 `quant-agent`；② `README.md` / `README_cn.md`、`AGENTS.md`、三个并入目录（`Intraday/` / `Swing/` / `gridtrader/`）的双语 README、`Swing/engine/data.py` 注释、`Swing/rotation/ROTATION.md` 等处的项目名与 GitHub 链接同步改名；③ 同批把文档里指向 Victor 项目名字（`DayTradingAgent` → `trading-agent`，含 `.claude/skills/quant/` 下的推导链接）改齐。

### 新增（CI 门禁：validate 检查 + main 分支 strict 保护）

- **为什么改**：用户 2026-11-03 立规「新建仓库一律必须 strict 模式的 PR CI」，并要求 28 个不合规的存量仓库统一执行最新标准。本项目此前在 PR 上没有门禁——改动合进 main 全靠人工把关，PR 可以拿旧 main 上通过的检查直接合并，回归防护存在缺口。本次补齐团队要求的「三件套」：PR 上真实跑的 CI 检查 + 挂成 required status check + `required_status_checks.strict = true`。
- **改了什么**（2026-11-03）：新增 `.github/workflows/validate.yml`（PR 与 main push 触发，检查名 `validate`）与 `.github/scripts/validate.py`（仓库卫生检查：必备文件存在、敏感文件未被跟踪、被跟踪文件不超过 10 MB、README 相对链接有效、Python / Shell 语法检查）；main 分支保护补挂 required check `validate` 并设 `strict=true`。
- **边界**：检查只做确定性、低误报的项目（required check 一旦误报就会卡住正常 PR，所以宁可少查）；不改动仓库任何原有内容。

## [Unreleased]

### 更正（Leader / Worker 定义：Agent 项目任何 worktree = Leader，2026-11-12）

- **为什么改**：用户 2026-11-12 重新定义 Leader / Worker——Leader / Worker 是 **Agent 项目 vs 它负责的子项目** 的概念，不是“主工作区 vs worktree”。Agent 项目任何分支 / worktree 根 = Leader；子项目任何分支 / worktree 根 = Worker。
- **改了什么**：`.pi/APPEND_SYSTEM.md` 与 `.dsh/APPEND_SYSTEM.md` 的 Leader / Worker 段同步改为“本仓库任何 worktree 都是 Leader；本项目没有独立子项目仓库，不产生 Worker”。
- **回归风险**：无——纯规则表述修正，不涉及代码、数据或回测口径。

### 变更（双团队形态：pi-team + dsh-team，2026-11-12）

- **为什么改**：用户 2026-11-12 决定把 quant-agent 改造成同一个 Agent 项目同时服务 pi-team（开源）与 dsh-team（闭源）的形态；同一个人、一套版本 / CHANGELOG，两套运行时规则分开。
- **改了什么**：
  - 根 `AGENTS.md` 只保留共享身份、量化铁律、产物契约、设计原则、工作流、数据约束与仓库结构；新增「运行时」分支：pi 读 `.pi/APPEND_SYSTEM.md`，DSH 读 `.dsh/APPEND_SYSTEM.md`。
  - `.pi/APPEND_SYSTEM.md` 收拢 pi-team 专属：pi 工作流、Leader / Worker、`agent_call`、嵌套规则读取、`.commit-cache.md`。
  - 新增 `.dsh/APPEND_SYSTEM.md`：dsh-team 专属，闭源策略 / 数据边界、DSH Worker、无 pi 工具。
  - quant skill 从 `.claude/skills/quant/` 迁到 `.agents/skills/quant/`（pi、DSH 共读）；删除 `.claude/` 与 `.pi/skills` 软链接。
  - 三个研究目录 `AGENTS.md` 与 Intraday 双语 README 的 skill 路径同步改为 `.agents/skills/quant/`；不再把 `~/.pi/agent` 当作共享规则源。
- **回归风险**：低——纯规则 / 目录形态调整，不涉及策略代码、回测口径或数据；skill 文件内容未改，仅路径迁移。


### 更正（并入历史表述：本仓库是单个提交接手，原历史在存档仓库里，2026-11-08）

- **为什么改**：上一版把并入说成「原仓库的提交历史完整保留在本仓库 git 历史里」，与事实不符——并入用的是 `git subtree add`（本地确实引入了完整历史），但 PR 走的是仓库规定的 **squash 合并**，历史在合进 main 时被压成一个提交。继续这么写等于给后来者一个错误前提（以为 `git log -- Intraday/` 能翻到原仓库的逐条提交）。
- **改了什么**：`README.md` / `README_cn.md` 的仓库版图说明、本条 CHANGELOG 的相应句子、以及 `Intraday/` / `Swing/` / `gridtrader/` 三个 `AGENTS.md` 的开头说明，统一改为「原仓库保留为只读存档、**完整提交历史在存档仓库里**；本仓库整块接手内容（单个提交）」。
- **原始历史仍在**：三个 GitHub 存档仓库（`xhqing/Intraday`、`xhqing/Swing`、`xhqing/gridtrader`）保存着并入前的全部提交；本地另有 `chore/absorb-subprojects` 分支留着 subtree 引入的那串历史。

### 变更（Intraday / Swing / gridtrader 三个子项目仓库并入本仓库成为三个目录，2026-11-08）

- **为什么改**：用户 2026-11-08 决定「这三个子项目退化为 Mark 仓库下面的三个目录」。三个研究项目各自占一个仓库，导致每次改动都要在三个仓库各走一遍 PR + CI + 发版流程，而它们其实是同一个研究方向的三条腿（日内 / 日 K 趋势 / 网格）；同时团队正在把「Agent 项目 ↔ 子项目」的关系改为完全独立，减少仓库数量是同一方向上的第一步。
- **改了什么**：
  - 三个仓库用 `git subtree add` 并入本仓库的 `Intraday/`、`Swing/`、`gridtrader/` 三个前缀，内容整块并入（三个 GitHub 远端仓库保留为只读存档，**它们原有的完整提交历史在存档仓库里**；本仓库是单个提交接手，不携带原仓库的逐条提交）。
  - 被忽略的数据与运行时产物（`Intraday/intraday/{data_cache,models,output}/`、`Swing/engine/{data_cache,output}/`、`Swing/rotation/output/`、`Intraday/DATA-LOCATION.local.md`）一并搬到新位置，保持原来的忽略口径；`__pycache__` 丢弃（可即时重生）。
  - 删除三个目录里已无意义的仓库级文件：`AGENTS.md` 软链接（改为各目录自己的实体 `AGENTS.md`，承载原 `.pi/APPEND_SYSTEM.md` 的全部硬约束）、`.pi/`、`.github/`（GitHub 不读子目录 workflow）、`LICENSE.md`（根许可证覆盖全仓库）、`VERSION`（版本权威收敛到仓库根）、`.commit-cache.md`。
  - 三份 `CHANGELOG.md` 添加「历史记录、自并入之日起定格」的说明并保留全部历史内容；新改动一律记本文件。
  - 仓库根 `AGENTS.md` 的「子项目清单」整节重写为「仓库结构：三个研究目录」＋「Leader / Worker 分工」，删掉已废止的「身份层软链接 + 能力层目录引用」机制描述与已移出名册的 NewsScoreStudy 条目；`README.md` / `README_cn.md` 的「Repository Landscape / 仓库版图」同步改为目录表，两处指向 `xhqing/Swing` 的链接改为仓库内相对路径。
  - 新增 `.pi/APPEND_SYSTEM.md`：把「动某个目录前先读它自己的 `AGENTS.md`」写成会话启动即加载的规则，并收拢三条跨目录底线（研究不接实盘 / 数据不入库 / 版本与记录单一口径）。
- **回归检查**：并入前三个仓库均与远端同步、工作区干净（已逐个核实）；`validate.py` 本地预检 126 个被跟踪文件全部通过（README 相对链接、Python 语法、敏感文件、大文件）。原 `quant` skill 未移动（实体在 `.claude/skills/quant/`）。

### 变更（项目规则文件本体统一改为 `AGENTS.md`，`CLAUDE.md` 弃用，2026-11-05）

- **为什么改**：用户 2026-11-05 立规「所有注册表项目与全局的规则文件本体统一用 `AGENTS.md`，`CLAUDE.md` 弃用」。pi 的上下文文件候选顺序是 `AGENTS.override.md` > `AGENTS.md` > `CLAUDE.md`（同一目录只加载第一个存在的），本体放在 `AGENTS.md` 后 `CLAUDE.md` 根本不参与加载——两份并存只会形成双份内容、随时漂移。
- **改了什么**：项目根 `CLAUDE.md` 改名为 `AGENTS.md`（实体本体，原先的 `AGENTS.md → CLAUDE.md` 软链接已删除）；项目内引用「项目 `CLAUDE.md` / 全局 `CLAUDE.md`」的条文、skill、hook、脚本与 README 同步改为 `AGENTS.md`。
- **回归检查**：加载顺序不受影响（`AGENTS.md` 本就排在 `CLAUDE.md` 之前）；子项目根目录的 `AGENTS.md` 软链接仍指向本项目 `AGENTS.md`，目标不变、照常解析。

### 变更（移除访问量徽章，配合停用集中式流量采集）

- **为什么改**：用户 2026-11-04 决定停用 xhqing 仓库的集中式访问量采集，同时不再在各仓库 README 展示访问量徽章。徽章的数据源必须由采集任务每天刷新，采集停掉之后徽章会冻结在旧值，而徽章文案写的是「Visits/day (14d)」——数字不再更新却仍显示成实时数据，留着反而误导读者。
- **改了什么**：`README.md` / `README_cn.md` 顶部徽章区删除那一枚 shields.io 访问量徽章（整行删除，数据源 `https://raw.githubusercontent.com/xhqing/xhqing/main/traffic/badges/<仓库名>.json`）；License / Version / Type 等其它徽章与正文一个字未动。
- **回归检查**：只删这一整行，徽章区所在的 `<p align="center">` / `<div>` 结构与相邻徽章不受影响；README 其余内容零改动。

### 变更（子项目 `.pi` 由软链接改为真实目录 + 资源引用：全局规矩 2026-10-30 修订）

- **为什么改**：全局 `~/.pi/agent/docs/agents-registry.md`「智能体专属文件归属与子项目映射（2026-10-30 修订）」取代 2026-10-17 版。旧形态把 `.pi` 整个挂成指向本项目的软链接，把「共享」和「独占」压在了同一个符号上：子项目要放自己的配置时无处安放，硬写进 `.pi` 等于直接写进本项目——会瞬间影响全部兄弟子项目、还进本项目 git 历史。而 `AGENTS.md` 没有 include / 导入机制，做超集只能靠复制、复制必然漂移。新规则分层：身份层（`AGENTS.md` 软链接）保持一致，能力层（`.pi/`）在子项目建真实目录、用 `settings.json` 以路径引用本项目资源目录，本地专属内容放子项目自己的目录——行为上是超集，结构上无副本、不漂移。
- **改了什么**：① 本文件（`AGENTS.md` 为指向它的软链接）「子项目清单」节标题改为「2026-10-30 修订」，规则段重写为两层模型（身份层软链接 / 能力层真实 `.pi/` + `settings.json` 路径引用），补上纪律（子项目 `.pi` 只允许追加，专属规则用 `APPEND_SYSTEM.md`、禁用 `SYSTEM.md`），并把子项目 worktree `NewsScoreStudy-code-review` 一并列入；② 子项目侧的 `.pi` 改造（删软链接 → 真实目录 + `settings.json`）：`Intraday` / `Swing` / `gridtrader` / `NewsScoreStudy`（`NewsScoreStudy-code-review` 为其 worktree，随仓库快进同步），各处具体变更与验证记在各自仓库的 CHANGELOG。本项目自己的 `.pi`（`skills -> ../.claude/skills`）不动。
- **验证**：据 pi 官方文档（`settings.md`「Resources」——项目 `.pi/settings.json` 的路径相对 `.pi` 解析、支持 `~`、路径可为目录并递归收集；`skills.md` / `extensions.md`——`.pi/{skills,extensions,prompts,themes}` 的自动发现照常生效）；在 `Intraday` 子项目目录实跑 `pi --approve -p --no-session`，技能列表里 `quant` 正常加载（经本项目 `.pi/skills -> ../.claude/skills` 解析），无 `settings.json` 解析报错。
- **规则出处**：全局 `~/.pi/agent/docs/agents-registry.md`「智能体专属文件归属与子项目映射（2026-10-30 修订）」节。

### 变更（新增子项目 NewsScoreStudy：由 Victor 移交本项目负责，2026-10-26 用户决定）

- **为什么改**：面试实操题研究仓库 `NewsScoreStudy`（评估「大模型给新闻打的分（-1~1）值不值得交易」）原归 Victor（DayTradingAgent），用户 2026-10-26 决定改由 Mark 负责——本项目正是量化策略研究岗，「信号值不值得交易」属策略研究范畴而非日内执行范畴。
- **改了什么**：① 「子项目清单」新增 NewsScoreStudy 条目（含路径 `/Users/xhq/Developer/NewsScoreStudy`、私有远程仓库 `xhqing/NewsScoreStudy`、内容与交付物、移交日期）；② 同节规则句里的「三个子项目根目录各挂两条软链接」改为「**各子项目（Intraday、Swing、gridtrader、NewsScoreStudy）**根目录各挂两条」，避免以后新增子项目时又出现写死的数字。仓库侧对应改动：该仓库根的 `AGENTS.md` / `.pi` 两条软链接已由指回 DayTradingAgent 改指本项目（加载 Mark 身份与 quant skill）。
- **边界与注意事项**：仓库物理位置未变，仍在原路径（`~/Developer/NewsScoreStudy`，与各子项目同级），**不在本项目目录内**；该仓库当前只有题目存档 + 原始数据 + 骨架，`PLAN.md` 未开写（用户尚未决定是否做该选题）。
- **回归风险**：无——只新增子项目清单条目与文档措辞，不动量化代码、数据与回测逻辑；`settle-repo-check` 收尾巡检从本次起把 NewsScoreStudy 纳入本项目「负责仓库必须干净」的检查范围（预期行为）。

### 变更（迁移收尾：CLAUDE.md 三处全局文档旧路径 `~/.claude/` → `~/.pi/agent/`）

- **为什么改**（2026-10-17）：全局权威源已于 2026-10-17 由 `~/.claude/` 迁至 `~/.pi/agent/`（见全局 `~/.pi/agent/docs/capability-sync.md`「2026-10-17 权威源迁移」），本项目 `CLAUDE.md` 仍残留三处迁移前旧路径引用（由跨会话迁移收尾任务检出）——旧路径在本机已不存在，引用悬空且会误导读者。
- **改了什么**（2026-10-17）：`CLAUDE.md`（`AGENTS.md` 为其软链）三处修正——① 「工作规则」节通用规范三件套句：全局 `~/.claude/CLAUDE.md` → `~/.pi/agent/CLAUDE.md`；② 「子项目清单」节标题句：`~/.claude/docs/agents-registry.md` → `~/.pi/agent/docs/agents-registry.md`；③ 同节 2026-10-17 规则段括号内的细则引用同步修正。历史 CHANGELOG 条目中的旧路径保持原样不改（历史记录照旧）。

### 变更（Agent 拟人名更名：Markowitz → Mark）

- **为什么改**（2026-10-17 用户决定）：用户要求把本项目 agent 的拟人名从「Markowitz」改为「Mark」（项目名 QuantStrategistAgent 不变）；全局命名注册表、全局 CLAUDE.md 缩写约定、xhqing 主页名册与采集脚本由 Kit 会话统一更新（CapabilityManagerAgent 镜像同步亦已提交）。
- **改了什么**（2026-10-17）：① `README.md` / `README_cn.md`——标题、正文 persona 称呼、logo alt、工作流图 alt 全部更名为 Mark；② `CLAUDE.md`（`AGENTS.md` 为其软链）——「Markowitz（马科维茨）」→「Mark（马克）」，正文全部现行指代同步；③ `.claude/skills/quant/SKILL.md` 与 `README.md`——核心 skill 描述与正文更名；④ `assets/logo.svg`——图内文字与 aria-label 更名；⑤ `assets/markowitz_workflow.svg` → `assets/workflow.svg`（文件名去掉旧名、按简单英文名规范改为 `workflow.svg`，图内文字与 aria-label 同步更名；README 引用同步）；⑥ `.commit-cache.md` 拟人名记录同步。历史条目保持原样不改。三个子项目（Intraday / Swing / gridtrader）现行引用同批更名（见各仓库 CHANGELOG）。

## [0.1.1] - 2026-10-17

### 变更（2026-10-17 新规：Agent 进驻子项目——新增 AGENTS.md 软链、子项目挂指回软链）

- **为什么改**（2026-10-17 用户决定）：全局规则修订「智能体专属文件归属与子项目映射（2026-10-17 修订）」，取代 2026-10-12 的「子项目一律不得存在」：智能体专属文件（`CLAUDE.md` / `AGENTS.md` / `.claude/` / 项目级 `.pi/`）的**实体只在 Agent 项目**；每个子项目根目录挂两条指回负责 Agent 的软链接（`AGENTS.md → ../<Agent项目>/AGENTS.md`、`.pi → ../<Agent项目>/.pi`，随仓库提交），在子项目目录开会话即继承负责 Agent 的身份 / 规则 / 技能 / 扩展 / 设置（「Agent 进驻子项目」）；后台开启 / 唤醒会话仍只能以 Agent 项目根目录为工作目录。
- **改了什么**（2026-10-17）：① 本项目新增 `AGENTS.md` 软链接（→ `CLAUDE.md`，与其它工具的项目指令约定名对齐）；② `CLAUDE.md`「子项目清单」节措辞更新为 2026-10-17 版（子项目不再携带实体副本、改挂两条指回本项目的软链接；quant skill 权威源仍只在本项目、不向子项目分发副本；后台会话仍只以本项目根为工作目录）；③ 三个子项目（Intraday / Swing / gridtrader）各新增 `AGENTS.md`、`.pi` 两条软链接（随各子项目仓库提交，见其 CHANGELOG）。

### 变更（「子项目清单」节改按 2026-10-12 新规表述：子项目不再携带智能体专属文件）

- **为什么改**（2026-10-12）：全局规则修订「智能体专属文件只属于 Agent 项目」（`~/.claude/docs/agents-registry.md`「智能体专属文件归属与子项目映射（2026-10-12 修订）」，取代原「子项目 `.claude/` 超集」规则）：`CLAUDE.md`、`AGENTS.md`、`.claude/`、项目级 `.pi/` 等只有 Agent 项目允许拥有，子项目一律不得存在；后台会话只能以 Agent 项目根目录为工作目录。
- **改了什么**（2026-10-12）：`CLAUDE.md`「子项目清单」节改写——删「`.claude/` 超集」「本文件全文随附」表述，改为新规口径（子项目不再携带智能体专属文件；quant skill 权威源只在本项目；后台会话只以本项目根目录为工作目录；引用全局 agents-registry「子项目映射」表）。三个子项目侧的遗留文件同一批清理完成（各子项目 CHANGELOG 有记）。

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
