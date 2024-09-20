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
