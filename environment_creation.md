# とりあえず最強のやつ

## 概要

1. debugger可能な基本環境の構築
2. 追加の基本ライブラリの追加
3. labextensionの導入（debugger, otc, variable_inspector)
4. Kiteの導入（Kiteインストール, pip, jupyter labextension)

## 詳細

### いちから入れていく場合

仮想環境の名前は好きにつける（以下では `default` にしてある）

``conda create -n default -c conda-forge xeus-python=0.8.0 notebook=6 jupyterlab=2 ptvsd nodejs``

``conda activate default``

``conda install numpy scipy pandas matplotlib seaborn bokeh scikit-learn -c conda-forge``

``jupyter labextension install @jupyterlab/debugger``

``jupyter labextension install @jupyterlab/otc``

``jupyter labextension install @lckr/jupyterlab_variableinspector``

``jupyter labextension list``

※ ここでKiteインストール

``pip install jupyter-kite``

``jupyter labextension install @kiteco/jupyterlab-kite``

``jupyter lab``

### チートする場合

``conda create -n default -f=just_install_these.yml``

``conda activate default``

※ ここでKiteインストール

あとは上の4つのエクステンション入れて終わり

## 参考サイト

- jupyterlab/debegger: https://github.com/jupyterlab/debugger
- kite: https://qiita.com/kirikei/items/f050683075d440292e32