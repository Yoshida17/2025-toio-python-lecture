---
layout: default
title: "制御構造"  # ページのタイトル
parent: "トップページ"
nav_order: 30
---

# 3. 制御構造

## 条件分岐（if文）

### 基本的なif文

「もし〜ならば〜する」という条件処理を行うための構文です．

```python
age = 13
if age >= 13:  # ageが13以上ならば
    print("13歳以上です")  # "13歳以上です"と画面に表示する
    print("中学生以上ですね")  # 同じコードブロック内なので続けて実行される

print("これは常に実行されます") # 別のコードブロック（if文の外）
```

### if-else文

「もし〜ならば〜する。そうでなければ～する。」という条件処理を行うための構文です．

```python
score = 85

if score >= 80:  # scoreが80以上ならば
    print("よくできました！")  # "よくできました！"と画面に表示する
else:  # そうでなければ
    print("もう少し頑張りましょう")  # "もう少し頑張りましょう"と画面に表示する
```

### if-elif-else文

「もし〜ならば〜する。そうでなく、もし～ならば～する。どちらでもなければ～する。」という複数の条件を順番に確認する処理を行うための構文です．

```python
weather = "rain"

if weather == "sunny":  # もしweatherがsunnyならば
    print("外で遊ぼう！")
elif weather == "cloudy":  # そうでなく、もしweatherがcloudyならば
    print("念のため傘を持っていこう")
elif weather == "rain":  # そうでなく、もしweatherがrainならば
    print("傘を持っていこう")
else:  # どちらでもなければ
    print("天気予報を確認しよう")
```

### 比較演算子

条件分岐でよく使う比較演算子：

```python
# 比較演算子の例
# ==  等しい
# !=  等しくない
# >   より大きい
# <   より小さい
# >=  以上
# <=  以下

a = 5
b = 10
print(a == b)  # False
print(a != b)  # True
print(a < b)   # True
print(a > b)   # False
```

---

## 繰り返し処理（for文）

決まった回数や範囲で繰り返す場合に使います．

### 5回繰り返す

```python
for i in range(5):
    print(i, "回目の繰り返し")
```

- `i` には `range(5)` の各値（0, 1, 2, 3, 4）が順番に入ります
- 最初のループでは `i = 0`、次は `i = 1`、...という具合に変化します
- `i` の代わりに好きな変数名（例：`num`, `count`）を使えます

**`range()`関数について**

`range()`関数は数値の範囲を作る関数です．

```python
# range(終了値) - 0から（終了値-1）まで
for i in range(3):
    print(i)  # 0, 1, 2 が表示される

# range(開始値, 終了値) - 開始値から（終了値-1）まで
for i in range(2, 6):
    print(i)  # 2, 3, 4, 5 が表示される

# range(開始値, 終了値, 増加値) - 指定した増加値で変化
for i in range(0, 10, 2):
    print(i)  # 0, 2, 4, 6, 8 が表示される
```

### forループの例：合計計算

```python
# 1から10までの合計を計算
total = 0
for num in range(1, 11):  # 1から10まで
    total = total + num

print("1から10までの合計:", total)
```

- `for num in range(a, b):`という書き方で`num`は`a`から`b-1`まで1つずつ増えていきます．
- 例えば`range(1, 6)`なら、`num` は 1, 2, 3, 4, 5 と変化します．

### リストの要素を順番に処理

```python
animals = ["dog", "cat", "rabbit", "hamster"]

for animal in animals:
    print(animal, "は可愛いですね")
```

- `animal` には `animals` リストの各要素が順番に入ります
- 最初のループでは `animal = "dog"`、次は `animal = "cat"`、...と変化します

**リストとは**

リストは複数のデータを順番に保存できるデータ型です．角括弧`[ ]`で囲み，要素をカンマ`,`で区切って書きます．

```python
# 文字列のリスト
fruits = ["apple", "orange", "banana"]

# 数値のリスト
numbers = [10, 20, 30, 40, 50]

# 要素を取得（インデックスは0から始まる）
print(fruits[0])    # "apple"
print(fruits[1])    # "orange"
```

---

## while文

条件が真である間、繰り返し処理を行います．

```python
# 5回繰り返す例
counter = 0

while counter < 5:  # counterが5未満のとき、次の処理を繰り返す
    print(counter, "回目の繰り返し")
    counter = counter + 1  # カウンターを増やす
```
