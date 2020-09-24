# `pymc3`のインストールの仕方

- Python 3.8

とりあえずデフォルト環境を`conda`で組んだあとに、

``pip install pymc3``

でインストール。現時点（2020/09/24）のバージョンの問題点として：

- ``xarray==0.16.1`` に非対応 -> ``xarray==0.16.0`` に変更
- ``arviz==0.10.0`` が ``xarray==0.16.1`` に対応してしまっている -> ``arviz==0.9`` に変更
- ``pymc3``使用時に使われる ``thetano`` が ``g++`` 使うので、``m2w64-toolchain`` を入れる

順番としては、

1. `pip install arviz==0.10.0`
2. `pip install xarray==0.16.0`
3. `conda install m2w64-toolchain`


## MinGW-w64のインストール

まずコンソール開いて

``gcc -v``

でバージョン出るかチェック。

https://www.javadrive.jp/cstart/install/index6.html

そのあと、

``conda install -c anaconda libpython``

現時点、``libpython-2.1``がインストールされた。

## チェック

``import theano`` がうまくいけば、多分大丈夫なはず！

## 注意

``libpython`` 入れたあと ``gcc -v``してみると、MinGW-w64で入れたバージョン（`gcc version 8.1.0 (x86_64-posix-seh-rev0, Built by MinGW-W64 project)`）でなく、

`gcc version 5.3.0 (Rev5, Built by MSYS2 project)` が現れるようになった……なぜ……

もしかしたら、`libpython` 入れたらそれで解決するのかも？
