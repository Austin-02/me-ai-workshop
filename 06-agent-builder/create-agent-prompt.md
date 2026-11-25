# WP05-01 创建并发布企业经营分析 Agent

版本：2026-11-25 用户确认；未实测。WP04 Skill：me-ecommerce-analysis v0.1.0；部署前核验实际安装版本。下方是学员在 Anti 输入的完整指令，与统一手册同步。

请读取我在 Antigravity 中已安装的 ME 电商经营分析 Skill（me-ecommerce-analysis），参考其 GitHub 源码及分析规则：

https://github.com/Austin-02/me-ai-workshop/tree/main/05-data-analysis/me-ecommerce-analysis

自动识别 BigQuery 中已导入的 Shopify 模拟经营数据，包括 products、customers、orders、order_items、inventory 五张表。

使用 Google Agents CLI 和 ADK，将该 Skill 的经营分析能力封装为一个名为「ME 电商经营分析助手」的 Agent，并配置 BigQuery 只读查询能力，完成实际数据分析测试。

请自动识别当前 Google Cloud Project 及可用的 Gemini Enterprise 应用，向我确认部署目标及计划后，将 Agent 部署至 Agent Runtime 并注册到 Gemini Enterprise。

完成后验证 Agent 的数据查询能力、GE 注册状态及用户访问权限，并提供 GE 访问入口。

如存在权限不足、环境缺失、数据集或应用无法识别等问题，请提示我处理，不得虚构执行结果。
