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
    , M01.closing_date  AS '締日'    -- DD形式
    , M01.payment_site  AS 'サイト'    -- 1:翌月　2:翌々月
    , CASE M01.payment_site
        WHEN 1 THEN '翌月'
        WHEN 2 THEN '翌々月'
        WHEN 3 THEN '翌々々月'
        ELSE ''
      END AS 'サイト'
    , M01.payment_date  AS '支払日'
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

### 製番一覧

``` sql
SELECT
    *
FROM
    f_product_no
;
```

``` sql
SELECT
      F01.inquiry_no AS '引合番号'
    , F01.product_code AS '分類記号'
    , F01.branch_no AS '枝番'
    , F01.inquiry_no + F01.product_code AS 'まとめ製番'
    , F01.inquiry_no + F01.product_code + F01.branch_no AS '製番'
    , F01.campus_id AS 'CAMPUS-ID'
    , (SELECT M01_01.company_name FROM m_partner AS M01_01 WHERE F01.campus_id = M01_01.campus_id) AS '会社名'
    , (SELECT M01_02.company_name2 FROM m_partner AS M01_02 WHERE F01.campus_id = M01_02.campus_id) AS '会社略名'
    , F01.subject AS '件名'
    , F01.variety_code AS '品種コード'
    , F01.item_name AS '品名'
    , F01.status_code AS 'ステータス'
    , F01.quotation_issued_date AS '見積提出日'
    , F01.suggested_date AS '提案日'
    , F01.order_accuracy AS '受注確度'
    , F01.drawing_issue_plan_date AS '出図予定日'
    , F01.drawing_issued_date AS '出図日'
    , F01.drawing_inspection_date AS '検図日'
    , F01.cost_estimate_document_plan_date AS '原価資料作成予定日'
    , F01.cost_estimate_document_date AS '原価資料作成日'
    , F01.sales_pic_code AS '営業担当者コード'
    , (SELECT M03_01.user_name FROM m_employee AS M03_01 WHERE F01.sales_pic_code = M03_01.user_id) AS '営業担当'
    , F01.procure_pic_code AS '調達担当者コード'
    , (SELECT M03_02.user_name FROM m_employee AS M03_02 WHERE F01.procure_pic_code = M03_02.user_id) AS '調達担当'
    , F01.main_drawing_pic_code AS '設計担当者コード(主)'
    , (SELECT M03_03.user_name FROM m_employee AS M03_03 WHERE F01.main_drawing_pic_code = M03_03.user_id) AS '設計主担当'
    , F01.sub_drawing_pic_code AS '設計担当者コード(従)'
    , (SELECT M03_04.user_name FROM m_employee AS M03_04 WHERE F01.sub_drawing_pic_code = M03_04.user_id) AS '設計従担当'
    , F01.drawing_inspection_pic_code AS '検図担当者コード'
    , (SELECT M03_05.user_name FROM m_employee AS M03_05 WHERE F01.drawing_inspection_pic_code = M03_05.user_id) AS '検図担当'
    , F01.cost_estimate_pic_code AS '原価資料担当者コード'
    , (SELECT M03_06.user_name FROM m_employee AS M03_06 WHERE F01.cost_estimate_pic_code = M03_06.user_id) AS '原価資料担当'
    , F01.inspection_pic_code AS '検査組立担当者コード'
    , (SELECT M03_07.user_name FROM m_employee AS M03_07 WHERE F01.inspection_pic_code = M03_07.user_id) AS '検査担当'
    , F01.order_no AS '受注番号'
    , F01.estimate_date AS '概算見積日'
    , F01.inquiry_date AS '引合日'
    , F01.order_lost_date AS '失注日'
    , F01.order_date AS '受注日'
    , F01.cancel_date AS 'キャンセル日'
    , F01.due_date AS '納期'
    , F01.complete_plan_date AS '完成予定日'
    , F01.shipment_instruction_date AS '出荷指示日'
    , F01.shipment_plan_date AS '出荷予定日'
    , F01.shipment_date AS '出荷日'
    , F01.final_sales_no AS '売上番号_最終'
    , F01.final_sales_date AS '売上日_最終'
    , F01.last_acceptance_date AS '検収日_最終'
    , F01.last_accounting_connected_date AS '会計連携日_最終'
    , F01.receipt_complete_date AS '仕入完了日' -- 全部品の仕入が完了した日付
    , F01.order_quantity AS '受注数量'
    , F01.uom AS '単位'
    , F01.currency_rate AS 'レート'
    , F01.unit_price AS '単価'
    , F01.order_amount AS '受注金額(内貨)'
    , F01.order_amount_fc AS '受注金額(外貨)'
    , F01.po_num AS '発注件数'
    , F01.po_amount AS '発注金額'
    , F01.receipt_num AS '受入件数'
    , F01.receipt_quantity AS '受入数量'
    , F01.receipt_amount AS '受入金額'
    , F01.acceptance_quantity AS '検収数量'
    , F01.defective_quantity AS '不良数量'
    , F01.acceptance_amount AS '検収金額'
    , F01.shipment_num AS '出荷件数'
    , F01.shipment_quantity AS '出荷数量'
    , F01.shipment_amount AS '出荷金額'
    , F01.sales_quantity AS '売上数量'
    , F01.sales_amount AS '売上金額(内貨)'
    , F01.sales_amount_fc AS '売上金額(外貨)'
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
-- LIMIT 3
;
```

### 売上一覧

``` sql
SELECT
    *
FROM
    t_sales
;
```

``` sql
SELECT
      T20.sales_no AS '売上番号'
    , T20.inquiry_no AS '引合番号'
    , T20.product_code AS '分類記号'
    , T20.branch_no AS '枝番'
    , T20.order_no AS '受注番号'
    , T20.split_num AS '分納回数' -- 受注トランの検収明細の回数を更新
    , T20.subject AS '件名'
    , T20.campus_id AS 'CAMPUS-ID'
    , (SELECT M01_01.company_name FROM m_partner AS M01_01 WHERE T20.campus_id = M01_01.campus_id) AS '会社名'
    , (SELECT M01_02.company_name2 FROM m_partner AS M01_02 WHERE T20.campus_id = M01_02.campus_id) AS '会社略名'
    , T20.sales_pic_code AS '営業担当者コード'
    , (SELECT M03_01.user_name FROM m_employee AS M03_01 WHERE T20.sales_pic_code = M03_01.user_id) AS '営業担当'
    , T20.shipment_pic_code AS '出荷担当者コード'
    , (SELECT M03_02.user_name FROM m_employee AS M03_02 WHERE T20.shipment_pic_code = M03_02.user_id) AS '出荷担当'
    , T20.acceptance_pic_code AS '検収担当者コード'
    , (SELECT M03_03.user_name FROM m_employee AS M03_03 WHERE T20.acceptance_pic_code = M03_03.user_id) AS '検収担当'
    , T20.shipment_date AS '出荷日'
    , T20.acceptance_date AS '検収日'
    , T20.withdrawal_plan_date AS '回収予定日'
    , T20.quantity AS '数量'
    , T20.uom AS '単位'
    , T20.unit_price AS '単価'
    , T20.currency_rate AS 'レート'
    , T20.currency_code AS '通貨コード'
    , T20.sales_amount_fc AS '売上額（外貨）'
    , T20.sales_amount AS '売上額（内貨）'
    , T20.remarks AS '備考'
    , T20.tax_type AS '税区分' -- 0:通常 1:非課税　2:軽減税率 
    , T20.tax_rate AS '税率'
    , T20.tax_amount AS '税額'
    , T20.is_ship_approved AS '承認区分（出荷）' -- 0:対象外 1:承認待ち 2：承認 3:否決
    , CASE T20.is_ship_approved
        WHEN 0 THEN '対象外'
        WHEN 1 THEN '承認待ち'
        WHEN 2 THEN '承認'
        WHEN 3 THEN '否決'
        ELSE ''
      END AS '承認区分（出荷）'
    , T20.ship_approver_pic_code AS '承認者コード（出荷）'
    , (SELECT M03_04.user_name FROM m_employee AS M03_04 WHERE T20.ship_approver_pic_code = M03_04.user_id) AS '承認者（出荷）'
    , T20.ship_approved_date AS '承認日（出荷）'
    , T20.is_sales_approved AS '承認区分（検収）' -- 0:対象外 1:承認待ち 2：承認 3:否決 
    , CASE T20.is_sales_approved
        WHEN 0 THEN '対象外'
        WHEN 1 THEN '承認待ち'
        WHEN 2 THEN '承認'
        WHEN 3 THEN '否決'
        ELSE ''
      END AS '承認区分（検収）'
    , T20.sales_approver_pic_code AS '承認者コード（検収）'
    , (SELECT M03_05.user_name FROM m_employee AS M03_05 WHERE T20.sales_approver_pic_code = M03_05.user_id) AS '承認者（検収）'
    , T20.sales_approved_date AS '承認日（検収）'
    , T20.data_output_date AS '連携データ出力日'
    , T20.interface_no AS '連携連番'
    , T20.interface_user_id AS '担当者コード' -- 会計連携データ出力を起動した担当者
    , (SELECT M03_06.user_name FROM m_employee AS M03_06 WHERE T20.interface_user_id = M03_06.user_id) AS '連携者'
    , T20.issued_date AS '発行日'
    , T20.invoice_no AS '請求書番号'
    , T20.user_id AS '担当者コード'
    , (SELECT M03_07.user_name FROM m_employee AS M03_07 WHERE T20.user_id = M03_07.user_id) AS '担当者'
FROM
    t_sales AS T20 -- 売上トラン(製番明細別の売上のヘッダー情報を管理する)
;
```

### 仕入一覧

``` sql
SELECT
    *
FROM
    t_receipt
;
```

``` sql
SELECT
      T18.receipt_no AS '受入番号'
    , T18.inquiry_no AS '引合番号'
    , T18.product_code AS '分類記号'
    , T18.branch_no AS '枝番'
    , T18.po_no AS '発注番号'
    , T18.po_detail_no AS '発注明細番号'
    , T18.quotation_reply_no AS '見積回答番号'
    , T18.reply_due_date AS '見積回答 納期'
    , T18.reply_unit_price AS '見積回答 単価'
    , T18.reply_amount AS '見積回答 金額'
    , T18.parts_list_pattern AS 'パターン' -- 1:引合　2:受注
    , T18.split_num AS '分納回数' -- 不要と思われるがひとまず保持
    , T18.campus_id AS 'CAMPUS-ID'
    , (SELECT M01_01.company_name FROM m_partner AS M01_01 WHERE T18.campus_id = M01_01.campus_id) AS '会社名'
    , (SELECT M01_02.company_name2 FROM m_partner AS M01_02 WHERE T18.campus_id = M01_02.campus_id) AS '会社略名'
    , T18.row_no AS '行番号' -- 部品リスト行番号
    , T18.parts_category_code AS '部品分類コード'
    , T18.parts_name AS '部品名'
    , T18.drawing_no AS '図面番号／型式'
    , T18.surface_process_code AS '表面処理コード'
    , T18.drawing_pic_code AS '設計担当者コード'
    , (SELECT M03_01.user_name FROM m_employee AS M03_01 WHERE T18.drawing_pic_code = M03_01.user_id) AS '設計担当'
    , T18.inspection_pic_code AS '検査組立担当者コード'
    , (SELECT M03_02.user_name FROM m_employee AS M03_02 WHERE T18.inspection_pic_code = M03_02.user_id) AS '検査担当'
    , T18.receipt_pic_code AS '受入担当者コード'
    , (SELECT M03_03.user_name FROM m_employee AS M03_03 WHERE T18.receipt_pic_code = M03_03.user_id) AS '受入担当'
    , T18.acceptance_pic_code AS '検収担当者コード'
    , (SELECT M03_04.user_name FROM m_employee AS M03_04 WHERE T18.acceptance_pic_code = M03_04.user_id) AS '検収担当'
    , T18.acceptance_type AS '計上区分' -- 1:受入基準 2:検収基準
    , T18.is_fully_delivered AS '完納区分' -- 0:未納 1:完納
    , T18.quantity_type AS '数量指定区分' -- 1:検収数　2:不良数
    , T18.tax_type AS '税区分' -- 0:通常 1:非課税　2:軽減税率
    , T18.tax_rate AS '税率'
    , T18.receipt_date AS '受入日'
    , T18.acceptance_date AS '検収日'
    , T18.receipt_quantity AS '受入数量'
    , T18.defective_quantity AS '不良数'
    , T18.acceptance_quantity AS '検収数量'
    , T18.uom AS '単位'
    , T18.unit_price AS '単価'
    , T18.tax_amount AS '消費税額'
    , T18.amount AS '金額'
    , T18.campus_usage_amount AS 'CAMPUS利用料'
    , T18.campus_usage_tax_amount AS 'CAMPUS利用料税額'
    , T18.payment_plan_date AS '支払予定日'
    , T18.remarks AS '備考'
    , T18.acceptance_report_printed AS '検収書' -- 発行日を更新
    , T18.campus_usage_printed AS '利用料' -- 〃
    , T18.acceptance_printed_num AS '検収書発行回数' -- 検収書の発行回数
    , T18.usage_printed_num AS '利用料発行回数' -- CAMPUS利用料の発行回数
    , T18.start_plan_date AS '予定' -- 測定チェックシート入力結果
    , T18.end_plan_date AS '予定' -- 〃
    , T18.actual_start_date AS '実績' -- 〃
    , T18.actual_end_date AS '実績' -- 〃
    , T18.is_approved AS '承認区分' -- 0:対象外 1:承認待ち 2：承認 3:否決 
    , CASE T18.is_approved
        WHEN 0 THEN '対象外'
        WHEN 1 THEN '承認待ち'
        WHEN 2 THEN '承認'
        WHEN 3 THEN '否決'
        ELSE ''
      END AS '承認区分'
    , T18.approver_pic_code AS '承認者コード'
    , (SELECT M03_05.user_name FROM m_employee AS M03_05 WHERE T18.approver_pic_code = M03_05.user_id) AS '承認者'
    , T18.approved_date AS '承認日'
    , T18.data_output_date AS '連携データ出力日' -- 仕入連携データ出力日
    , T18.interface_no AS '連携連番' -- 連携データ連番
    , T18.user_id AS '担当者コード' -- 会計連携データ出力を起動した担当者
    , (SELECT M03_06.user_name FROM m_employee AS M03_06 WHERE T18.user_id = M03_06.user_id) AS '連携者'
    , T18.usage_data_output_date AS '利用料連携データ出力日' -- 利用料連携データ出力日
    , T18.usage_interface_no AS '利用料連携連番' -- 連携データ連番
    , T18.usage_user_id AS '利用料担当者コード' -- 会計連携データ出力を起動した担当者
    , (SELECT M03_07.user_name FROM m_employee AS M03_07 WHERE T18.usage_user_id = M03_07.user_id) AS '利用料担当'
FROM
    t_receipt AS T18 -- 仕入トラン(受入・検収実績を管理する)
;
```

### リスト出力一覧

``` sql
SELECT
    *
FROM
    m_list_config
WHERE
    deleted_at IS NULL;
;
```

### 会計連携一覧

``` sql
SELECT
    *
FROM
    h_if_data
;
```


