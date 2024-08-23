- https://tech.pjin.jp/blog/tag/sql%E7%B7%B4%E7%BF%92%E5%95%8F%E9%A1%8C/page/8/
- https://www.db-fiddle.com/
- https://boost-tool.com/ja/tools/md_table

``` sql
SELECT
 * 
FROM
 countries
# goals
# pairings
# players
LIMIT
 3
```
### 構造

#### countries

|id|name|ranking|group_name|
|:----|:----|:----|:----|
|1|ブラジル|3|A|
|2|メキシコ|20|A|
|3|カメルーン|56|A|

#### goals

|id|pairing_id|player_id|goal_time|
|:----|:----|:----|:----|
|1|122|8|前半7分|
|2|122|8|前半7分|
|3|113|10|前半18分|

#### pairings

|id|kickoff|my_country_id|enemy_country_id|
|:----|:----|:----|:----|
|1|2014-06-13 05:00:00|1|4|
|2|2014-06-14 01:00:00|2|3|
|3|2014-06-14 04:00:00|5|6|

#### players

|id|country_id|uniform_num|position|name|club|birth|height|weight|
|:----|:----|:----|:----|:----|:----|:----|:----|:----|
|1|1|12|GK|ジュリオセザール|トロント（カナダ）|1979-09-03|186|79|
|2|1|1|GK|ジェフェルソン|ボタフォゴ（ブラジル）|1983-01-02|188|80|
|3|1|22|GK|ビクトル|アトレチコ・ミネイロ（ブラジル）|1983-01-21|193|84|


### 01
> 問題：各グループの中でFIFAランクが最も高い国と低い国のランキング番号を表示してください。

``` sql
SELECT group_name AS グループ, MIN(ranking) AS ランキング最上位, MAX(ranking) AS ランキング最下位
FROM countries
GROUP BY group_name
```

### 02
> 問題：全ゴールキーパーの平均身長、平均体重を表示してください

``` sql
SELECT AVG(height) AS 平均身長, AVG(weight) AS 平均体重
FROM players
WHERE position = 'GK'
```

### 03
> 問題：各国の平均身長を高い方から順に表示してください。ただし、FROM句はcountriesテーブルとしてください。

``` sql
SELECT c.name AS 国名, AVG(p.height) AS 平均身長
FROM countries c
JOIN players p ON p.country_id = c.id
GROUP BY c.id, c.name
ORDER BY AVG(p.height) DESC
```

### 04

> 問題：各国の平均身長を高い方から順に表示してください。ただし、FROM句はplayersテーブルとして、テーブル結合を使わず副問合せを用いてください。

``` sql
SELECT (SELECT c.name FROM countries c WHERE p.country_id = c.id) AS 国名, AVG(p.height) AS 平均身長
FROM players p
GROUP BY p.country_id
ORDER BY AVG(p.height) DESC
```

### 05

> 問題：キックオフ日時と対戦国の国名をキックオフ日時の早いものから順に表示してください。

``` sql
SELECT kickoff AS キックオフ日時, c1.name AS 国名1, c2.name AS 国名2
FROM pairings p
LEFT JOIN countries c1 ON p.my_country_id = c1.id
LEFT JOIN countries c2 ON p.enemy_country_id = c2.id
ORDER BY kickoff
```

### 06

> 問題：すべての選手を対象として選手ごとの得点ランキングを表示してください。（SELECT句で副問合せを使うこと）

``` sql
SELECT p.name AS 名前, p.position AS ポジション, p.club AS 所属クラブ, 
    (SELECT COUNT(id) FROM goals g WHERE g.player_id = p.id) AS ゴール数
FROM players p
ORDER BY ゴール数 DESC
```

### 07

> 問題：すべての選手を対象として選手ごとの得点ランキングを表示してください。（テーブル結合を使うこと）

``` sql
SELECT p.name AS 名前, p.position AS ポジション, p.club AS 所属クラブ, 
    COUNT(g.id) AS ゴール数
FROM players p
LEFT JOIN goals g ON g.player_id = p.id
GROUP BY p.id, p.name, p.position, p.club
ORDER BY ゴール数 DESC
```

### 68

> 問題：
全選手の平均身長より低い選手をすべて抽出してください。表示する列は、背番号、ポジション、名前、身長としてください。

``` sql
SELECT uniform_num, position, name, height
FROM players
WHERE height < (SELECT AVG(height) FROM players)
```

### 69

> 問題：
各グループの最上位と最下位を表示し、その差が50より大きいグループを抽出してください。

``` sql
SELECT group_name, MAX(ranking), MIN(ranking)
FROM countries
GROUP BY group_name
HAVING MAX(ranking) -  MIN(ranking) > 50
```

### 70

> 問題：
1980年生まれと、1981年生まれの選手が何人いるか調べてください。ただし、日付関数は使用せず、UNION句を使用してください。

``` sql
SELECT '1980' AS '誕生年', COUNT(id)
FROM players
WHERE birth BETWEEN '1980-1-1' AND '1980-12-31'
UNION
SELECT '1981', COUNT(id)
FROM players
WHERE birth BETWEEN '1981-1-1' AND '1981-12-31'
```


### 71

> 問題：
身長が195㎝より大きいか、体重が95kgより大きい選手を抽出してください。ただし、どちらの条件にも合致する場合には2件分のデータとして抽出してください。また、結果はidの昇順としてください。

``` sql
SELECT id, position, name, height, weight
FROM players
WHERE height > 195
UNION ALL
SELECT id, position, name, height, weight
FROM players
WHERE weight > 95
ORDER BY id
```

<!--
###

>

``` sql

```
-->


