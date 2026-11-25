# WP05｜经营分析 Agent 发布与体验

对应实战 T5 的 Agent 构建/发布及 PPT 输出；不包含独立的 GE 企业 AI 场景建议 Agent。

- [WP05-01 创建并发布 Agent（已确认指令）](create-agent-prompt.md)
- [会前授权及实际验收清单](authorization-checklist.md)
- [统一学员 Prompt 手册](../prompts/ANTIGRAVITY-PROMPTS.md)

流程：在 Anti 输入一个 Prompt → 读取 WP04 已安装 Skill me-ecommerce-analysis 及完整 references，封装为运行时可用规则 → 识别 WP03 BigQuery 教学 Dataset → 通过 ADK/Google Agents CLI 封装、实际查询测试 → 确认项目/应用/区域/部署计划 → 部署 Google Cloud Agent Runtime → 注册 GE → 以获授权的企业用户身份验证 GE 可访问及真实查询 → 将 GE 分析结果复制到 Anti 调用 PPT Skill。

Skill 不等于 GE Agent；必须验证封装、运行时部署、注册、最终用户访问四个环节。不得以本地测试代替 GE 体验。

状态：指令已归档；WP04 已明确 Skill ID 为 me-ecommerce-analysis v0.1.0；实际安装、部署方式、IAM、费用及外部用户测试均待验证。不会配置账号或替用户开通环境。
