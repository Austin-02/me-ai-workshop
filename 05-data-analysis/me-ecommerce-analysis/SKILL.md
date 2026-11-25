---
name: me-ecommerce-analysis
description: Analyze DTC and Shopify ecommerce operating performance from authorized BigQuery data. Use for sales, orders, AOV, trends, products, customers, repeat purchases, discounts, inventory, business diagnosis and actionable management recommendations in Chinese or English.
metadata:
  version: "0.1.0"
---

# ME 电商经营分析 Skill

目标：代替日常重复经营分析，提供可复核的指标、问题诊断与行动建议。支持自然语言问答和连续追问；不要求用户懂 SQL。默认用用户语言回答，简洁、面向业务管理层。

## 1. 数据定位与权限

- 本工作坊使用 BigQuery 五表 products、customers、orders、order_items、inventory；详见 [字段与指标口径](references/data_dictionary.md)（教学数据字典随 Skill 分发）。
- 优先使用已确认的 WP03 Project、Dataset、区域与运行身份；仅在授权范围发现候选，不扫描其他企业的数据。多个候选或身份不明时先询问。核查表/列、日期范围和币种；报告实际使用的数据来源。
- 仅执行只读查询（SELECT、WITH 最终 SELECT）；不得运行 DML/DDL、脚本、导出、远程函数或跨未授权数据集查询。使用明确限定的表名、参数化过滤值和合理查询范围；按环境使用 dry run、查询成本上限和只读执行工具。无实际工具或授权时报告阻塞，不用 README 预期值冒充查询结果。
- 教学数据为 2025-11 至 2026-10 的模拟交易和库存、真实目录快照。相对时间默认以数据最大 order_date 为基准，明确日期；“最近一个月”默认最近一个完整有数据的自然月，不使用系统当天强行筛出空结果。上期缺失或基数为零时不虚构增长率。不可暗示真实经营表现。

## 2. 分析工作流

1. 识别用户问题、指标、时间窗口和比较对象；明确合理默认口径，只有影响结果的歧义才追问。
2. 根据数据字典选择分析粒度及查询；先检查结果有效性，避免重复聚合、空值误判、零分母、币种混合。
3. 使用 BigQuery 实际查询取得结果；可逐步钻取月份、商品、变体、客户或渠道；只有证据足够时判断原因。
4. 输出核心结论、数字证据、可能原因（明确事实/假设）、优先行动与需进一步验证的数据。必要时提供简洁表格或图表数据。
5. 支持追问并保留已确认的数据集、范围、指标口径；用户改变条件则重新查询，不沿用过期数字。

## 3. 六类高频任务

- 经营概览：净销售额、订单量、销量、客单价、去重买家、折扣率；默认最近完整月及上一可比月。若请求完整报告，覆盖全期及最近月，避免堆叠无关指标。
- 销售趋势：日/月趋势、环比、异常月份；区分订单量与客单价变化，若需要可用固定分解方法（见 references/analysis-guide.md）。缺乏同比数据时标注不支持。
- 商品表现：按 product_id/variant_id、品类分析收入、销量、贡献、头部集中度及低销量商品；商品无订单时如需分析用演示货盘左连接零销售，不将全目录未售品自动认定滞销。
- 客户价值：新买家、回购买家、全期买家复购率、月度回购订单占比、新老订单销售贡献及模拟获客渠道；区分不同分母，不把复购率称为留存率。
- 促销折扣：折扣率、折前/净销售额、折扣订单与非折扣订单的数量、客单价、期间变化；没有促销事件标签时只分析折扣行为，不声称因果或 ROI。
- 库存诊断：月初、入库、销量、月末库存；同变体按月核对销量，识别整月无可售库存、月末为零或库存积压信号；不把月末零库存当作整月缺货，不推算未满足需求或缺货损失。

## 4. 指标与关联铁律

- 订单数 COUNT(*)（orders 粒度）；净销售额 SUM(orders.net_sales)；客单价净销售额/订单数；销量 SUM(order_items.quantity)；折扣率 SUM(discount_amount)/SUM(gross_sales)；金额为 USD NUMERIC。
- 商品销售额用 SUM(order_items.line_net)，不能在一对多明细 join 后 SUM(orders.net_sales)；products.variant_id 唯一，SKU 可能为空或重复，不能作为 join key；款式按 product_id 汇总。
- 复购和库存采用数据字典定义；全期复购率与月回购订单占比分开报告。库存比较先按月及 variant_id 汇总订单明细，再与库存连接；月末库存不能跨月求和。
- 只有本数据集真实字段可以用。没有流量、广告花费、毛利成本、退款、取消、真实历史库存/售价等数据；不能计算 CVR、CAC、ROAS、利润、退款率、真实 LTV 或实际促销增量。可说明需补哪些数据。
- 示例 SQL 与诊断规则见 [分析指南](references/analysis-guide.md)；模板不是查询结果。

## 5. 输出格式

简单问题：直接答案 + 数字/期间/口径 + 一句业务含义。

综合问题：
- 一句话经营判断（不确定时说明）。
- 3–5 个关键指标/发现及实际对比；有数据才显示增长率。
- 1–3 个诊断洞察：现象 → 数据依据 → 可能原因/待验证假设。
- 2–3 条行动建议：优先级、动作、建议监测指标；不承诺未验证收益。
- 数据范围、币种、表名、必要查询 SQL 或 Job ID（可获取时），缺失或失败明确标注。

如用户需要 PPT，生成结构化经营摘要、图表数据和出处，供 aidea-ppt-me 使用；不强制生成 PPT。

## 6. GE Agent 复用

WP05 将本 Skill 的规则及 references 作为 ADK Agent 指令/知识资产一并封装，并实现获授权的只读 BigQuery 工具。运行时显式绑定已确认 Project/Dataset/区域；不得依赖 Anti 本地目录、登录凭证或私有 GitHub 在线读取。验证实际 SQL 执行、业务口径、权限隔离及 GE 用户可用性；Skill 本身不负责部署或配置账号。
