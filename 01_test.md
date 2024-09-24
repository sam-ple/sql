

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
