---
layout: default
title: "Java Kuche 第 10 回勉強会"
---
# Java Kuche 第 10 回勉強会

[Java Kuche 第 10 回勉強会](http://www.java-kuche.org/modules/eguide/event.php?eid=15)に発表者として参加してきました。発表ネタはマルコフ連鎖とよのぼっとについて。一応発表資料を slide share に上げてあります([http://www.slideshare.net/yono05/javakuche-yono]())。

## 質疑応答で覚えてる分
- Q. 形態素解析はしてないんですか？
    - A. MeCab 使ってます。ただ、MeCab が分けた状態だと単語として扱うには適切でない場合があるので、ある程度前処理で再結合してます（[http://github.com/yono/py-extractword]())。
- Q. よのぼっとへの reply を意味解析したりなどの予定は？
    - A. 将来的にはそういう方向に進めたいです。ただ、現状ではまだまだそこまで行くのは難しそうです。
- Q. 任意の N 階マルコフ連鎖に対応する場合、DB のカラムはどうしてるんですか？
    - A. CREATE TABLE する際にカラムの数を動的に決定しています。

