# シミュレーション

## 前回まで
クラス Ball のリストを定義し、複数のボールを扱えるようにした。

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
t_scale = 0.001 # s/frame

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
        self.x = 0.0 #x座標 m
        self.y = 0.0
        self.vx = 1.0 #速度のx成分 m/s
        self.vy = 1.0
        self.r = 0.2 #半径 m
        self.m = 0.2 #質量 kg
        self.color = (255,255,255)
    def gravity(self):
        #重力を働かせる
        self.vy += -9.8*t_scale
    def move(self):
        #粒子を動かす
        self.x += self.vx*t_scale
        self.y += self.vy*t_scale
    def show(self):
        #ボールを表示する
        pygame.draw.circle(screen, self.color, (xtogx(self.x), ytogy(self.y)), rtogr(self.r))
    def set(self, v):
        #ボールをランダムな位置に移動
        #速度はv、方向はランダム
        self.x = random.uniform(-grtor(width/2), grtor(width/2)) #xを画面の幅に設定
        self.y = random.uniform(-grtor(height/2), grtor(height/2))
        angle = random.uniform(0, 2*math.pi) #ラジアン単位の角度を乱数で作る
        self.vx = v*math.cos(angle) #angleを使ってvのx成分を求める 
        self.vy = v*math.sin(angle) #angleを使ってvのy成分を求める
        
    #class Ball おわり

#初期処理
number = 200
ball = [Ball(x) for x in range(0,number)] #実際にballという変数にBallをわりあてる

for i in range(0,number): #全ての粒子について
    ball[i].set(1) #速さ1m/s, 位置と方向をランダムにセット

#メインループ
while True: 
    screen.fill((0,0,0)) #黒で塗りつぶす

    showtime("s")
    for i in range(0,number): #ボールの数だけ処理を繰り返す
        ball[i].gravity() #重力を働かせる
        ball[i].move() #粒子を動かす
        ball[i].show() #粒子を表示する
        
    pygame.display.update() #画面を更新

    time.sleep(0.01) #ウェイト

    #pygameのイベント処理
    for event in pygame.event.get(): #pygameからくるイベントを順に取り出す
        #終了処理
        if event.type == QUIT: #もしイベントがQUITなら
            pygame.image.save(screen,"tokei.png") #画面をpngファイルとしてセーブ
            pygame.quit() #pygameモジュールの終了
            sys.exit() #プログラムの強制終了
```
</details>

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







