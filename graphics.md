# pygameを使う

<p>2次元グラフィクスを扱います。<br />モジュール pygame を使います。</p>

```.py
import pygame
import sys
from pygame.locals import *

pygame.init() #モジュール初期化
screen = pygame.display.set_mode( (400,330) ) #ウィンドウ生成

screen.fill( (0,0,0) ) #画面を黒く塗りつぶす
pygame.draw.rect(screen,(255,0,0),(20,70,10,180)) #赤い四角を描く
pygame.display.update() #画面表示を更新

while True:
    for event in pygame.event.get():
        if event.type == QUIT: #終了処理
            pygame.image.save(screen, &quot;test.png&quot;) #ウィンドウイメージをセーブ
            pygame.quit() #モジュールの終了
            sys.exit() #プログラム修了
```

<h3>ウィンドウ生成</h3>
<pre class="brush: python; gutter: false">screen = pygame.display.set_mode(画面サイズ)</pre>
<p>画面サイズ = (横, 縦)</p>
<h3>四角を描く</h3>
<pre class="brush: python; gutter: false">pygame.draw.rect(スクリーンオブジェクト, 色, 四角の形)</pre>
<p>色 = (R, G, B) #RGBは0~255 (2<sup>8</sup>。3原色合わせて 2<sup>24</sup>=1670万色)<br />四角の形 = (左上のx座標, 左上のy座標, 横幅, 縦幅)</p>
<h3>円を描く</h3>
<pre class="brush: python; gutter: false">pygame.draw.circle(スクリーンオブジェクト, 色, 中心座標, 半径)</pre>
<p>中心座標 = (x座標, y座標)</p>
<h3>楕円を描く</h3>
<pre class="brush: python; gutter: false">pygame.draw.ellipse(スクリーンオブジェクト, 色, 楕円の形)</pre>
<p>楕円の形 = (中心のx座標, 中心のy座標, 横幅, 縦幅)</p>
<h3>線を描く</h3>
<pre class="brush: python; gutter: false">pygame.draw.line(スクリーンオブジェクト, 色, 始点, 終点, 幅)</pre>
<p>始点 = (x座標, y座標)<br />終点 = (x座標, y座標)</p>
<h3>多角形を描く</h3>
<pre class="brush: python; gutter: false">pygame.draw.polygan(スクリーンオブジェクト, 座標のリスト, 幅)</pre>
<p>座標のリスト = [(x座標, y座標), (x座標, y座標), (x座標, y座標) ...]<br />幅を0にすると塗りつぶし<br /><br /></p>

[戻る](./README.md) 
