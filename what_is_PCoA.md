# はじめに：PCoAとはなんなのか

DNA解析や生態学系でよく出てくるPCoAですが、その理論的な実情にあまり触れられている記事がなかったのでまとめておきたい。

# 調べましたが

マジで説明出てこない。やっと見つけた一つ目がこれ（[URL](https://stats.stackexchange.com/questions/222664/how-does-principal-coordinate-analysis-pcoa-work-as-compared-to-pca?noredirect=1&lq=1)）：


> PCoA, aka **Torgerson's metric MDS**, is quite transparently described in some answers to this question. I even tend to consider your question as a duplicate to it. Both @amoeba and me have explained there how PCoA is actually a PCA in its core. For example, in my answer find link to a document describing PCoA's math and link to an answer explaining "double centration" which is a switch from distances to scalar products of a centered "Gram matrix". The coordinates yielded by PCoA are actually "loadings" of the PCA performed on that matrix. 

簡単に意訳しておきます：

>「Torgersonの計量MDS」として知られるPCoAについては（中略）前にも質問があったのでそっちも見てください（PCoAが実際どんな感じでその核心がPCAなのか説明してます）。たとえば、私の回答の中にあるリンク先では、PCoAの数学的議論と、距離行列をスカラー積（中心化した「グラム行列」）に変換する方法（「double centration」）の説明があります。PCoAで生成される座標(coordinates)は、その行列で実施されたPCAのローディングに相当します。

すぐあとで「以前の質問」も見てみますが、かいつまむと：

- PCoAとはメトリックMDSの一種（Torgerson法）
- 距離行列から中心化したグラム行列を作成し、それを使ってPCAする
- PCoAの「座標」とは、PCAのローディングに相当する

次にこれ（[URL](https://stats.stackexchange.com/questions/14002/whats-the-difference-between-principal-component-analysis-and-multidimensional)）。「PCAと古典的(classic)MDSのちがいは？」に対して：

> Classic Torgerson's metric MDS is actually done by transforming distances into similarities and performing PCA (eigen-decomposition or singular-value-decomposition) on those. [The other name of this procedure (distances between objects -> similarities between them -> PCA, whereby loadings are the sought-for coordinates) is Principal Coordinate Analysis or PCoA.] So, PCA might be called the algorithm of the simplest MDS.

基本さっきと同じことを書いてあるが、

- 距離行列 -> 類似度行列 -> PCA という流れ
- MDSの最も単純なアルゴリズム（つまりユークリッド距離の行列）がPCAとも言える

というのはなるほどって感じ。

数学的背景もリンクあったが（[URL](http://web.archive.org/web/20160315120635/http://forrest.psych.unc.edu/teaching/p230/Torgerson.pdf)）、張るだけに留める。

と思いきや、とてもいいMDSに関する日本語記事を見つけた（ありがとうございます）：

https://fronori.hatenadiary.org/entry/20140517/1400517060

> 古典的（Classical）MDS：　別名、主座標分析（Principal Coordinate Analysis, PCoA, PCO）、Torgerson Scaling、Torgerson & Gower scaling。数学的にはEckart & Young (1936)、およびYoung & Householder (1938)の定理に基づき、非類似度行列を**二重中心化**したものの固有値と固有ベクトルを求め、スペクトル分解することに対応する（Torgerson, 1952, 1958; Gower, 1966)。Strainと呼ばれる損失関数を最小化する布置を求める。PCoAは距離としてユークリッド距離を用いた場合は、主成分分析（PCA）と数学的に同等となる。

二重中心化というのは、ヤング・ハウスホルダー変換のことでもあるらしく、下記のページに記載があった。

http://lbm.ab.a.u-tokyo.ac.jp/~omori/similar_visual.html

これのp.22とかもよき。

http://www.is.ouj.ac.jp/lec/data/C07/C07.pdf


# 結局なんなのか

MDSでした。

…とはいえ、結構妙技であるので一応ちゃんと言葉にしておく。

PCoAは、与えられた距離行列の距離関係をなるべく保つようにユークリッド空間にデータを埋め込む手法です。これは言い換えると、「ユークリッド空間において、各点間の距離が距離行列を満たすようにする」ということです。ユークリッド空間に埋め込むということは、距離行列をユークリッド距離とみなすということであり、それはすなわち**埋め込み後のユークリッド空間において2点間の内積を決めてしまう**のに相当するはずです。であれば、距離行列からなんとか頑張って分散共分散行列を作って、それを対角化すれば、PCAに相当することができるのでは？

PCoAはカーネルPCAと非常に近いアイデアで、ただし、カーネルではなく非線形な距離関数を与えてやることで、ユークリッド空間に点集団を埋め込んでやろうという方法っぽいです。

まあいずれにせよ、単なるMDSの一種でした。``sklearn.manifold.MDS``を使おう！

しかし、PCoAでは各軸の説明分散率 `explain_variance_ratio` が出るが、上のMDSだとそういうのは出てくれない。

であれば、上の説明が正しければ、**過剰次元にMDSしてからPCA** して座標軸整えてやればいいのではないか？ぽかぽか地獄はそう思うわけでした。


