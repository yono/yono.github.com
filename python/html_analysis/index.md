---
layout: default
title: "HTML から文章抽出して解析する際の Tips"
---
## 概要

HTML から文章などを抽出して解析する際の Tips をまとめたいと思います。
今回は、HTML 中に出現する単語を数える場合を想定しています。

このような解析の際に問題になるのは、文字コードと言語かと思います。

そこで、Python で文字コードと言語を判定する方法をまとめてみます。

## 文字コード判定

### 1. HTTP ヘッダーの charset を確認する

まずは HTTP ヘッダーの情報を利用するのが確実かと思います。
ただし、返された charset に Python が対応してない場合があるので、
codecs.lookup() で確認しています。
対応していない場合、もしくは charset の記述が間違っている場合は
LookupError となります。

    import urllib
    import codecs
    url = 'http://example.com'
    response = urllib.urlopen(url)
    charset = response.headers.getparam('charset')
    html = response.read()
   
    if charset != '':
        try:
            codecs.lookup(charset)
            html = html.decode(charset, 'replace')
        except:
            pass

### 2. head タグ内の meta charset を確認する

HTML 中に書かれている charset は特にスペルミスなどで間違っている
ことが多いので、注意する必要があります。
また、x-sjis や x-euc は Python の文字コード判定に弾かれるので
それぞれ Shift\_JIS と EUC-JP として扱うようにしています。

    import re
    char_re = re.compile(r"(?is)content=[\"'].*?;\s*charset=(.*?)[\"']")
    enc_dic = {"x-sjis": "Shift_JIS", "x-euc": "EUC-JP"}
    result = char_re.search(html)
    if result is not None:
        enc = resulg.group(1)
        if enc in enc_dic:
            enc = enc_dic[enc]
    else:
        enc = ''

### 3. Try and Error で手当たりしだいに decode する

ここに関しては[Python Tips](http://mimosa-pudica.net/python-tips.html)
を参考、というかほぼそのまま使わせていただいています。

判定する文字コードの順番を変えてますが、これは確かこっちの方が正解率が
高かったとかそういう理由だったと思います。

    encodings = [
            "ascii",
            "utf-8",
            "euc-jp",
            "cp932",
            "iso-2022-jp",
    ]


    def detect( text ):
            bestScore = -1
            bestEnc = None
            for enc in encodings:
                    try:
                            unicode( text, enc )
                    except UnicodeDecodeError, err:
                            if err.end > bestScore:
                                    bestScore = err.end
                                    bestEnc = enc
                    else:
                            return {
                                    "encoding": enc,
                                    "confidence": 1.0,
                            }

            return {
                    "encoding": bestEnc,
                    "confidence": bestScore / (bestScore + 2.0),

### 4. chardet を使う

chardet([Universal Encode Detector](http://chardet.feedparser.org/)) とは Python 用の文字コード判定モジュールです。
精度は決して悪くないんですが、処理が重たいので優先順位を低めにして
使用しています。

    import chardet
    enc = chardet.detect(html)['encoding']

## 言語判定

ngram.py と Lingua::LanguageGuesser の言語モデルを組み合わせる方法を
使ってました。

ngram.py: [http://thomas.mangin.me.uk/](http://thomas.mangin.me.uk/)

ページ右側の検索窓で ngram.py で検索するとダウンロードリンクを見つけることができます。

もしくは [ngram.py](http://thomas.mangin.me.uk/data/source/ngram.py) から直接。

Lingua::LanguageGuesser: [http://gensen.dl.itc.u-tokyo.ac.jp/LanguageGuesser/hajimete\_monogatari.html](http://gensen.dl.itc.u-tokyo.ac.jp/LanguageGuesser/hajimete_monogatari.html)

この二つのモジュールは TextCat という Perl で書かれた言語判定スクリプトの

- Python 移植版 → ngram.py
- Perl モジュール化 & UTF-8 への対応を強化 → Lingua::LanguageGuesser

という関係性があります。

ngram.py を動かすには言語モデル（言語ごとのプロファイル）を記録したファイルが必要です。配布元では TextCat の言語モデルを使用するように、とされてますがこれに Lingua::LanguageGuesser の言語モデルを使用することで UTF-8 へ対応できるようにします。

ちなみに、UTF-8 かどうかで判定に使用する言語モデルを変更する必要が
あるので、使用する場合はその辺りも考慮する必要があります。

    imporg ngram

    n = ngram._NGram()
    if enc == 'UTF-8':
        l = ngram.NGram('/path/to/utf8/modeldir')
    else:
        l = ngram.NGram('/path/to/nonutf8/modeldir')
    lang = l.classify(text)

いろいろはしょってますがこんな感じで。ちなみに、LanguageGuesser の
言語モデルは /usr/lib/perl5/site\_perl/5.8.8/Lingua/LM\_utf8/ 
辺りにありました（CentOS の場合）。

一つ注意点として。この方法だと、例えば英語スパムが大量に付けられた
ブログ記事を英語と判定してしまったり、ソースコードが大量に書かれた
記事を英語として判定してしまったり、といったことが起こりえます。

そのような場合には、あらかじめ記事本文のみを抽出したり、
ソースコードを取り除くといった処理が必要になります。
