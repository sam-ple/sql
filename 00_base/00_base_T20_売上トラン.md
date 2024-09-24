### T20：売上トラン

``` sql
SELECT
    *
FROM
    t_sales
;
```

``` sql
SELECT
      T20.sales_no                     AS '売上番号'
    , T20.inquiry_no                   AS '引合番号'
    , T20.product_code                 AS '分類記号'
    , T20.branch_no                    AS '枝番'
    , T20.order_no                     AS '受注番号'
    , T20.split_num                    AS '分納回数' -- 受注トランの検収明細の回数を更新
    , T20.subject                      AS '件名'
    , T20.campus_id                    AS 'CAMPUS-ID'
    , T20.sales_pic_code               AS '営業担当者コード'
    , T20.shipment_pic_code            AS '出荷担当者コード'
    , T20.acceptance_pic_code          AS '検収担当者コード'
    , T20.shipment_date                AS '出荷日'
    , T20.acceptance_date              AS '検収日'
    , T20.withdrawal_plan_date         AS '回収予定日'
    , T20.quantity                     AS '数量'
    , T20.uom                          AS '単位'
    , T20.unit_price                   AS '単価'
    , T20.currency_rate                AS 'レート'
    , T20.currency_code                AS '通貨コード'
    , T20.sales_amount_fc              AS '売上額（外貨）'
    , T20.sales_amount                 AS '売上額（内貨）'
    , T20.remarks                      AS '備考'
    , T20.tax_type                     AS '税区分' -- 0:通常 1:非課税　2:軽減税率 
    , T20.tax_rate                     AS '税率'
    , T20.tax_amount                   AS '税額'
    , T20.is_ship_approved             AS '承認区分（出荷）' -- 0:対象外 1:承認待ち 2：承認 3:否決 
    , T20.ship_approver_pic_code       AS '承認者コード（出荷）'
    , T20.ship_approved_date           AS '承認日（出荷）'
    , T20.is_sales_approved            AS '承認区分（検収）' -- 0:対象外 1:承認待ち 2：承認 3:否決 
    , T20.sales_approver_pic_code      AS '承認者コード（検収）'
    , T20.sales_approved_date          AS '承認日（検収）'
    , T20.data_output_date             AS '連携データ出力日'
    , T20.interface_no                 AS '連携連番'
    , T20.interface_user_id            AS '担当者コード' -- 会計連携データ出力を起動した担当者
    , T20.issued_date                  AS '発行日'
    , T20.invoice_no                   AS '請求書番号'
    , T20.user_id                      AS '担当者コード'
    , T20.created_at                   AS '作成日'
    , T20.created_by                   AS '作成者コード'
    , T20.created_pid                  AS '作成プログラムID'
    , T20.updated_at                   AS '更新日'
    , T20.updated_by                   AS '更新者コード'
    , T20.updated_pid                  AS '更新プログラムID'
    , T20.deleted_at                   AS '削除日'
    , T20.deleted_by                   AS '削除者コード'
    , T20.deleted_pid                  AS '削除プログラムID'
FROM
    t_sales AS T20 -- 売上トラン(製番明細別の売上のヘッダー情報を管理する)
```