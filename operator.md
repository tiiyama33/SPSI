# 変数と演算子

<p>変数はデータを保持します。</p>

<h3>変数</h3>

```.py
a = 5 #代入文

b = 3+5

c = "Hello!"
```

<p>変数名はアルファベットと数字の組み合わせにより自由に設定できます(ただし、数字が先頭に来てはいけない)。大文字と小文字は区別されます。なお、メソッド名として使われているもの(printなど)は使えません。</p>
<h3>演算子</h3>
<p>数値に関する演算子（算術演算子）には次のようなものがあります。</p>
<table style="width: 99.8413%;" border="1"><caption> </caption>
<thead>
<tr>
<td style="width: 28.6169%;">演算子</td>
<td style="width: 32.7504%;">意味</td>
<td style="width: 20.6677%;">例</td>
<td style="width: 17.806%;">結果</td>
</tr>
</thead>
<tbody>
<tr>
<td style="width: 28.6169%;">+</td>
<td style="width: 32.7504%;">加算 たす</td>
<td style="width: 20.6677%;">3+2</td>
<td style="width: 17.806%;">5</td>
</tr>
<tr>
<td style="width: 28.6169%;">−</td>
<td style="width: 32.7504%;">減算 ひく</td>
<td style="width: 20.6677%;">3-2</td>
<td style="width: 17.806%;">1</td>
</tr>
<tr>
<td style="width: 28.6169%;">*</td>
<td style="width: 32.7504%;">乗算 かける</td>
<td style="width: 20.6677%;">3*2</td>
<td style="width: 17.806%;">6</td>
</tr>
<tr>
<td style="width: 28.6169%;">/</td>
<td style="width: 32.7504%;">除算 わる</td>
<td style="width: 20.6677%;">3/2.0</td>
<td style="width: 17.806%;">1.5</td>
</tr>
<tr>
<td style="width: 28.6169%;">**</td>
<td style="width: 32.7504%;">指数 乗</td>
<td style="width: 20.6677%;">3**2</td>
<td style="width: 17.806%;">9</td>
</tr>
<tr>
<td style="width: 28.6169%;">//</td>
<td style="width: 32.7504%;">整数の除算</td>
<td style="width: 20.6677%;">5//2</td>
<td style="width: 17.806%;">2</td>
</tr>
<tr>
<td style="width: 28.6169%;">%</td>
<td style="width: 32.7504%;">剰余 あまり</td>
<td style="width: 20.6677%;">5%2</td>
<td style="width: 17.806%;">1</td>
</tr>
</tbody>
</table>

<p>数値は基本的には整数として扱われます。</p>
<p>小数として扱うには、次のように .0 を付けるとよいでしょう。</p>

```.py
r = 50.0
```

<h3>データ型</h3>
<p>データには「型」があります。</p>
<table border="1"><caption> </caption>
<tbody>
<tr>
<td>float</td>
<td>浮動小数型</td>
</tr>
<tr>
<td>int</td>
<td>整数型</td>
</tr>
<tr>
<td>bool</td>
<td>論理型 (True / False)</td>
</tr>
<tr>
<td>str</td>
<td>文字列型</td>
</tr>
</tbody>
</table>

<p>Pythonでは、変数の型は自動で決まります。<br />定義した変数がどの型なのかは、type という命令で知ることができます。</p>

```.py
r = 50

print(type(r))
```

<p>&lt;type 'int'&gt; と表示されるので、 r という変数の型は int型、整数型であることがわかります。</p>
<p>小数型、整数型の変数には数値を代入することができます。</p>
<p>データの型によって、演算子の働きが変わることがあります。</p>
<table style="border-collapse: collapse; width: 100%; height: 175px;">
<thead>
<tr style="height: 35px;">
<td style="width: 25%; height: 35px;">演算子</td>
<td style="width: 25%; height: 35px;">意味</td>
<td style="width: 25%; height: 35px;">例</td>
<td style="width: 25%; height: 35px;">結果</td>
</tr>
</thead>
<tbody>
<tr style="height: 35px;">
<td style="width: 25%; height: 70px;" rowspan="2">+</td>
<td style="width: 25%; height: 35px;">整数型では「加算」</td>
<td style="width: 25%; height: 35px;">2+3</td>
<td style="width: 25%; height: 35px;">5</td>
</tr>
<tr style="height: 35px;">
<td style="width: 25%; height: 35px;">文字列型では「連結」</td>
<td style="width: 25%; height: 35px;">'2'+'3'</td>
<td style="width: 25%; height: 35px;">'23'</td>
</tr>
<tr style="height: 35px;">
<td style="width: 25%; height: 70px;" rowspan="2">*</td>
<td style="width: 25%; height: 35px;">整数型では「乗算」</td>
<td style="width: 25%; height: 35px;">2*3</td>
<td style="width: 25%; height: 35px;">6</td>
</tr>
<tr style="height: 35px;">
<td style="width: 25%; height: 35px;">文字列型では「繰り返し」</td>
<td style="width: 25%; height: 35px;">'2'*3</td>
<td style="width: 25%; height: 35px;">'222'</td>
</tr>
</tbody>
</table>

[戻る](./README.md) [次へ](list.md)
