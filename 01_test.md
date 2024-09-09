### 支払先一覧

``` sql
SELECT
    *
FROM
    m_partner
WHERE
    partner_type = '2';
```


``` sql
SELECT
	M01.partner_type  AS 'パートナー区分' -- 1:得意先　2:仕入先（生産委託先）
	,M01.campus_id     AS 'CAMPUS-ID'
	,M01.company_name  AS '会社名'     
	,M01.company_name2 AS '会社名略称'
	,M01.closing_date  AS '締日'          -- DD形式
	,M01.payment_site  AS 'サイト'         -- 1:翌月　2:翌々月
	,M01.payment_date  AS '支払日'         
	,M01.remarks       AS '備考'           
FROM
	m_partner AS M01 -- パートナーマスタ(得意先と生産委託先を管理するマスタ)
WHERE
--    M01.partner_type = '1' -- 1:得意先
    M01.partner_type = '2' -- 2:仕入先（生産委託先）
--    AND
--    M01.company_name LIKE '% %'
--    M01.company_name2 LIKE '% %'
--    LIMIT 3
```

### 製番一覧

``` sql
SELECT
    F01.inquiry_no AS '引合番号'
    ,F01.product_code AS '分類記号'
    ,F01.branch_no AS '枝番'
    ,F01.inquiry_no + F01.product_code AS 'まとめ製番'
    ,F01.inquiry_no + F01.product_code + F01.branch_no AS '製番'
    ,F01.campus_id AS 'CAMPUS-ID'
    ,(SELECT M01_01.company_name FROM m_partner AS M01_01 WHERE F01.campus_id = M01_01.campus_id) AS '会社名'
    ,(SELECT M01_02.company_name FROM m_partner AS M01_02 WHERE F01.campus_id = M01_02.campus_id) AS '会社略名'
    ,F01.subject AS '件名'
    ,F01.variety_code AS '品種コード'
    ,F01.item_name AS '品名'
    ,F01.status_code AS 'ステータス'
    ,F01.quotation_issued_date AS '見積提出日'
    ,F01.suggested_date AS '提案日'
    ,F01.order_accuracy AS '受注確度'
    ,F01.drawing_issue_plan_date AS '出図予定日'
    ,F01.drawing_issued_date AS '出図日'
    ,F01.drawing_inspection_date AS '検図日'
    ,F01.cost_estimate_document_plan_date AS '原価資料作成予定日'
    ,F01.cost_estimate_document_date AS '原価資料作成日'
    ,F01.sales_pic_code AS '営業担当者コード'
    ,(SELECT M03_01.user_name FROM m_employee AS M03_01 WHERE F01.sales_pic_code = M03_01.user_id) AS '営業担当'
    ,F01.procure_pic_code AS '調達担当者コード'
    ,(SELECT M03_02.user_name FROM m_employee AS M03_02 WHERE F01.sales_pic_code = M03_02.user_id) AS '調達担当'
    ,F01.main_drawing_pic_code AS '設計担当者コード(主)'
    ,(SELECT M03_03.user_name FROM m_employee AS M03_03 WHERE F01.sales_pic_code = M03_03.user_id) AS '設計主担当'
    ,F01.sub_drawing_pic_code AS '設計担当者コード(従)'
    ,(SELECT M03_04.user_name FROM m_employee AS M03_04 WHERE F01.sales_pic_code = M03_04.user_id) AS '設計従担当'
    ,F01.drawing_inspection_pic_code AS '検図担当者コード'
    ,(SELECT M03_05.user_name FROM m_employee AS M03_05 WHERE F01.sales_pic_code = M03_05.user_id) AS '検図担当'
    ,F01.cost_estimate_pic_code AS '原価資料担当者コード'
    ,(SELECT M03_06.user_name FROM m_employee AS M03_06 WHERE F01.sales_pic_code = M03_06.user_id) AS '原価資料担当'
    ,F01.inspection_pic_code AS '検査組立担当者コード'
    ,(SELECT M03_07.user_name FROM m_employee AS M03_07 WHERE F01.sales_pic_code = M03_07.user_id) AS '検査担当'
    ,F01.order_no AS '受注番号'
    ,F01.estimate_date AS '概算見積日'
    ,F01.inquiry_date AS '引合日'
    ,F01.order_lost_date AS '失注日'
    ,F01.order_date AS '受注日'
    ,F01.cancel_date AS 'キャンセル日'
    ,F01.due_date AS '納期'
    ,F01.complete_plan_date AS '完成予定日'
    ,F01.shipment_instruction_date AS '出荷指示日'
    ,F01.shipment_plan_date AS '出荷予定日'
    ,F01.shipment_date AS '出荷日'
    ,F01.final_sales_no AS '売上番号_最終'
    ,F01.final_sales_date AS '売上日_最終'
    ,F01.last_acceptance_date AS '検収日_最終'
    ,F01.last_accounting_connected_date AS '会計連携日_最終'
    ,F01.receipt_complete_date AS '仕入完了日' -- 全部品の仕入が完了した日付
    ,F01.order_quantity AS '受注数量'
    ,F01.uom AS '単位'
    ,F01.currency_rate AS 'レート'
    ,F01.unit_price AS '単価'
    ,F01.order_amount AS '受注金額(内貨)'
    ,F01.order_amount_fc AS '受注金額(外貨)'
    ,F01.po_num AS '発注件数'
    ,F01.po_amount AS '発注金額'
    ,F01.receipt_num AS '受入件数'
    ,F01.receipt_quantity AS '受入数量'
    ,F01.receipt_amount AS '受入金額'
    ,F01.acceptance_quantity AS '検収数量'
    ,F01.defective_quantity AS '不良数量'
    ,F01.acceptance_amount AS '検収金額'
    ,F01.shipment_num AS '出荷件数'
    ,F01.shipment_quantity AS '出荷数量'
    ,F01.shipment_amount AS '出荷金額'
    ,F01.sales_quantity AS '売上数量'
    ,F01.sales_amount AS '売上金額(内貨)'
    ,F01.sales_amount_fc AS '売上金額(外貨)'
FROM
    f_product_no AS F01 -- 製番管理ファイル(製番の進捗や詳細を管理する)
WHERE
    F01.order_date >= 2024/08/01
--    AND
--    F01.order_date <= 2024/08/31
--    AND
--    F01.shipment_plan_date IS NULL
ORDER BY
    F01.order_date DESC
-- LIMIT 3
```

<!--
``` sql
```
-->

``` sql
SELECT
    *
FROM
    m_partner
WHERE
    partner_type = '2';
```

``` sql
SELECT
    partner_type  AS パートナー区分 --メモ
FROM
    m_partner /* メモ */
WHERE
    partner_type = '2'
```

``` sql
SELECT
    M01.partner_type  AS パートナー区分,  --メモ
    M01.campus_id     AS CAMPUS_ID  --メモ
FROM
    m_partner AS M01
WHERE
    M01.partner_type = '2'
```

``` sql
SELECT
    F01.inquiry_no AS 引合番号,
    (SELECT M01_01.company_name FROM m_partner AS M01_01 WHERE F01.campus_id = M01_01.campus_id) AS 会社名
FROM
    f_product_no AS F01
```

