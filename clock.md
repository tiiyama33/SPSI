# 時計を作ろう

前回と同じ初期処理、終了処理にプログラムを加えていき、時計を作ってみよう。

```.py
import pygame #モジュールpygameの読み込み
import sys
import math
import datetime #時間を扱うためのモジュール
from pygame.locals import *

pygame.init() #pygameモジュールの初期化
screen = pygame.display.set_mode((800,800)) #ウィンドウの表示

#t = 0
while True: #無限ループ
    screen.fill((0,0,0)) #黒で塗りつぶす
    #メインループ
    pygame.display.update() #画面を更新

    #プログラムの終了処理
    for event in pygame.event.get(): #pygameからくるイベントを順に取り出す
        if event.type == QUIT: #もしイベントがQUITなら
            pygame.image.save(screen,"tokei.png") #画面をpngファイルとしてセーブ
            pygame.quit() #pygameモジュールの終了
            sys.exit() #プログラムの強制終了
```
## 材料

### 円を描く
```
pygame.draw.circle(スクリーンオブジェクト, 色, 中心座標, 半径)
```
中心座標 = (x座標, y座標)

<h3>楕円を描く</h3>
<pre class="brush: python; gutter: false">pygame.draw.ellipse(スクリーンオブジェクト, 色, 楕円の形)</pre>
<p>楕円の形 = (中心のx座標, 中心のy座標, 横幅, 縦幅)</p>
<h3>線を描く</h3>
<pre class="brush: python; gutter: false">pygame.draw.line(スクリーンオブジェクト, 色, 始点, 終点, 幅)</pre>
<p>始点 = (x座標, y座標)<br />終点 = (x座標, y座標)</p>
<h3>多角形を描く</h3>
<pre class="brush: python; gutter: false">pygame.draw.polygan(スクリーンオブジェクト, 座標のリスト, 幅)</pre>
<p>座標のリスト = [(x座標, y座標), (x座標, y座標), (x座標, y座標) ...]<br />幅を0にすると塗りつぶし<br /><br /></p>

### 文字を表示する

```/py
    font1 = pygame.font.SysFont("PlemolJP", 50) #フォントを指定 数字はフォントサイズ
    text1 = font1.render("Rolex", True, (255,255,255)) #いったんtext1に文字列を出力 
    screen.blit(text1, (350,350)) #screenに文字列を重ねる 数値は座標
```

## 関数
繰り返し行う処理や、ひとかたまりでまとめた方がわかりやすい部分を
関数
としてまとめる。pythonの関数は
def 関数名()
で定義する。

ここでは
- 画面中央から放射状の線を引く関数 hline()
- 文字盤を描く関数 mojiban()

を作成しよう

```.py
def hline(m, r, width, length, color=(255,255,255)):
    #放射状の線を引く
    #m 角度(0~60, 「分」で指定 1分が6°)
    #r 中心からの距離(pixel)
    #width 幅(pixel)
    #length 長さ(pixel)
    #color 色


def mojiban():
    #時計の文字盤を描く
    font1 = pygame.font.SysFont("PlemolJP", 50) #フォントを指定
    text1 = font1.render("Rolex", True, (255,255,255))
    screen.blit(text1, (350,350))
```

## メインループ

プログラム開始後、
現在の時間を取得してそれに合わせて時針、分針、秒針を描く
という処理を繰り返せば、時計が出来上がる

```.py
    mojiban() #文字盤を描く
    dt_now = datetime.datetime.now() #現在時間の取得
    #秒針を描く
    #print(dt_now.second)
    #分針を描く
    #print(dt_now.minute)
    #時針を描く
    #print(dt_now.hour)
```
