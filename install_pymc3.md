# `pymc3`のインストールの仕方

- Python 3.8

とりあえずデフォルト環境を`conda`で組んだあとに、

``pip install pymc3``

でインストール。そのあと、一緒に入る `xarray`モジュールが 0.16.1 だと互換性ないらしいので、

``pip install xarray==0.16.0``

で再インストールしたら使えるようになりました。

....

と思ったら、``arviz==0.10.0`` が ``xarray==0.16.1`` に対応しているらしく、incompatible起きちゃったので、

``pip install arviz==0.9.0``

で再インストールしなおした。
