記　号		日本語名称			テーブル名												
T18		仕入トラン			t_receipt												
                                                                    
№	項目分類	　項　目　名		項目ID		 ﾀｲﾌﾟ	桁	数	NOT NULL		初期値			備　　　　考			
001	PK	受入番号		receipt_no		varchar	10		Y								
002		引合番号		inquiry_no		varchar	10		Y								
003		分類記号		product_code		char	2		Y								
004		枝番		branch_no		varchar	3		Y								
005	発注	発注番号		po_no		varchar	10		Y								
006		発注明細番号		po_detail_no		int			Y								
007	見積回答	見積回答番号		quotation_reply_no		varchar	10										
008		見積回答 納期		reply_due_date		datetime											
009		見積回答 単価		reply_unit_price		decimal	15	4	Y		0						
010		見積回答 金額		reply_amount		decimal	15	4	Y		0						
011	部品リスト	パターン		parts_list_pattern		char	1		Y					1:引合　2:受注			
012		分納回数		split_num		int			Y		0			不要と思われるがひとまず保持			
013		CAMPUS-ID		campus_id		char	8		Y								
014		行番号		row_no		int			Y					部品リスト行番号			
015		部品分類コード		parts_category_code		varchar	10		Y								
016		部品名		parts_name		nvarchar	200		Y								
017		図面番号／型式		drawing_no		nvarchar	30										
018		表面処理コード		surface_process_code		varchar	4										
019	担当者	設計担当者コード		drawing_pic_code		char	6		Y								
020		検査組立担当者コード		inspection_pic_code		char	6		Y								
021		受入担当者コード		receipt_pic_code		char	6		Y								
022		検収担当者コード		acceptance_pic_code		char	6										
023	計上区分・	計上区分		acceptance_type		char	1		Y					1:受入基準 2:検収基準			
024	税率	完納区分		is_fully_delivered		char	1		Y		0			0:未納 1:完納			
025		数量指定区分		quantity_type		char	1		Y		0			1:検収数　2:不良数			
026		税区分		tax_type		char	1		Y					0:通常 1:非課税　2:軽減税率			
027		税率		tax_rate		decimal	15	4	Y		0						
028	受入実績	受入日		receipt_date		datetime			Y								
029		検収日		acceptance_date		datetime											
030		受入数量		receipt_quantity		decimal	15	4	Y		0						
031		不良数		defective_quantity		decimal	15	4	Y		0						
032		検収数量		acceptance_quantity		decimal	15	4	Y		0						
033		単位		uom		nvarchar	4		Y								
034		単価		unit_price		decimal	15	4	Y		0						
035		消費税額		tax_amount		decimal	15	4	Y		0						
036		金額		amount		decimal	15	4	Y		0						
037		CAMPUS利用料		campus_usage_amount		decimal	15	4	Y		0						
038		CAMPUS利用料税額		campus_usage_tax_amount		decimal	15	4	Y		0						
039		支払予定日		payment_plan_date		datetime											
040		備考		remarks		nvarchar	500										
041	発行管理	検収書		acceptance_report_printed		datetime								発行日を更新			
042		利用料		campus_usage_printed		datetime								　　〃			
043		検収書発行回数		acceptance_printed_num		int			Y		0			検収書の発行回数			
044		利用料発行回数		usage_printed_num		int			Y		0			CAMPUS利用料の発行回数			
045	測定情報	予定	開始日	start_plan_date		datetime								測定チェックシート入力結果			
046		予定	終了日	end_plan_date		datetime								〃			
047		実績	開始日	actual_start_date		datetime								〃			
048		実績	終了日	actual_end_date		datetime								〃			
049	承認情報	承認区分		is_approved		char	1		Y		0			0:対象外 1:承認待ち 2：承認 3:否決 			
050		承認者コード		approver_pic_code		char	6										
051		承認日		approved_date		datetime											
052	会計連携	連携データ出力日		data_output_date		datetime								仕入連携データ出力日			
053	（仕入）	連携連番		interface_no		varchar	10							連携データ連番			
054		担当者コード		user_id		char	6							会計連携データ出力を起動した担当者			
055	会計連携	利用料連携データ出力日		usage_data_output_date		datetime								利用料連携データ出力日			
056	（利用料）	利用料連携連番		usage_interface_no		varchar	10							連携データ連番			
057		利用料担当者コード		usage_user_id		varchar	6							会計連携データ出力を起動した担当者			
058		作成日		created_at		datetime											
059		作成者コード		created_by		char	6										
060		作成プログラムID		created_pid		varchar	30										
061		更新日		updated_at		datetime											
062		更新者コード		updated_by		char	6										
063		更新プログラムID		updated_pid		varchar	30										
064		削除日		deleted_at		datetime											
065		削除者コード		deleted_by		char	6										
066		削除プログラムID		deleted_pid		varchar	30												

を貼り付けたら


``` sql
-- T18：仕入トラン		
SELECT
      T18.receipt_no                   AS '受入番号'
    , T18.inquiry_no                   AS '引合番号'
    , T18.product_code                 AS '分類記号（区分記号）'
    , T18.branch_no                    AS '枝番'
    , T18.po_no                        AS '発注番号'
    , T18.po_detail_no                 AS '発注明細番号'
    , T18.quotation_reply_no           AS '見積回答番号'
    , T18.reply_due_date               AS '見積回答 納期'
    , T18.reply_unit_price             AS '見積回答 単価'
    , T18.reply_amount                 AS '見積回答 金額'
    , T18.parts_list_pattern           AS 'パターン' -- 1:引合　2:受注
    , T18.split_num                    AS '分納回数' -- 不要と思われるがひとまず保持
    , T18.campus_id                    AS 'CAMPUS-ID'
    , T18.row_no                       AS '行番号' -- 部品リスト行番号
    , T18.parts_category_code          AS '部品分類コード'
    , T18.parts_name                   AS '部品名'
    , T18.drawing_no                   AS '図面番号／型式'
    , T18.surface_process_code         AS '表面処理コード'
    , T18.drawing_pic_code             AS '設計担当者コード'
    , T18.inspection_pic_code          AS '検査組立担当者コード'
    , T18.receipt_pic_code             AS '受入担当者コード'
    , T18.acceptance_pic_code          AS '検収担当者コード'
    , T18.acceptance_type              AS '計上区分' -- 1:受入基準 2:検収基準
    , T18.is_fully_delivered           AS '完納区分' -- 0:未納 1:完納
    , T18.quantity_type                AS '数量指定区分' -- 1:検収数　2:不良数
    , T18.tax_type                     AS '税区分' -- 0:通常 1:非課税　2:軽減税率
    , T18.tax_rate                     AS '税率'
    , T18.receipt_date                 AS '受入日'
    , T18.acceptance_date              AS '検収日'
    , T18.receipt_quantity             AS '受入数量'
    , T18.defective_quantity           AS '不良数'
    , T18.acceptance_quantity          AS '検収数量'
    , T18.uom                          AS '単位'
    , T18.unit_price                   AS '単価'
    , T18.tax_amount                   AS '消費税額'
    , T18.amount                       AS '金額'
    , T18.campus_usage_amount          AS 'CAMPUS利用料'
    , T18.campus_usage_tax_amount      AS 'CAMPUS利用料税額'
    , T18.payment_plan_date            AS '支払予定日'
    , T18.remarks                      AS '備考'
    , T18.acceptance_report_printed    AS '検収書' -- 発行日を更新
    , T18.campus_usage_printed         AS '利用料' -- 〃
    , T18.acceptance_printed_num       AS '検収書発行回数' -- 検収書の発行回数
    , T18.usage_printed_num            AS '利用料発行回数' -- CAMPUS利用料の発行回数
    , T18.start_plan_date              AS '予定' -- 測定チェックシート入力結果
    , T18.end_plan_date                AS '予定' -- 〃
    , T18.actual_start_date            AS '実績' -- 〃
    , T18.actual_end_date              AS '実績' -- 〃
    , T18.is_approved                  AS '承認区分' -- 0:対象外 1:承認待ち 2：承認 3:否決 
    , T18.approver_pic_code            AS '承認者コード'
    , T18.approved_date                AS '承認日'
    , T18.data_output_date             AS '連携データ出力日' -- 仕入連携データ出力日
    , T18.interface_no                 AS '連携連番' -- 連携データ連番
    , T18.user_id                      AS '担当者コード' -- 会計連携データ出力を起動した担当者
    , T18.usage_data_output_date       AS '利用料連携データ出力日' -- 利用料連携データ出力日
    , T18.usage_interface_no           AS '利用料連携連番' -- 連携データ連番
    , T18.usage_user_id                AS '利用料担当者コード' -- 会計連携データ出力を起動した担当者
    , T18.created_at                   AS '作成日'
    , T18.created_by                   AS '作成者コード'
    , T18.created_pid                  AS '作成プログラムID'
    , T18.updated_at                   AS '更新日'
    , T18.updated_by                   AS '更新者コード'
    , T18.updated_pid                  AS '更新プログラムID'
    , T18.deleted_at                   AS '削除日'
    , T18.deleted_by                   AS '削除者コード'
    , T18.deleted_pid                  AS '削除プログラムID'
FROM
    t_receipt AS T18
;
``` 

を出力したい。