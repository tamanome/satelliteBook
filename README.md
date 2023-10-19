# Pythonで学ぶ衛星データ解析基礎

<a href="https://gihyo.jp/book/2022/978-4-297-13232-3"><img src="https://gihyo.jp/assets/images/cover/2022/thumb/TH320_9784297132323.jpg" title="book" width="150" border="0" /></a>

## 本書籍の概要

本書籍は，Pythonによる衛星データ解析に興味がある初学者に向けた入門書となっています。学校の情報の授業等で利用する際の副教材になることを意識し，衛星データだけでなくデータサイエンスの基礎的な内容も含めました。学校で地球環境やご自身が住んでいる地域がどのように変化しているか調べたい方はもちろんのこと，衛星データを使って何かビジネスを始めたい方にも読んでいただきたいと思っています。従来のデータサイエンスの教材の場合には身近なデータを利用することが難しかった中で，衛星データであれば身近な地域のデータを利用して解析することができます。少しのプログラミング変更で解析対象地域を変えることができるようになっているので，関心のある地域の変化についてぜひ調べてみてください。

本書籍のコンテンツの一部は[宙畑](https://sorabatake.jp/)や[初心者向けTellus学習コース](https://tellusxdp.github.io/start-python-with-tellus/index.html)をベースに教科書の副教材として利用しやすくなることを意識して改変・追記しています。
書籍として編成する都合上、情報としてすべてを盛り込むことはできておらず、衛星データの概要から知りたい方は以下の記事などを合わせてご参照ください。
・[衛星データのキホン\~分かること、種類、頻度、解像度、活用事例\~](https://sorabatake.jp/279/)
・[光の波長って何？ なぜ人工衛星は人間の目に見えないものが見えるのか](https://sorabatake.jp/364/)
・[課題に応じて変幻自在？ 衛星データをブレンドして見えるモノ・コト #マンガでわかる衛星データ](https://sorabatake.jp/5192/)


## 本書籍のコードを実行する

コードを実行する上での最初のハードルは解析環境を構築することかと思います。
解析が初めての方には、解析環境の構築が不要なGoogle Colabで解析を実行することをお勧めしています。
解析が慣れたらご自身のパソコンへの環境構築や、Tellus等での仮想マシン構築を行うことでより高度な解析が可能になります。

Google Colabで解析するためにはGoogleのアカウントが必要になります。
Google Colabの利用方法については「Google Colab　利用方法」等で検索してください。

Colabは特別な環境をインストールすることなしに分析を進めることができますので、以下のブックマークレットから対象のノートブックを実行してください。

<a href="https://colab.research.google.com/github/tamanome/satelliteBook/blob/main/" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a> 

## 本書籍で用いるデータについて

利用データについての概要は、[こちらのスプレッドシート](https://docs.google.com/spreadsheets/d/1WKeu6_c-MV-zW2TMJaJ93d4f1Mx5yf7Uh9GGg0N2q0k/edit?usp=sharing)を参考にしてください。

API等を利用してデータをダウンロードする方法は紹介していますが、全てのデータをコードを通してダウンロードしているわけではありません。そのため、**一部のデータはデータは前もってダウンロードをしておく必要があります**。Google Drive、または技術評論社のダウンロードページ（書籍参考）からデータをダウンロードできますので、ご利用ください。

[Google Driveからのダウンロードはこちらから](https://drive.google.com/drive/folders/19DTm31Q8G_IIO5WqP6WUi35xfsw1guO7?usp=sharing)。

※本書籍で利用するデータは無料でご利用頂けるもので構成されていますが，一部会員登録（無料）が必要となります。

## 修正箇所

[こちらのスプレッドシート](https://docs.google.com/spreadsheets/d/1dNwlP8ZvFief8ZRS22i-b_T17BwQmhEMHpFv81cUmLc/edit#gid=0)が対応表となります。

## 更新履歴

### 19/10/2023

**01_ch3-1DataAccess.ipynbの修正：**

`goal.Translate`パラメータ修正。

```python
gdal.Translate(imgJpg,
               imgTif,
               format='JPEG',
               scaleParams=[[0, 5000, 0, 255]],
               outputType=gdal.GDT_Byte, 
               bandList=[3,2,1])
# scaleParamsの部分は空白[[]]でも動作します。その場合は、gdal.Translateが自動で最適なコントラストに設定しますが、多くの場合手動でやるほうが良い結果になります
im = Image.open('Masked_' +str(object_name) +'.jpg')
im = Image.open('/content/Masked_Tokyo_Bay.jpg')
im
```

STAC itemのプロパティ名が変更されたので修正した。また波長の名称も変更されていたため、それについても修正を行なった。

```python
# 最も雲の量が少ないシーンを選択し、サムネイル画像も取得する関数を定義します。
def sel_items(scene_items, product_id):
 item = [x.assets for x in scene_items\
         if x.properties['s2:granule_id'] == product_id]
 thumbUrl = [x.assets['thumbnail'].href for x in scene_items\
             if x.properties['s2:granule_id'] == product_id]
 return item, thumbUrl

selected_item, thumbUrl = sel_items(items, dfSorted['s2:granule_id'][0])
print(thumbUrl)
print(selected_item)
```

また、ノートブック内の説明についても若干更新を行なった。

### 20/06/2023

**01_ch3-1DataAccess.ipynbの修正：**

クライアントオブジェクトのconforms_toを更新した。

```python
from pystac_client import Client
api_url = 'https://earth-search.aws.element84.com/v0'
collection = "sentinel-s2-l2a-cogs"  # Sentinel-2, Level 2A (BOA)
s2STAC = Client.open(api_url, headers=[])
```

上記セルに対して`s2STAC.add_conforms_to("ITEM_SEARCH")`を追加。

```python
from pystac_client import Client
api_url = 'https://earth-search.aws.element84.com/v0'
collection = "sentinel-s2-l2a-cogs"  # Sentinel-2, Level 2A (BOA)
s2STAC = Client.open(api_url, headers=[])
s2STAC.add_conforms_to("ITEM_SEARCH")
```

`pystac_client`を利用している全てのノートブックも併せて修正した。

### 02/04/2023

**09_ch5-1linear.ipynbの修正：**

`intake`を利用していた箇所を修正。正しくデータのダウンロードが行えるようにした。

### 18/02/2023

**03_ch3-3GDAL.ipynbの修正：**

```python
import gdal
```

上記インポート方法を下記に修正。

```python
from osgeo import gdal
```

### 11/02/2023

**02_ch3-2Coordinate.ipynbの修正**：

- geopandasのインストールを修正。

### 06/01/2023

**07_ch4-4Agri.ipynbの修正**：

- EVIが意図せぬ計算結果になっていたため`modisEviTiff`のクラスを修正。

- 筆ポリゴンのCRSが変更されており読み込みができないエラーを修正。

```python
from fiona.crs import from_epsg
gdf = gpd.GeoDataFrame(pd.concat([gpd.read_file(path,encoding='Shift_JIS') for path in filepaths],\
                                 ignore_index=True))
gdf = gdf.to_crs(crs="epsg:32654") # CRSの変更
```

以上を下に変更。

```python
shpList = []
for file in filepaths:
  gdf = gpd.read_file(file,encoding='shift_jis')
  gdf = gdf.to_crs(crs="epsg:32654") # CRSの変更
  shpList.append(gdf)
  
gdf = gpd.GeoDataFrame(pd.concat(shpList,ignore_index=True))
```

### 16/12/2022

**12_appendix_hcluster.ipynbの修正**：

GEE APIがアップデートされたため、GEEのPythonパッケージも同時にアップデートしないといけないため以下のコードを挿入。

```python
!pip install earthengine-api --upgrade
```

GDAL関連：分類精度評価のところでosgeoのサブモジュールが読み込まれていなかったため、下記を追加しました。

```python
 from osgeo import gdal, gdalconst, gdal_array
```

### 11/12/2022

**11_ch6_classification.ipynb**のs2folderに割り当てるパスの変更。

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

`intake-stack`を用いたデータの取得方法を`pystac-client`へ変更。

## 注意事項

収録物、プログラムの内容および使用方法などに関して、電話でのお問い合わせを含むサポート業務は一切お受けしておりませんのであらかじめご了承の上ご利用ください。

本書籍の著作権は著者に帰属します。
本書籍のコードを引用する場合には必要に応じて適切に引用するようにしてください。
なお、本書で公開しているプログラムについては一切の許諾なしに自由に使用してかまいません。ただし、改変等する際にはご自身の責任の下で実行してください。
商用利用する場合には適宜利用するデータのデータポリシー等をご確認ください。

