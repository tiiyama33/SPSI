# csvファイルの扱い

<p>csvファイルを扱ってみましょう。</p>
[sample.csv](sample.csv)

```.py
import csv # csvモジュール(標準)を読み込み

with open(&#039;sample.csv&#039;) as f:
    reader = csv.reader(f)
    l = [row for row in reader]
    #リストのリストとしてlにcsvファイルの内容が入る

print(l) 

with open(&#039;output.csv&#039;, &#039;w&#039;) as f:
    writer = csv.writer(f)
    writer.writerows(l) #書き出し</pre>
```
