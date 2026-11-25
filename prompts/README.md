# Antigravity 实战 Prompt 中心

**统一跟练的研发、安装及部署 Prompt 输入入口：Google Antigravity（Anti）。** WP05 发布完成后，学员在 Gemini Enterprise（GE）中体验自己创建的 Agent；PPT 制作回到 Anti。 本目录集中维护 WP01–WP05 的学员可复制完整指令，不混入账号配置、GE 场景推荐 Agent、讲师答案或凭证。必要的登录、授权和审批仍由学员/管理员完成。

## 使用入口

[打开完整 Prompt 手册（按实战顺序）](ANTIGRAVITY-PROMPTS.md)

| 工作包 | 学员动作 | 当前状态 | 技术资源 |
|---|---|---|---|
| WP01 | 安装 PPT Skill、生成测试 PPT | 指令已归档，未实测 | [PPT Skill](../01-ppt-skill/README.md) |
| WP02 | 配置 Shopify MCP、只读查询商品 | 指令草案，最终连接方案待选，未实测 | [Shopify MCP](../02-shopify-mcp/README.md) |
| WP03 | 从 GitHub 读取模拟数据，导入 BQ | 指令草案，数据已准备，BQ 未实测 | [数据包](../03-demo-data/README.md) · [BQ](../04-bigquery/README.md) |
| WP04 | 安装经营分析 Skill | V0.1.0 Skill 与安装指令已归档，待实测 | [分析 Skill](../05-data-analysis/README.md) |
| WP05 | Anti 创建并部署 Agent、注册至 GE；GE 体验；回 Anti 生成 PPT | WP05-01/02 指令已归档，依赖 WP03/04 及授权，待实测 | [Agent](../06-agent-builder/README.md) |

## 维护规则

- 模块目录保留技术说明、源码引用、配置模板、脚本及测试证据；已存在的模块 Prompt 为技术来源，集中手册是统一学员版，更新源指令时同次提交同步手册并检查一致性。新增学员 Prompt 必须进入集中手册；不要要求学员逐个寻找模块文件。
- 仅标记真实完成的状态：草案 → 已归档/待实测 → 实测通过 → 发布冻结。归档不等于安装或运行成功。
- 2026-12-18 验收冻结前统一核对实际平台版本、权限、输入输出、失败处理和学员访问；冻结 Git commit 或 release。
- 当前是内部 Private 研发仓库。对外分发前另行审核并准备可访问的学员版；严禁公开凭证、真实客户数据、私有知识资产、未经授权的品牌素材。
