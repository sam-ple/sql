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
    , T18.product_code AS '区分記号'
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
