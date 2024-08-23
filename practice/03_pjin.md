- https://tech.pjin.jp/blog/tag/sql%E7%B7%B4%E7%BF%92%E5%95%8F%E9%A1%8C/page/8/
- https://www.db-fiddle.com/
- https://boost-tool.com/ja/tools/md_table

``` sql
SELECT
 * 
FROM
 countries
# goals
# goals_tmp
# pairings
# pairings_tmp
# players
# players_tmp
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

#### goals_tmp

|my_country|enemy_country|goal_time|player_name|
|:----|:----|:----|:----|
|クロアチア|ブラジル|前半11分| |
|ブラジル|クロアチア|前半29分|ネイマール|
|ブラジル|クロアチア|後半26分|ネイマール|

#### pairings

|id|kickoff|my_country_id|enemy_country_id|
|:----|:----|:----|:----|
|1|2014-06-13 05:00:00|1|4|
|2|2014-06-14 01:00:00|2|3|
|3|2014-06-14 04:00:00|5|6|

#### pairings_tmp

|kickoff|my_country|enemy_country|
|:----|:----|:----|
|2014-06-13 05:00:00|ブラジル|クロアチア|
|2014-06-14 01:00:00|メキシコ|カメルーン|
|2014-06-14 04:00:00|スペイン|オランダ|

#### players

|id|country_id|uniform_num|position|name|club|birth|height|weight|
|:----|:----|:----|:----|:----|:----|:----|:----|:----|
|1|1|12|GK|ジュリオセザール|トロント（カナダ）|1979-09-03|186|79|
|2|1|1|GK|ジェフェルソン|ボタフォゴ（ブラジル）|1983-01-02|188|80|
|3|1|22|GK|ビクトル|アトレチコ・ミネイロ（ブラジル）|1983-01-21|193|84|

#### players_tmp

|country|uniform_num|position|name|club|birth|height|weight|
|:----|:----|:----|:----|:----|:----|:----|:----|
|ブラジル|12|GK|ジュリオセザール|トロント（カナダ）|1979-09-03|186|79|
|ブラジル|1|GK|ジェフェルソン|ボタフォゴ（ブラジル）|1983-01-02|188|80|
|ブラジル|22|GK|ビクトル|アトレチコ・ミネイロ（ブラジル）|1983-01-21|193|84|


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

<!--
###

>

``` sql

```
-->


