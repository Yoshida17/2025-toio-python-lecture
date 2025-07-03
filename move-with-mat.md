---
layout: default
title: "マットを使った移動制御"  # ページのタイトル
parent: "toioロボットの制御"
nav_order: 30
---

# マットを使った移動制御

## 指定した座標に移動する

```python
from toio.simple import SimpleCube
target_cube_name = "toio-xxx" #実際のキューブ名に変更
with SimpleCube(name=target_cube_name) as cube:
    print("マットの特定の座標に移動します")

    # 現在位置の取得
    current_pos = cube.get_current_position()

    if current_pos is not None:
        x, y = current_pos
        print(f"現在位置: X = {x}, Y = {y}")

    # 目標位置（マットの中心付近）
    target_x = 0
    target_y = 0

    print(f"目標位置: X = {target_x}, Y = {target_y}に移動します")

    # 指定位置へ移動
    success = cube.move_to(speed=70, x=target_x, y=target_y)

    if success:
        print("目標位置に到着しました！")
    else:
        print("移動に失敗しました")
```

---

## 位置制御のAPI

### 指定した座標に移動

```python
success = cube.move_to(speed, x, y)
```

- `speed`: 移動速度（1〜100）
- `x`: 目標のX座標
- `y`: 目標のY座標
- 戻り値: 成功したかどうか（論理型）

### 指定した角度に回転

```python
success = cube.set_orientation(speed, degree)
```

- `speed`: 回転速度（1〜100）
- `degree`: 目標の角度（0〜359）
- 戻り値: 成功したかどうか（論理型）

---

## 指定した向きに回転する

```python
from toio.simple import SimpleCube
target_cube_name = "toio-xxx" #実際のキューブ名に変更
with SimpleCube(name=target_cube_name) as cube:
    print("指定した向きに回転します")

    # 現在の角度の取得
    current_angle = cube.get_orientation()

    if current_angle is not None:
        print(f"現在の角度: {current_angle}度")

    # 目標角度（90度 = マットの右方向）
    target_angle = 90

    print(f"目標角度: {target_angle}度に回転します")

    # 指定角度へ回転
    success = cube.set_orientation(speed=50, degree=target_angle)

    if success:
        print("目標角度に回転しました！")
    else:
        print("回転に失敗しました")
```

---

## 四角形を描く（位置確認付き）

```python
from toio.simple import SimpleCube
target_cube_name = "toio-xxx" #実際のキューブ名に変更
with SimpleCube(name=target_cube_name) as cube:
    print("位置情報を確認しながら四角形を描きます")
    # 四角形の各頂点を設定する
    points = [
        [50, 50],    # 右上
        [50, -50],   # 右下
        [-50, -50],  # 左下
        [-50, 50],   # 左上
        [50, 50]     # 最初の位置に戻る
    ]
    for i in range(len(points)):
        target_x, target_y = points[i]  # リストから2つの値を同時に取得
        print("頂点", i+1, ": X =", target_x, ", Y =", target_y, "に移動します")
        # 指定位置へ移動
        success = cube.move_to(speed=30, x=target_x, y=target_y)
        if success:
            print("目標位置に到着しました！")
        else:
            print("移動に失敗しました")

        # 現在位置の確認
        current_pos = cube.get_current_position()
        if current_pos is not None:
            x, y = current_pos
            print("現在位置: X =", x, ", Y =", y)
```

### コードの詳細解説

```python
for i in range(len(points)):
    target_x, target_y = points[i]
```

- `len(points)`: pointsリストの要素数（この場合は5）を取得
- `range(len(points))`: 0から4までの数値を順番に生成
- `points[i]`: i番目の座標リスト（例：`[50, 50]`）を取得
- `target_x, target_y = points[i]`: リストの2つの値を別々の変数に代入

具体的な動作例：

```python
points = [[50, 50], [50, -50], [-50, -50], [-50, 50], [50, 50]]

# i = 0 の場合
# points[0] は [50, 50]
# target_x = 50, target_y = 50 になる

# i = 1 の場合  
# points[1] は [50, -50]
# target_x = 50, target_y = -50 になる
```

リストの要素として別のリストを入れることができます．これを「2次元リスト」や「入れ子リスト」と呼びます．

---

## 条件分岐と位置情報を組み合わせた例

### 特定のエリアに応じて動作を変える

```python
from toio.simple import SimpleCube
target_cube_name = "toio-xxx" #実際のキューブ名に変更
with SimpleCube(name=target_cube_name) as cube:
    print("位置に応じた動作を行います")

    # 10秒間繰り返す
    for i in range(20):  # 0.5秒 × 20回 = 10秒
        # 現在位置の取得
        position = cube.get_current_position()

        if position is not None:
            x, y = position
            print(f"現在位置: X = {x}, Y = {y}")

            # 位置に応じた条件分岐
            if x > 0 and y > 0:  # 第1象限
                print("第1象限にいます")
                cube.turn_on_cube_lamp(255, 0, 0, 0.5)  # 赤色
                cube.move(30, 0.5)
            elif x < 0 and y > 0:  # 第2象限
                print("第2象限にいます")
                cube.turn_on_cube_lamp(0, 255, 0, 0.5)  # 緑色
                cube.spin(30, 0.5)
```

### プログラムの解説：位置に応じた動作

このプログラムは、マット上のキューブの位置に応じて異なる動作を行います：

1. 第1象限（x > 0, y > 0）にいる場合：
   
   - LEDを赤色に点灯，前進する

2. 第2象限（x < 0, y > 0）にいる場合：
   
   - LEDを緑色に点灯，その場で回転する

プログラムの流れ：

1. 各繰り返しで現在位置を取得
2. 位置に応じて異なる動作を実行
3. 0.5秒モータを動かして次の繰り返しへ（20回）

---

## 角度に応じて動作を変える

```python
from toio.simple import SimpleCube
target_cube_name = "toio-xxx" #実際のキューブ名に変更
with SimpleCube(name=target_cube_name) as cube:
    print("角度に応じた動作を行います")

    # 10秒間繰り返す
    for i in range(20):  # 0.5秒 × 20回 = 10秒
        # 現在の角度の取得
        angle = cube.get_orientation()

        if angle is not None:
            print(f"現在の角度: {angle}度")

            # 角度に応じた条件分岐
            if 0 <= angle < 90:  # 上向き〜右向き
                print("上〜右を向いています")
                cube.turn_on_cube_lamp(255, 0, 0, 0.5)  # 赤色
                cube.run_motor(20, 10, 0.5)
            elif 90 <= angle < 180:  # 右向き〜下向き
                print("右〜下を向いています")
                cube.turn_on_cube_lamp(0, 255, 0, 0.5)  # 緑色
                cube.run_motor(-10, 10, 0.5)
```

### プログラムの解説：角度に応じた動作

このプログラムは、マット上のキューブの角度に応じて異なる動作を行います：

1. 0度から90度未満（上向き〜右向き）の場合：
   
   - LEDを赤色に点灯
   - 左車輪速度20，右車輪速度10で時計回りに旋回しつつ前進

2. 90度から180度未満（右向き〜下向き）の場合：
   
   - LEDを緑色に点灯
   - 左車輪速度-10，右車輪速度10で反時計回りに旋回

このプログラムを使うと、キューブの向きによって動作が変わるインタラクティブな動きを作れます。

---

## toio プログラミングのヒント

1. **with文を必ず使う**
   
   - プログラム終了時に自動的に切断される
   - エラーが発生しても安全に切断される

2. **位置情報はNoneチェックを忘れずに**
   
   - マット上にない場合はNoneが返る
   - `if position is not None:` のようなチェックが必要

3. **動作確認は少しずつ**
   
   - 最初は速度を低めに設定（30〜50）
   - 動作を少しずつ確認しながら進める

`cube.move_to()`で移動が失敗した際に再挑戦する工夫．

```python
while True:
    success = cube.move_to(speed=30, x=target_x, y=target_y)
    if success:
        break  # 移動成功時はループを抜ける
    else:
        print("移動失敗，微調整して再挑戦")
        cube.spin(10, 0.1)  # 0.1秒回転して再挑戦
```

---

## チャレンジ：大きなコースのタイムアタック！

### 目標

- 大きなコースを、toioキューブで周回します
- プログラムを工夫して、最速タイムに挑戦！

### ポイント

- 位置情報を活用してコースを認識
- カーブでの速度調整
- 最適な経路の計算

何秒で周回できるか競争しましょう！
