### 会計連携一覧

``` sql
SELECT
    *
FROM
    h_if_data
;
```

``` sql
SELECT
      H06.interface_type AS '連携対象区分' -- 01:仕入データ 02:CAMPUS利用料 03:売上データ
    , CASE H06.interface_type
        WHEN 01 THEN '仕入データ'
        WHEN 02 THEN 'CAMPUS利用料'
        WHEN 03 THEN '売上データ'
        ELSE ''
      END AS '連携対象区分'
    , H06.seq_no AS '連番' -- 連携用の連番（発番管理）
    , H06.row_no AS '行番号' -- 同一連携対象区分・連番内のレコード番号。1からの連番
    , H06.output_status AS '処理ステータス' -- 0:未出力 1:ファイル出力済み
    , CASE H06.output_status
        WHEN 0 THEN '未出力'
        WHEN 1 THEN 'ファイル出力済'
        ELSE ''
      END AS '処理ステータス'
    , H06.column_001 AS '伝票日付'
    , H06.column_002 AS '伝票No.'
    , H06.column_003 AS '借方科目ｺｰﾄﾞ'
    , CASE H06.column_003
        WHEN 109 THEN '売掛金'
        WHEN 602 THEN '製品売上高'
        WHEN 638 THEN '外注加工費'
        WHEN 402 THEN '買掛金'
        ELSE ''
      END AS '借方科目'
    , H06.column_004 AS '借方内訳ｺｰﾄﾞ'
    , (SELECT M01_01.company_name FROM m_partner AS M01_01 WHERE '00' + H06.column_004 = M01_01.campus_id) AS '会社名'
    , H06.column_005 AS '借方税ｺｰﾄﾞ'
    , H06.column_006 AS '借方部門ｺｰﾄﾞ'
    , H06.column_007 AS '借方金額'
    , H06.column_008 AS '借方消費税'
    , H06.column_009 AS '借方税区分'
    , H06.column_010 AS '借方摘要'
    , H06.column_011 AS '貸方科目ｺｰﾄﾞ'
    , CASE H06.column_003
        WHEN 109 THEN '売掛金'
        WHEN 602 THEN '製品売上高'
        WHEN 638 THEN '外注加工費'
        WHEN 402 THEN '買掛金'
        ELSE ''
      END AS '貸方科目'
    , H06.column_012 AS '貸方内訳ｺｰﾄﾞ'
    , CASE H06.column_012
        WHEN 0 THEN '国内売上'
        WHEN 1 THEN '海外売上'
        ELSE ''
      END AS '貸方内訳'
    , H06.column_013 AS '貸方税ｺｰﾄﾞ'
    , CASE H06.column_013
        WHEN 112 THEN '国内'
        WHEN 130 THEN '海外'
        ELSE ''
      END AS '貸方内訳'
    , H06.column_014 AS '貸方部門ｺｰﾄﾞ'
    , H06.column_015 AS '貸方金額'
    , H06.column_016 AS '貸方消費税'
    , H06.column_017 AS '貸方税区分'
    , H06.created_at AS '作成日'
    , H06.created_by AS '作成者コード'
    , H06.created_pid AS '作成プログラムID'
    , H06.updated_at AS '更新日'
    , H06.updated_by AS '更新者コード'
    , H06.updated_pid AS '更新プログラムID'
    , H06.deleted_at AS '削除日'
    , H06.deleted_by AS '削除者コード'
    , H06.deleted_pid AS '削除プログラムID'
FROM
    h_if_data AS H06
WHERE
    H06.interface_type = '03'
;
```