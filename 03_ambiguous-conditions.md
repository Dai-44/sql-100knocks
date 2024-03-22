# 第10問
## 設問
S-010: 店舗データ（store）から、店舗コード（store_cd）が"S14"で始まるものだけ全項目抽出し、10件表示せよ。
  
## 回答
```sql
SELECT *
FROM store
WHERE store_cd LIKE 'S14%'
limit 10;
```
  
## 模範解答
```sql
SELECT
    *
FROM store
WHERE
    store_cd LIKE 'S14%'
LIMIT 10
;
```
  

# 第11問
## 設問
S-011: 顧客データ（customer）から顧客ID（customer_id）の末尾が1のものだけ全項目抽出し、10件表示せよ。
  
## 回答
```sql
SELECT *
FROM customer
WHERE customer_id LIKE '%1'
limit 10;
```
  
## 模範解答
```sql
SELECT * FROM customer WHERE customer_id LIKE '%1' LIMIT 10;
```
  


# 第12問
## 設問
S-012: 店舗データ（store）から、住所 (address) に"横浜市"が含まれるものだけ全項目表示せよ。
  
## 回答
```sql
SELECT *
FROM store
WHERE address LIKE '%横浜市%';
```
  
## 模範解答
```sql
SELECT * FROM store WHERE address LIKE '%横浜市%';
```
# 第13問
## 設問
S-013: 顧客データ（customer）から、ステータスコード（status_cd）の先頭がアルファベットのA〜Fで始まるデータを全項目抽出し、10件表示せよ。
  
## 回答
```sql
SELECT *
FROM customer
WHERE status_cd ~ '^[ABCDEF].*'
limit 10;
```
  
## 模範解答
```sql
SELECT * FROM customer WHERE status_cd ~ '^[A-F]' LIMIT 10;
```
  

# 第14問
## 設問
S-014: 顧客データ（customer）から、ステータスコード（status_cd）の末尾が数字の1〜9で終わるデータを全項目抽出し、10件表示せよ。
  
## 回答
```sql
SELECT *
FROM customer
WHERE status_cd ~ '.*[123456789]$'
limit 10;
```
  
## 模範解答
```sql
SELECT * FROM customer WHERE status_cd ~ '[1-9]$' LIMIT 10;
```
  


# 第15問
## 設問
S-015: 顧客データ（customer）から、ステータスコード（status_cd）の先頭がアルファベットのA〜Fで始まり、末尾が数字の1〜9で終わるデータを全項目抽出し、10件表示せよ。
  
## 回答
```sql
SELECT *
FROM customer
WHERE status_cd ~ '^[ABCDEF].*[123456789]$'
limit 10;
```
  
## 模範解答
```sql
SELECT * FROM customer WHERE status_cd ~ '^[A-F].*[1-9]$' LIMIT 10;
```
# 第16問
## 設問
S-016: 店舗データ（store）から、電話番号（tel_no）が3桁-3桁-4桁のデータを全項目表示せよ。
  
## 回答
```sql
SELECT *
FROM store
WHERE tel_no ~ '^.{3}-.{3}-.{4}$';
```
  
## 模範解答
```sql
SELECT * FROM store WHERE tel_no ~ '^[0-9]{3}-[0-9]{3}-[0-9]{4}$';
```