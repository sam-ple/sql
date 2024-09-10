> https://www.youtube.com/watch?v=kHROFydu3JA

```
SELECT * 
FROM categories 
INNER JOIN book_categories
ON categories.id = book_categories.category_id 
```

```
SELECT * 
FROM book_categories 
INNER JOIN categories
ON book_categories.category_id = categories.id 
```

```
SELECT * 
FROM book_categories 
RIGHT OUTER JOIN categories
ON book_categories.category_id = categories.id 
```

```
SELECT * 
FROM book_categories 
LEFT OUTER JOIN categories
ON book_categories.category_id = categories.id
```


SQLの基本を覚える【初心者向け】
https://qiita.com/chida09/items/d4b33a28b918958f267f

【これだけ覚えてたらOK！】SQL構文まとめ
https://qiita.com/tatsuya4150/items/69c2c9d318e5b93e6ccd

【新人教育 資料】SQLへの道 ?DB編?
https://qiita.com/devopsCoordinator/items/9b70e506150888e190be

OracleSQLパズル
https://oraclesqlpuzzle.ninja-web.net/

SQL演習問題 | なろう講習会
https://traptitech.github.io/naro-text/chapter1/section4/2_sql_exercise.html

