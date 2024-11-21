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
    centerx = screen.get_width()/2.0
    centery = screen.get_height()/2.0 #画面の中心座標を得る

    #針の方向
    theta = math.radians(90.0-6.0*m)
    theta2 = math.radians(180.0-6.0*m)

    c1 = [centerx+r*math.cos(theta), centery-r*math.sin(theta)] #針の内側の中心座標
    c2 = [centerx+(r+length)*math.cos(theta), centery-(r+length)*math.sin(theta)] #針の外側の中心座標
    hw = [(width/2.0)*math.cos(theta2) , -(width/2.0)*math.sin(theta2)]

    p1 = [c1[0]-hw[0], c1[1]-hw[1]] #内側の中心から幅のベクトルを引く
    p2 = [c1[0]+hw[0], c1[1]+hw[1]] #内側の中心に幅のベクトルを足す
    p3 = [c2[0]-hw[0], c2[1]-hw[1]] #外側の中心から幅のベクトルを引く
    p4 = [c2[0]+hw[0], c2[1]+hw[1]] #外側の中心に幅のベクトルを足す

    pygame.draw.polygon(screen, color, [p1, p2, p4, p3], 0)

    pygame.draw.line(screen, color, c1, c2, width)


def mojiban():
    #時計の文字盤を描く
    font1 = pygame.font.SysFont("PlemolJP", 50) #フォントを指定
    text1 = font1.render("Rolex", True, (255,255,255))
    screen.blit(text1, (350,350))
    for a in range(0,60):
        hline(a, 280, 5, 10) #秒の目盛りを描く
    for b in range(0,12):
        hline(b*5, 260, 10, 30, (255,0,0))
```

## メインループ

プログラム開始後、
現在の時間を取得してそれに合わせて時針、分針、秒針を描く
という処理を繰り返せば、時計が出来上がる

```.py
    mojiban() #文字盤を描く
    dt_now = datetime.datetime.now() #現在時間の取得
    #秒針を描く
    hline(dt_now.second, -50, 5, 250)
    #print(dt_now.second)
    #分針を描く
    hline((dt_now.minute+dt_now.second/60.0), -20, 20, 220)
    #print(dt_now.minute)
    hline((dt_now.hour+dt_now.minute/60.0)*5.0, -20, 30, 180)
    #時針を描く
    #print(dt_now.hour)
    #t = t + 0.01
    #hoshi(5,(255,0,0),(100+t,100+0.5*t),100,38) #関数を使って星を描く
```
