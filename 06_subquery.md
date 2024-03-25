# 第34問
## 設問
S-034: レシート明細データ（receipt）に対し、顧客ID（customer_id）ごとに売上金額（amount）を合計して全顧客の平均を求めよ。ただし、顧客IDが"Z"から始まるものは非会員を表すため、除外して計算すること。
  
## 回答
```sql
-- FROM句でサブクエリを利用
SELECT AVG(sum_amount)
FROM (
  SELECT customer_id, SUM(amount) AS sum_amount
  FROM receipt
  WHERE customer_id NOT LIKE 'Z%'
  GROUP BY customer_id
) AS calc_avg;
```

```sql
-- WITHで仮テーブルを作成してからSELECT実行
WITH sum_count AS (
    SELECT customer_id, SUM(amount) AS sum_amount
    FROM receipt
    WHERE customer_id NOT LIKE 'Z%'
    GROUP BY customer_id
)
SELECT AVG(sum_amount)
FROM sum_count;
```
  
## 模範解答
```sql
WITH customer_amount AS (
    SELECT
        customer_id,
        SUM(amount) AS sum_amount
    FROM receipt
    WHERE
        customer_id NOT LIKE 'Z%'
    GROUP BY customer_id
)
SELECT
    AVG(sum_amount)
FROM customer_amount
;
```
  

# 第35問
## 設問
S-035: レシート明細データ（receipt）に対し、顧客ID（customer_id）ごとに売上金額（amount）を合計して全顧客の平均を求め、平均以上に買い物をしている顧客を抽出し、10件表示せよ。ただし、顧客IDが"Z"から始まるものは非会員を表すため、除外して計算すること。
  
## 回答
```sql
SELECT customer_id, SUM(amount)
FROM receipt
GROUP BY customer_id
HAVING SUM(amount) >= 
(
    WITH sum_count AS (
        SELECT customer_id, SUM(amount) AS sum_amount
        FROM receipt
        WHERE customer_id NOT LIKE 'Z%'
        GROUP BY customer_id
    )
    SELECT AVG(sum_amount)
    FROM sum_count
)
LIMIT 10;
```
  
## 模範解答
```sql
WITH customer_amount AS (
    SELECT
        customer_id,
        SUM(amount) AS sum_amount
    FROM receipt
    WHERE
        customer_id NOT LIKE 'Z%'
    GROUP BY customer_id
)
SELECT
    customer_id,
    sum_amount
FROM customer_amount
WHERE
    sum_amount >= (
        SELECT
            AVG(sum_amount)
        FROM customer_amount
    )
LIMIT 10
;
```
