# 時計を作ろう

```.py
import pygame #モジュールpygameの読み込み
import sys
import math
import datetime #時間を扱うためのモジュール
from pygame.locals import *

pygame.init() #pygameモジュールの初期化
screen = pygame.display.set_mode((800,600)) #ウィンドウの表示

#t = 0
while True: #無限ループ
    screen.fill((0,0,0)) #黒で塗りつぶす
    pygame.display.update() #画面を更新

    #プログラムの終了処理
    for event in pygame.event.get(): #pygameからくるイベントを順に取り出す
        if event.type == QUIT: #もしイベントがQUITなら
            pygame.image.save(screen,"tokei.png") #画面をpngファイルとしてセーブ
            pygame.quit() #pygameモジュールの終了
            sys.exit() #プログラムの強制終了
```
