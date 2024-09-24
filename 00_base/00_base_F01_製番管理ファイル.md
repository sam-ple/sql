### F01：製番管理ファイル

``` sql
SELECT
    *
FROM
    f_product_no
;
```

``` sql
SELECT
      ROW_NUMBER() OVER(ORDER BY inquiry_no ASC) AS '項番'
    , F01.inquiry_no                        AS '引合番号'
    , F01.product_code                      AS '区分記号'
    , F01.branch_no                         AS '枝番号'
    , F01.inquiry_no + F01.product_code AS 'まとめ番号'
    , F01.inquiry_no + F01.product_code + F01.branch_no AS '製造番号'
    , F01.campus_id                         AS 'CAMPUS-ID'
    , (SELECT M01_01.company_name FROM m_partner AS M01_01 WHERE F01.campus_id = M01_01.campus_id) AS '会社名'
    , (SELECT M01_02.company_name2 FROM m_partner AS M01_02 WHERE F01.campus_id = M01_02.campus_id) AS '会社略名'
    , F01.subject                           AS '件名'
    , F01.variety_code                      AS '品種コード'
    , F01.item_name                         AS '品名'
    , F01.status_code                       AS 'ステータス'
    , CASE F01.status_code
        WHEN  10 THEN '引合'
        WHEN  20 THEN '提案'
        WHEN  30 THEN '部品リスト（仮）'
        WHEN  40 THEN '見積依頼（概算）'
        WHEN  50 THEN '見積回答（概算）'
        WHEN  60 THEN '見積'
        WHEN  70 THEN '受注'
        WHEN  80 THEN '出図'
        WHEN  90 THEN '部品リスト（本）'
        WHEN 100 THEN '工程設計（調達）'
        WHEN 200 THEN '見積依頼（本）'
        WHEN 210 THEN '見積回答（本）'
        WHEN 220 THEN '発注'
        WHEN 230 THEN '工程設計（製造）'
        WHEN 300 THEN '受入'
        WHEN 310 THEN '不具合管理'
        WHEN 400 THEN '仕入'
        WHEN 410 THEN '仕入連携'
        WHEN 420 THEN '出荷'
        WHEN 430 THEN '売上'
        WHEN 440 THEN '売上連携'
        WHEN 900 THEN '失注'
        WHEN 910 THEN 'キャンセル'
        ELSE ''
      END AS 'ステータス'
    , F01.quotation_issued_date             AS '見積提出日'
    , F01.suggested_date                    AS '提案日'
    , F01.order_accuracy                    AS '受注確度'
    , F01.drawing_issue_plan_date           AS '出図予定日'
    , F01.drawing_issued_date               AS '出図日'
    , F01.drawing_inspection_date           AS '検図日'
    , F01.cost_estimate_document_plan_date  AS '原価資料作成予定日'
    , F01.cost_estimate_document_date       AS '原価資料作成日'
    , F01.sales_pic_code                    AS '営業担当者コード'
    , (SELECT M03_01.user_name FROM m_employee AS M03_01 WHERE F01.sales_pic_code = M03_01.user_id) AS '営業担当'
    , F01.procure_pic_code                  AS '調達担当者コード'
    , (SELECT M03_02.user_name FROM m_employee AS M03_02 WHERE F01.procure_pic_code = M03_02.user_id) AS '調達担当'
    , F01.main_drawing_pic_code             AS '設計担当者コード(主)'
    , (SELECT M03_03.user_name FROM m_employee AS M03_03 WHERE F01.main_drawing_pic_code = M03_03.user_id) AS '設計主担当'
    , F01.sub_drawing_pic_code              AS '設計担当者コード(従)'
    , (SELECT M03_04.user_name FROM m_employee AS M03_04 WHERE F01.sub_drawing_pic_code = M03_04.user_id) AS '設計従担当'
    , F01.drawing_inspection_pic_code       AS '検図担当者コード'
    , (SELECT M03_05.user_name FROM m_employee AS M03_05 WHERE F01.drawing_inspection_pic_code = M03_05.user_id) AS '検図担当'
    , F01.cost_estimate_pic_code            AS '原価資料担当者コード'
    , (SELECT M03_06.user_name FROM m_employee AS M03_06 WHERE F01.cost_estimate_pic_code = M03_06.user_id) AS '原価資料担当'
    , F01.inspection_pic_code               AS '検査組立担当者コード'
    , (SELECT M03_07.user_name FROM m_employee AS M03_07 WHERE F01.inspection_pic_code = M03_07.user_id) AS '検査担当'
    , F01.order_no                          AS '受注番号'
    , F01.estimate_date                     AS '概算見積日'
    , F01.inquiry_date                      AS '引合日'
    , F01.order_lost_date                   AS '失注日'
    , F01.order_date                        AS '受注日'
    , F01.cancel_date                       AS 'キャンセル日'
    , F01.due_date                          AS '納期'
    , F01.complete_plan_date                AS '完成予定日'
    , F01.shipment_instruction_date         AS '出荷指示日'
    , F01.shipment_plan_date                AS '出荷予定日'
    , F01.shipment_date                     AS '出荷日'
    , F01.final_sales_no                    AS '売上番号（最終）'
    , F01.final_sales_date                  AS '売上日（最終）'
    , F01.last_acceptance_date              AS '検収日（最終）'
    , F01.last_accounting_connected_date    AS '会計連携日（最終）'
    , F01.receipt_complete_date             AS '仕入完了日' -- 全部品の仕入が完了した日付
    , F01.is_inspection_report_required     AS '検査成績書区分' -- 1:検査成績書要
    , F01.order_quantity                    AS '受注数量'
    , F01.uom                               AS '単位'
    , F01.currency_rate                     AS 'レート'
    , F01.unit_price                        AS '単価'
    , F01.order_amount                      AS '受注金額（内貨）'
    , F01.order_amount_fc                   AS '受注金額（外貨）'
    , F01.po_num                            AS '発注件数'
    , F01.po_amount                         AS '発注金額'
    , F01.receipt_num                       AS '受入件数'
    , F01.receipt_quantity                  AS '受入数量'
    , F01.receipt_amount                    AS '受入金額'
    , F01.acceptance_quantity               AS '検収数量'
    , F01.defective_quantity                AS '不良数量'
    , F01.acceptance_amount                 AS '検収金額'
    , F01.shipment_num                      AS '出荷件数'
    , F01.shipment_quantity                 AS '出荷数量'
    , F01.shipment_amount                   AS '出荷金額'
    , F01.sales_quantity                    AS '売上数量'
    , F01.sales_amount                      AS '売上金額（内貨）'
    , F01.sales_amount_fc                   AS '売上金額（外貨）'
    , F01.created_at                        AS '作成日'
    , F01.created_by                        AS '作成者コード'
    , F01.created_pid                       AS '作成プログラムID'
    , F01.updated_at                        AS '更新日'
    , F01.updated_by                        AS '更新者コード'
    , F01.updated_pid                       AS '更新プログラムID'
    , F01.deleted_at                        AS '削除日'
    , F01.deleted_by                        AS '削除者コード'
    , F01.deleted_pid                       AS '削除プログラムID'
FROM
    f_product_no AS F01 -- 製番管理ファイル(製番の進捗や詳細を管理する)
--WHERE
--     F01.order_date >= '2024/08/01'
--     AND
--     F01.order_date <= '2024/08/31'
--     AND
--     F01.shipment_plan_date IS NULL
ORDER BY
--    F01.order_date DESC -- 受注日
    F01.due_date DESC -- 納期
;
```


