---
layout: default
title: "式／文"
---
Python における演算子や制御構造について。

## 演算子

### 算術演算子
Python で扱える算術演算子は次の通りです。

<table>
<thead>
<tr>
<th>演算子</th>
<th>内容</th>
<th>例</th>
</tr>
</thead>
<tbody>
<tr>
    <td>*</td>
    <td>乗算</td>
    <td>4 * 3 = 12</td>
</tr>
<tr>
    <td>/</td>
    <td>除算</td>
    <td>8 / 3.0 = 2.6666666666666665</td>
</tr>
<tr>
    <td>//</td>
    <td>切り捨て除算</td>
    <td>8 // 3.0 = 2.0</td>
</tr>
<tr>
    <td>%</td>
    <td>剰余</td>
    <td>5 % 3 = 2</td>
</tr>
<tr>
    <td>+</td>
    <td>加算</td>
    <td>2 + 8 = 10</td>
</tr>
<tr>
    <td>-</td>
    <td>減算</td>
    <td>3 - 1 = 2</td>
</tr>
</tbody>
</table>

### べき乗演算子
べき乗演算子は ** です。

    >>> 3 ** 2
    9
    >>>

## 文

### while 文
while 文は、式の値が真である間、実行を繰り返すために使われます。

    i = 0
    while i < 5:
        print i
        i += 1

### for 文
for 文は、シーケンス（文字列、タプルまたはリスト）や、その他の反復可能なオブジェクト内の要素に渡って反復処理を行うために使われます。

    text = 'aiueo'
    for c in text:
        print c

    for i in xrange(10):
        print i

### if 文
if 文は、条件分岐を実行するために使われます。

    for i in xrange(1, 31):
        if i % 15 == 0:
            print 'FIzzBuzz'
        elif i % 3 == 0:
            print 'Fizz'
        elif i % 5 == 0:
            print 'Buzz'
        else:
            print i

### pass 文
pass 文は NULL 操作です。何も実行されません。

    # a function that does nothing
    def function():
        pass 

    # a class with no methods
    class Klass:
        pass

    
