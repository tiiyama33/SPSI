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
gycenter = width/2.0

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
$\displaystyle r = -G\frac{Mm}{r^2}$  

G = 6.67430 x 10^-11 m^3 kg^-1 s^-2

		質量	軌道長半径	離心率	近点距離	遠点距離	標準重力パラメータ	近点距離における速度	遠点距離における速度	公転周期
	太陽	1.9891E+30								
planet	水星	3.301E+23	0.3871	0.20563069	0.30750036	0.46669964	1.32759E+20	5.8987E+04	3.8865E+04	87.95444537
	金星	4.8673E+24	0.72333199	0.00677323	0.718432696	0.728231284	1.32759E+20	3.5265E+04	3.4790E+04	224.6617052
	地球	5.972E+24	1	0.0167	0.9833	1.0167	1.32759E+20	3.0292E+04	2.9296E+04	365.1929807
	火星	6.4171E+23	1.5237	0.09348	1.381264524	1.666135476	1.32759E+20	2.6505E+04	2.1974E+04	686.8662933
	木星	1.8986E+27	5.2026	0.04851	4.950221874	5.454978126	1.32885E+20	1.3717E+04	1.2447E+04	4331.582193
	土星	5.688E+26	9.53707032	0.0541506	9.02063224	10.0535084	1.32796E+20	1.0185E+04	9.1387E+03	10754.33262
	天王星	8.6811E+25	19.19126393	0.04716771	18.28605596	20.0964719	1.32764E+20	7.1290E+03	6.4867E+03	30702.16235
	海王星	1.02413E+26	30.181	0.0097	29.8882443	30.4737557	1.32765E+20	5.4755E+03	5.3703E+03	60549.74681
dwarf planet	冥王星	1.3E+22	39.44506973	0.250248713	29.5739918	49.31614766	1.32759E+20	6.1251E+03	3.6731E+03	90471.57777
	ケレス	9.393E+20	2.769	0.076	2.558556	2.979444	1.32759E+20	1.9319E+04	1.6590E+04	1682.703147
	エリス	1.6466E+22	68.004	0.433	38.558268	97.449732	1.32759E+20	5.7429E+03	2.2723E+03	204797.5513
	ハウメア	4.006E+21	43.032	0.197	34.554696	51.509304	1.32759E+20	5.5445E+03	3.7195E+03	103088.5355
	マケマケ	4E+21	45.354	0.163	37.961298	52.746702	1.32759E+20	5.2142E+03	3.7526E+03	111544.0648
	ハレー彗星	2.2E+14	17.83414429	0.96714291	0.585978084	35.0823105	1.32759E+20	5.4582E+04	9.1167E+02	27504.35643
![image](https://github.com/user-attachments/assets/f8223bd3-7a8f-41b6-8f45-b4c08cf98e0f)





