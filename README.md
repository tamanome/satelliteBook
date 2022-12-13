# Pythonで学ぶ衛星データ解析基礎

<a href="https://gihyo.jp/book/2022/978-4-297-13232-3"><img src="https://gihyo.jp/assets/images/cover/2022/thumb/TH320_9784297132323.jpg" title="book" width="150" border="0" /></a>

## 本書籍の概要

本書籍は，Pythonによる衛星データ解析に興味がある初学者に向けた入門書となっています。学校の情報の授業等で利用する際の副教材になることを意識し，衛星データだけでなくデータサイエンスの基礎的な内容も含めました。学校で地球環境やご自身が住んでいる地域がどのように変化しているか調べたい方はもちろんのこと，衛星データを使って何かビジネスを始めたい方にも読んでいただきたいと思っています。従来のデータサイエンスの教材の場合には身近なデータを利用することが難しかった中で，衛星データであれば身近な地域のデータを利用して解析することができます。少しのプログラミング変更で解析対象地域を変えることができるようになっているので，関心のある地域の変化についてぜひ調べてみてください。

## 本書籍のコードを実行する

本書籍ではGoogle Colaboratoryを利用して学習を進めることを想定しています。Colabはインストールすることなしに、分析を進めることができますので、以下のブックマークレットから対象のノートブックを実行してください。

<a href="https://colab.research.google.com/github/tamanome/satelliteBook/blob/main/" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a> 

## 本書籍で用いるデータについて

API等を利用してデータをダウンロードする方法は紹介しているものの、本書籍で利用するデータの多くは技術評論社のページにアップロードしています。
データは以下から適宜ダウンロードしてコードを実行してください。

（準備中）[ダウンロードはこちらから]()。

本書籍で利用するデータは無料でご利用頂けるもので構成されていますが，一部会員登録（無料）が必要となります．
・[Sentinel]()：01_ch3-1DataAccess.ipynb,

## 修正箇所

[こちらのスプレッドシート](https://docs.google.com/spreadsheets/d/1dNwlP8ZvFief8ZRS22i-b_T17BwQmhEMHpFv81cUmLc/edit#gid=0)が対応表となります。

## 更新履歴

### 11/12/2022

11_ch6_classification.ipynbのs2folderに割り当てるパスの変更。

```python
pointfile = '/content/stratified_points.gpkg' #任意のパス
s2folder = r'/content/s2_classification' #任意のパス

randomPoints = gpd.read_file(pointfile)

# 各点のピクセル値を読み取る
for root, folders, files in os.walk(s2folder):
    for file in files:
        f = os.path.join(root, file)
        if os.path.isfile(f) and f.endswith('.tif'):
          bandRaster = rxr.open_rasterio(f).sel(band=1)
          randomPoints_stats = pd.DataFrame(point_query
                                            (randomPoints,\
                                             bandRaster.values,\
                                             affine=bandRaster.rio.transform(),\
                                             nodata=bandRaster.rio.nodata))
          randomPoints_stats.columns=['{0}'.format(file.split('.')[0])]
          randomPoints = randomPoints.join(randomPoints_stats)
```

### 10/12/2022

`intake-stac`を使ってのSentinel-2データが取得できない問題を修正。変更をかけたファイルは以下の通り。

- 01_ch3-1DataAccess.ipynb
- 05_ch4-2Forest.ipynb
- 06_ch4-3Road.ipynb
- 08_ch4-5Coast.ipynb
- 11_ch6_classification.ipynb

`intake-stack`を用いたデータの取得方法を`pystic-client`へ変更。

### 注意事項
収録物、プログラムの内容および使用方法などに関して、電話でのお問い合わせを含むサポート業務は一切お受けしておりませんのであらかじめご了承の上ご利用ください。

本書籍の著作権は著者に帰属します。
本書籍のコードを引用する場合には必要に応じて適切に引用するようにしてください。
なお、本書で公開しているプログラムについては一切の許諾なしに自由に使用してかまいません。ただし、改変等する際にはご自身の責任の下で実行してください。
商用利用する場合には適宜利用するデータのデータポリシー等をご確認ください。

