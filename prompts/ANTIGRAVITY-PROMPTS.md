# Antigravity 实战完整 Prompt 手册

版本 V0.1 · 2026-11-25 · 内部研发草案（并非正式发布或通过实测）。
按照 WP01 → WP02 → WP03 → WP04 → WP05 顺序，学员只在 Antigravity 中粘贴对应指令。每项通过实际运行核验后再进入依赖该项的任务；Shopify MCP 独立于 BQ 分析链路。

## WP01｜安装与体验 ME PPT Skill

状态：指定来源已确认，指令已归档，待 Antigravity 实测和外部使用授权。

### WP01-01 安装指令

请安装或更新 aidea-ppt-me，来源 https://github.com/stephenzzf/skill_aidea-ppt-mebrand，main 分支。已审查 commit 为 ef7ba76c015b3e40b5b92102a394f0790ee250ad（v0.3.0）。优先使用该已审查版本；若 main 已更新，先报告差异及版本，不自行切换到未经审查的版本。

1. 先定位当前 Antigravity 实际使用的 Skill 目录和发现机制，确认其支持此 Skill 的格式；不要照搬 Codex 路径。
2. 阅读上游 README.md、SKILL.md、package.json、锁文件及依赖、安装和检查脚本；检查本地是否已有安装及自定义修改。
3. 首次安装在确认目录和权限后获取指定版本，按上游说明安装必要依赖（Git、Node.js >=20.9、Python 3，npm ci 及 Python 虚拟环境/requirements）；额外工具按实际任务需要检查，系统级安装先获得确认。
4. 现有 Git 安装先检查来源、工作区、分支；仅允许符合已审查版本的快进更新；源码同步先比较差异，将自定义内容备份至技能发现目录之外，再经确认同步。不得强制覆盖或重置本地修改。
5. 运行 npm run check（按需要设置 AIDEA_PPT_PYTHON），如实记录退出状态及缺失依赖；验证 Antigravity 发现与调用方式。
6. 报告实际安装路径、版本/commit、依赖、检查结果、Skill 识别情况、重载方法、未解决问题。遇到兼容性或权限问题先停止并说明，不能虚构执行成功。

安装检查不等于 PPT 生成测试；任何新 PPT（含测试稿）须先由学员确认逐页方案。

### WP01-02 生成测试稿

请调用已安装的 aidea-ppt-me，为 ME Workshop 生成 2–3 页可编辑的 16:9 PPTX 测试稿，受众为企业管理层，视觉风格标准 ME2026，使用模式 talk。

素材：虚拟服装零售商希望了解销售趋势和商品库存问题；本测试不使用真实经营数据，不编造指标。请先给出逐页内容与配图/原生对象计划（可不配图），等待我明确确认后再生成。确认后完成输出及 QA，报告文件路径、页数、可编辑性检查、字体/渲染问题和未执行的检查项。若无法调用 Skill 或运行环境不完整，直接报告原因。

## WP02｜Shopify MCP 查询体验

状态：内部候选方案/草案；最终本地或远程连接方式、确切版本、配置格式、授权和实测待确认。不向学员分发 ME 店铺后台凭证；以下安装指令仅供内部评估，正式学员接入指令须随部署方式确定后更新。

### WP02-01 内部安装与连接（草案）

请在当前 Google Antigravity 环境中连接 Shopify MCP，目标店铺为 meet-wear-me。

内部候选源码：https://github.com/mgcrea/mcp-shopify

1. 阅读仓库 README、版本、依赖、运行脚本及权限说明，确认当前 Antigravity MCP 配置位置和支持协议。
2. 检查现有配置并备份，保留其他 MCP；确认安全配置方案和拟安装版本，经我确认后执行。
3. 只使用获批的商品读取权限；若授权或安全凭证配置不足，停止并报告，不在聊天输出、日志或 GitHub 中暴露密钥。
4. 重新加载 MCP，确认连接和实际可用工具。不得修改商品、订单、库存、客户或其他店铺数据。
5. 报告真实连接状态、版本、工具清单及未解决问题；不能以网页或模拟数据代替 MCP 实际结果。

### WP02-02 查询体验（草案）

请使用已连接的 Shopify MCP，查询 meet-wear-me 店铺中 5 件在售商品的名称、价格、币种，并从中选择一件查询其实际商品变体、颜色和尺码（如店铺存在相关选项）。输出工具调用结果及来源；如字段缺失、工具不可用或连接失败，明确报告，不编造数据。仅查询，不执行任何写入。

## WP03｜将虚拟经营数据导入 BigQuery

状态：数据包及文件级校验已完成；入仓指令为草案，BigQuery 实际执行尚未验证。当前 URL 为内部私有研发仓库，正式活动需学员有读取权限或替换为获批的数据包地址。

### WP03-01 数据导入指令（简化版，待实测）

请将以下 GitHub 目录中的 Shopify 模拟经营数据导入 BigQuery：

https://github.com/Austin-02/me-ai-workshop/tree/main/03-demo-data

请自动读取 README.md、data_dictionary.md 和五张 CSV，按照数据字典设计表结构、处理空值，并使用 variant_id 关联商品变体。

先检查 BigQuery 连接、目标 Project、Dataset、区域及权限，给出导入计划，经我确认后再建表并导入。权限不足或目标表已存在时停止，不覆盖或追加已有数据。

导入完成后，使用实际 SQL 按 README 中的入仓基准核对记录数、销量、销售额、折扣、主键及关联完整性、订单金额和库存平衡。输出五张表的完整名称、实际校验结果、SQL、Job ID 和异常情况。

仅处理教学数据，不修改 Shopify 或泄露凭证。校验不通过时明确说明原因，不进入下一环节，不得编造执行结果。


## WP04｜安装电商经营分析 Skill

状态：V0.1.0 轻量 Skill 与安装指令已归档，依赖 WP03 已验证的教学数据集；Anti 识别及真实 BQ 查询待实测。

### WP04-01 安装并测试经营分析 Skill

请在当前 Antigravity 工作空间安装 ME 电商经营分析 Skill（`me-ecommerce-analysis`）。

https://github.com/Austin-02/me-ai-workshop/tree/main/05-data-analysis/me-ecommerce-analysis

读取并检查 SKILL.md 及 references，按当前 Anti 实际支持的 Skills 目录安装完整技能，保留已有自定义内容。确认 Skill 可被识别，自动发现上一环节已导入且获授权的 BigQuery 教学数据集，并使用实际只读查询测试经营概览。输出 Skill 安装路径、版本、数据集、实际查询结果和异常；若存在多个匹配数据集请让我确认，不得编造数据或扩大权限。

## WP05｜在 Anti 创建 Agent，发布至 GE 并输出 PPT

状态：WP05-01 指令已确认并归档，尚未实测。依赖 WP03 入仓验收、WP04 分析 Skill、获授权的 Agent 部署环境与 GE 应用。WP04 Skill ID 为 me-ecommerce-analysis（V0.1.0），实际安装及部署仍待验证。所有研发/部署 Prompt 输入 Anti；发布后学员在 GE 中实际体验自己创建的经营分析 Agent。会前权限准备见 [技术清单](../06-agent-builder/authorization-checklist.md)。

### WP05-01 创建并发布企业经营分析 Agent（已确认，待实测）

请读取我在 Antigravity 中已安装的 ME 电商经营分析 Skill（me-ecommerce-analysis），参考其 GitHub 源码及分析规则：

https://github.com/Austin-02/me-ai-workshop/tree/main/05-data-analysis/me-ecommerce-analysis

自动识别 BigQuery 中已导入的 Shopify 模拟经营数据，包括 products、customers、orders、order_items、inventory 五张表。

使用 Google Agents CLI 和 ADK，将该 Skill 的经营分析能力封装为一个名为「ME 电商经营分析助手」的 Agent，并配置 BigQuery 只读查询能力，完成实际数据分析测试。

请自动识别当前 Google Cloud Project 及可用的 Gemini Enterprise 应用，向我确认部署目标及计划后，将 Agent 部署至 Agent Runtime 并注册到 Gemini Enterprise。

完成后验证 Agent 的数据查询能力、GE 注册状态及用户访问权限，并提供 GE 访问入口。

如存在权限不足、环境缺失、数据集或应用无法识别等问题，请提示我处理，不得虚构执行结果。

### WP05-02 经营分析报告 PPT 指令（已归档，待联调）

请调用 aidea-ppt-me，将我从 Gemini Enterprise 中自建经营分析 Agent 获取、并复制到当前 Antigravity 对话中的 BigQuery 实际查询分析结果制作成 5–8 页可编辑 PPTX（建议标准 ME2026 / report）。面向企业管理层；仅使用已经查询核验的数据，注明模拟数据、期间、币种、指标口径、SQL 或结果来源。建议包含经营概况、趋势、商品、客户/库存发现、待验证假设和行动建议。不得编造转化率、ROAS、利润或未经验证的因果结论。先提交逐页内容、数据图表和配图计划，等待我明确确认后再生成并进行 QA。若当前上下文缺少分析结果，先要求提供，不用假数据替代。

---

维护说明：本手册汇总已归档指令及明确标识的草案；不是学员发布版本。禁止使用占位项冒充可运行指令。完成后统一同步模块原文、版本和测试状态。
