# クラスの拡張

## 前回まで
クラス Ball を定義し、等速直線運動を行うボールをシミュレートした。

<details>

```.py
import pygame #モジュールpygameの読み込み
import sys
import math
import time #時間を扱うためのモジュール
import random
from pygame.locals import *

#シミュレーション変数の設定
r_scale = 0.01 # m/pixel
t_scale = 0.01 # s/frame

#pygameの初期化
pygame.init() #pygameモジュールの初期化
screen = pygame.display.set_mode((800,800)) #ウィンドウの表示
font1 = pygame.font.SysFont("PlemolJP", 50) #フォントを指定

#画面変数の設定
width = screen.get_width()
height = screen.get_height()
gxcenter = width/2.0
gycenter = height/2.0

#関数 シミュレーション座標→画面座標
def xtogx(x):
    gx = x/r_scale+gxcenter
    return(gx)

def ytogy(y):
    gy = -y/r_scale+gycenter
    return(gy)

def rtogr(r):
    gr = r/r_scale
    return(gr)

#関数 画面座標→シミュレーション座標
def gxtox(gx):
    x = r_scale*(gx-gxcenter)
    return(x)

def gytoy(gy):
    y = -r_scale*(gy-gycenter)
    return(y)

def grtor(gr):
    r = r_scale*gr
    return(r)

#関数 経過したシミュレーション時間の表示
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
    
#粒子のクラスの定義
class Ball:

    def __init__(self, id):
        #インスタンス作成時の処理
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
        #粒子を動かす
        self.x += self.vx*t_scale
        self.y += self.vy*t_scale
    def show(self):
        #粒子を表示する
        pygame.draw.circle(screen, self.color, (xtogx(self.x), ytogy(self.y)), rtogr(self.r))
                
    #class Ball おわり

#初期処理
ball = Ball(0)

#メインループ
while True: 
    screen.fill((0,0,0)) #黒で塗りつぶす

    showtime("s")
    ball.move()
    ball.show()
    #pygame.draw.circle(screen, (255,255,255), ctogc((0,0)), 20)
    pygame.display.update() #画面を更新

    #time.sleep(0.5)

    #pygameのイベント処理
    for event in pygame.event.get(): #pygameからくるイベントを順に取り出す
        #終了処理
        if event.type == QUIT: #もしイベントがQUITなら
            pygame.image.save(screen,"tokei.png") #画面をpngファイルとしてセーブ
            pygame.quit() #pygameモジュールの終了
            sys.exit() #プログラムの強制終了
```
</details>

# クラス内関数を追加する

```.py
    def set(self, v):
    #ランダムな位置、ランダムな方向にセット(速度はv)
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

# クラスのリストを作成する
## クラスのリストを作成する
```.py
ball = [Ball(0), Ball(1), Ball(2)]
```

## リストの内包表記
```.py
ball = [Ball(x) for x in range(0,5)] #for文を使ってリストを生成
```
リストの内包表記を使えば、大量の粒子を一気に作成できる
```.py
#初期処理
number = 200
ball = [Ball(x) for x in range(0, number)]
```

#クラスのリストの繰り返し処理
リスト化することで、for文を使った処理が可能になる
```.py
#初期処理
for i in range(0, number):
    ball[i].set(1) #速さ1m/s
```
```
#メインループ
    for i in range(0, number): #ボールの数だけ処理を繰り返す
        ball[i].move()
        ball[i].show()
```

![image](https://github.com/user-attachments/assets/e29212e3-1fae-4c68-a56b-fc91006365ee)

