# 字段与指标口径

五份 UTF-8 CSV 均含一行英文表头，逗号分隔。下表类型为 BigQuery 目标类型。金额用 NUMERIC 保留两位小数，日期采用 YYYY-MM-DD。标为「可空」的字段将空单元格加载为 NULL，其余字段必填。商品编号、买家编号和订单编号均为教学数据集专用字符串。

## products

每个变体一行，以 variant_id 为主键。product_id 可跨变体重复，同一 handle 对应一个 product_id。除教学编号和货盘标记外，商品属性取自 2026-11-25 的 Shopify 快照。

| 字段 | 类型 | 含义 |
|---|---|---|
| product_id | STRING | 教学商品编号，按款式汇总使用 |
| variant_id | STRING | 教学变体编号，订单明细与库存共用的关联键 |
| handle | STRING | Shopify 商品 handle |
| product_title | STRING | 后台导出的原始商品名称，保留源字符 |
| shopify_category | STRING | 后台商品分类路径，可空 |
| product_type | STRING | 商家维护的产品类型，可空 |
| product_status | STRING | 快照中的商品状态，如 active、unlisted |
| published | BOOL | Shopify 导出列 Published 的布尔值 |
| sku | STRING | Shopify 的 Variant SKU，可空，可重复 |
| option1_name、option2_name、option3_name | STRING | 三个独立列，各选项的名称，未设定时可空 |
| option1_value、option2_value、option3_value | STRING | 三个独立列，同序号选项对应的变体原始值，未设定时可空 |
| catalog_price | NUMERIC | 当日基础币种目录价，允许零价 |
| currency | STRING | 店铺基础币种 USD |
| demo_assortment | BOOL | 是否纳入合成交易和库存的演示货盘，筛选规则见 README |

## customers

每位合成买家一行，以 customer_id 为主键。样本只包含在模拟期内至少下过一单的买家。

| 字段 | 类型 | 含义 |
|---|---|---|
| customer_id | STRING | 虚拟买家编号 |
| first_order_date | DATE | 此买家在模拟经营记录中的首单日期 |
| acquisition_channel | STRING | 模拟首次获客渠道，取 organic、paid_search、email 或 social |

## orders

每笔订单一行，以 order_id 为主键。全部模拟订单均已成交并完成出库，模型仅计商品金额，未模拟税、运费、取消或退款。

| 字段 | 类型 | 含义 |
|---|---|---|
| order_id | STRING | 合成订单编号 |
| order_date | DATE | 模拟业务日期，直接用于日、月聚合 |
| customer_id | STRING | 关联 customers.customer_id |
| currency | STRING | 订单币种 USD |
| gross_sales | NUMERIC | 订单各行数量乘折前单价之和 |
| discount_amount | NUMERIC | 订单各行折扣金额之和，正数表示减免 |
| net_sales | NUMERIC | 商品净销售额，等于 gross_sales 减 discount_amount |

## order_items

每条订单明细一行，以 (order_id, line_number) 为联合主键。按商品分摊收入时使用行净额；连接后直接累加订单头金额会按行数重复计入。

| 字段 | 类型 | 含义 |
|---|---|---|
| order_id | STRING | 关联 orders.order_id |
| line_number | INT64 | 同一订单中从 1 开始的行序号 |
| variant_id | STRING | 关联 products.variant_id |
| quantity | INT64 | 购买件数，正整数 |
| unit_price | NUMERIC | 本次模拟采用的折前单价，等于该变体快照目录价 |
| line_discount | NUMERIC | 整行折扣，按折前行金额乘模拟折扣率四舍五入至分 |
| line_net | NUMERIC | 数量乘单价减去整行折扣，币种继承订单 |

## inventory

仅覆盖演示货盘。每个变体每个自然月一行，以 (month_end, variant_id) 为联合主键。全表为单一虚拟仓的合成库存，数量单位为件。

| 字段 | 类型 | 含义 |
|---|---|---|
| month_end | DATE | 月末日 |
| variant_id | STRING | 关联 products.variant_id |
| opening_qty | INT64 | 月初库存，首月为模拟期初值，此后等于上月期末 |
| received_qty | INT64 | 当月模拟入库量 |
| sold_qty | INT64 | 当月已售出库量，等于同变体同月订单行数量之和 |
| closing_qty | INT64 | 月末库存，等于 opening_qty 加 received_qty 减 sold_qty |

## 指标与核对

- **订单数、销量及客单价**。订单数取 orders 行数，销量取 order_items.quantity 之和，客单价取订单净销售额之和除以订单数。
- **整体折扣率**。将订单折扣合计除以折前商品销售额合计。该比率按销售额加权。
- **全期买家复购率**。期内有至少两笔订单的去重买家数除以全期去重买家数。本版分子为 427，分母为 1,359。
- **月度回购订单占比**。当月 order_date 晚于该买家 first_order_date 的订单数除以当月订单数。本模拟中首日每位买家只下单一次。月度回购买家数按上述订单去重计人，新买家数按首次购买月份计人；同一人可在首购月先买后复购。
- **月末库存**。在同一月末可跨变体加总，跨月观察使用月末序列。若月初、当月入库和月末均为零，可确认整月无可售货量。只有月末为零时，无法确定缺货天数或未满足需求。
- **关联顺序**。orders → customers 为多对一，order_items → orders 和 products 均为多对一。销量与库存比较时，先按订单月份、variant_id 汇总明细，再与同月库存连接。
