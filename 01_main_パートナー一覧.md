### パートナー一覧

``` sql
SELECT
    *
FROM
    m_partner
;
```

``` sql
SELECT
      M01.partner_type  AS 'パートナー区分'    -- 1:得意先　2:仕入先（生産委託先）
    , CASE M01.partner_type
        WHEN 1 THEN '得意先'
        WHEN 2 THEN '仕入先'
        ELSE ''
      END AS 'パートナー区分'
    , M01.campus_id     AS 'CAMPUS-ID'
    , M01.company_name  AS '会社名'
    , M01.company_name2 AS '会社名略称'
    , M01.language_code AS '基軸言語コード' -- コード管理(LANUGAGE_CODE)
    , M01.currency_code AS '基軸通貨コード' -- レートマスタにてコードを管理
    , M01.closing_date  AS '締日'    -- DD形式
    , M01.payment_site  AS 'サイト'    -- 1:翌月　2:翌々月
    , CASE M01.payment_site
        WHEN 1 THEN '翌月'
        WHEN 2 THEN '翌々月'
        WHEN 3 THEN '翌々々月'
        ELSE ''
      END AS 'サイト'
    , M01.payment_date  AS '支払日'
    , M01.member_type   AS '会員区分' -- 1:有償会員、2:無償会員、3:非対象
    , CASE M01.member_type
        WHEN 1 THEN '有償会員'
        WHEN 2 THEN '無償会員'
        WHEN 3 THEN '非対象'
        ELSE ''
      END AS '会員区分'
    , M01.remarks       AS '備考'
FROM
    m_partner AS M01    -- パートナーマスタ(得意先と生産委託先を管理するマスタ)
--WHERE
--    M01.partner_type = '1'    -- 1:得意先
--    M01.partner_type = '2'    -- 2:仕入先（生産委託先）
--    AND
--    M01.company_name LIKE '% %'
--    M01.company_name2 LIKE '% %'
--    LIMIT 3
;
```