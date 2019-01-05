+++
date = "2015-08-28T14:08:53+09:00"
draft = false
title = "\"Promoting Positive Post-Click Experience for In-Stream Yahoo Gemini Users\"読んだ"
tags = ["KDD2015"]
categories = ["paper-review"]
+++

- [http://www.dcs.gla.ac.uk/~mounia/Papers/kdd2015.pdf](http://www.dcs.gla.ac.uk/~mounia/Papers/kdd2015.pdf)
- [Promoting Positive Post-Click Experience for In-Stream Yahoo Gemini Users](http://dl.acm.org/citation.cfm?id=2788581)
- (スライド)[Promoting Positive Post-click Experience for In-Stream Yahoo Gemini U…](http://www.slideshare.net/mounialalmas/promoting-positive-postclick-experience-for-instream-yahoo-gemini-users)

## 概要

Yahoo!のニュース記事のネイティブアドにおいて，広告を踏んだ後の体験(post-click experience)というものを滞留時間と直帰率で表現し，それらを推定するモデルを作り広告を表示するランキングに組み込むことによってCTRなどを向上させたという論文．

## Post-click Experience

広告のコンバージョンに繋げるためにも，ユーザが広告を踏んだ後の体験というものは重要となる．このpost-click experienceを広告のランディングページの滞留時間と直帰率で表現する．

- 滞留時間 (dwell time)
  - 広告を踏んでから元のページに戻るまでの時間
- 直帰率 (bounce rate)
  - 広告を踏んでから一瞬で元のページに戻ったユーザの割合(閾値次第だが，モバイルでは5秒以内という感じで指定)

ユーザが広告を見てくれているという意味では，滞留時間が長く，直帰率が低い方が良い．

## 質の高い広告かどうかを二値分類

広告の質を測るとしても，コンバージョンの情報は広告主しか持ってないし，直接コンバージョン率を推定するのは難しい．そこで上記の滞留時間と直帰率が良いものを質の高い広告とする．それぞれに滞留時間の閾値$t\_{\delta}$と直帰率の閾値$\tau\_{\beta}$を任意に設定して，$t\_{\delta}$以上の滞留時間または$\tau\_{\beta}$以下の直帰率のものを質が高いページ($Y_i = 1$)，それ以外を$Y_i = -1$とラベル付けした．

特徴量としては以下の3タイプを用意する．それぞれの特徴量の詳細は論文参照．

- コンテンツの特徴量 \(C\)
  - 広告のランディングページに画像が幾つあってリンクが幾つあって……といったコンテンツ自体の情報
- 類似度の特徴量 (S)
  - 広告のリンクと飛んだ先のランディングページとのテキストの類似度．ネイティブアドに書かれてることに興味を持って広告を踏んだのにリンク先の内容が全然違った，みたいなことを測る指標として
- 過去のデータの特徴量 (H)
  - 広告ごとのインプレッション数，クリック数，CTRなどの情報

これらの特徴量の組み合わせを変えつつ，ロジスティック回帰，SVM，GBRTなどで，異なる閾値($t\_{\delta}$,$\tau\_{\beta}$)の条件のもと，AUCやF値などを評価した．滞留時間の推定結果はTable 2，直帰率の推定結果はTable 3を参照．

GBRTにおいては重要度も算出し，推定に効いている特徴量は何なのかも評価している(Table 4)．

## A/Bテストで実証

今回提案した広告を踏んだ後の体験を推定するモデルを，広告の表示ランキングのロジックに組み込み，A/Bテストで実際に評価した．その結果，広告単位で見た時にCTRが向上し，滞留時間の増加(+30%)や直帰率の低下(-6.7%)が見られた．それに加えて，ユーザごとに見た時も同様に滞留時間の増加と直帰率の低下が見られたことから，質の高い広告を提示することによって広告を踏んだ後の行動に影響を及ぼしていることがわかった．

## メモ

今回は大まかな流れしか追わなかったが，論文の中ではもっと詳しく広告のクオリティの計測方法や実際のpost-click experienceの解析，既存研究の紹介がされている．
