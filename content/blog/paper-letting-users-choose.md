+++
date = "2015-11-02T22:23:56+09:00"
draft = false
title = "\"Letting Users Choose Recommender Algorithms\"読んだ"
tags = ["Recsys2015"]
categories = ["paper-review"]
+++

[RecSys2015勉強会](http://connpass.com/event/20664/)で紹介した論文．勉強会での発表自体は著者の[スライド](http://md.ekstrandom.net/research/pubs/choose-recs/Ekstrand-RecSys2015-Slides.pdf)を使用した．

- [Letting Users Choose Recommender Algorithms](http://dl.acm.org/citation.cfm?id=2800195)

## 概要

MovieLensにおいて推薦アルゴリズムをユーザに自由に選択させたときにどういう挙動を示すのかを実験した論文．被験者のうち約25%のユーザが推薦アルゴリズムを変更し，推薦アルゴリズムの人気度合いやユーザの機能の使い方などから，ユーザの趣向を反映したと思われる実験結果が得られた．

## MovieLensにおける実験

現状のウェブサービスにおける推薦システムは，ユーザから見て仕組みがわからず推薦結果のみが示される状態にある．これまでに提案されてきた推薦アルゴリズムはそれぞれに出力の特徴が異なることから，推薦アルゴリズムの選択はユーザの趣向をある程度反映すると考えられる．

実験は映画レビューサイトの[MovieLens](http://grouplens.org/datasets/movielens/)で行われた．ウェブのインターフェイスを改良し，以下のようにサイトのメニューバーから4つの推薦アルゴリズムを選択できるようにした．

![](/img/recsys2015-movielens.png)

- "The Peasant"
  - baseline: ユーザ-アイテムのレーティング予測を用いた手法
- "The Bard"
  - pick-groups: 映画のグループベースの推薦アルゴリズム
- "The Warrior"
  - item-item: アイテム間の協調フィルタリング
- "The Wizard"
  - SVD: FunkSVDを用いた協調フィルタリング

実験対象とするユーザは既にMovieLensを利用したことのある既存ユーザのみ．対象者が検証環境にログインすると，今回の実験の概要が表示されるとともに，ランダムに上記4つのうちの一つの推薦アルゴリズムが選択された状態となる．

## 結果

対象としたユーザは合計3005人，そのうち748人(24.9%)のユーザが一度でも推薦アルゴリズムを変更し，539人が異なる推薦アルゴリズムを最終的に選択した．殆どのユーザは数回しか変更せず(だいたい20回以下)，最初に選択された推薦アルゴリズムが最終的な選択に影響することはなかった．最終的に選択された推薦アルゴリズムの人気はSVD>item-item>>>pick-groups≒baselineの順だった．

それぞれの推薦アルゴリズムの出力を比較したところ，推薦アルゴリズムの評価指標としてのRMSEはSVDが最も良く次にitem-item，baselineの順となった．推薦されるアイテム集合の多様性という面では，baselineとitem-itemは結果が類似しており，svdは2つと比較して少し異なるアイテムを推薦する傾向にあった．またbaselineは人気ある一般的な映画を推薦しがちなのに対し，SVDは一般的でないマイナーな映画も含めて幅広く推薦していた．

## 参考

- [Michael Ekstrand :: Letting Users Choose Recommenders](http://md.ekstrandom.net/blog/2015/09/choose-recommenders)
- [Michael Ekstrand :: Why User Control in Recommender Systems?](http://md.ekstrandom.net/blog/2015/09/user-control)
- [md.ekstrandom.net/research/pubs/choose-recs/Ekstrand-RecSys2015-Slides.pdf](http://md.ekstrandom.net/research/pubs/choose-recs/Ekstrand-RecSys2015-Slides.pdf)
