---
layout: default
title: "位置情報と角度情報の取得（マット使用）"  # ページのタイトル
parent: "toioロボットの制御"
nav_order: 20
---

# 位置情報と角度情報の取得（マット使用）

## マットの原点とキューブの向き

マットの中心が (0, 0) でキューブの向きは以下のようになります:

- 0度: マットの上方向
- 90度: マットの右方向
- 180度，-180度: マットの下方向
- -90度: マットの左方向

*画像出典: https://github.com/toio/toio.py/blob/main/SIMPLE_API.ja.md*

![toio coordinate system](https://github.com/toio/toio.py/raw/main/image/IMG-2023-02-27-15-14-45.png)
![toio coordinate system](https://github.com/toio/toio.py/raw/main/image/IMG-2023-02-27-15-15-11.png)

---

## マット上の位置情報を取得するプログラム例

```python
from toio.simple import SimpleCube
target_cube_name = "toio-xxx" #実際のキューブ名に変更
with SimpleCube(name=target_cube_name) as cube:
    # 現在の位置情報を取得
    position = cube.get_current_position()

    if position is not None:
        x, y = position
        print("現在の位置: X =", x, "Y =", y)
    else:
        print("位置情報が取得できませんでした")
        print("キューブをマットの上に置いてください")
```

---

## 位置情報と角度情報のAPI

### 現在の座標を取得

```python
position = cube.get_current_position()  # (x, y)のタプルまたはNoneを返す
x = cube.get_x()  # x座標またはNoneを返す
y = cube.get_y()  # y座標またはNoneを返す
```

### 現在の角度を取得

```python
angle = cube.get_orientation()  # 角度（度）またはNoneを返す
```

**注意**: これらの関数はキューブがマット上にある場合のみ正しい値を返します。
マット上にない場合は `None` が返ります。

---

## マット上での角度情報を取得

```python
from toio.simple import SimpleCube
target_cube_name = "toio-xxx" #実際のキューブ名に変更
with SimpleCube(name=target_cube_name) as cube:
    # 現在の角度情報を取得
    angle = cube.get_orientation()

    if angle is not None:
        print("現在の角度:", angle, "度")
    else:
        print("角度情報が取得できませんでした")
        print("キューブをマットの上に置いてください")
```

このプログラムの動作：

1. 現在の角度情報を取得
2. 角度情報があれば表示、なければエラーメッセージを表示

---

## 位置と角度を継続的に表示

```python
from toio.simple import SimpleCube
import signal
import sys

# Ctrl+Cで終了するための設定
running = True

def signal_handler(sig, frame):
    global running
    running = False
    print("\n終了します")

signal.signal(signal.SIGINT, signal_handler)

target_cube_name = "toio-xxx" #実際のキューブ名に変更
with SimpleCube(name=target_cube_name) as cube:
    print("位置と角度を読み取ります（Ctrl+Cで終了）")

    while running:
        # 現在の位置情報と角度情報を取得
        position = cube.get_current_position()
        angle = cube.get_orientation()

        if position is not None and angle is not None:
            x, y = position
            print(f"位置: X = {x}, Y = {y}, 角度: {angle}度")
        else:
            print("位置情報が取得できません")

        cube.sleep(0.5)  # 0.5秒待つ
```
