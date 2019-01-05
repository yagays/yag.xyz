+++
date = "2015-06-21T19:14:29+09:00"
draft = false
title = "Ipython Notebookでグラフ描画時に出力を抑制する"

+++

Ipython Notebookの上でmatplotlibを使ってグラフを描画していると，値の出力の下にグラフが描画されるときがある．plt.plotなどでは` <matplotlib.lines.Line2D at 0x111762fd0> `の1行で済むのだけれども，plt.histではn, bins, patchesといった値が出力されるので，グラフが画面下に追いやられることがある．

![](/img/ipython_notebook1.png)

そういうときは，以下のように行末にセミコロンを付けると出力を抑制することができる．

```python
plt.hist([sp.stats.uniform.rvs(0,1) for x in xrange(10000)], bins=100, normed=True);
```

![](/img/ipython_notebook2.png)

ほかには，`_ = plt.hist`のようにアンダースコアに出力を吸収させる方法もあるようだ．あと自分は何故か2行目に`print`を書くという良くわからない方法を今まで使っていたのだが，どこで仕入れた知識なのか全く思い出せない……．

### 参考

- [IPython Tips & Tricks — IPython 3.0.0-b1 documentation](https://ipython.org/ipython-doc/dev/interactive/tips.html)
- [pandas - Suppress output of object when plotting in ipython - Stack Overflow](http://stackoverflow.com/questions/14506583/suppress-output-of-object-when-plotting-in-ipython)
