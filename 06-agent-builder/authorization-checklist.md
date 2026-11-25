# WP05 会前权限准备与验收

供 ME / Google 技术负责人参考；不是学员 Prompt。以下为建议权限框架，未做真实环境验证，需以实际 Agents CLI、ADK、Agent Runtime、GE 版本与组织策略校验。无需在现场让学员自行研究 IAM。

## 身份及权限

| 身份 | 建议权限 / 条件 | 范围 |
|---|---|---|
| 学员部署身份 | 已授权 Google Cloud 身份；roles/aiplatform.user 或部署所需最小角色 | 获批教学项目及 Agent 部署资源 |
| GE 注册身份 | 按官方发布流程确认所需最小权限；roles/discoveryengine.agentspaceAdmin 为候选管理角色，不默认授予每位学员 | 仅目标 GE 应用；必要时由管理员预授权或批准发布 |
| Agent 运行身份 | roles/bigquery.dataViewer | 仅对应企业的教学 Dataset |
| Agent 运行身份 | roles/bigquery.jobUser | 获批执行查询的项目 |
| GE 使用者 | roles/discoveryengine.agentspaceUser（按实际配置核对）及应用/Agent 访问授权 | 对应企业可使用范围 |

服务账号使用（actAs）、API 启用、运行环境身份及计费、组织限制等按实际部署机制确认；不得自行扩大权限或泄露凭证。不要把部署者凭证交给 Agent。若仅管理员可注册，提前设计受控注册流程并实测；不能把管理员成功当作学员成功。

## 环境发现及审批

- [ ] Anti 可识别当前有效身份、项目、区域及已安装 WP04 Skill me-ecommerce-analysis；Skill 路径、来源和版本可核验。
- [ ] WP03 五表完整、实际入仓校验通过；同名数据集有多个时明确选择，禁止默认选择他人数据集。
- [ ] ADK/Agents CLI 版本及官方命令可用；所需 API、运行环境、配额及计费已预备。
- [ ] 能列出可访问 GE 应用；一个则提出建议供确认，多个则让学员选择，无可用应用则阻塞。不得擅自新建 GE 应用或变更 IAM。
- [ ] 部署前确认目标项目、应用、区域、Agent 名称/标识、运行身份、数据范围及可能费用。重复名称或已有资源不得擅自覆盖。
- [ ] 企业之间的数据集和 Agent 使用范围隔离，至少使用一个真实外部学员身份完成验证。

## 实际验收（全部待测试）

- [ ] 将 Skill 及 references 作为 Agent 运行时可用规则/知识封装，显式绑定授权教学 Dataset，不依赖本机路径或用户凭证。
- [ ] Agent 本地功能测试实际调用 BQ；结果与数据字典及 README 校验基准一致。
- [ ] Runtime 实际部署成功，记录资源 ID、版本、部署状态及实际执行证据。
- [ ] GE 注册成功，核验实际应用与 Agent 状态；返回入口。
- [ ] 企业学员身份登录 GE 找到并调用自己创建的 Agent，实际查询 BQ 并核对结果；不可用项单独记录。
- [ ] 其他企业账号不能修改或访问未获授权的 Agent/数据集。
- [ ] GE 返回的分析结果可复制回 Anti，由已验证的 PPT Skill 生成可编辑 PPTX。
- [ ] 准备经批准的备用 Agent/数据访问路径与清理策略，记录费用责任和失败处理。

参考官方教程：https://codelabs.developers.google.com/enterprise-cloud-scale-deploying-the-expense-agent-to-agent-runtime-on-google-cloud

本清单只描述前置条件与验收，不代表已配置账号、已授权或已部署。
