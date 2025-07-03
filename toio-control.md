---
layout: default
title: "toioロボットの制御"  # ページのタイトル
parent: "トップページ"
nav_order: 40
---

# 4. toioロボットの制御

## toioとは？

toioはSONYが開発した小さなロボットキューブです．プログラムでモーターやライト，音などを制御できます．

![toio cube](https://toio.io/_next/static/media/cube.b4d70949.webp)

---

## toioの位置認識技術

- toioキューブの底面には特殊なセンサーがあり，マット上のドットパターンを読み取ります
- これにより，正確な位置と角度を把握できます
- 位置認識技術は様々な移動ロボットにとって重要な技術です

*画像出典: https://lot.or.jp/project/7271/*

![toio position detection](https://lot.or.jp/wp/wp-content/uploads/2022/01/20211208toio_slide0-03.jpg)

![mobile robot](https://lot.or.jp/wp/wp-content/uploads/2022/01/20211208toio_slide0-13.jpg)

---

## 電源の入れ方・切り方

![toio_power](https://toio.github.io/toio-spec/assets/images/cube_basics_power_on_off-16ad212a37d733ec6cb6c5df5b1bf91c.svg)

キューブの底面に電源ボタンがあります．電源ボタンを押すとキューブの電源が入ります． また，電源ボタンを長押しすると電源を切ることができます．

---

## はじめてのtoioプログラム

```python
from toio.simple import SimpleCube
target_cube_name = "toio-xxx" #実際のキューブ名に変更
with SimpleCube(name=target_cube_name) as cube:  # toioに接続する
    # LEDライトを青色に点灯する（R=0, G=0, B=255）
    cube.turn_on_cube_lamp(0, 0, 255, 2)  # 2秒間青色に光る
    print("青色に光らせました")
```

このプログラムの動作：

1. キューブに接続
2. LEDを青色に2秒間点灯
3. 接続を自動的に切断

---

## toioプログラミングの基本構造

すべてのtoioプログラムは，次の3つの重要な行から始まります：

```python
from toio.simple import SimpleCube  # toioライブラリをインポート
target_cube_name = "toio-xxx" #接続先のキューブを指定
with SimpleCube(name=target_cube_name) as cube: # キューブに接続
    # ここにtoioを動かすコードを書きます
    cube.turn_on_cube_lamp(0, 0, 255, 1)  # 例：青色LEDを点灯
```

### 重要ポイント

- `from toio.simple import SimpleCube`: toioライブラリを使えるようにします
- `target_cube_name = "toio-xxx"`: `xxx`の部分を手元のキューブに書かれた文字列に修正します．
- `with SimpleCube(name=target_cube_name) as cube:`: キューブに安全に接続します。この方法を使うと、プログラムが終わった時やエラーが起きた時に自動的に切断されます
- **すべてのtoio操作コードは、この `with` ブロックの中に書きます**

---

## LED制御のAPI

### LEDを点灯する

```python
cube.turn_on_cube_lamp(r, g, b, duration)
```

パラメータ：

- `r`: 赤色の強さ（0〜255）
- `g`: 緑色の強さ（0〜255）
- `b`: 青色の強さ（0〜255）
- `duration`: 点灯時間（秒）

色の例：

- 赤: (255, 0, 0)
- 緑: (0, 255, 0)
- 青: (0, 0, 255)
- 黄: (255, 255, 0)
- 紫: (255, 0, 255)
- 水色: (0, 255, 255)

### LEDを消灯する

```python
cube.turn_off_cube_lamp()
```
