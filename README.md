# me-ai-workshop

ME Enterprise AI Workshop Toolkit — Skills, MCP, BigQuery, AI Agents and hands-on learning resources for cross-border e-commerce businesses.

出海企业 AI 转型实战工作坊 · A1 初始化骨架

状态：**结构草案，未部署、未安装、未联调。** 研发验收截止 2026-12-18，活动 2026-12-24（深圳）；当天具体议程待定。

## 实战路径

- Build an Agent：安装 Antigravity → 安装 PPT Skill → Shopify MCP 只读查询体验 → 模拟历史数据导入 BigQuery → 安装经营分析 Skill → 在 Anti 创建并部署 BQ 经营分析 Agent、注册至 GE → 学员在 GE 体验 → 将分析结果带回 Anti 生成 PPT。
- Use an Enterprise Agent：访问 ME 预置 GE Agent → 输入企业背景及痛点 → Top 3 场景 → 选择一个 → 专项方案 → 脱敏分享与专家初步建议。

MCP 查询体验独立，不强制分析 Agent 调用 Shopify MCP。交互 Demo 即 GE Agent。

## Antigravity 统一实战 Prompt 入口

[**WP01–WP05 完整 Prompt 手册**](prompts/ANTIGRAVITY-PROMPTS.md) · [状态与维护说明](prompts/README.md)

统一跟练的研发、安装及部署 Prompt 均在 Antigravity 中输入；WP05 发布后在 GE 中体验自建经营分析 Agent。模块研发资料与学员指令分开管理；草案及待研发项清晰标注，真实平台通过后再冻结发布。

## 目录索引

- [Antigravity 环境](00-environment/README.md) — 安装入口、系统兼容性、账号授权和环境检查。
- [PPT Skill](01-ppt-skill/README.md) — 已审核的 GitHub 来源、固定版本、安装 Prompt 和 PPTX 验收。
- [Shopify MCP](02-shopify-mcp/README.md) — 只读查询连接、配置模板、模拟订单状态查询；不要求接入分析 Agent。
- [模拟历史数据](03-demo-data/README.md) — 脱敏或合成历史数据、数据字典、预设经营问题与数据来源说明。
- [BigQuery 入仓与查询](04-bigquery/README.md) — 数据导入 Prompt/脚本、表结构、记录数校验及 BQ 查询连接。
- [已有数据分析 Skill](05-data-analysis/README.md) — 原始来源、授权、安装说明、BQ 适配及指标口径；不重复研发。
- [数据分析 Agent](06-agent-builder/README.md) — 基于 BQ 创建、部署及注册 GE 分析 Agent 的 Prompt、会前权限准备与实际体验验收。
- [GE Agent 体验](07-enterprise-agent/README.md) — 仅放学员访问指引、对话任务、Top 3 场景及专项方案输出说明；交互 Demo 即 GE Agent。
- [测试与验收](08-testing/README.md) — 测试用例、预期结果、环境/版本、结果记录、备用方案。标准答案发布前审查。
- [学员与讲师指引](09-workshop-guide/README.md) — 按任务顺序组织操作；具体时刻和时长待确认；分享脱敏、专家初步反馈。

## 发布边界

研发仓库默认 Private。学员分发采用独立净化仓库或经审查的发布包；不通过同仓库不同分支隔离私有知识。不得提交密钥、令牌、真实客户数据、私有 GE Prompt/知识库。第三方资源须记录来源、版本及许可，审核后再分发。

## A1 验收

- [x] 规划目录与模块边界
- [x] 初始化 README、资源登记模板和安全约定
- [x] 创建 GitHub 空仓库并授权
- [x] 提交骨架并回读确认目录（2026-11-25）

A2 之后补充真实资源与测试记录。
