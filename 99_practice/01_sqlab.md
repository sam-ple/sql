- https://sqlab.net/
- PostgreSQL ver14.4

### SQLトライアル編

> 出版年(release_year)が不明の書籍一覧を取得してください。  

``` sql
SELECT *
FROM books
WHERE release_year IS NULL;
```

> 書籍一覧をページ数が多い順に並び替えてください。  
> 出力項目はname(書籍名)とtotal_page(総ページ数)です。  

``` sql
SELECT name, total_page
FROM books
ORDER BY total_page DESC;
```

> 書籍一覧をカテゴリー毎に集計して多い順に、カテゴリー名は昇順に並び替えてください。  
> データの取得件数は先頭から3件のみにしてください。  
> 出力項目はname(カテゴリー名)とnum(書籍数)です。  

``` sql
SELECT cat.name, COUNT(book.name) AS num
FROM categories AS cat
INNER JOIN book_categories AS book_cat
ON cat.id = book_cat.category_id
INNER JOIN books AS book
ON book_cat.book_id = book.id
GROUP BY cat.name
ORDER BY num DESC, cat.name ASC
LIMIT 3;
```

### SQL初級編

> 「マンガ」というキーワードを含む書籍一覧を取得してください。

``` sql
SELECT *
FROM books
WHERE name LIKE '%マンガ%';
```

> 発行年が明記されている書籍一覧を取得してください。

``` sql
SELECT *
FROM books
WHERE release_year IS NOT NULL;
```

> 総ページ数が300~400ページの書籍一覧を取得してください。

``` sql
SELECT *
FROM books
WHERE total_page BETWEEN 300 and 400;
```

> 発行年が2004, 2008, 2018年の書籍一覧を取得してください。

``` sql
SELECT *
FROM books
WHERE release_year IN (2004, 2008, 2018);
```

> 発行年が2000年以上で総ページ数が200ページ以下の書籍一覧を取得してください。

``` sql
SELECT *
FROM books
WHERE release_year >= 2000
AND total_page <= 200;
```

### SQL中級編

> 女性の著者数を取得してください。

``` sql
SELECT COUNT(*)
FROM authors
WHERE gender = '女性';
```

> 書籍の販売数(figure)の合計値を取得してください。

``` sql
SELECT SUM(figure)
FROM book_sales;
```

> 書籍の総ページ数の平均値を取得してください。

``` sql
SELECT AVG(total_page)
FROM books;
```

> 書籍の総ページ数の最大値、最小値を取得してください。

``` sql
SELECT MAX(total_page), MIN(total_page)
FROM books;
```

> 書籍一覧を発売年が新しい順に並び替えて取得してください。

``` sql
SELECT *
FROM books
ORDER BY release_year DESC;
```

> 発行年毎の書籍数を取得してください。また、書籍数は降順に並び替えてください。  
> 出力項目はrelease_year(発行年)とbooks_num(書籍数)です。

``` sql
SELECT release_year, COUNT(*) AS books_num
FROM books
GROUP BY release_year
ORDER BY books_num DESC;
```

> 発行年別の書籍数を取得してください。また、書籍数は降順に並び替え、書籍数が2つ以上のデータを取得してください。  
> 出力項目はrelease_year(発行年)とbooks_num(書籍数)です。

``` sql
SELECT release_year, COUNT(*) AS books_num
FROM books
GROUP BY release_year
HAVING COUNT(*) >= 2
ORDER BY books_num DESC;
```

> 書籍テーブルと著者テーブルを結合してください。
> 出力項目はbook_name(書籍名)とauthor_name(著者名)です。

``` sql
SELECT books.name AS book_name, authors.name AS author_name
FROM books
JOIN book_authors
ON books.id = book_authors.book_id
JOIN authors
ON book_authors.author_id = authors.id;
```

> 書籍名「コードと回路」より総ページ数の多い書籍一覧を取得してください。

``` sql
SELECT * 
FROM books 
WHERE total_page > (
  SELECT total_page 
  FROM books 
  WHERE name = 'コードと回路'
);
```

> 書籍名「時短レシピ100」「かもめ飛行」と発行年が同じ書籍一覧を取得してください。

``` sql
SELECT * 
FROM books
WHERE release_year IN (
  SELECT release_year 
  FROM books 
  WHERE name = '時短レシピ100' 
  OR name = 'かもめ飛行'
);
```

### SQLチャレンジ編


> 性別毎の著者数を取得してください。また、著者数は降順に並び替えてください。  
> 出力項目はgender(性別)とnum(著者数)です。

``` sql
SELECT gender, COUNT(*) AS num
FROM authors
GROUP BY gender
ORDER BY num DESC;
```

> 在庫がない書籍名の一覧を取得してください。  
> 出力項目はname(書籍名)です。

``` sql
SELECT name
FROM books
JOIN book_sales
ON books.id = book_sales.book_id
GROUP BY name
HAVING SUM(book_sales.stock) = 0;
```

> 店舗毎の書籍の売上(price × figure)を取得してください。また、店舗名は昇順に並び替えてください。  
> 出力項目はname(店舗名)とsales(売上)です。

``` sql
SELECT stores.name, SUM(book_sales.price * book_sales.figure) AS sales
FROM book_sales
JOIN stores
ON book_sales.store_id = stores.id
GROUP BY stores.name
ORDER BY stores.name;
```

> 書籍名と価格、消費税の一覧を取得してください。  
> 出力項目はname(書籍名)とprice(価格)、tax(消費税)です。

``` sql
SELECT books.name, book_sales.price, (book_sales.price * 1.1 - book_sales.price) AS tax
FROM books
JOIN book_sales
ON books.id = book_sales.book_id
JOIN stores
ON book_sales.store_id = stores.id;
```

> オンラインのみで購入できる書籍名の一覧を取得してください。  
> 出力項目はname(書籍名)です。

``` sql
SELECT name
FROM books
WHERE id NOT IN (
  SELECT DISTINCT books.id
  FROM books
  JOIN book_sales
  ON books.id = book_sales.book_id
  JOIN stores 
  ON book_sales.store_id = stores.id
  WHERE stores.name <> 'オンライン'
);
```

> 売上が多いカテゴリーTOP3を取得してください。売上はprice × figureで求められます。  
> 出力項目はname(カテゴリー名)とsales(売上)です。

``` sql
SELECT categories.name, SUM(book_sales.price * book_sales.figure) AS sales
FROM books
JOIN book_sales
ON books.id = book_sales.book_id
JOIN book_categories
ON books.id = book_categories.book_id
JOIN categories
ON book_categories.category_id = categories.id
GROUP BY categories.name
ORDER BY sales DESC
LIMIT 3;
```

> 出版タイトル数が多い著者TOP3を取得してください。また、著者名は昇順で並び替えてください。  
> 出力項目はname(著者名)とpublished_title_num(出版タイトル数)です。

``` sql
SELECT authors.name, COUNT(*) AS published_title_num
FROM books
JOIN book_authors
ON books.id = book_authors.book_id
JOIN authors
ON book_authors.author_id = authors.id
GROUP BY authors.name
ORDER BY published_title_num DESC, authors.name
LIMIT 3;
```

> 複数のカテゴリーに属している書籍名の一覧を取得してください。
> 出力項目はname(書籍名)です。

``` sql
SELECT books.name
FROM books
JOIN book_categories
ON books.id = book_categories.book_id
JOIN categories
ON book_categories.category_id = categories.id
GROUP BY books.name
HAVING COUNT(books.name) >= 2;
```

> 書籍名に「宇宙」または「星」を含んでおり、かつ著者が女性の書籍名を取得してください。  
> 出力項目はname(書籍名)です。

``` sql
SELECT books.name
FROM books
JOIN book_authors
ON books.id = book_authors.book_id
JOIN authors
ON book_authors.author_id = authors.id
WHERE (
  books.name LIKE '%宇宙%'
  OR books.name LIKE '%星%'
)
AND authors.gender = '女性';
```

> idが1のイベントを削除し、idが2のイベントの最大人数を200に更新してください。  
> イベントテーブルに「イベント名：古本まつり、最大人数：75人」のイベントを追加してください。  
> イベント一覧を取得してください。

``` sql
DELETE
FROM events
WHERE id = 1;

UPDATE events
SET max_num = 200
WHERE id = 2;

INSERT INTO events (id, name, max_num) VALUES (3, '古本まつり', 75);

SELECT *
FROM events;
```
