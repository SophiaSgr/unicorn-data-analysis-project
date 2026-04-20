# Unicorn E-Commerce Profitability Analysis
### SQL · Tableau · Google Sheets · 2015–2018

## The Finding in One Line
Discounting is destroying margin. The fix is specific, not broad.

## Project Overview
An end-to-end data analysis of a US e-commerce business across 
9,994 order lines and 531 cities, generating $2.3M in revenue 
and $286K in profit (12.5% overall margin).

The analysis identifies exactly where and why the business is 
losing money — and delivers three targeted actions to recover it.

## Tools Used
- **SQL (PostgreSQL)** — data extraction and custom queries
- **Google Sheets** — data cleaning and pivot analysis  
- **Tableau** — interactive dashboards and visualizations
👉 [View Interactive Dashboard on Tableau Public](https://public.tableau.com/app/profile/sophia.sager/viz/UNICORN-Whichproductsmakeusmoneywhichloseusmoneyandisdiscountingthereason/Dashboard1)

## Dataset
Four related tables:
- `customers` — 795 unique customers and segments
- `orders` — shipping city, state, and order dates
- `order_details` — sales, profits, discounts, quantities
- `product` — categories, sub-categories, manufacturers

## Key Findings

### The Discount Threshold
20% is the line. Every order above it loses money.

| Discount Level | Avg Profit per Order |
|---------------|---------------------|
| 0% | +$66.78 |
| 20% | +$24.95 |
| 30% | -$39.43 |
| 50% | -$310.73 |

1,393 current order lines sit above the 20% threshold.

### Category Performance
| Category | Margin |
|----------|--------|
| Technology | 17.4% |
| Office Supplies | 17.0% |
| Furniture | 2.5% |

Three sub-categories produce a net loss across the entire period:
- Tables: -$17,733 at -8.6% margin
- Bookcases: -$3,479 at -3.0% margin
- Supplies: -$1,187 at -2.5% margin

### Furniture Is Profitable at Full Price
Furniture sold at full price averages **+$69.53 profit per line**.
With any discount applied: **-$30.88 loss per line**.
The swing for Tables alone: **$44,279** — driven entirely by 
discount policy, not by the product itself.

### Worst Performing Products
| Product | Total Loss | Avg Discount |
|---------|-----------|--------------|
| Cubify CubeX 3D (Double) | -$8,880 | 53% |
| Lexmark MX611dhe | -$4,590 | 40% |
| Cubify CubeX 3D (Triple) | -$3,840 | 50% |

### Best Performing Product
Canon imageCLASS 2200 Copier — **$25,200 profit at 12% discount**.
More profit than the entire Furniture category combined.

## Recommendations

**01 — Set a 20% Discount Ceiling**
Require manager sign-off for any discount above 20%.
Every order below this threshold has positive average profit.
Every order above it does not.

**02 — Review & Discontinue Loss-Making Products**
Five products identified for catalogue removal.
The Cubify CubeX Double Head loses $987 per unit at 53% discount.
These products lose money regardless of pricing policy.

**03 — Introduce Minimum Margin Floor for Furniture**
Before any Furniture discount is approved, require that 
post-discount margin stays positive.
This single policy recovers the majority of the $44,279 swing 
identified in the Tables analysis.

## SQL Queries

### Standard Queries (Questions 1–14)
Covering business scale, city profitability, customer segments, 
product performance, order analysis, and manufacturer breakdown.

### Custom Analytical Queries
Written specifically to support the executive summary:

**1. Sub-category profitability**
```sql
SELECT
  product_subcategory,
  SUM(order_profits) AS total_profit,
  SUM(order_profits) / SUM(order_sales) AS profit_margin
FROM order_details
JOIN product ON order_details.product_id = product.product_id
GROUP BY product_subcategory
ORDER BY total_profit ASC;
```

**2. Discount threshold analysis**
```sql
SELECT
  od.order_discount,
  COUNT(*) AS num_lines,
  AVG(od.order_profits) AS avg_profit
FROM order_details od
GROUP BY od.order_discount
ORDER BY od.order_discount;
```

**3. Furniture discount impact**
```sql
SELECT
  product_subcategory,
  CASE WHEN order_discount = 0 
       THEN 'No discount' 
       ELSE 'Discount applied' END AS pricing,
  ROUND(AVG(order_profits)::numeric, 2) AS avg_profit,
  ROUND(SUM(order_profits)::numeric, 0) AS total_profit,
  COUNT(*) AS num_lines
FROM order_details
JOIN product ON order_details.product_id = product.product_id
WHERE product_category = 'Furniture'
GROUP BY product_subcategory, pricing
ORDER BY product_subcategory, pricing;
```

## Skills Demonstrated
- Multi-table JOINs
- Aggregations with GROUP BY and HAVING
- Conditional logic with CASE WHEN
- Subqueries
- Profit margin calculations
- Data-driven storytelling and executive presentation
- Translating SQL findings into business recommendations

## Authors
Sophia Sager & Federica Lucarelli
Masterschool Data Analytics Program · 2026
