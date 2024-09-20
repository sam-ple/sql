`="    "&G5&" AS "&B5`
`="    , "&$B$5&"."&F8&" "&"AS"&" '"&D8&"'"&" --"&P8`

### テーブル一覧

``` sql
    s_control               AS S01 -- 制御マスタ(システムで一律となる情報を管理するマスタ)
    s_number                AS S02 -- 連番管理マスタ(システムで発番する連番を管理するマスタ)
    s_calendar              AS S03 -- カレンダーマスタ(システムで利用するカレンダーを管理するマスタ)
    s_menu                  AS S04 -- メニューマスタ(メニューIDを管理するマスタ)
    s_menu_apl              AS S05 -- メニュー機能マスタ(メニューIDに紐づくメニューや画面機能を管理するマスタ)
    s_application           AS S06 -- アプリケーションマスタ(アプリケーションのIDや他システムへのリンクを管理するマスタ)
    s_code_key              AS S07 -- 汎用コードキーマスタ(コードキーの名称と画面での編集可否を管理するマスタ)
    s_code                  AS S08 -- 汎用コードマスタ(コードキー配下のコードと値を管理するマスタ)
    s_search_col            AS S09 -- 検索項目マスタ(検索項目の項目設定)
    s_search_col_attribute  AS S10 -- 検索項目参照マスタ(検索項目のコードと名称を登録（リスト項目と名称）)
    s_search_keyword        AS S11 -- 検索キーワードマスタ(見積依頼の工場検索で利用するキーワードを管理するマスタ)
    s_feedback_base         AS S12 -- フィードバック基準値マスタ(年間フィードバック作成時の基準値を管理するマスタ)
    s_news                  AS S13 -- ニュースマスタ(画面右上に表示するニュースを管理するマスタ)
    s_password_resets       AS S14 -- パスワードリセット(パスワードリセット用のテーブル)

    m_partner               AS M01 -- パートナーマスタ(得意先と生産委託先を管理するマスタ)
    m_partner_user          AS M02 -- パートナー担当者マスタ(得意先と生産委託先の担当者情報を管理するマスタ)
    m_employee              AS M03 -- 担当者マスタ(社内担当者を管理するマスタ)
    m_item_category         AS M04 -- 製品情報マスタ(参考見積に利用)
    m_parts_category        AS M05 -- 部品分類マスタ(部品分類を管理するマスタ)
    m_product_code          AS M06 -- 分類記号マスタ(分類記号を管理するマスタ)
    m_email_template        AS M07 -- メール定型文マスタ(システムから送信するメールの件名や文章の定型文を管理するマスタ)
    m_partner_keyword       AS M08 -- パートナーキーワードマスタ(見積依頼入力のキーワードメンテ画面で登録した内容をパートナー別に管理するマスタ)
    m_currency_rate         AS M09 -- 通貨レートマスタ(通貨コード毎の最新為替レートを管理するマスタ)
    m_list_config           AS M10 -- リスト出力設定マスタ(リスト出力の設定情報を管理するマスタ)
    m_list_condition        AS M11 -- リスト出力条件マスタ(リスト出力時の条件を管理するマスタ)

    t_reference_estimate    AS T01 -- 参考見積トラン(参考見積入力で登録された内容と概算金額を管理する)
    t_quotation             AS T02 -- 見積トラン(見積入力で登録された見積のヘッダー情報を管理する)
    t_quotation_detail      AS T03 -- 見積トラン（明細）(見積入力で登録された見積の明細情報を管理する)
    t_inquiry               AS T04 -- 引合トラン(引合番号毎に引合情報を管理する)
    t_inquiry_receipt       AS T05 -- 引合トラン（受付）(引合番号＋分類記号で引合受け明細を管理する)
    t_order                 AS T06 -- 受注トラン(受注番号毎に受注情報を管理する)
    t_order_product         AS T07 -- 受注トラン（製番明細）(引合番号＋分類記号＋枝番で製番明細を管理する)
    t_pl                    AS T08 -- 部品リストトラン(製番別の部品リストのヘッダー情報を管理する)
    t_pl_detail             AS T09 -- 部品リストトラン（明細）(部品リストの部品明細情報を管理する)
    t_pl_filepath           AS T10 -- 設計図書ファイルパス(設計図書のフォルダを管理する)
    t_rfq                   AS T11 -- 見積依頼トラン(見積依頼のヘッダー情報を管理する)
    t_rfq_detail            AS T12 -- 見積依頼トラン（依頼情報）(見積依頼の部品＋依頼先（パートナー工場）の明細を管理する)
    t_rfq_reply             AS T13 -- 見積回答トラン(見積依頼明細への回答のヘッダー情報を管理する)
    t_rfq_reply_detail      AS T14 -- 見積回答トラン（回答明細）(見積依頼への回答の明細を管理する)
    t_po                    AS T15 -- 発注トラン(発注のヘッダー情報を管理する)
    t_po_detail             AS T16 -- 発注トラン（部品明細）(発注の部品＋仕入先（パートナー工場）の明細を管理する)
    t_po_detail_revision    AS T17 -- 発注トラン（変更明細）(発注明細の納期・金額変更履歴を管理する)
    t_receipt               AS T18 -- 仕入トラン(受入・検収実績を管理する)
    t_checksheet            AS T19 -- 測定チェックシートトラン(測定チェックシートの入力情報を管理する)
    t_sales                 AS T20 -- 売上トラン(製番明細別の売上のヘッダー情報を管理する)
    t_partner_search_result AS T21 -- グローバル検索結果情報(ウェブクローラーの検索結果を管理する)
    t_quotation_accessory   AS T22 -- 見積トラン（付属品明細）(見積入力で登録された付属品明細の情報を管理する)

    f_product_no            AS F01 -- 製番管理ファイル(製番の進捗や詳細を管理する)
    f_product_no_info       AS F02 -- 製番管理ファイル（連絡確認）(製番に関するメッセージを管理する)
    f_product_spec          AS F03 -- 製番管理ファイル（製品仕様）(製品仕様を管理する)
    f_partner_rate          AS F04 -- パートナー評価ファイル(パートナー工場の評価結果を管理する)
    f_account_if            AS F05 -- 実行管理テーブル（会計連携）(会計連携データの実行履歴を管理する)
    f_process_control       AS F06 -- 処理管理テーブル(重複処理制御・ロック用のテーブル)

    h_login                 AS H01 -- ログイン履歴(ログイン履歴を管理する)
    h_print                 AS H02 -- 印刷履歴(帳票印刷履歴を管理する)
    h_mail                  AS H03 -- メール送信履歴(メール送信履歴を管理する)
    h_file_extract          AS H04 -- 設計ファイル抽出履歴(設計ファイル抽出履歴を管理する)
    h_public_offer          AS H05 -- 公募閲覧履歴(公募閲覧履歴を管理する)
    h_if_data               AS H06 -- 連携データ履歴(外部システムへの出力データ履歴を管理する)
    h_file_upload           AS H04 -- ファイルアップロード履歴(GlobalPocketへのファイルアップロードAPI実行履歴を管理する)

    w_partner_rate          AS W02 -- パートナー評価ワーク(年間フィードバック集計処理のログおよび集計時に抽出した実績データを保持し加減点を編集)
```

### M01：パートナーマスタ

``` sql
SELECT
      M01.partner_type AS 'パートナー区分' -- 1:得意先　2:仕入先（生産委託先）
    , M01.campus_id AS 'CAMPUS-ID'
    , M01.company_name AS '会社名'
    , M01.company_name2 AS '会社名略称'
    , M01.postal_code AS '郵便番号'
    , M01.address1 AS '住所①'
    , M01.address2 AS '住所②'
    , M01.tel_no AS '電話番号'
    , M01.email AS 'E-mail'
    , M01.language_code AS '基軸言語コード' -- コード管理(LANUGAGE_CODE)
    , M01.currency_code AS '基軸通貨コード' -- レートマスタにてコードを管理
    , M01.member_registered_date AS '会員情報登録日' -- yyyy/mm/dd
    , M01.joined_date AS '入会日' -- yyyy/mm/dd
    , M01.member_type AS '会員区分' -- 1:有償会員、2:無償会員、3:非対象
    , M01.is_sendable AS 'メール送信不可区分' -- 0:送信可 1:送信不可（採用・不採用メール）
    , M01.resigned_date AS '退会日' -- CAMPUS退会日、論理削除
    , M01.register_no AS '登録番号' -- インボイス登録番号
    , M01.receipt_tax_deduction_no AS '仕入税額控除率' -- インボイス猶予措置対応、会計で対応    パートナー区分2（仕入先のみ）
    , M01.is_sns_icon_visible AS 'SNSアイコン表示' -- 0:表示 1:非表示
    , M01.is_excluded AS '抽出対象外' -- 0:対象　1:対象外
    , M01.remarks AS '備考'
    , M01.closing_date AS '締日' -- DD形式
    , M01.payment_site AS 'サイト' -- 1:翌月　2:翌々月　… 請求書に利用。
    , M01.payment_date AS '支払日' -- 請求書に利用
    , M01.created_at AS '作成日'
    , M01.created_by AS '作成者コード'
    , M01.created_pid AS '作成プログラムID'
    , M01.updated_at AS '更新日'
    , M01.updated_by AS '更新者コード'
    , M01.updated_pid AS '更新プログラムID'
    , M01.deleted_at AS '削除日'
    , M01.deleted_by AS '削除者コード'
    , M01.deleted_pid AS '削除プログラムID'
FROM
    m_partner AS M01 -- パートナーマスタ(得意先と生産委託先を管理するマスタ)
```

### M03：担当者マスタ

``` sql
SELECT
      M03.user_id AS '担当者コード'
    , M03.user_name AS '担当者名'
    , M03.user_name_alt AS '担当者名（外国語）'
    , M03.department_code AS '所属部門コード' -- コード管理（DEPARTMENT_CODE)
    , M03.email AS 'E-mail'
    , M03.password AS 'パスワード'
    , M03.role_group_id AS 'ロールグループID' -- メニューの割り当て
    , M03.menu_id AS 'メニューID'
    , M03.password_change_date AS 'パスワード変更日' -- yyyy/mm/dd
    , M03.is_retired AS '在職区分' -- 0:退職　1:在職
    , M03.supervisor_pic_code AS '上長担当者コード' -- 承認依頼先の担当者コード
    , M03.is_approver AS '承認者区分' -- 0:非承認者 1:承認者
    , M03.authority_type AS '権限区分' -- 0:運用者　1:管理者
    , M03.created_at AS '作成日'
    , M03.created_by AS '作成者コード'
    , M03.created_pid AS '作成プログラムID'
    , M03.updated_at AS '更新日'
    , M03.updated_by AS '更新者コード'
    , M03.updated_pid AS '更新プログラムID'
    , M03.deleted_at AS '削除日'
    , M03.deleted_by AS '削除者コード'
    , M03.deleted_pid AS '削除プログラムID'
FROM
    m_employee AS M03 -- 担当者マスタ(社内担当者を管理するマスタ)
```

### T18：仕入トラン

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
    , T18.row_no AS '行番号' -- 部品リスト行番号
    , T18.parts_category_code AS '部品分類コード'
    , T18.parts_name AS '部品名'
    , T18.drawing_no AS '図面番号／型式'
    , T18.surface_process_code AS '表面処理コード'
    , T18.drawing_pic_code AS '設計担当者コード'
    , T18.inspection_pic_code AS '検査組立担当者コード'
    , T18.receipt_pic_code AS '受入担当者コード'
    , T18.acceptance_pic_code AS '検収担当者コード'
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
    , T18.approver_pic_code AS '承認者コード'
    , T18.approved_date AS '承認日'
    , T18.data_output_date AS '連携データ出力日' -- 仕入連携データ出力日
    , T18.interface_no AS '連携連番' -- 連携データ連番
    , T18.user_id AS '担当者コード' -- 会計連携データ出力を起動した担当者
    , T18.usage_data_output_date AS '利用料連携データ出力日' -- 利用料連携データ出力日
    , T18.usage_interface_no AS '利用料連携連番' -- 連携データ連番
    , T18.usage_user_id AS '利用料担当者コード' -- 会計連携データ出力を起動した担当者
    , T18.created_at AS '作成日'
    , T18.created_by AS '作成者コード'
    , T18.created_pid AS '作成プログラムID'
    , T18.updated_at AS '更新日'
    , T18.updated_by AS '更新者コード'
    , T18.updated_pid AS '更新プログラムID'
    , T18.deleted_at AS '削除日'
    , T18.deleted_by AS '削除者コード'
    , T18.deleted_pid AS '削除プログラムID'
FROM
    t_receipt AS T18 -- 仕入トラン(受入・検収実績を管理する)
```

### T20：売上トラン

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
    , T20.sales_pic_code AS '営業担当者コード'
    , T20.shipment_pic_code AS '出荷担当者コード'
    , T20.acceptance_pic_code AS '検収担当者コード'
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
    , T20.ship_approver_pic_code AS '承認者コード（出荷）'
    , T20.ship_approved_date AS '承認日（出荷）'
    , T20.is_sales_approved AS '承認区分（検収）' -- 0:対象外 1:承認待ち 2：承認 3:否決 
    , T20.sales_approver_pic_code AS '承認者コード（検収）'
    , T20.sales_approved_date AS '承認日（検収）'
    , T20.data_output_date AS '連携データ出力日'
    , T20.interface_no AS '連携連番'
    , T20.interface_user_id AS '担当者コード' -- 会計連携データ出力を起動した担当者
    , T20.issued_date AS '発行日'
    , T20.invoice_no AS '請求書番号'
    , T20.user_id AS '担当者コード'
    , T20.created_at AS '作成日'
    , T20.created_by AS '作成者コード'
    , T20.created_pid AS '作成プログラムID'
    , T20.updated_at AS '更新日'
    , T20.updated_by AS '更新者コード'
    , T20.updated_pid AS '更新プログラムID'
    , T20.deleted_at AS '削除日'
    , T20.deleted_by AS '削除者コード'
    , T20.deleted_pid AS '削除プログラムID'
FROM
    t_sales AS T20 -- 売上トラン(製番明細別の売上のヘッダー情報を管理する)
```

### F01：製番管理ファイル

``` sql
SELECT
      F01.inquiry_no AS '引合番号'
    , F01.product_code AS '分類記号'
    , F01.branch_no AS '枝番'
    , F01.campus_id AS 'CAMPUS-ID'
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
    , F01.sales_pic_code AS '営業担当者コード' -- 受注入力より更新
    , F01.procure_pic_code AS '調達担当者コード' -- 受注入力より更新
    , F01.main_drawing_pic_code AS '設計担当者コード（主）' -- 設計情報入力より更新
    , F01.sub_drawing_pic_code AS '設計担当者コード（従）' -- 設計情報入力より更新
    , F01.drawing_inspection_pic_code AS '検図担当者コード' -- 設計情報入力より更新
    , F01.cost_estimate_pic_code AS '原価資料担当者コード' -- 設計情報入力より更新
    , F01.inspection_pic_code AS '検査組立担当者コード' -- 測定チェックシート入力の検査担当者
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
    , F01.final_sales_no AS '売上番号（最終）'
    , F01.final_sales_date AS '売上日（最終）'
    , F01.last_acceptance_date AS '検収日（最終）'
    , F01.last_accounting_connected_date AS '会計連携日（最終）'
    , F01.receipt_complete_date AS '仕入完了日' -- 全部品の仕入が完了した日付
    , F01.is_inspection_report_required AS '検査成績書区分' -- 1:検査成績書要
    , F01.order_quantity AS '受注数量'
    , F01.uom AS '単位'
    , F01.currency_rate AS 'レート'
    , F01.unit_price AS '単価'
    , F01.order_amount AS '受注金額（内貨）'
    , F01.order_amount_fc AS '受注金額（外貨）'
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
    , F01.sales_amount AS '売上金額（内貨）'
    , F01.sales_amount_fc AS '売上金額（外貨）'
    , F01.created_at AS '作成日'
    , F01.created_by AS '作成者コード'
    , F01.created_pid AS '作成プログラムID'
    , F01.updated_at AS '更新日'
    , F01.updated_by AS '更新者コード'
    , F01.updated_pid AS '更新プログラムID'
    , F01.deleted_at AS '削除日'
    , F01.deleted_by AS '削除者コード'
    , F01.deleted_pid AS '削除プログラムID'
FROM
    f_product_no AS F01 -- 製番管理ファイル(製番の進捗や詳細を管理する)
```


