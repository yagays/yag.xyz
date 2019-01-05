+++
date = "2016-02-13T15:18:58+09:00"
draft = false
title = "オレオレJupyter運用法 ~常にJupyter Notebookを開きつつ任意のディレクトリをターミナルから開く"

+++

## tl;dr

Jupyter Notebookは裏で常に動かしておいて，ノートブックを作りたくなったら任意のディレクトリに移動して`jd`コマンドで開くようにしとくと便利．

![](/img/jupyter.gif)

### Jupyter/ipython notebookの運用について

ぶっちゃけ[Jupyter](http://jupyter.org/)の使い方が良くわからない．とりあえず自分のやり方を書いてみるので，他に便利な使い方をしている人がいれば是非教えてほしい．

といっても，別にノートブックの使い方がわからないんじゃなくて，日常的に使い倒すにあたってどういう運用をすればよいかがわ分からないのだ．一番分からないのは，Jupyter Notebookをいつ起動するか？という問題．よし解析するかとなった時に必要に応じて起動する人もいれば，裏で常に起動しておいていつでも作業できるようにしておくというのもある．サーバに立てといてリモートで使えるようにしておくとか，あとはローカルに置いてある.ipynbのファイルをダブルクリックで起動できるようにしておくということもできるようだ([参考](http://buq.hateblo.jp/entry/2015/08/19/220453))．他に気になる運用上の問題としては，計算が重すぎてJupyter Notebook上のコマンドがkillできないときはどうするかとか，Google Cloud Datalab便利だよねとか，まあそこら辺は別の話なので今回は触れない．

Jupyter Notebookは基本的にウェブブラウザから操作するものとはいえ，やはり個人的には作業の中心には常にターミナルがあるので．そこからなんとかターミナルをベースにJupyterに適宜移動するということをしたい．毎回Jupyter Notebookを起動して使い終わったら消すのも面倒だし，かといってターミナルで作業しているのにウェブ画面からポチポチディレクトリ構造を追うのも大変だということで，自分はコマンド経由で今いるディレクトリのJupyter Notebookのページを開くエイリアスを作成して使っている．

### ターミナルから任意のディレクトリでJupyter Notebookを開けるようにする

コマンドは以下の通り．これを.bashrcなり.zshrcなりに書いておけば使えるようになる．やっていることは至極単純で，Jupyter NotebookのURLに今いるディレクトリをくっつけてopenでブラウザから開くようにしているだけ．上の例ではjdという名前のエイリアスを作成しているけれども，Jupyter-openでもjjでも他のコマンドとバッティングしなければ好きなエイリアスをつければ良い．

```
# Jupyter
export JUPYTER_URL_PATH="http://localhost:8888/"
alias jd='PWDPATH=`pwd`;open $JUPYTER_URL_PATH"tree${PWDPATH/#$HOME}"'
```

上の方に実際に使っているgifアニメを貼っているので使い方を書くまでもないけれども，ノートブックを作りたい！というディレクトリに移動して`jd`と入力するだけ．

```
# http://localhost:8888/tree/path/to/open/を開く
$ cd path/to/open/
$ jd
```

### まとめ

ということで，Jupyter Notebookをいつ起動しておけばいいか問題について自分のやり方を書き出してみた．Rをメインで使っている人たちはRStudioがあってknitrがあっていいなーと思いつつ，PythonだってJupyterになり他言語を飲み込んでデータ分析の開発環境になりつつあるので，このあたりもっとノウハウが増えればいいなと思う．

<div class="amazlet-box" style="margin-bottom:0px;"><div class="amazlet-image" style="float:left;margin:0px 12px 1px 0px;"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4873117488/yagays-22/ref=nosim/" name="amazletlink" target="_blank"><img src="http://ecx.images-amazon.com/images/I/51UNyto6saL._SL160_.jpg" alt="IPythonデータサイエンスクックブック ―対話型コンピューティングと可視化のためのレシピ集" style="border: none;" /></a></div><div class="amazlet-info" style="line-height:120%; margin-bottom: 10px"><div class="amazlet-name" style="margin-bottom:10px;line-height:120%"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4873117488/yagays-22/ref=nosim/" name="amazletlink" target="_blank">IPythonデータサイエンスクックブック ―対話型コンピューティングと可視化のためのレシピ集</a><div class="amazlet-powered-date" style="font-size:80%;margin-top:5px;line-height:120%">posted with <a href="http://www.amazlet.com/" title="amazlet" target="_blank">amazlet</a> at 16.02.13</div></div><div class="amazlet-detail">Cyrille Rossant <br />オライリージャパン <br />売り上げランキング: 5,814<br /></div><div class="amazlet-sub-info" style="float: left;"><div class="amazlet-link" style="margin-top: 5px"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4873117488/yagays-22/ref=nosim/" name="amazletlink" target="_blank">Amazon.co.jpで詳細を見る</a></div></div></div><div class="amazlet-footer" style="clear: left"></div></div>
