# 画面に文字を表示

```.py
font1 = pygame.font.SysFont("PlemolJP", 50) #フォントを指定
gt = 100
unit ="s"
timestr = "%1.4f %s" % (gt, unit) #表示したい文字列
text1 = font1.render(timestr, True, (255,255,255)) #文字列を画像化する
screen.blit(text1, (10,10)) #画像をスクリーンに重ねる

```

# 画面の大きさを変える

ウィンドウを作るときに RESIAZABLE という属性を与える。  
実際にウィンドウの大きさが(ユーザーによって)変更されると、  
VIDEORESIZE というイベントが発生するので、必要な処理を行う。

```.py
screen = pygame.display.set_mode((800,800),flags=(pygame.DOUBLEBUF|pygame.RESIZABLE))
#ウィンドウを作成する際に、画面の大きさの変更可能(RESIZABLE)という属性を与える(フラグを立てる)

#メインループ内
    #pygameのイベント処理
    for event in pygame.event.get(): #pygameからくるイベントを順に取り出す
        #終了処理
        if event.type == QUIT: #もしイベントがQUITなら
            pygame.image.save(screen,"tokei.png") #画面をpngファイルとしてセーブ
            pygame.quit() #pygameモジュールの終了
            sys.exit() #プログラムの強制終了
        if event.type == VIDEORESIZE: #画面の大きさが変更されたら
            width = screen.get_width()
            height = screen.get_height()
            gxcenter = width/2.0
            gycenter = height/2.0
            xmax = gxtox(width)
            xmin = gxtox(0)
            ymax = gytoy(0)
            ymin = gytoy(height)            
  ```

# 粒子の軌跡を表示する
各粒子ごとに過去の表示座標を記録し、毎ターン描画する
```.py
#クラスBall内
    def __init__(self, id):
        #インスタンス作成時の処理
        #インスタンスの保持するデータ
        self.tx = [0]*500 #過去のx座標(500点分)
        self.ty = [0]*500 #過去のy座標(500点分)
        self.tc = [0]*500 #表示する透明度(500点分)
        self.t = 0 #最新の記録番号(0~499)

    def show(self):
        #ボールを表示する
        gx = xtogx(self.x) #表示する座標
        gy = ytogy(self.y)
        self.tx[self.t] = gx #self.txのリストに保存 番号[self.t]
        self.ty[self.t] = gy
        self.tc[self.t] = 255.0 #番号[self.t] 不透明度を255に設定
        self.t += 1 #次ターンのために記録番号に1を加える
        if(self.t == 500): #500点に達したら
            self.t = 0 #記録番号を0に戻す(上書き)
        pygame.draw.circle(screen, self.color, (gx, gy), rtogr(self.r))
```

軌跡の表示は 4要素による色指定 (R, G, B, 不透明度)により行うが、  
pygame.display.set_mode で作成したウィンドウは不透明度を使った描画が出来ないので、  
別に Surface と呼ばれる画像イメージを作成してそこに描画し、  
後で screen　に重ねる。

```.py
#pygameの初期化
pygame.init() #pygameモジュールの初期化
screen = pygame.display.set_mode((800,800),flags=(pygame.DOUBLEBUF|pygame.RESIZABLE)) #ウィンドウの表示
font1 = pygame.font.SysFont("PlemolJP", 50) #フォントを指定
trs = pygame.Surface((800,800), flags=pygame.SRCALPHA) #軌跡の描画用の画像イメージtrs 不透明度あり(SRCALPHA)にする

#クラスBall内
    def trajectory(self):
        #軌跡の表示
        for i in range(0,500): #500点分の過去のボールを描写
            n = self.t+i #n番目の記録についてボールを描く。描いた順に上に重なっていくので、古いものから描写する。(self.tが100だとすると、100番目から順に描く
            if n>=500: #499番目まで書いたら、0番からを描く
                n += -500
            cl = (self.color[0], self.color[1], self.color[2], self.tc[n]) #4つ目に不透明度を入れた色指定値
            pygame.draw.circle(trs, cl, (self.tx[n], self.ty[n]), rtogr(self.r)) #trsに過去のボールを描く
            self.tc[n] -= 0.5 #描いたら不透明度を(次のターンのために) 0.5 だけ下げる (古いものは色が薄くなる)
            if(self.tc[n]<0): #不透明度としてマイナスを指定するとエラーになるので、
                self.tc[n] = 0 #0より小さいときは0に設定

#メインループ内
    for i in range(0,number): #ボールの数だけ処理を繰り返す
        #ball[i].gravity() #重力を働かせる
        #ball[i].reflect()
        ball[i].solar_gravity() #重力を働かせる
        ball[i].move() #粒子を動かす
        ball[i].show() #粒子を表示する
        ball[i].trajectory() #軌跡を表示

    screen.blit(trs,(0,0)) #screenにtrsを重ねる
    pygame.display.update() #画面を更新
```

 







![image](https://github.com/user-attachments/assets/0beb48ce-0138-4186-aaaa-19ec982137ef)




