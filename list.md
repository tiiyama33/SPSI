# リスト

<pre class="brush: python; gutter: false">d = [3, 7, 9, 1]</pre>
<p>リストは複数の値(要素)を保持します。</p>
<p>リストは角かっこ [] で囲い、要素をカンマ , で区切ります。</p>
<p>[n] で n 番目の要素にアクセスできます。(最初が 0 番)</p>
<pre class="brush: python; gutter: false">d = [3, 7, 9, 1]

print(d)    # [3,7,9,1]

print(d[2])    # 9

print(min(d))    # 1

d[2] = 3    #要素の書き替え

print(d)    # [3,7,3,1]

d.append(6)    #要素の追加

print(d)    # [3,7,3,1,6]

del d[1]    #要素の削除

print(d)    # [3,3,1,6] 
</pre>
<h3>リストのリスト</h3>
<p>リストには異なる型のデータを格納できます。</p>
<p>また、リストの中にリストを入れることもできます。</p>
<pre class="brush: python; gutter: true">e = [3, 2.0, &#039;Yeah&#039;, True]

print(e[2])    # &#039;Yeah&#039;

f = [6, [2, 5], 4]    # 1番目の要素としてリストを入れる    

print(f[1])    # [2, 5]

print(f[2])    # 4

print(f[1][1])    # 5  リストの中のリストの要素取り出し</pre>
<h3>タプル</h3>
<p>リストに似たデータの集合体としてタプルがあります。</p>
<p>タプルは丸かっこ () で囲い、要素をカンマ , で区切ります。</p>
<p>タプルは要素の変更ができません。</p>
<pre class="brush: python; gutter: false">g = (3, 4, 9, 2)

print(g[2])    # 9 アクセス法はリストと同じ</pre>
