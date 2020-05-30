# Simple Geocoder

オープンソースのジオコーディング API です。

経産省の [IMI コンポーネントツール](https://info.gbiz.go.jp/tools/imi_tools/)のジオコーディングの仕組みからインスピレーションをうけて開発しました。

## 特徴

* 国土交通省の位置参照情報の「大字・町丁目レベル位置参照情報」を利用したジオコーディングサービスです。
  * JavaScript API と緯度経度の API を提供しています。
* API は GitHub ページで静的ファイルとしてホストしています。
* ソースコードのライセンスは MIT ライセンスです。取得した位置情報のご利用はご自由にどうぞ。

経産省の [IMI コンポーネントツール](https://info.gbiz.go.jp/tools/imi_tools/) でも同じレベルの精度のジオコーディングが可能ですが、IMI コンポーネントツールは、自力で Node.js サーバーをホストする必要があります。

本プロジェクトは、GitHub ページ上に静的ファイルとしてジオコーディング用の API をホストしているため、独自のサーバーを用意しなくても利用することができます。

## 仕組み

「東京都千代田区霞が関1-3-1」のような住所を受け取ると、この住所を分解し、IMI コンポーネントツールからフォークしたライブラリで「大字町丁目コード」を取得します。

このコードは、「032030003000」のような12桁の数字になっており、API 上にある同名の JSON ファイルにアクセスすることで、その中に記述されている緯度経度を取得しています。

API 内には、膨大な量のファイルが存在するため、`都道府県コード/市町村コード/大字町丁目コード.json` のようなディレクトリ構造とすることで、ファイルシステムの制限を回避しています。

## 使い方

以下の JavaScript API をウェブページから読み込んでください。

```
<script src="https://geolonia.github.io/simple-geocoder/api.js"></script>
```

API 関数 `getLatLng()` を任意のクリックイベント等でコールしてください。

```
document.getElementById('exec').addEventListener('click', () => {
  if (document.getElementById('address').value) {
    getLatLng(document.getElementById('address').value, (latlng) => {
      map.setCenter(latlng)
    })
  }
})
```

### `getLatLng(address, callback)`

* `address` - 緯度経度を取得したい住所の文字列
* `callback` - 緯度経度を取得したあとで実行したいコールバック関数。コールバック関数の第一引き数には緯度経度のオブジェクトが渡されます。

コールバック関数に引き数として渡される緯度経度のオブジェクトは以下のようになっています。

```
{
  lat: 35.1234.
  lng: 135.1234
}
```