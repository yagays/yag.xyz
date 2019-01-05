+++
date = "2015-06-27T17:23:25+09:00"
draft = false
title = "scikit-learnやpandasのドキュメントをDashから確認する"

+++

最近家ではもっぱらscikit-learnやpandasを書いているのだけれども，コードを書いている時の大半はドキュメントを引いているかサンプルコードをネットで探していると言っても過言ではない．最初はわざわざGoogleで"scikit-learn randomforest"とか"pandas read_csv"などで検索して情報を探していたが，この方法だと明らかに面倒だし，ブラウザのタブは増えるわ何度も重複して検索してしまうわと何かと不便だった．また，ネットワーク速度の出ない外出先ではまともに表示すらできないということで，ローカルで検索できるドキュメント環境を整備する必要があった．当初はipythonで

```
In [1]: import pandas as pd

In [2]: pd.read_csv?
```

などとしてドキュメントを眺めていたのだけれども，とにかく検索性が悪く，また常にipythonを開いているわけでもないので，結局は「Dash - API Docs & Snippets」というアプリケーションを使うことにした．Dashは基本的に無料で利用することができるが，機能制限としてドキュメント表示まで10秒ほど待たされる．Dashのアプリ内課金で解除することができるので，一度使ってみて良かったら購入すれば良いだろう．2015/06/27現在で2400円と安くはない値段だが，少なくとも購入に値するだけの便利さはあると思う．

- [Dash for OS X - API Documentation Browser, Snippet Manager - Kapeli](https://kapeli.com/dash)

<a href="https://geo.itunes.apple.com/us/app/dash-api-docs-snippets/id458034879?mt=12&uo=6" target="itunes_store" style="display:inline-block;overflow:hidden;background:url(http://linkmaker.itunes.apple.com/images/badges/en-us/badge_macappstore-lrg.png) no-repeat;width:165px;height:40px;@media only screen{background-image:url(http://linkmaker.itunes.apple.com/images/badges/en-us/badge_macappstore-lrg.svg);}"></a>

### User Contributedなドキュメントのインストール
さて，Pythonなどのメジャーな言語であったりNumpyやScipyなどのライブラリのドキュメントはAll Docsetsで標準的に提供されているが，一方でscikit-learnやpandasなどのマイナーなものは有志によってDocsetが作成されており，それをインストールすることができる．以下のようにDashのPreferenceからUser Contributedを選択することで，目的のドキュメントを探すことができる．

![](/img/dash1.png)

現在のところ自分が入れているPython系のドキュメントは以下の通り．普段使わない余計なのも含まれているが，必要なものはだいたい揃っている．

- Numpy
- Scipy
- python 2
- python 3
- scikit-learn
- pandas
- NLTK
- Seaborn
- matplotlib
- Theano

あと，これはすべてのドキュメントに言えることだが，ドキュメントのバージョンは常に気にしたほうが良いだろう．Dashはドキュメントのバージョンのアップデートを自動で行ってくれるが，そもそものドキュメントが常に最新のバージョンを追ってくれているかは保証されないので，利用の際には注意が必要となる．

### Dashでドキュメントを検索する

実際に検索すると，ウェブ上のドキュメントと同様のフォーマットで表示される．こうやってオフラインで高速に検索できるということで，ドキュメントを引く煩わしさはだいぶ解消された．あとは，安易に人のblogなどのコードを丸写しするのではなく，ちゃんとドキュメントを見る癖が付けばいいのだけれども……．

![](/img/dash2.png)

Dashはドキュメント検索の他にも，GoogleやStack Overflowでの検索機能や，エディターとの連携やSnippetの登録などができるので，これから色々と試してみたいところ．

### 参考

- [Kapeli/Dash-User-Contributions](https://github.com/Kapeli/Dash-User-Contributions)
