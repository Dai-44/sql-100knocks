# 第1問
## 設問
S-001: レシート明細データ（receipt）から全項目の先頭10件を表示し、どのようなデータを保有しているか目視で確認せよ。
  
## 回答
```sql
SELECT *
FROM receipt
limit 10;
```
  
## 模範解答
```sql
SELECT
    *
FROM receipt
LIMIT 10
;
```
  

# 第2問
## 設問
S-002: レシート明細データ（receipt）から売上年月日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、10件表示せよ。
  
## 回答
```sql
SELECT sales_ymd, customer_id, product_cd, amount
FROM receipt
limit 10;
```
  
## 模範解答
```sql
SELECT
    sales_ymd,
    customer_id,
    product_cd,
    amount
FROM receipt
LIMIT 10
;
```
  


# 第3問
## 設問
S-003: レシート明細データ（receipt）から売上年月日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、10件表示せよ。ただし、sales_ymdをsales_dateに項目名を変更しながら抽出すること。
  
## 回答
```sql
SELECT sales_ymd AS sales_date, customer_id, product_cd, amount
FROM receipt
limit 10;
```
  
## 模範解答
```sql
SELECT
    sales_ymd AS sales_date,
    customer_id,
    product_cd,
    amount 
FROM receipt
LIMIT 10
;
```
