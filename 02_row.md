# 第4問
## 設問
S-004: レシート明細データ（receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、以下の条件を満たすデータを抽出せよ。

顧客ID（customer_id）が"CS018205000001"
  
## 回答
```sql
SELECT sales_ymd, customer_id, product_cd, amount
FROM receipt
WHERE customer_id = "CS018205000001";
```
  
## 模範解答
```sql
SELECT
    sales_ymd,
    customer_id,
    product_cd,
    amount
FROM
    receipt
WHERE
    customer_id = 'CS018205000001'
;
```
  

# 第5問
## 設問
S-005: レシート明細データ（receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、以下の全ての条件を満たすデータを抽出せよ。

顧客ID（customer_id）が"CS018205000001"
売上金額（amount）が1,000以上
  
## 回答
```sql
SELECT sales_ymd, customer_id, product_cd, amount
FROM receipt
WHERE customer_id = "CS018205000001"
AND amount >= 1000;
```
  
## 模範解答
```sql
SELECT
    sales_ymd,
    customer_id,
    product_cd,
    amount
FROM
    receipt
WHERE
    customer_id = 'CS018205000001'
    AND amount >= 1000
;
```
  


# 第6問
## 設問
S-006: レシート明細データ（receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上数量（quantity）、売上金額（amount）の順に列を指定し、以下の全ての条件を満たすデータを抽出せよ。

顧客ID（customer_id）が"CS018205000001"
売上金額（amount）が1,000以上または売上数量（quantity）が5以上
  
## 回答
```sql
SELECT sales_ymd, customer_id, product_cd, quantity, amount
FROM receipt
WHERE customer_id = "CS018205000001"
AND ( amount >= 1000 OR quantity >= 5 );
```
  
## 模範解答
```sql
SELECT
    sales_ymd,
    customer_id,
    product_cd,
    quantity,
    amount
FROM
    receipt
WHERE
    customer_id = 'CS018205000001'
    AND
    (
        amount >= 1000
        OR quantity >= 5
    )
;
```

# 第7問
## 設問
S-007: レシート明細データ（receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、以下の全ての条件を満たすデータを抽出せよ。

顧客ID（customer_id）が"CS018205000001"
売上金額（amount）が1,000以上2,000以下
  
## 回答
```sql
SELECT sales_ymd, customer_id, product_cd, amount
FROM receipt
WHERE customer_id = "CS018205000001"
AND amount BETWEEN 1000 AND 2000;
```
  
## 模範解答
```sql
SELECT
    sales_ymd,
    customer_id,
    product_cd,
    amount
FROM
    receipt
WHERE
    customer_id = 'CS018205000001'
    AND amount BETWEEN 1000 AND 2000
;
```
  

# 第8問
## 設問
S-008: レシート明細データ（receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、以下の全ての条件を満たすデータを抽出せよ。

顧客ID（customer_id）が"CS018205000001"
商品コード（product_cd）が"P071401019"以外
  
## 回答
```sql
SELECT sales_ymd, customer_id, product_cd, amount
FROM receipt
WHERE customer_id = "CS018205000001"
AND product_cd <> "P071401019";
```
  
## 模範解答
```sql
SELECT
    sales_ymd,
    customer_id,
    product_cd, amount
FROM
    receipt
WHERE
    customer_id = 'CS018205000001'
    AND product_cd != 'P071401019'
;
```
  


# 第9問
## 設問
S-009: 以下の処理において、出力結果を変えずにORをANDに書き換えよ。

SELECT * FROM store WHERE NOT (prefecture_cd = '13' OR floor_area > 900)
  
## 回答
```sql
SELECT * 
FROM store 
WHERE (prefecture_cd <> '13' AND floor_area <= 900);
```
  
## 模範解答
```sql
SELECT * FROM store WHERE prefecture_cd != '13' AND floor_area <= 900;
```