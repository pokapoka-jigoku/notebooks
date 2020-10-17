# ぽかぽか地獄のとりあえず最強のやつ

## 概要

1. debugger可能な基本環境の構築
2. 追加の基本ライブラリの追加
3. labextensionの導入
4. Kiteの導入（Kiteインストール, pip, jupyter labextension)

## 詳細

### Pythonモジュールインスコ

- 仮想環境の名前は好きにつける（以下では `default` にしてある）
- 将来に期待して、jupyter labの`debugger`機能が使えるような環境にしてある。現状は悲しきかな、xeus-pythonがmatplotlibのインライン表示に対応していないので使っていない。

``conda create -n default -c conda-forge xeus-python=0.8.0 notebook=6 jupyterlab=2 ptvsd nodejs``

``conda activate default``

``conda install numpy scipy pandas matplotlib seaborn bokeh scikit-learn -c conda-forge``

``conda install -c conda-forge umap-learn`` も入れとこ。

### jupyter lab のエクステンション追加

まあこの辺かなあ、というのを。


``jupyter labextension install @jupyterlab/debugger``

``jupyter labextension install @jupyterlab/otc``

``jupyter labextension install @lckr/jupyterlab_variableinspector``

```
pip install jupytext --upgrade  
jupyter labextension install jupyterlab-jupytext
```

```
pip install --upgrade jupyterlab-git  
jupyter lab build
```

### ipywidgets への対応

これは正直微妙で、`bokeh`でなんとかしろという感じもするが、一応。

``pip install ipywidgets``

``jupyter nbextension enable --py widgetsnbextension``

``jupyter labextension install @jupyter-widgets/jupyterlab-manager``

これで基本使えるはず。なんだけど、labで使うときの注意点として：

1. 定義したウィジェットはきちんと表示する：

```
from IPython.display import display  
display(WIDGET)
```

2. コールバック中になにかしら`print`したいときは、`Output`にきちんと食わせる：

```
output = widgets.Output()  
display(output)

@output.capture(clear_output=True)  
def callback():  
    print('Call back!!')  
```

### Kiteインスコ

※ ここでKiteインストール

``pip install jupyter-kite``

``jupyter labextension install @kiteco/jupyterlab-kite``

``jupyter lab``


## 各拡張環境

``conda create -n [作りたい環境名] --clone default`` でデフォルト環境クローンしてから、適宜他の入れていったりしている

### default-lite

やっぱnotebookの概念にデバッガっていらんくね？！！？と思ったので、liteバージョン作ることにした。

``conda create -n default-lite python=3.8 numpy scipy pandas matplotlib seaborn bokeh scikit-learn nodejs notebook jupyterlab -c conda-forge``

``conda install -c conda-forge umap-learn``

``jupyter labextension install @jupyterlab/toc @lckr/jupyterlab_variableinspector @kiteco/jupyterlab-kite @jupyter-widgets/jupyterlab-manager``

``pip install jupyter-kite``

``pip install ipywidgets``

``jupyter nbextension enable --py widgetsnbextension``

こんなもんかな？


### skbio

``pip install scikit-bio``

### Gpy

``conda install -c conda-forge gpy``

### 深層学習&PPL系

- numpyro: Win10だとなんかうまくいかずパス
- pyro: ``pip install pyro-ppl`` で素直に入った
- tensorflow2: ``pip install tensorflow`` で素直に入った（``numpy``がバージョン変わった？）
- pytorch: ``conda install pytorch torchvision cudatoolkit=10.2 -c pytorch`` で素直に入った

参考として：
- https://deepblue-ts.co.jp/%E7%B5%B1%E8%A8%88%E5%AD%A6/%E3%83%99%E3%82%A4%E3%82%BA%E7%B5%B1%E8%A8%88/pyro-try/
- https://pytorch.org/get-started/locally/
- https://www.tensorflow.org/install/pip?hl=ja


### pymc3

``pip install pymc3``

でインストール。現時点（2020/09/24）のバージョンの問題点として：

- ``xarray==0.16.1`` に非対応 -> ``xarray==0.16.0`` に変更
- ``arviz==0.10.0`` が ``xarray==0.16.1`` に対応してしまっている -> ``arviz==0.9`` に変更
- ``pymc3``使用時に使われる ``thetano`` が ``g++`` 使うので、``m2w64-toolchain`` を入れる

順番としては、

1. `pip install arviz==0.10.0`
2. `pip install xarray==0.16.0`
3. `conda install m2w64-toolchain`


#### MinGW-w64のインストール

まずコンソール開いて

``gcc -v``

でバージョン出るかチェック。

https://www.javadrive.jp/cstart/install/index6.html

そのあと、

``conda install -c anaconda libpython``

現時点、``libpython-2.1``がインストールされた。

``import theano`` がうまくいけば、多分大丈夫なはず！

#### 注意

``libpython`` 入れたあと ``gcc -v``してみると、MinGW-w64で入れたバージョン（`gcc version 8.1.0 (x86_64-posix-seh-rev0, Built by MinGW-W64 project)`）でなく、
`gcc version 5.3.0 (Rev5, Built by MSYS2 project)` が現れるようになった……なぜ……

もしかしたら、`libpython` 入れたらそれで解決するのかも？

### opencv2

ここから該当パッケージインストールする：https://www.lfd.uci.edu/~gohlke/pythonlibs/#opencv

[参考](https://qiita.com/fiftystorm36/items/1a285b5fbf99f8ac82eb#%E4%BB%A3%E6%9B%BF%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E6%96%B9%E6%B3%95windows)によれば：

- opencv_python-3.4.6: モジュール自体のバージョン
- contrib: contrib モジュールを含む
- cp37: Python 自体のバージョン (Python 3.7.X)
- win32/win64/win_amd64
- win32: Intel CPU 32bit Windows
- win64: Intel CPU 64bit Windows
- win_amd64: AMD CPU 64bit Windows

とのことなので、`opencv_python‑4.4.0‑cp38‑cp38‑win_amd64.whl` (Python ver.によって調整）をダウンロードでOK

``pip install opencv_python‑4.4.0‑cp38‑cp38‑win_amd64.whl` で完了

## 参考サイト

- jupyterlab/debegger: https://github.com/jupyterlab/debugger
- kite: https://qiita.com/kirikei/items/f050683075d440292e32
- https://qiita.com/yuichi-takeuchi/items/f53d4feb2e28a69bd4c3
- https://qiita.com/hyt-sasaki/items/2a940cd7387c408988be
