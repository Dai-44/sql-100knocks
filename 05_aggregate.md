# 第21問
## 設問
S-021: レシート明細データ（receipt）に対し、件数をカウントせよ。
  
## 回答
```sql
SELECT COUNT(*)
FROM receipt;
```
  
## 模範解答
```sql
-- コード例1
SELECT COUNT(1) FROM receipt;

-- コード例2（*でもOK）
SELECT COUNT(*) FROM receipt;
```
  

# 第22問
## 設問
S-022: レシート明細データ（receipt）の顧客ID（customer_id）に対し、ユニーク件数をカウントせよ。
  
## 回答
```sql
SELECT COUNT(DISTINCT customer_id)
FROM receipt;
```
  
## 模範解答
```sql
SELECT
    COUNT(DISTINCT customer_id)
FROM receipt
;
```
  
# 第23問
## 設問
S-023: レシート明細データ（receipt）に対し、店舗コード（store_cd）ごとに売上金額（amount）と売上数量（quantity）を合計せよ。
  
## 回答
```sql
SELECT store_cd, SUM(amount) AS total_amount, SUM(quantity) AS total_quantity
FROM receipt
GROUP BY store_cd;
```
  
## 模範解答
```sql
SELECT store_cd
    , SUM(amount) AS amount
    , SUM(quantity) AS quantity
FROM receipt
group by store_cd
;
```

# 第24問
## 設問
S-024: レシート明細データ（receipt）に対し、顧客ID（customer_id）ごとに最も新しい売上年月日（sales_ymd）を求め、10件表示せよ。
  
## 回答
```sql
SELECT customer_id, MAX(sales_ymd) AS recent_sales_ymd
FROM receipt
GROUP BY customer_id
LIMIT 10;
```
  
## 模範解答
```sql
SELECT
    customer_id,
    MAX(sales_ymd)
FROM receipt
GROUP BY customer_id
LIMIT 10
;
```
  
# 第25問
## 設問
S-025: レシート明細データ（receipt）に対し、顧客ID（customer_id）ごとに最も古い売上年月日（sales_ymd）を求め、10件表示せよ。
  
## 回答
```sql
SELECT customer_id, MIN(sales_ymd) AS old_sales_ymd
FROM receipt
GROUP BY customer_id
LIMIT 10;
```
  
## 模範解答
```sql
SELECT
    customer_id,
    MIN(sales_ymd)
FROM receipt
GROUP BY customer_id
LIMIT 10
;
```
  
# 第26問
## 設問
S-026: レシート明細データ（receipt）に対し、顧客ID（customer_id）ごとに最も新しい売上年月日（sales_ymd）と古い売上年月日を求め、両者が異なるデータを10件表示せよ。
  
## 回答
```sql
SELECT customer_id, MAX(sales_ymd) AS recent_sales_ymd, MIN(sales_ymd) AS old_sales_ymd
FROM receipt
GROUP BY customer_id
HAVING MAX(sales_ymd) <> MIN(sales_ymd)
LIMIT 10;
```
  
## 模範解答
```sql
SELECT
    customer_id,
    MAX(sales_ymd),
    MIN(sales_ymd)
FROM receipt
GROUP BY customer_id
HAVING MAX(sales_ymd) != MIN(sales_ymd)
LIMIT 10
;
```

# 第27問
## 設問
S-027: レシート明細データ（receipt）に対し、店舗コード（store_cd）ごとに売上金額（amount）の平均を計算し、降順でTOP5を表示せよ。
  
## 回答
```sql
SELECT store_cd, AVG(amount) AS avg_amount
FROM receipt
GROUP BY store_cd
ORDER BY avg_amount DESC
LIMIT 5;
```
  
## 模範解答
```sql
SELECT
    store_cd,
    AVG(amount) AS avg_amount
FROM receipt
GROUP BY store_cd
ORDER BY avg_amount DESC
LIMIT 5
;
```
  

# 第28問
## 設問
S-028: レシート明細データ（receipt）に対し、店舗コード（store_cd）ごとに売上金額（amount）の中央値を計算し、降順でTOP5を表示せよ。
  
## 回答
```sql
わからない。
```
  
## 模範解答
```sql
SELECT 
    store_cd, 
    PERCENTILE_CONT(0.5) WITHIN GROUP(ORDER BY amount) AS amount_50per
FROM receipt
GROUP BY store_cd
ORDER BY amount_50per DESC
LIMIT 5
;
```


# 第29問
## 設問
S-029: レシート明細データ（receipt）に対し、店舗コード（store_cd）ごとに商品コード（product_cd）の最頻値を求め、10件表示させよ。
  
## 回答
```sql
わからない。
```
  
## 模範解答
```sql
-- コード例1: window関数や分析関数で最頻値を集計する
WITH product_cnt AS (
    SELECT
        store_cd,
        product_cd,
        COUNT(1) AS mode_cnt
    FROM receipt
    GROUP BY
        store_cd,
        product_cd
),
product_mode AS (
    SELECT
        store_cd,
        product_cd,
        mode_cnt,
        RANK() OVER(PARTITION BY store_cd ORDER BY mode_cnt DESC) AS rnk
    FROM product_cnt
)
SELECT
    store_cd,
    product_cd,
    mode_cnt
FROM product_mode
WHERE
    rnk = 1
ORDER BY
    store_cd,
    product_cd
LIMIT 10
;
```

```sql
-- コード例2: MODE()を使う簡易ケース（早いが最頻値が複数の場合は一つだけ選ばれる）
SELECT
    store_cd,
    MODE() WITHIN GROUP(ORDER BY product_cd)
FROM receipt
GROUP BY store_cd
ORDER BY store_cd
LIMIT 10
;
```

# 第30問
## 設問
S-030: レシート明細データ（receipt）に対し、店舗コード（store_cd）ごとに売上金額（amount）の分散を計算し、降順で5件表示せよ。
  
## 回答
```sql
わからない。
```
  
## 模範解答
```sql
SELECT
    store_cd,
    VAR_POP(amount) AS vars_amount
FROM receipt
GROUP BY store_cd
ORDER BY vars_amount DESC 
LIMIT 5
;
```
  
# 第31問
## 設問
S-031: レシート明細データ（receipt）に対し、店舗コード（store_cd）ごとに売上金額（amount）の標準偏差を計算し、降順で5件表示せよ。
  
## 回答
```sql
わからない。
```
  
## 模範解答
```sql
SELECT
    store_cd,
    STDDEV_POP(amount) as stds_amount
FROM receipt
GROUP BY store_cd
ORDER BY stds_amount DESC
LIMIT 5
;
```
  
# 第32問
## 設問
S-032: レシート明細データ（receipt）の売上金額（amount）について、25％刻みでパーセンタイル値を求めよ。
  
## 回答
```sql
わからない。
```
  
## 模範解答
```sql
SELECT
    PERCENTILE_CONT(0.25) WITHIN GROUP(ORDER BY amount) AS amount_25per,
    PERCENTILE_CONT(0.50) WITHIN GROUP(ORDER BY amount) AS amount_50per,
    PERCENTILE_CONT(0.75) WITHIN GROUP(ORDER BY amount) AS amount_75per,
    PERCENTILE_CONT(1.0) WITHIN GROUP(ORDER BY amount) AS amount_100per
FROM receipt
;
```

# 第33問
## 設問
S-033: レシート明細データ（receipt）に対し、店舗コード（store_cd）ごとに売上金額（amount）の平均を計算し、330以上のものを抽出せよ。
  
## 回答
```sql
SELECT store_cd, AVG(amount) AS avg_amount
FROM receipt
GROUP BY store_cd
HAVING AVG(amount) >= 330;
```
  
## 模範解答
```sql
SELECT
    store_cd,
    AVG(amount) AS avg_amount
FROM receipt
GROUP BY store_cd
HAVING
    AVG(amount) >= 330
;
```