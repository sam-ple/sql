
### H06：連携データ履歴

``` sql
SELECT
      H06.interface_type AS '連携対象区分' --01:仕入データ 02:CAMPUS利用料 03:売上データ
    , H06.seq_no AS '連番' --連携用の連番（発番管理）
    , H06.row_no AS '行番号' --同一連携対象区分・連番内のレコード番号。1からの連番
    , H06.output_status AS '処理ステータス' --0:未出力 1:ファイル出力済み
    , H06.column_001 AS '伝票日付'
    , H06.column_002 AS '伝票No.'
    , H06.column_003 AS '借方科目ｺｰﾄﾞ'
    , H06.column_004 AS '借方内訳ｺｰﾄﾞ'
    , H06.column_005 AS '借方税ｺｰﾄﾞ'
    , H06.column_006 AS '借方部門ｺｰﾄﾞ'
    , H06.column_007 AS '借方金額'
    , H06.column_008 AS '借方消費税'
    , H06.column_009 AS '借方税区分'
    , H06.column_010 AS '借方摘要'
    , H06.column_011 AS '貸方科目ｺｰﾄﾞ'
    , H06.column_012 AS '貸方内訳ｺｰﾄﾞ'
    , H06.column_013 AS '貸方税ｺｰﾄﾞ'
    , H06.column_014 AS '貸方部門ｺｰﾄﾞ'
    , H06.column_015 AS '貸方金額'
    , H06.column_016 AS '貸方消費税'
    , H06.column_017 AS '貸方税区分'
    , H06.column_018 AS '項目018'
    , H06.column_019 AS '項目019'
    , H06.column_020 AS '項目020'
    , H06.column_021 AS '項目021'
    , H06.column_022 AS '項目022'
    , H06.column_023 AS '項目023'
    , H06.column_024 AS '項目024'
    , H06.column_025 AS '項目025'
    , H06.column_026 AS '項目026'
    , H06.column_027 AS '項目027'
    , H06.column_028 AS '項目028'
    , H06.column_029 AS '項目029'
    , H06.column_030 AS '項目030'
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
;
```
