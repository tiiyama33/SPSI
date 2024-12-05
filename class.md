# ボールのシミュレーション

いよいよシミュレーションの作成に入っていこう。

## 基本構造
前回と同じくpygameを使って作成する
```.py
import pygame #モジュールpygameの読み込み
import sys
import math
import time #時間を扱うためのモジュール
import random
from pygame.locals import *

#初期値の設定 setting initial parameter

#初期処理 initial procedure
pygame.init() #pygameモジュールの初期化
screen = pygame.display.set_mode((800,800)) #ウィンドウの表示 show screen
font1 = pygame.font.SysFont("PlemolJP", 50) #フォントを指定 setting font

#関数の設定 setting functions

#クラスの設定 setting classes


while True: #無限ループ main loop
    screen.fill((0,0,0)) #黒で塗りつぶす fill black

    showtime("s")
    ball.move()
    ball.show()
    #pygame.draw.circle(screen, (255,255,255), ctogc((0,0)), 20)
    pygame.display.update() #画面を更新 update display

    #time.sleep(0.5)

    #プログラムの終了処理 end procedure
    for event in pygame.event.get(): #pygameからくるイベントを順に取り出す check events
        if event.type == QUIT: #もしイベントがQUITなら
            pygame.image.save(screen,"save.png") #画面をpngファイルとしてセーブ
            pygame.quit() #pygameモジュールの終了
            sys.exit() #プログラムの強制終了 program end

```

## 初期値の設定 setting initial parameters

```.py
r_scale = 0.01 # m/pixel scale constant
t_scale = 0.01 # s/frame time constant

width = screen.get_width()
height = screen.get_height()
gxcenter = width/2.0
gycenter = width/2.0
```

## 関数の設定 setting functions

### 情報の表示 display informations
```.py
gt = 0.0
def showtime(unit):
    global gt
    if unit=="s":
        timestr = str(gt)
        timestr = "%1.4f %s" % (gt, unit)
    if unit=="day":
        timestr = str(gt/86400.0)

    text1 = font1.render(timestr, True, (255,255,255))
    screen.blit(text1, (10,10))
    gt += t_scale
```

### 座標変換 convert positions between real-space and vertual-space

```.py
#シミュレーション座標→画面座標 coodinate convert from real to sim
def xtogx(x):
    gx = x/r_scale+gxcenter
    return(gx)

def ytogy(y):
    gy = -y/r_scale+gycenter
    return(gy)

def rtogr(r):
    gr = r/r_scale
    return(gr)

#画面座標→シミュレーション座標  coodinate convert from sim to real
def gxtox(gx):
    x = r_scale*(gx-gxcenter)
    return(x)

def gytoy(gy):
    y = -r_scale*(gy-gycenter)
    return(y)

def grtor(gr):
    r = r_scale*gr
    return(r)
```


## クラス

```
#粒子のクラスの定義
class Ball:

    #初期処理
    def __init__(self, id):
        #インスタンスの保持するデータ
        self.id = id
        self.x = 0.0
        self.y = 0.0
        self.vx = 1.0
        self.vy = 1.0
        self.r = 0.2
        self.m = 0.2
        self.color = (255,255,255)
    def move(self):
        self.x += self.vx*t_scale
        self.y += self.vy*t_scale
    def show(self):
        pygame.draw.circle(screen, self.color, (xtogx(self.x), ytogy(self.y)), rtogr(self.r))

        
    #class Ball おわり

ball = Ball(0)
```

## メインループ

```.py
    screen.fill((0,0,0)) #黒で塗りつぶす

    showtime("s")
    ball.move()
    ball.show()
    #pygame.draw.circle(screen, (255,255,255), ctogc((0,0)), 20)
    pygame.display.update() #画面を更新
```

## 乱数
```.py
import random

x = random.uniform(0,1) #0~1の乱数を生成
```

## 円周率
```.py
math.pi
```

## リストの内包表記
```.py
ball = [Ball(x) for x in range(0,5)] #for文を使ってリストを生成
```
