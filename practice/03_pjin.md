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

<!--
###

>

``` sql

```
-->

### 66

> 各ポジションごと（GK、FWなど）に最も身長と、その選手名、所属クラブを表示してください。ただし、FROM句に副問合せを使用してください。

``` sql
SELECT p1.position, p1.最大身長, p2.name, p2.club
FROM (
    SELECT position, MAX(height) AS 最大身長
    FROM players
    GROUP BY position
    ) p1
LEFT JOIN players p2 ON p1.最大身長 = p2.height AND p1.position = p2.position
```

SQLを覚えたばかりの方がよくしてしまうのが以下のような間違いです。

``` sql
SELECT position, MAX(height) AS 最大身長, name, club
FROM players
GROUP BY position
```

これは、「グループ化しているとき（GROUP BY句を記述しているとき）は、SELECT句にはグループ関数を用いた列かGROUP BY句で指定した列しか記述できない」というルールに反してしまっています。  
（SELECT句にname、clubがあるのが誤りです。）  
MySQLではこのような誤ったSQLもエラーにならず実行できてしまいますが、構文間違いとなりますので注意しましょう。

### 67

> 各ポジションごと（GK、FWなど）に最も身長と、その選手名を表示してください。ただし、SELECT句に副問合せを使用してください。

``` sql
SELECT p1.position, MAX(p1.height) AS 最大身長, 
    (
    SELECT p2.name
    FROM players p2
    WHERE MAX(p1.height) = p2.height AND p1.position = p2.position
    ) AS 名前
FROM players p1
GROUP BY p1.position
```

問題66で使用した副問合せを、FROM句ではなくSLECT句に使用するパターンです。このパターンで注意しなくてはならないのは、playersテーブル内のデータ構造です。今回使用しているサンプルデータでは問題なく実行することが可能ですが、仮に同じポジションで同じ最大身長の選手が2名以上いる場合には、実行するとエラーとなってしまいます。  
SELECT句に副問合せを記述する場合には、その副問合せでは1件しかデータを返さないようにしないといけません。複数件のデータが返るようなデータ構造ならば、前問のようにFROM句での副問合せを使用しましょう。

### 68

> 問題：
全選手の平均身長より低い選手をすべて抽出してください。表示する列は、背番号、ポジション、名前、身長としてください。

``` sql
SELECT uniform_num, position, name, height
FROM players
WHERE height < (SELECT AVG(height) FROM players)
```

平均身長を求めるためにはグループ関数AVGを使用する必要があります。この結果を使ってWHERE句に条件を設定することで目的を達成することができます。このようにWHERE句に副問合せを使用する場合には、内側のSELECT句の結果を1行だけ返すようにする必要があります。なお、条件式に<や=ではなく、IN句を用いていれば複数行返すようなSELECT句でも問題ありません。

### 69

> 問題：
各グループの最上位と最下位を表示し、その差が50より大きいグループを抽出してください。

``` sql
SELECT group_name, MAX(ranking), MIN(ranking)
FROM countries
GROUP BY group_name
HAVING MAX(ranking) -  MIN(ranking) > 50
```

グループ関数（MAXやMIN）の結果をつかって抽出条件を作りたい場合は、WHERE句ではなくHAVING句に記述します。WHERE句に記述するとエラーとなってしまいますので、注意してください。

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

日付関数を使用すると同じことが可能かもしれませんが、今回はUNION句を使用してみました。UNION句を使うと、2つのSELECTの結果を行方向（縦）につなげることができます。注意点としては、2つのSELECT句は同じ列数を返す必要があることです。それほど使用頻度の多いものではありません。

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

たとえば、フォースター選手が2件分抽出されています。  
「どちらの条件にも合致する場合には2件分のデータとして抽出」という指示があるため、WHERE句で条件を設定する方法ではうまく抽出することができません。そこでUNION句を使用するのですが、前問のようにUNION ALL句ではなくUNION句を使用してしまうと、相変わらず2件として抽出することができません。これは、UNION句では同じ行は自動的にマージ（2行が1行にまとまってしまう）されてしまうためです。同じ行があったとしてもマージせず2件分として抽出したい場合にはUNION ALL句を使います。  
注意事項としては、UNION句と同じように2つのSELECTの列数を同じにしておく必要があります。

