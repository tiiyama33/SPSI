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

#画面変数の設定
width = screen.get_width()
height = screen.get_height()
gxcenter = width/2.0
gycenter = height/2.0

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

メインループ内で、速度による位置の変化を反映させればよい。

位置xは

$\displaystyle v = \frac{{\rm d}x}{{\rm d}t}$

より

$\Delta x = v \Delta t$

位置 $x$ に、速度 $v$ と時間の刻み幅 $\Delta t$ の積を加えればよい。

2次元のシミュレーションなので、x方向、y方向について行う。

```.py
#クラスBall内
    def move(self):
        #粒子を動かす
        self.x += self.vx*t_scale
        self.y += self.vy*t_scale

#メインループ内
    for i in range(0,number): #ボールの数だけ処理を繰り返す
        ball[i].move() #粒子を動かす
        ball[i].show() #粒子を表示する
```

また、速度 $v$ は

運動方程式  

$\displaystyle f=ma$  

に従い、力を受けると変化する。
上式より  

$\displaystyle a = \frac{f}{m}$  

a は加速度なので  

$\displaystyle a=\frac{{\rm d}v}{{\rm d}t}=\frac{f}{m}$  

$\displaystyle \Delta v = \frac{f}{m} \Delta t$

速度 $v$ に、 $\frac{f}{m}$ と時間の刻み幅 $\Delta t$ の積を加えればよい。


## 地球表面での重力場
$f = mg$  

$g = \rm 9.80665 \ m \ s^{-2}$

```.py
#クラスBall内
    def gravity(self):
        #地球表面での重力
        self.vy += -9.80665*t_scale

#メインループ内
    for i in range(0,number): #ボールの数だけ処理を繰り返す
        ball[i].gravity() #地球表面での重力
        ball[i].move() #粒子を動かす
        ball[i].show() #粒子を表示する
```        

## 床との反射
床の位置(y座標)を取得しておく
```.py
ymin = gytoy(height)
```
ボールのy座標が(床の位置-ボールの半径)より小さくなったら、反射処理をする

```.py
#クラスBall内
    def reflect(self):
        #床での反射
        if((self.y<(ymin+self.r)) and (self.vy<0)): #ボールの位置が床より低く、かつ下向きに進んでいる場合
            self.vy *= -1 #進む向きを反転させる 1ではなく0.8等にすると非弾性衝突になる
            self.y = 2*(ymin+self.r)-self.y #床にめり込んでいる距離だけ床から離す

#メインループ内
    for i in range(0,number): #ボールの数だけ処理を繰り返す
        ball[i].gravity() #地球表面での重力
        ball[i].reflect() #床での反射
        ball[i].move() #粒子を動かす
        ball[i].show() #粒子を表示する

```        

## 重力相互作用
惑星の動きのシミュレーションをする

画面の中心に太陽を置く  
太陽は動かないものとする
```.py
#メインループ内
    pygame.draw.circle(screen, (255,255,0), (xtogx(0), ytogy(0)), 10)
```

太陽からの引力は
$\displaystyle f = -G\frac{Mm}{r^2}$  

力は太陽(画面の中心)に向かって働くことに注意

$G = \rm 6.67430 \times 10^{-11}\ m^3\ kg^{-1}\ s^{-2}$  
1 au = 149597870700 m  
1 day = 86400 s

r_scaleを画面の大きさが地球の公転半径の4倍ぐらいになるように設定

$\rm 1\ pixel = (1.5 \times 10^{11} \rm\ m)*4/(800\ pixel) = 7.5 \times 10^{8}\ m$  
$\rm 7.5 \times 10^{8}$ m/pixel

t_scaleを画面上30秒ぐらいで地球が太陽の周りを1周するぐらいに設定

(100 frame s^-1)(30 s) = (365\*24\*60\*60 s)  
10500 s/frame 

```.py
r_scale = 7.5*10**8
t_scale = 10000
```


天体名<br>name|質量<br>mass|軌道長半径<br>semi-major axis|離心率<br>eccentricity|近点距離<br>pericenter|遠点距離<br>apocenter|近点距離における速度<br>speed at pericenter|遠点距離における速度<br>speed at apocenter|公転周期<br>pariod
---|---|---|---|---|---|---|---|---
||kg|au||au|au|m/s|m/s|day
太陽<br>sun|1.9891E+30|||||||
水星|3.301E+23|0.3871|0.20563069|0.30750036|0.46669964|5.8987E+04|3.8865E+04|87.95444537
金星|4.8673E+24|0.72333199|0.00677323|0.718432696|0.728231284|3.5265E+04|3.4790E+04|224.6617052
地球|5.972E+24|1|0.0167|0.9833|1.0167|3.0292E+04|2.9296E+04|365.1929807
火星|6.4171E+23|1.5237|0.09348|1.381264524|1.666135476|2.6505E+04|2.1974E+04|686.8662933
木星|1.8986E+27|5.2026|0.04851|4.950221874|5.454978126|1.3717E+04|1.2447E+04|4331.582193
土星|5.688E+26|9.53707032|0.0541506|9.02063224|10.0535084|1.0185E+04|9.1387E+03|10754.33262
天王星|8.6811E+25|19.19126393|0.04716771|18.28605596|20.0964719|7.1290E+03|6.4867E+03|30702.16235
海王星|1.02413E+26|30.181|0.0097|29.8882443|30.4737557|5.4755E+03|5.3703E+03|60549.74681
冥王星|1.3E+22|39.44506973|0.250248713|29.5739918|49.31614766|6.1251E+03|3.6731E+03|90471.57777
ケレス|9.393E+20|2.769|0.076|2.558556|2.979444|1.9319E+04|1.6590E+04|1682.703147
エリス|1.6466E+22|68.004|0.433|38.558268|97.449732|5.7429E+03|2.2723E+03|204797.5513
ハウメア|4.006E+21|43.032|0.197|34.554696|51.509304|5.5445E+03|3.7195E+03|103088.5355
マケマケ|4E+21|45.354|0.163|37.961298|52.746702|5.2142E+03|3.7526E+03|111544.0648
ハレー彗星|2.2E+14|17.83414429|0.96714291|0.585978084|35.0823105|5.4582E+04|9.1167E+02|27504.35643

上記の値は質量、軌道長半径、離心率を文献値(wikipedia等)から得て、
他の値はそこから計算で求めた。(ので、実際の値とは少しずれている)

![image](https://github.com/user-attachments/assets/0c75ee76-1256-4b90-abc8-006c051a1b5f)






