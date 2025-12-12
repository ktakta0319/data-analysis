# Google Trends 都道府県別トレンド可視化

`google_trends_by_prefecture.ipynb` は、Google Trends から 47 都道府県それぞれの検索トレンド（例: 「旅行」）を取得し、正規化した上でコロプレスマップとして可視化するノートブックです。

## できること
- 指定した検索語を 2024 年の期間で各都道府県について取得（`pytrends` 利用）。
- 先頭の基準セルを用いて系列を正規化し、都道府県平均を算出して Min-Max スケーリング。
- dataofjapan/land の GeoJSON を使い、`folium` で日本地図のコロプレスマップを生成。
- 生成したマップを `japan_trend_map.html` として保存。

## 必要環境
- Python 3.12 以降を想定
- 主要ライブラリ: `pandas`, `pytrends`, `requests`, `folium`, `numpy`
- インターネット接続（Google Trends API と GeoJSON 取得のため）

## 使い方（概要）
1) ノートブックを開き、上から順に実行。  
2) `prefectures` と `search_word` を必要に応じて編集。検索語は最大 5 件ずつに分割して送る実装です。  
3) 実行後、`m` に可視化結果が表示され、`japan_trend_map.html` が出力されます。  

## 実装メモ
- `build_payload` の `cat=67` は「旅行」カテゴリ。期間は `timeframe="2024-01-01 2024-12-31"`, 地域は `geo="JP"`。  
- 負荷対策として各リクエスト間に `time.sleep(2)` を挿入。  
- GeoJSON 側の表記に合わせるため、都道府県名は `北海道` を除き末尾に「県/府/都」を付与しています。  

## 出力物
- `trends_df`: 都道府県別トレンド時系列データ（正規化済み）
- `trends_mean`: 都道府県平均の正規化データ
- `japan_trend_map.html`: Folium で描画したコロプレスマップ

