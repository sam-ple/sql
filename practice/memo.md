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
