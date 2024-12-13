# シミュレーション

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

r_scale = 0.01 # m/pixel
t_scale = 0.01 # s/frame

pygame.init() #pygameモジュールの初期化
screen = pygame.display.set_mode((800,800)) #ウィンドウの表示
font1 = pygame.font.SysFont("PlemolJP", 50) #フォントを指定

width = screen.get_width()
height = screen.get_height()
gxcenter = width/2.0
gycenter = height/2.0

#シミュレーション座標→画面座標
def xtogx(x):
    gx = x/r_scale+gxcenter
    return(gx)

def ytogy(y):
    gy = -y/r_scale+gycenter
    return(gy)

def rtogr(r):
    gr = r/r_scale
    return(gr)

#画面座標→シミュレーション座標
def gxtox(gx):
    x = r_scale*(gx-gxcenter)
    return(x)

def gytoy(gy):
    y = -r_scale*(gy-gycenter)
    return(y)

def grtor(gr):
    r = r_scale*gr
    return(r)

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
#t = 0
while True: #無限ループ
    screen.fill((0,0,0)) #黒で塗りつぶす

    showtime("s")
    ball.move()
    ball.show()
    #pygame.draw.circle(screen, (255,255,255), ctogc((0,0)), 20)
    pygame.display.update() #画面を更新

    #time.sleep(0.5)

    #プログラムの終了処理
    for event in pygame.event.get(): #pygameからくるイベントを順に取り出す
        if event.type == QUIT: #もしイベントがQUITなら
            pygame.image.save(screen,"tokei.png") #画面をpngファイルとしてセーブ
            pygame.quit() #pygameモジュールの終了
            sys.exit() #プログラムの強制終了
```
</details>

# クラス内関数を追加する

```.py
    def set(self, v):
    #ランダムな位置、ランダムな方向にせっと(速度はv)
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

## リストの内包表記
```.py
ball = [Ball(x) for x in range(0,5)] #for文を使ってリストを生成
```

# シミュレーション

運動方程式  

$\displaystyle f=ma$  

から力場の中での運動をシミュレートできる。  
上式より  

$\displaystyle a = \frac{f}{m}$  

a は加速度なので  

$\displaystyle a=\frac{{\rm d}v}{{\rm d}t}=\frac{f}{m}$  

$\displaystyle \Delta v = \frac{f}{m} \Delta t$

## 地球表面での重力場
$f = mg$  

g = 9.80665 m s^-2

## 重力相互作用
$\displaystyle f = -G\frac{Mm}{r^2}$  

G = 6.67430 x 10^-11 m^3 kg^-1 s^-2

![image](https://github.com/user-attachments/assets/f8223bd3-7a8f-41b6-8f45-b4c08cf98e0f)

質量 kg  
距離単位 au  
1au = 149597870700 m
速度 m/s  
公転周期 day = 86400 s







