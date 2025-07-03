---
layout: default
title: "モーター制御の基本"  # ページのタイトル
parent: "toioロボットの制御"
nav_order: 10
---

# モーター制御の基本

## モーター制御のプログラム例

```python
from toio.simple import SimpleCube
target_cube_name = "toio-xxx" #実際のキューブ名に変更
with SimpleCube(name=target_cube_name) as cube:
    cube.move(50, 2)  # 前進（速度50で2秒間）
    cube.sleep(1)  # 1秒待つ
    cube.move(-50, 2)  # 後退（速度-50で2秒間）
    cube.sleep(1)  # 1秒待つ
    cube.spin(60, 3)  # 回転（速度60で3秒間）
    cube.stop_motor()# モーターを止める
```

---

## モーター制御のAPI

### 前後移動

```python
cube.move(speed, duration)
```

- `speed`: 速度（-100〜100）。正の値で前進、負の値で後退
- `duration`: 動作時間（秒）。最大2.55秒。0を入力した場合は制限時間なし。

### その場で回転

```python
cube.spin(speed, duration)
```

- `speed`: 速度（-100〜100）。正の値で反時計回り、負の値で時計回り
- `duration`: 動作時間（秒）。最大2.55秒。0を入力した場合は制限時間なし。

### 左右のモーターを個別に制御

```python
cube.run_motor(left_speed, right_speed, duration)
```

- `left_speed`: 左モーターの速度（-100〜100）
- `right_speed`: 右モーターの速度（-100〜100）
- `duration`: 動作時間（秒）。最大2.55秒。0を入力した場合は制限時間なし。

---

## 左右のモーターを別々に制御する方法

```python
from toio.simple import SimpleCube
target_cube_name = "toio-xxx" #実際のキューブ名に変更
with SimpleCube(name=target_cube_name) as cube:
    # 左右のモーターを別々に動かす（左：70、右：30で2秒間）
    cube.run_motor(70, 30, 2)  # 右カーブしながら前進

    # 1秒待つ
    cube.sleep(1)

    # 反対側に曲がる（左：30、右：70で2秒間）
    cube.run_motor(30, 70, 2)  # 左カーブしながら前進

    # その場で回転（左：50、右：-50で2秒間）
    cube.run_motor(50, -50, 2)  # その場で左回転
```

---

## 待機と停止のAPI

### 指定時間待機する

```python
cube.sleep(seconds)
```

- `seconds`: 待機時間（秒）

**重要**: Pythonの `time.sleep()` の代わりに `cube.sleep()` を使用してください。
`time.sleep()` を使うとキューブとの通信が途切れることがあります。

### モーターを停止する

```python
cube.stop_motor()
```
