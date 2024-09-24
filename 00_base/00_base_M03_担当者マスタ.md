### M03：担当者マスタ

``` sql
SELECT
      M03.user_id                 AS '担当者コード'
    , M03.user_name               AS '担当者名'
    , M03.user_name_alt           AS '担当者名（外国語）'
    , M03.department_code         AS '所属部門コード' -- コード管理（DEPARTMENT_CODE)
    , M03.email                   AS 'E-mail'
    , M03.password                AS 'パスワード'
    , M03.role_group_id           AS 'ロールグループID' -- メニューの割り当て
    , M03.menu_id                 AS 'メニューID'
    , M03.password_change_date    AS 'パスワード変更日' -- yyyy/mm/dd
    , M03.is_retired              AS '在職区分' -- 0:退職　1:在職
    , CASE M03.is_retired
        WHEN 0 THEN '退職'
        WHEN 1 THEN '在職'
        ELSE ''
      END AS '在職区分'
    , M03.supervisor_pic_code     AS '上長担当者コード' -- 承認依頼先の担当者コード
    , (SELECT M03_01.user_name FROM m_employee AS M03_01 WHERE M03.supervisor_pic_code = M03_01.user_id) AS '上長'
    , M03.is_approver             AS '承認者区分' -- 0:非承認者 1:承認者
    , CASE M03.is_approved
        WHEN 0 THEN '非承認者'
        WHEN 1 THEN '承認者'
        ELSE ''
      END AS '承認者区分'
    , M03.authority_type          AS '権限区分' -- 0:運用者　1:管理者
    , CASE M03.authority_type
        WHEN 0 THEN '運用者'
        WHEN 1 THEN '管理者'
        ELSE ''
      END AS '権限区分'
    , M03.created_at              AS '作成日'
    , M03.created_by              AS '作成者コード'
    , (SELECT M03_90.user_name FROM m_employee AS M03_90 WHERE M03.created_by = M03_90.user_id) AS '作成者'
    , M03.created_pid             AS '作成プログラムID'
    , M03.updated_at              AS '更新日'
    , M03.updated_by              AS '更新者コード'
    , (SELECT M03_91.user_name FROM m_employee AS M03_91 WHERE M03.updated_by = M03_91.user_id) AS '更新者'
    , M03.updated_pid             AS '更新プログラムID'
    , M03.deleted_at              AS '削除日'
    , M03.deleted_by              AS '削除者コード'
    , (SELECT M03_92.user_name FROM m_employee AS M03_92 WHERE M03.deleted_by = M03_92.user_id) AS '削除者'
    , M03.deleted_pid             AS '削除プログラムID'
FROM
    m_employee AS M03 -- 担当者マスタ(社内担当者を管理するマスタ)
```