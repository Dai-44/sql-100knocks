# 第43問
## 設問
S-043: レシート明細データ（receipt）と顧客データ（customer）を結合し、性別コード（gender_cd）と年代（ageから計算）ごとに売上金額（amount）を合計した売上サマリデータを作成せよ。性別コードは0が男性、1が女性、9が不明を表すものとする。

ただし、項目構成は年代、女性の売上金額、男性の売上金額、性別不明の売上金額の4項目とすること（縦に年代、横に性別のクロス集計）。また、年代は10歳ごとの階級とすること。
  
## 回答
```sql
WITH customer_data AS (
    SELECT customer_id, gender_cd,
      CASE 
        WHEN age >= 10 AND age < 20 THEN '10代'
        WHEN age >= 20 AND age < 30 THEN '20代'
        WHEN age >= 30 AND age < 40 THEN '30代'
        WHEN age >= 40 AND age < 50 THEN '40代'
        WHEN age >= 50 AND age < 60 THEN '50代'
        WHEN age >= 60 AND age < 70 THEN '60代'
        WHEN age >= 70 AND age < 80 THEN '70代'
        WHEN age >= 80 AND age < 90 THEN '80代'
        WHEN age >= 90 AND age < 100 THEN '90代'
        ELSE '100歳以上'
      END AS age_group
    FROM customer
),
sales_amount_by_gender_and_age AS (
    SELECT c.gender_cd, c.age_group, SUM(r.amount) AS sum_amount
    FROM customer_data c
    INNER JOIN receipt r ON c.customer_id = r.customer_id
    GROUP BY c.gender_cd, c.age_group
)
SELECT age_group,
       MAX(CASE gender_cd WHEN '0' THEN sum_amount else NULL end) AS men_total,
       MAX(CASE gender_cd WHEN '1' THEN sum_amount else NULL end) AS women_total,
       MAX(CASE gender_cd WHEN '9' THEN sum_amount else NULL end) AS unknown_total
FROM sales_amount_by_gender_and_age
GROUP BY age_group;
```
  
## 模範解答
```sql
DROP TABLE IF EXISTS sales_summary;

CREATE TABLE sales_summary AS
    WITH gender_era_amount AS (
        SELECT
            TRUNC(age / 10) * 10 AS era,
            c.gender_cd,
            SUM(r.amount) AS amount
        FROM customer c
        JOIN receipt r
        ON
            c.customer_id = r.customer_id
        GROUP BY
            era,
            c.gender_cd
    )
    SELECT
        era,
        SUM(CASE WHEN gender_cd = '0' THEN amount END) AS male,
        SUM(CASE WHEN gender_cd = '1' THEN amount END) AS female,
        SUM(CASE WHEN gender_cd = '9' THEN amount END) AS unknown
    FROM gender_era_amount
    GROUP BY
        era
    ORDER BY
        era
;

SELECT
    *
FROM sales_summary
;
```
  

# 第44問
## 設問
S-044: 043で作成した売上サマリデータ（sales_summary）は性別の売上を横持ちさせたものであった。このデータから性別を縦持ちさせ、年代、性別コード、売上金額の3項目に変換せよ。ただし、性別コードは男性を"00"、女性を"01"、不明を"99"とする。
  
## 回答
```sql
WITH customer_data AS (
    SELECT customer_id, gender_cd,
      CASE 
        WHEN age >= 10 AND age < 20 THEN '10代'
        WHEN age >= 20 AND age < 30 THEN '20代'
        WHEN age >= 30 AND age < 40 THEN '30代'
        WHEN age >= 40 AND age < 50 THEN '40代'
        WHEN age >= 50 AND age < 60 THEN '50代'
        WHEN age >= 60 AND age < 70 THEN '60代'
        WHEN age >= 70 AND age < 80 THEN '70代'
        WHEN age >= 80 AND age < 90 THEN '80代'
        WHEN age >= 90 AND age < 100 THEN '90代'
        ELSE '100歳以上'
      END AS age_group
    FROM customer
),
sales_amount_by_gender_and_age AS (
    SELECT c.gender_cd, c.age_group, SUM(r.amount) AS sum_amount
    FROM customer_data c
    INNER JOIN receipt r ON c.customer_id = r.customer_id
    GROUP BY c.gender_cd, c.age_group
),
sales_amount_by_gender_and_age_horizontal AS (
    SELECT age_group,
       MAX(CASE gender_cd WHEN '0' THEN sum_amount else NULL end) AS men_total,
       MAX(CASE gender_cd WHEN '1' THEN sum_amount else NULL end) AS women_total,
       MAX(CASE gender_cd WHEN '9' THEN sum_amount else NULL end) AS unknown_total
    FROM sales_amount_by_gender_and_age
    GROUP BY age_group
)
SELECT age_group, '00' AS gender_cd, men_total AS sum_amount
FROM sales_amount_by_gender_and_age_horizontal
UNION ALL
SELECT age_group, '01' AS gender_cd, women_total AS sum_amount
FROM sales_amount_by_gender_and_age_horizontal
UNION ALL
SELECT age_group, '99' AS gender_cd, unknown_total AS sum_amount
FROM sales_amount_by_gender_and_age_horizontal;
```
  
## 模範解答
```sql
SELECT カラム FROM テーブル WHERE 条件;
```
  
