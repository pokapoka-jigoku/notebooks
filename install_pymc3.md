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
