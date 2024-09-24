### M03：担当者マスタ

``` sql
SELECT
      M03.user_id            AS '担当者コード'
    , M03.user_name          AS '担当者名'
    , M03.user_name_alt      AS '担当者名（外国語）'
    , M03.department_code    AS '所属部門コード' -- コード管理（DEPARTMENT_CODE)
    , M03.email              AS 'E-mail'
    , M03.password           AS 'パスワード'
    , M03.role_group_id      AS 'ロールグループID' -- メニューの割り当て
    , M03.menu_id            AS 'メニューID'
    , M03.password_change_date    AS 'パスワード変更日' -- yyyy/mm/dd
    , M03.is_retired         AS '在職区分' -- 0:退職　1:在職
    , M03.supervisor_pic_code     AS '上長担当者コード' -- 承認依頼先の担当者コード
    , M03.is_approver        AS '承認者区分' -- 0:非承認者 1:承認者
    , M03.authority_type     AS '権限区分' -- 0:運用者　1:管理者
    , M03.created_at         AS '作成日'
    , M03.created_by         AS '作成者コード'
    , M03.created_pid        AS '作成プログラムID'
    , M03.updated_at         AS '更新日'
    , M03.updated_by         AS '更新者コード'
    , M03.updated_pid        AS '更新プログラムID'
    , M03.deleted_at         AS '削除日'
    , M03.deleted_by         AS '削除者コード'
    , M03.deleted_pid        AS '削除プログラムID'
FROM
    m_employee AS M03 -- 担当者マスタ(社内担当者を管理するマスタ)
```