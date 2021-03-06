---
layout: default
title: "Python のマルコフ連鎖ライブラリとその活用方法（Twitter Bot)"
---
## マルコフ連鎖とは

- ひとつ前の状態における情報のみを参考に、現在の状態が決定する
- 応用例
    - 手書き文字認識
    -  音声認識

## マルコフ連鎖による文章生成とは

- マルコフ連鎖を利用して新しい文章を生成する
- チャットや Twitter などに存在する「人口無脳」に応用される
- [ 実例を見せる ]

## マルコフ連鎖による文章生成をライブラリとして開発

- python-markovchains の紹介
    - 読み込ませた文章をもとに文章生成
    - 文章をDBに読み込ませる
    - DBのデータをもとに文章生成
- github で公開してます → <http://github.com/yono/python-markovchains>

## よのぼっと

- Twitter 上で動作する Bot → <http://twitter.com/yonobot>
- python-markovchains を内部で利用している
- <http://twilog.org/yono> から発言を収集

## API提供 on GAE

<http://markovchain-y.appspot.com/>

- REST API を実装すればどこからでも扱えて便利
- python-markovchains をそのまま用いるだけでは使い物にならない
    - 発言生成時に CPU に負荷がかかりすぎ
        - cron で定期的に発言を生成して、発言キューに保存
        - アクセスがあったら発言キューから取り出す

