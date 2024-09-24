### F05：実行管理テーブル（会計連携）

``` sql
SELECT
      F05.seq_no             AS '連番'
    , F05.interfaced_date    AS '連携日付'
    , F05.user_id            AS '担当者コード'
    , F05.start_date         AS '開始日'
    , F05.end_date           AS '終了日'
    , F05.receipt            AS '仕入'
    , F05.campus_usage       AS '利用料'
    , F05.sales              AS '売上'
    , F05.created_at         AS '作成日'
    , F05.created_by         AS '作成者コード'
    , F05.created_pid        AS '作成プログラムID'
    , F05.updated_at         AS '更新日'
    , F05.updated_by         AS '更新者コード'
    , F05.updated_pid        AS '更新プログラムID'
    , F05.deleted_at         AS '削除日'
    , F05.deleted_by         AS '削除者コード'
    , F05.deleted_pid        AS '削除プログラムID'
FROM
    f_account_if AS F05
;
```