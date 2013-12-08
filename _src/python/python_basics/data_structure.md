---
layout: default
title: "データ構造"
---
# データ構造

Python で扱えるデータ構造について。

## リスト

いわゆる配列です。任意のオブジェクトを要素として持つことができます。

    ## リストの生成
    alist = [1, 2, 'hoge']

    ## リスト自体もリストの要素として持つことができる
    alist = [1, 2, ['fuga', 3]]

    ## リストの末尾に要素を追加
    alist.append(3)
    print alist
    [1, 2, ['fuga', 3], 3]

    ## リストの末尾から要素を取り除く
    alist.pop()
    print alist
    alist = [1, 2, ['fuga', 3]]

    ## 指定した位置に要素を追加
    alist.insert(0, 'hoge')
    print alist
    alist = ['hoge', 1, 2, ['fuga', 3]]
    
    ## 指定したリストを対象のリストに追加し、リストを拡張
    alist.extend(['aaa', 'bbb'])
    print alist
    alist = ['hoge', 1, 2, ['fuga', 3], 'aaa', 'bbb']

## タプル

リストによく似ていますが、一旦作成すると変更することができません。

    >>> atuple = (1, 2, 'aaa')
    >>> atuple[0] = 2
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: 'tuple' object does not support item assignment
    >>> 

タプルは () で囲みます。要素が一つだけの場合は要素の後にカンマが必要です。

    >>> ('hoge')
    'hoge'
    >>> ('hoge',)
    ('hoge',)
    >>> 

## 辞書

いわゆる連想配列（ハッシュ）です。{ key1: value1, key2:value2, ...} の形式で生成します。

    >>> adict = {'hoge':1, 'fuga':2}
    >>> adict['hoge']
    1
    >>> adict['piyo']
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    KeyError: 'piyo'
    >>> adict.get('piyo', 0)
    0
    >>> 'hoge' in adict
    True
    >>> 'piyo' in adict
    False
    >>> adict.keys()
    ['fuga', 'hoge']
    >>> adict.values()
    [2, 1]
    >>> adict.items()
    [('fuga', 2), ('hoge', 1)]
    >>> 

