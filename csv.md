# csvファイルの扱い

<p>csvファイルを扱ってみましょう。</p>

[sample.csv](sample.csv)

```.py
import csv # csvモジュール(標準)を読み込み

with open("sample.csv") as f:
    reader = csv.reader(f)
    l = [row for row in reader]
    #リストのリストとしてlにcsvファイルの内容が入る

print(l) 

with open("output.csv", "w") as f:
    writer = csv.writer(f)
    writer.writerows(l) #書き出し
```

[戻る](./README.md) [次へ](graphics.md)
