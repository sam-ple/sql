- https://www.db-fiddle.com/

### 01
- https://qiita.com/_hiro_dev/items/ece39759879c5d1f8536

``` sql
CREATE TABLE `students` (
  `id` int(4) unsigned zerofill NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `name` varchar(255) NOT NULL,
  `gender` varchar(1) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

INSERT INTO `students` (`id`, `name`, `gender`)
VALUES
    (0001, '長岡 一馬', '男'),
    (0002, '中本 知佳', '女'),
    (0003, '松本 義文', '男'),
    (0004, '佐竹 友香', '女');

CREATE TABLE `exam_results` (
  `student_id` int(4) unsigned zerofill NOT NULL,
  `subject` varchar(255) NOT NULL,
  `score` bigint(20) unsigned NOT NULL,
  PRIMARY KEY(`student_id`, `subject`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

INSERT INTO `exam_results` (`student_id`, `subject`, `score`)
VALUES
    (0001, '国語', 30),
    (0001, '英語', 30),
    (0002, '国語', 70),
    (0002, '数学', 80),
    (0003, '理科', 92),
    (0004, '社会', 90),
    (0004, '理科', 35),
    (0004, '英語', 22);
```

> 性別が男である生徒の名前を一覧で表示せよ。

``` sql
SELECT
    name
FROM
    students
WHERE
    gender = '男'
```

> 1教科でも30点以下の点数を取った生徒の名前を一覧で表示せよ。
> ただし、重複は許さないものとする。

``` sql
SELECT DISTINCT
    s.name
FROM
    exam_results er
INNER JOIN students s
    ON er.student_id = s.id
WHERE
    er.score <= 30
```

> 性別ごとに、最も高かった試験の点数を表示せよ。

``` sql
SELECT
    s.gender,
    MAX(er.score) AS max_score
FROM
    students s
INNER JOIN exam_results er
    ON s.id = er.student_id
GROUP BY
    s.gender
```

> 教科ごとの試験の平均点が50点以下である教科を表示せよ。

``` sql
SELECT
    subject,
    AVG(score) AS avg_score
FROM
    exam_results
GROUP BY
    subject
HAVING
    AVG(score) <= 50
```

> 試験結果テーブルの点数の右に、その教科の平均点を表示せよ。

``` sql
SELECT
    er1.*,
    (
        SELECT
            AVG(er2.score)
        FROM
            exam_results er2
        WHERE
            er1.subject = er2.subject
    ) AS subject_avg_score
FROM
    exam_results er1
```

``` sql
SELECT
    *,
    AVG(score) OVER(PARTITION BY subject) AS subject_avg_score
FROM
    exam_results
ORDER BY 
    student_id
# MySQL8.0以上でしか動かない
```

> 試験結果に理科が含まれない生徒の名前を一覧で表示せよ。

``` sql
SELECT
    s.name
FROM
    students s
INNER JOIN exam_results er
    ON s.id = er.student_id
GROUP BY
    s.id
HAVING
    SUM(CASE WHEN er.subject = '理科'  THEN 1 ELSE 0 END) = 0
```

``` sql
SELECT
    name
FROM
    students
WHERE 
    id NOT IN (
        SELECT
            student_id
        FROM
            exam_results
        WHERE
            subject = '理科'
    )
```

``` sql
SELECT
    name
FROM
    students s
WHERE 
    NOT EXISTS (
        SELECT
            *
        FROM
            exam_results
        WHERE
            subject = '理科'
            AND
            student_id = s.id
    )
```

### 02
- https://qiita.com/_hiro_dev/items/e16fa143487d2c3686c9

``` sql
CREATE TABLE `employees` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `name` varchar(255) NOT NULL,
  `department` varchar(255) NOT NULL,
  `hobby1` varchar(255),
  `hobby2` varchar(255),
  `hobby3` varchar(255)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

INSERT INTO `employees` (`id`, `name`, `department`, `hobby1`, `hobby2`, `hobby3`)
VALUES
	(1, '杉山 圭佑', '営業部', 'サッカー', 'ドライブ', '映画鑑賞'),
	(2, '佐藤 結菜', '人事部', '映画鑑賞', '旅行', 'インスタ'),
	(3, '高橋 絵里', '経理部', 'ゲーム', NULL, NULL),
	(4, '早川 良太', '人事部', 'ドライブ', '料理', NULL),
	(5, '佐藤 一弥', '経理部', NULL, NULL, NULL),
	(6, '佐藤 優穂', '営業部', 'インスタ', 'TikTok', NULL);
```

> 趣味に映画鑑賞が含まれる社員の名前を一覧で表示せよ。

``` sql
SELECT
	name
FROM
	employees
WHERE
	hobby1 = '映画鑑賞'
	OR
	hobby2 = '映画鑑賞'
	OR
	hobby3 = '映画鑑賞'
```

``` sql
SELECT
	name
FROM
	employees
WHERE
	'映画鑑賞' IN (hobby1, hobby2, hobby3)
```

> 趣味1～3を縦に表示せよ。

``` sql
SELECT name, hobby1 AS hobby FROM employees
UNION ALL
SELECT name, hobby2 FROM employees
UNION ALL
SELECT name, hobby3 FROM employees
```

> 名字が佐藤である社員の、趣味の数を表示せよ。

``` sql
SELECT
    name,
    COUNT(hobby1) + COUNT(hobby2) + COUNT(hobby3) AS hobby_count
FROM
    employees
WHERE
    name LIKE '佐藤 %'
GROUP BY
    name
```

``` sql
SELECT
	name,
    CASE WHEN hobby1 IS NOT NULL THEN 1 ELSE 0 END + CASE WHEN hobby2 IS NOT NULL THEN 1 ELSE 0 END + CASE WHEN hobby3 IS NOT NULL THEN 1 ELSE 0 END AS hobby_count
FROM
	employees
WHERE
	name LIKE '佐藤 %'
ORDER BY
	name
```

> 同じ趣味を持つ社員の一覧を表示せよ。  
> なお、氏名リストの並び順は社員番号の昇順で、区切り文字は「, 」とする。

``` sql
SELECT
    hobby,
    GROUP_CONCAT(name ORDER BY id separator ', ') AS name_list
FROM (
    SELECT id, name, hobby1 AS hobby FROM employees
    UNION ALL
    SELECT id, name, hobby2 FROM employees
    UNION ALL
    SELECT id, name, hobby3 FROM employees
) hobby_table
WHERE
    hobby IS NOT NULL
GROUP BY
    hobby
HAVING
    COUNT(*) > 1
```

### 03
- https://qiita.com/_hiro_dev/items/9e547f80a0388d68ec13

``` sql
CREATE TABLE `users` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `name` varchar(255) NOT NULL,
  `email` varchar(255) NOT NULL,
  `age` int(3) unsigned NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

INSERT INTO `users` (`id`, `name`, `email`, `age`)
VALUES
	(1, 'もっくん', 'mokkun@example.com', 19),
	(2, 'みみこ', 'mimiko@example.net', 20),
	(3, 'さくら', 'sakura@example.com', 31),
	(4, 'ひよこ', 'hiyoko@example1.jp', 23),
	(5, 'すずき', 'suzuki@example.jp', 28);

CREATE TABLE `follows` (
  `follower_id` int(11) unsigned NOT NULL,
  `followee_id` int(11) unsigned NOT NULL,
  PRIMARY KEY(`follower_id`, `followee_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

INSERT INTO `follows` (`follower_id`, `followee_id`)
VALUES
	(1, 2),
	(1, 3),
	(1, 4),
	(1, 5),
	(3, 1),
	(3, 2),
	(4, 5),
	(5, 1),
	(5, 2),
	(5, 3),
	(5, 4);
```

> さくらがフォローしているユーザーの名前を一覧で表示せよ。

``` sql
SELECT
	u2.name
FROM
	users u1
INNER JOIN follows f
	ON u1.id = f.follower_id
INNER JOIN users u2
	ON f.followee_id = u2.id
WHERE
	u1.id = 3
```

> 誰もフォローしていないユーザーの名前を表示せよ。

``` sql
SELECT
	u.name
FROM
	users u
LEFT JOIN follows f
	ON u.id = f.follower_id
WHERE
	f.follower_id IS NULL
```

> 10代、20代、30代といった年代別にフォロー数の平均を表示せよ。

``` sql
SELECT
	CONCAT((FLOOR(age / 10) * 10), '代') AS age_group,
	AVG(count) AS avg_count
FROM (
	SELECT
		u.age,
		SUM(CASE WHEN f.follower_id IS NOT NULL THEN 1 ELSE 0 END) AS count
	FROM
		users u
	LEFT JOIN follows f
		ON u.id = f.follower_id
	GROUP BY
		u.id
) follows_count
GROUP BY
	CONCAT((FLOOR(age / 10) * 10), '代')
```

``` sql
SELECT
    CONCAT(age_group * 10, '代') AS age_group,
    AVG(count) AS avg_count
FROM (
    SELECT
        FLOOR(age / 10) AS age_group,
        count(f.follower_id) AS count
    FROM
        users u
    LEFT JOIN follows f
        ON u.id = f.follower_id
    GROUP BY
        u.id
) follows_count
GROUP BY
    age_group
```

``` sql
SELECT
    CONCAT((FLOOR(age / 10) * 10), '代') AS age_group,
    SUM(CASE WHEN f.follower_id THEN 1 ELSE 0 END) / COUNT(DISTINCT u.id) AS avg_count
FROM
    users u
LEFT JOIN follows f
    ON  u.id = f.follower_id
GROUP BY
    CONCAT((FLOOR(age / 10) * 10), '代')
```

> 相互フォローしているユーザーのIDを表示せよ。  
> なお、重複は許さないものとする。

``` sql
SELECT
	CASE WHEN f1.follower_id > f1.followee_id THEN f1.followee_id ELSE f1.follower_id END AS id1,
	CASE WHEN f1.follower_id > f1.followee_id THEN f1.follower_id ELSE f1.followee_id END AS id2
FROM
	follows f1
INNER JOIN follows f2
	ON f1.follower_id = f2.followee_id
	AND f1.followee_id = f2.follower_id
GROUP BY
	CASE WHEN f1.follower_id > f1.followee_id THEN f1.followee_id ELSE f1.follower_id END,
	CASE WHEN f1.follower_id > f1.followee_id THEN f1.follower_id ELSE f1.followee_id END
```

``` sql
SELECT 
	f1.follower_id As id1,
	f1.followee_id As id2
FROM
	follows f1
INNER JOIN follows f2
	ON f1.follower_id = f2.followee_id
	AND f1.followee_id = f2.follower_id
WHERE
	f1.follower_id < f1.followee_id
```

``` sql
SELECT
	*
FROM
	follows f1
WHERE
	EXISTS (
		SELECT
			*
		FROM
			follows f2
		WHERE
			f1.follower_id = f2.followee_id
			AND f2.follower_id = f1.followee_id
	)
	AND f1.follower_id < f1.followee_id
```






