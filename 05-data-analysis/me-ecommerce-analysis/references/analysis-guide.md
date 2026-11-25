# 日常经营分析指南（V0.1）

此文档为 me-ecommerce-analysis 的轻量方法与示例。所有表名占位须替换成已确认的授权教学表，字段以 data_dictionary.md 为准。以下 SQL 为 BigQuery Standard SQL 查询模板，未实际执行；不可硬编码预期结果。对用户选择的范围使用参数或经过验证的日期。

## 查询模板

### 经营概况与月趋势（订单粒度，销量另算）

```sql
SELECT DATE_TRUNC(order_date, MONTH) AS month,
 COUNT(*) AS orders, COUNT(DISTINCT customer_id) AS buyers,
 SUM(net_sales) AS net_sales, SAFE_DIVIDE(SUM(net_sales), COUNT(*)) AS aov,
 SAFE_DIVIDE(SUM(discount_amount), SUM(gross_sales)) AS discount_rate
FROM `PROJECT.DATASET.orders`
GROUP BY month ORDER BY month
```

销量按订单日期关联明细后 SUM(quantity)，单独按月聚合，再与月度订单结果连接。同比需有去年同期；环比用上月可比期间。若要解释净销售额变化，可用 (本期订单数-上期订单数)×上期客单价 + 本期订单数×(本期客单价-上期客单价)，用未四舍五入数计算。

### 商品贡献（明细粒度）

```sql
SELECT p.product_id, ANY_VALUE(p.product_title) AS title,
 SUM(i.quantity) AS units, SUM(i.line_net) AS net_sales
FROM `PROJECT.DATASET.order_items` i
JOIN `PROJECT.DATASET.products` p USING(variant_id)
JOIN `PROJECT.DATASET.orders` o USING(order_id)
GROUP BY p.product_id ORDER BY net_sales DESC
```

增加日期过滤可比较期间。品类可使用 shopify_category，空分类单列；按变体分析用 variant_id。销售贡献分母为同窗口总明细净额；头部商品占比不能混用不同时间范围。目录未纳入演示货盘的商品不纳入滞销判断。

### 全期复购与月度回购

```sql
WITH buyer_orders AS (
 SELECT customer_id, COUNT(*) AS n FROM `PROJECT.DATASET.orders` GROUP BY customer_id
)
SELECT COUNT(*) AS buyers, COUNTIF(n >= 2) AS repeat_buyers,
 SAFE_DIVIDE(COUNTIF(n >= 2), COUNT(*)) AS repeat_buyer_rate
FROM buyer_orders
```

```sql
SELECT DATE_TRUNC(o.order_date, MONTH) AS month,
 COUNTIF(o.order_date > c.first_order_date) AS repeat_orders,
 COUNT(DISTINCT IF(o.order_date > c.first_order_date, o.customer_id, NULL)) AS repeat_buyers,
 COUNT(DISTINCT IF(DATE_TRUNC(c.first_order_date, MONTH) = DATE_TRUNC(o.order_date, MONTH), o.customer_id, NULL)) AS new_buyers,
 SAFE_DIVIDE(COUNTIF(o.order_date > c.first_order_date), COUNT(*)) AS repeat_order_share,
 SUM(IF(o.order_date = c.first_order_date, o.net_sales, 0)) AS first_order_sales,
 SUM(IF(o.order_date > c.first_order_date, o.net_sales, 0)) AS repeat_order_sales
FROM `PROJECT.DATASET.orders` o
JOIN `PROJECT.DATASET.customers` c USING(customer_id)
GROUP BY month ORDER BY month
```

复购买家和新买家同月可重叠，不直接相加当作去重买家。首次获客渠道是模拟首购渠道，不等于订单级归因；无成本不能评估投放效率。

### 折扣对比

```sql
SELECT DATE_TRUNC(order_date, MONTH) AS month, discount_amount > 0 AS discounted,
 COUNT(*) AS orders, SUM(net_sales) AS net_sales,
 SAFE_DIVIDE(SUM(net_sales), COUNT(*)) AS aov,
 SAFE_DIVIDE(SUM(discount_amount), SUM(gross_sales)) AS discount_rate
FROM `PROJECT.DATASET.orders`
GROUP BY month, discounted ORDER BY month, discounted
```

可比较促销前后同长度窗口，但观察性差异不是因果；无营销日历不识别真实促销活动，无成本不计算利润影响。

### 库存异常

```sql
WITH monthly_sales AS (
 SELECT LAST_DAY(o.order_date) AS month_end, i.variant_id, SUM(i.quantity) AS units
 FROM `PROJECT.DATASET.order_items` i
 JOIN `PROJECT.DATASET.orders` o USING(order_id)
 GROUP BY month_end, i.variant_id
)
SELECT inv.month_end, inv.variant_id, p.product_id, p.product_title,
 inv.opening_qty, inv.received_qty, inv.sold_qty, inv.closing_qty,
 COALESCE(s.units,0) AS order_units,
 inv.opening_qty=0 AND inv.received_qty=0 AND inv.closing_qty=0 AS no_supply_full_month,
 inv.closing_qty=0 AS zero_month_end
FROM `PROJECT.DATASET.inventory` inv
JOIN `PROJECT.DATASET.products` p USING(variant_id)
LEFT JOIN monthly_sales s USING(month_end,variant_id)
ORDER BY inv.month_end,inv.variant_id
```

月末为零只提示风险；整月无可售货量且销售归零、随后恢复供货及销量，是支持缺货解释的证据，不是损失销量的估计。检查库存数量平衡和明细销量一致性。

## 日常输出示例结构

问题：最近一个完整月经营怎么样？先实际查询最新日期、最近完整月和上一月，输出销售、订单、AOV、销量、购买人数、折扣率；如有异常再钻取商品/客户/库存；给出 2–3 条行动建议。不得在示例里预设结论或金额。

建议跟练问题：全期经营概况；近月环比；Top 5 商品及贡献；新老订单贡献与复购；折扣期间表现；库存无供货异常；综合经营简报。真实 SQL 校验及 GE 体验均待测试。
