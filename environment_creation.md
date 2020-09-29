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

### skbio

``pip install scikit-bio``

### pymc3

これもややこしい。あとでかく

### opencv2

これがややこしい。あとでかく

### 

## 参考サイト

- jupyterlab/debegger: https://github.com/jupyterlab/debugger
- kite: https://qiita.com/kirikei/items/f050683075d440292e32
- https://qiita.com/yuichi-takeuchi/items/f53d4feb2e28a69bd4c3
- https://qiita.com/hyt-sasaki/items/2a940cd7387c408988be
