- 【SQL】CASEとWHENによる条件分岐について、さまざまな使い方を紹介 | Engineer Labo エンジニアラボ
  - https://x-tech.pasona.co.jp/media/detail.html?p=9486
- https://www.db-fiddle.com/

``` sql
CREATE TABLE product (
  `product_cd` INTEGER,
  `category_cd` INTEGER,
  `unit_cost` INTEGER
);

INSERT INTO product
  (`product_cd`, `category_cd`, `unit_cost`)
VALUES
  ('1001', '2001', '300'),
  ('1002', '2001', '500'),
  ('1003', '2002', '620'),
  ('1004', '2002', '450'),
  ('1005', '2003', '800'),
  ('1006', '2003', '1020');
```

``` sql
SELECT
  pr.*,
  CASE category_cd
    WHEN 2001 THEN 'meat'
    WHEN 2002 THEN 'vegetable'
    WHEN 2003 THEN 'fruit'
    ELSE 'no_category'
  END AS category_name
FROM product AS pr
```

``` sql
SELECT
  pr.*,
  CASE WHEN unit_cost < 500 THEN 'low'
       WHEN unit_cost < 1000 THEN 'middle'
       ELSE 'high'
  END AS lank
FROM product AS pr
```

``` sql
SELECT
  pr.*,
  CASE WHEN category_cd = 2001 AND unit_cost < 500 THEN 1
       ELSE 0
  END AS meat_under500
FROM product AS pr
```

``` sql
SELECT
  pr.*,
  CASE WHEN product_cd IN (1001,1003,1005) THEN 1
       ELSE 0
  END AS product_cd_check
FROM product AS pr
```

``` sql
SELECT
  SUM(CASE WHEN unit_cost > 500 THEN 1 ELSE 0 END) AS over500_num
FROM product AS pr
```

``` sql
SELECT
  pr.*,
  CASE WHEN category_cd IN (2001,2002) THEN
          CASE WHEN unit_cost > 500 THEN unit_cost*0.9
                ELSE unit_cost
          END
       WHEN category_cd IN (2003) THEN
          CASE WHEN unit_cost > 1000 THEN unit_cost*0.8
                ELSE unit_cost
          END
  END AS cost
FROM product AS pr
```

``` sql
SELECT
  pr.*
FROM product AS pr
WHERE
  CASE WHEN unit_cost < 500 THEN category_cd = 2001
    ELSE TRUE
END
```

``` sql
SELECT
  pr.*
FROM product AS pr
ORDER BY
  CASE WHEN product_cd = 1001 THEN 4
        WHEN product_cd = 1002 THEN 1
        WHEN product_cd = 1003 THEN 5
        WHEN product_cd = 1004 THEN 2
        WHEN product_cd = 1005 THEN 6
        WHEN product_cd = 1006 THEN 3
        ELSE 99
END
```