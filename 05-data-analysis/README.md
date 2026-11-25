# WP04｜ME 电商经营分析 Skill

Skill ID：`me-ecommerce-analysis` · V0.1.0 · 内部跟练 MVP，未实测。

- [SKILL.md](me-ecommerce-analysis/SKILL.md)
- [安装 Prompt](install-prompt.md)
- [查询与分析指南](me-ecommerce-analysis/references/analysis-guide.md)
- [数据字典](me-ecommerce-analysis/references/data_dictionary.md)（与 03-demo-data/data_dictionary.md 当前版本一致；数据变更时同步）

覆盖日常经营概览、销售趋势、商品、客户、折扣、库存。以授权 BigQuery 数据进行只读查询、事实/假设区分及行动建议；不另建应用、不配置账号，不额外安装运行依赖。本 Skill 为标准目录式知识资产，实际查询依赖 WP03 已准备的 BQ 连接。WP05 将规则和引用文件封装进 GE Agent 并绑定运行身份、指定数据集与查询工具。

参考 Anti 官方 Skill 规范：https://antigravity.google/docs/skills 。工作空间优先使用 .agents/skills/me-ecommerce-analysis（以实际环境发现机制为准）。

状态：源码、指标口径、SQL 模板及安装 Prompt 已准备；安装识别、真实 SQL、GE 部署均待测试。内部教学授权/对外分发范围待确认。
