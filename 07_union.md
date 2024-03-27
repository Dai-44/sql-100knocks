# 第36問
## 設問
S-036: レシート明細データ（receipt）と店舗データ（store）を内部結合し、レシート明細データの全項目と店舗データの店舗名（store_name）を10件表示せよ。
  
## 回答
```sql
SELECT receipt.*, store.store_name
FROM receipt
INNER JOIN store ON receipt.store_cd = store.store_cd
LIMIT 10;
```
  
## 模範解答
```sql
SELECT
    r.*,
    s.store_name
FROM receipt r
JOIN store s
ON
    r.store_cd = s.store_cd
LIMIT 10
;
```
  

# 第37問
## 設問
S-037: 商品データ（product）とカテゴリデータ（category）を内部結合し、商品データの全項目とカテゴリデータのカテゴリ小区分名（category_small_name）を10件表示せよ。
  
## 回答
```sql
SELECT product.*, category.category_small_name
FROM product
INNER JOIN category ON product.category_small_cd = category.category_small_cd
LIMIT 10;
```
  
## 模範解答
```sql
SELECT
    p.*,
    c.category_small_name
FROM product p
JOIN category c
ON
    p.category_small_cd = c.category_small_cd
LIMIT 10
;
```
  

# 第38問
## 設問
S-038: 顧客データ（customer）とレシート明細データ（receipt）から、顧客ごとの売上金額合計を求め、10件表示せよ。ただし、売上実績がない顧客については売上金額を0として表示させること。また、顧客は性別コード（gender_cd）が女性（1）であるものを対象とし、非会員（顧客IDが"Z"から始まるもの）は除外すること。
  
## 回答
```sql
SELECT customer.customer_id, customer.customer_name, SUM(receipt.amount) AS sum_amount
FROM customer
INNER JOIN receipt
ON customer.customer_id = receipt.customer_id
WHERE customer.gender_cd = '1'
AND customer.customer_id NOT LIKE 'Z%'
GROUP BY customer.customer_id
LIMIT 10;
```
  
## 模範解答
```sql
WITH customer_amount AS (
    SELECT
        customer_id,
        SUM(amount) AS sum_amount
    FROM receipt
    GROUP BY
        customer_id
),
customer_data AS (
    SELECT
        customer_id
    FROM customer
    WHERE
        gender_cd = '1'
        AND customer_id NOT LIKE 'Z%'
)
SELECT
    c.customer_id,
    COALESCE(a.sum_amount, 0)
FROM customer_data c
LEFT OUTER JOIN customer_amount a
ON
    c.customer_id = a.customer_id
LIMIT 10
;
```

# 第39問
## 設問
S-039: レシート明細データ（receipt）から、売上日数の多い顧客の上位20件を抽出したデータと、売上金額合計の多い顧客の上位20件を抽出したデータをそれぞれ作成し、さらにその2つを完全外部結合せよ。ただし、非会員（顧客IDが"Z"から始まるもの）は除外すること。
  
## 回答
```sql
WITH sales_date_order AS (
         SELECT customer_id, COUNT(*)
         FROM receipt
         WHERE customer_id NOT LIKE 'Z%'
         GROUP BY customer_id
         ORDER BY COUNT(*)
         LIMIT 20
     ),
     sales_amount_order AS (
         SELECT customer_id, SUM(amount)
         FROM receipt
         WHERE customer_id NOT LIKE 'Z%'
         GROUP BY customer_id
         ORDER BY SUM(amount)
         LIMIT 20
     )
SELECT *
FROM sales_date_order
CROSS JOIN sales_amount_order
```
  
## 模範解答
```sql
WITH customer_data AS (
    select
        customer_id,
        sales_ymd,
        amount
    FROM receipt
    WHERE
        customer_id NOT LIKE 'Z%'
),
customer_days AS (
    select
        customer_id,
        COUNT(DISTINCT sales_ymd) come_days
    FROM customer_data
    GROUP BY
        customer_id
    ORDER BY
        come_days DESC
    LIMIT 20
),
customer_amount AS (
    SELECT
        customer_id,
        SUM(amount) buy_amount
    FROM customer_data
    GROUP BY
        customer_id
    ORDER BY
        buy_amount DESC
    LIMIT 20
)
SELECT
    COALESCE(d.customer_id, a.customer_id) customer_id,
    d.come_days,
    a.buy_amount
FROM customer_days d
FULL OUTER JOIN customer_amount a
ON
    d.customer_id = a.customer_id
;
```
  

# 第40問
## 設問
S-040: 全ての店舗と全ての商品を組み合わせたデータを作成したい。店舗データ（store）と商品データ（product）を直積し、件数を計算せよ。
  
## 回答
```sql
WITH new_store AS (
         SELECT store.*
         FROM store
         LEFT OUTER JOIN receipt
         ON store.store_cd = receipt.store_cd
     ),
     new_product AS (
         SELECT product.*
         FROM product
         LEFT OUTER JOIN receipt
         ON product.product_cd = receipt.product_cd
     )
SELECT COUNT(*)
FROM new_store
CROSS JOIN new_product;
```
  
## 模範解答
```sql
SELECT
    COUNT(1)
FROM store
CROSS JOIN product
;
```
  


# 第41問
## 設問
S-041: レシート明細データ（receipt）の売上金額（amount）を日付（sales_ymd）ごとに集計し、前回売上があった日からの売上金額増減を計算せよ。そして結果を10件表示せよ。
  
## 回答
```sql
わからない。
```
  
## 模範解答
```sql
WITH sales_amount_by_date AS (
    SELECT
        sales_ymd,
        SUM(amount) AS amount
    FROM receipt
    GROUP BY
        sales_ymd
),
sales_amount_by_date_with_lag as (
    SELECT
        sales_ymd,
        LAG(sales_ymd, 1) OVER(ORDER BY sales_ymd) lag_ymd,
        amount,
        LAG(amount, 1) OVER(ORDER BY sales_ymd) AS lag_amount
    FROM sales_amount_by_date
)
SELECT
    sales_ymd,
    amount,
    lag_ymd,
    lag_amount,
    amount - lag_amount AS diff_amount
FROM sales_amount_by_date_with_lag
ORDER BY
    sales_ymd
LIMIT 10
;
```

# 第42問
## 設問
S-042: レシート明細データ（receipt）の売上金額（amount）を日付（sales_ymd）ごとに集計し、各日付のデータに対し、前回、前々回、3回前に売上があった日のデータを結合せよ。そして結果を10件表示せよ。
  
## 回答
```sql
WITH each_day_amount AS (
    SELECT sales_ymd, SUM(amount) AS amount
    FROM receipt
    GROUP BY sales_ymd
)
SELECT sales_ymd, SUM(amount) OVER (ORDER BY sales_ymd ROWS 3 PRECEDING)
FROM each_day_amount
LIMIT 10;
```
  
## 模範解答
```sql
-- コード例1:縦持ちケース
WITH sales_amount_by_date AS (
    SELECT
        sales_ymd,
        SUM(amount) AS amount
    FROM receipt
    GROUP BY
        sales_ymd
),
sales_amount_lag_date AS (
    SELECT
        sales_ymd,
        LAG(sales_ymd, 3) OVER (ORDER BY sales_ymd) AS lag_date_3,
        amount
    FROM sales_amount_by_date
)
SELECT
    a.sales_ymd,
    a.amount,
    b.sales_ymd AS lag_ymd,
    b.amount AS lag_amount
FROM sales_amount_lag_date a
JOIN sales_amount_lag_date b
ON
    (
        a.lag_date_3 IS NULL
        OR a.lag_date_3 <= b.sales_ymd
    )
    AND b.sales_ymd < a.sales_ymd
ORDER BY
    sales_ymd,
    lag_ymd
LIMIT 10
;
```

```sql
-- コード例2:横持ちケース
WITH sales_amount_by_date AS (
    SELECT
        sales_ymd,
        SUM(amount) AS amount
    FROM receipt
    GROUP BY
        sales_ymd
),
sales_amount_with_lag AS (
    SELECT
        sales_ymd,
        amount, 
        LAG(sales_ymd, 1) OVER (ORDER BY sales_ymd) AS lag_ymd_1,
        LAG(amount, 1) OVER (ORDER BY sales_ymd) AS lag_amount_1,
        LAG(sales_ymd, 2) OVER (ORDER BY sales_ymd) AS lag_ymd_2,
        LAG(amount, 2) OVER (ORDER BY sales_ymd) AS lag_amount_2,
        LAG(sales_ymd, 3) OVER (ORDER BY sales_ymd) AS lag_ymd_3,
        LAG(amount, 3) OVER (ORDER BY sales_ymd) AS lag_amount_3
    FROM sales_amount_by_date
)
SELECT
    sales_ymd,
    amount, 
    lag_ymd_1,
    lag_amount_1,
    lag_ymd_2,
    lag_amount_2,
    lag_ymd_3,
    lag_amount_3
FROM sales_amount_with_lag
WHERE
    lag_ymd_3 IS NOT NULL
ORDER BY
    sales_ymd
LIMIT 10
;
```
