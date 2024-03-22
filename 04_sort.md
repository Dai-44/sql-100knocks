# 第17問
## 設問
S-017: 顧客データ（customer）を生年月日（birth_day）で高齢順にソートし、先頭から全項目を10件表示せよ。
  
## 回答
```sql
SELECT *
FROM customer
ORDER BY birth_day
LIMIT 10;
```
  
## 模範解答
```sql
SELECT * FROM customer ORDER BY birth_day LIMIT 10;
```
  

# 第18問
## 設問
S-018: 顧客データ（customer）を生年月日（birth_day）で若い順にソートし、先頭から全項目を10件表示せよ。
  
## 回答
```sql
SELECT *
FROM customer
ORDER BY birth_day DESC
LIMIT 10;
```
  
## 模範解答
```sql
SELECT * FROM customer ORDER BY birth_day DESC LIMIT 10;
```
  


# 第19問
## 設問
S-019: レシート明細データ（receipt）に対し、1件あたりの売上金額（amount）が高い順にランクを付与し、先頭から10件表示せよ。項目は顧客ID（customer_id）、売上金額（amount）、付与したランクを表示させること。なお、売上金額（amount）が等しい場合は同一順位を付与するものとする。
  
## 回答
```sql
SELECT customer_id, amount, RANK() OVER(ORDER BY amount DESC) AS rank_result
FROM receipt
LIMIT 10;
```
  
## 模範解答
```sql
SELECT
    customer_id,
    amount,
    RANK() OVER(ORDER BY amount DESC) AS ranking 
FROM receipt
LIMIT 10
;
```

# 第20問
## 設問
S-020: レシート明細データ（receipt）に対し、1件あたりの売上金額（amount）が高い順にランクを付与し、先頭から10件表示せよ。項目は顧客ID（customer_id）、売上金額（amount）、付与したランクを表示させること。なお、売上金額（amount）が等しい場合でも別順位を付与すること。
  
## 回答
```sql
SELECT customer_id, amount, ROW_NUMBER() OVER(ORDER BY amount DESC) AS rank_result
FROM receipt
LIMIT 10;
```
  
## 模範解答
```sql
SELECT
    customer_id,
    amount,
    ROW_NUMBER() OVER(ORDER BY amount DESC) AS ranking 
FROM receipt
LIMIT 10
;
```
