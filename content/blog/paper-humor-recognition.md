+++
date = "2015-10-30T22:03:16+09:00"
draft = false
title = "\"Humor Recognition and Humor Anchor Extraction\"読んだ"

+++

- [http://aclweb.org/anthology/D/D15/D15-1284.pdf](http://aclweb.org/anthology/D/D15/D15-1284.pdf)
- [http://www.cs.cmu.edu/~hovy/papers/15EMNLP-humor.pdf](http://www.cs.cmu.edu/~hovy/papers/15EMNLP-humor.pdf)

10/24に[EMNLP2015読み会](http://connpass.com/event/20393/)に参加して"Humor Recognition and Humor Anchor Extraction"というユーモアに関する論文を紹介しました．

## スライド

<iframe src="//www.slideshare.net/slideshow/embed_code/key/FUOEqQlhmbT7tN" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/yag_ays/humor-recognition-and-humor-anchor-extraction" title="Humor Recognition and Humor Anchor Extraction" target="_blank">Humor Recognition and Humor Anchor Extraction</a> </strong> from <strong><a href="//www.slideshare.net/yag_ays" target="_blank">yag ays</a></strong> </div>

### 補足：負例を判定するタスクになっていないかどうか

質疑応答で指摘があったが，ドメインを揃える工夫をしているとはいえ，負例にニュース記事などを使っている以上はそちらの分類問題になっていないかは気になるところ．今回作った識別器を流用したAnchor Extractionでのある程度の抽出精度をもってして，ユーモアかどうかの二値分類はきちんとユーモアを識別しているとは言えるかもしれない．
