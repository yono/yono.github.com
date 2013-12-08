---
layout: default
title: "日本語を扱う"
---
# 日本語を扱う

Python で日本語を扱う際の諸注意、Tips をまとめました。

## ソースコード中に日本語を書く
試しにコメントに日本語を使ってみましょう

    #!/usr/bin/env python
    
    # テスト
    print 'hoge'

実行してみましょう。

    $ python test.py
    SyntaxError: Non-ASCII character '\xe3' in file test.py on line 3, 
    but no encoding declared; see http://www.python.org/peps/pep-0263.html
    for details

Python はデフォルトだとソースコード中の文字列を ascii と想定して
処理を行います。そのため、マルチバイト文字を使うとエラーが発生します。

ソースコードの文字コードを指定する場合は下記の通りに書きます
(UTF-8を使う場合)。

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # テスト
    print 'hoge'

二行目に 
    # -*- coding: utf-8 -*-
と追記しました。これによって、このスクリプトが UTF-8 で書かれていることを宣言しています。

    $ python test.py
    hoge

適切に実行されました。

## Unicode 文字列に変換する
[文字列の節](../standard.html)で紹介したように、文字列には通常の文字列の他にUnicode 文字列があります。

その違いを見るために、以下のソースを実行してみましょう。

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # テスト
    hoge = 'ほげ'
    u_hoge = u'ほげ'
    print '通常の文字列: ', len(hoge), hoge
    print 'Unicode 文字列: ', len(u_hoge), u_hoge

実行結果は以下のようになったはずです。

    通常の文字列:  6 ほげ
    Unicode 文字列:  2 ほげ

どちらも文字列の長さは 2 のはずですが、通常の文字列は何故か 6 と表示されています。実は通常の文字列はバイト単位でカウントされるため、このような結果になってしまいます。

例えばスライスの結果もおかしくなってしまいます。

    print hoge[1] ## 文字化け
    print u_hoge[1] ## 「げ」が表示される

そのため、文字列に対して何かしらの処理を行う場合は Unicode 文字列に変換する必要があります。

文字列を変換する場合のメソッドをそれぞれ示します。

<img src="https://cacoo.com/diagrams/ZTEuvTZvPtnw8fc3.png" alt="文字列の相互変換">

    >>> hoge = 'ほげ'
    >>> u_hoge = unicode(hoge)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    UnicodeDecodeError: 'ascii' codec can't decode byte 0xe3 in position 0: ordinal not in range(128)
     >>> u_hoge = unicode(hoge, 'utf-8')
     >>> u_hoge = hoge.decode('utf-8')
     >>> hoge = u_hoge.encode('utf-8')
     >>>

文字コードを指定しないと Python は ascii だと想定して変換しようとするので、
ちゃんと文字コードを指定してあげましょう。

## 扱う文字コードを常に UTF-8 にする
とは言っても、毎回文字コードを指定するのは面倒です。
そこで、Python が文字列を変換する際のデフォルト文字コードを ascii から UTF-8に置き換えましょう。

そこで、まずは sitecustomize.py というファイルを作成し、以下の内容を記述してください。

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
    import sys
    sys.setdefaultencoding('utf-8')

次に、以下のコマンドを実行してみてください。

    python -c "from distutils.sysconfig import get_python_lib; print get_python_lib()"

おそらく site-packages というディレクトリの絶対パスが表示されたはずです。そのディレクトリに、先程作成した sitecustomize.py を cp してください。

これで、文字列を変換する際のデフォルトの文字コードが UTF-8 へと変更されました。

    >>> hoge = 'ほげ'
    >>> u_hoge = unicode(hoge)
    >>>

おそらく、エラーは起こらないはずです。

site-packages は、サードパーティ製のライブラリが保存されるディレクトリです。何かの際に覚えておくと便利かもしれません。
