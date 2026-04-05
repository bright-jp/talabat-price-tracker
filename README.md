# Talabat 価格トラッカー

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://brightdata.jp)
[![Talabat Price Tracker](https://img.shields.io/badge/Talabat%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://brightdata.jp/products/insights/price-tracker/talabat)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://brightdata.jp/products/insights/price-tracker/talabat)

中東を代表するフードデリバリープラットフォーム Talabat のリアルタイム価格追跡。開始方法は 2 つあります。**フルマネージド**のインテリジェンスプラットフォームを使う方法と、Bright Data の AI Scraper Builder で構築する**カスタム scraper**を使う方法です。

---

## Option 1: Bright Insights - AI 搭載の価格トラッキング（推奨）

**[Bright Insights](https://brightdata.jp/products/insights/price-tracker/talabat)** は、Bright Data のフルマネージドなリテールインテリジェンスプラットフォームです。scraper を構築する必要も、インフラを保守する必要もありません。分析可能な構造化価格データが、ダッシュボード、data feed、または BI ツールにそのまま配信されます。

**チームが Bright Insights を選ぶ理由:**
- 🚀 **セットアップ不要** - すぐに使えるダッシュボードと data feed で数分で本番稼働
- 🤖 **AI によるレコメンデーション** - 会話型 AI アシスタントが数百万件のデータポイントを即座に実用的なインサイトへ変換
- ⚡ **リアルタイム監視** - 1 時間ごとから日次までの更新頻度と即時アラート（email、Slack、webhook）
- 🌍 **無制限のスケール** - あらゆる website、あらゆる地域、あらゆる更新頻度に対応
- 🔗 **プラグアンドプレイ統合** - AWS、GCP、Databricks、Snowflake など
- 🛡️ **フルマネージド** - Bright Data が schema 変更、site 更新、データ品質を自動で処理

**主なユースケース:**
- ✅ Talabat の**メニュー価格変更を追跡**
- ✅ **配送料とプロモーションを**リアルタイムで監視
- ✅ **レストラン間およびプラットフォーム間で価格を比較**
- ✅ MAP ポリシー準拠を監視し、価格違反を検出
- ✅ 競合のプロモーションと販促動向を追跡
- ✅ クリーンで正規化されたデータを動的価格設定アルゴリズムや AI モデルへ直接投入

> **月額 $250 から - [最適な見積もりを取得 →](https://brightdata.jp/products/insights/price-tracker/talabat)**

---

## Option 2: 独自の Talabat scraper を構築

Talabat 向けの事前構築済み scraper API がない？問題ありません。Bright Data の **AI Scraper Builder** なら、数クリックでカスタム Talabat scraper を生成できます。コーディングは不要です。

### 数分で Talabat scraper を構築

**[Talabat AI Scraper Builder を開く →](https://brightdata.jp/products/web-scraper/talabat)**

ドメインを選択し、必要なデータ要件を記述するだけで、AI scraper builder が自動的に API を作成します。

1. **必要なデータを平易な英語で記述**
2. **AI が scraper API を即座に生成**
3. **API リクエストを実行してすぐに結果を取得**
4. **必要に応じて組み込み IDE でコードを編集**

構築が完了すると、scraper には **Web Scraper ID**（`gd_xxxxxxxxxxxx`）が付与されます。以下の Setup 手順で使うためにコピーしてください。

### 前提条件

- Python 3.9 以上
- [Bright Data account](https://brightdata.jp)（無料トライアルあり）
- Bright Data の **API token**（[取得方法](https://docs.brightdata.jp/general/account/account-settings#api-token)）
- Talabat 用の **Web Scraper ID**（上記の構築手順で取得）

### Setup

1. **この repository を clone**

   ```bash
   git clone https://github.com/bright-jp/talabat-price-tracker.git
   cd talabat-price-tracker
   ```

2. **依存関係をインストール**

   ```bash
   pip install -r requirements.txt
   ```

3. **認証情報を設定**

   `.env.example` を `.env` にコピーし、値を入力します:

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **あなたの Web Scraper ID**
   > [AI Scraper Builder dashboard](https://brightdata.jp/products/web-scraper/talabat) から Web Scraper ID をコピーし、
   > `BRIGHTDATA_DATASET_ID` に貼り付けてください（形式: `gd_xxxxxxxxxxxx`）。

---

## 使い方

Talabat scraper の構築が完了し、Web Scraper ID を `.env` に設定すると、Python インターフェースは同じ方法で利用できます。

### 1. URL で特定の商品を追跡

Talabat の商品 URL のリストを渡して、構造化された価格データを取得します:

```python
from price_tracker import track_prices

urls = [
    "https://www.talabat.com/product/sample-item-123456",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

または直接実行:

```bash
python price_tracker.py
```

### 2. キーワードで商品を検索

キーワード検索に一致する商品を見つけます:

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. カテゴリ URL で商品を閲覧

Talabat のカテゴリページからすべての商品を収集します:

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://talabat.com/category/example",
    limit=100,
)
```

---

## 出力フィールド

各結果レコードには次のフィールドが含まれます:

| Field | Description |
|-------|-------------|
| `url` | レストラン / メニュー項目 URL |
| `name` | 商品名 |
| `restaurant_name` | レストラン名 |
| `price` | 商品価格 |
| `currency` | 通貨コード |
| `category` | フードカテゴリ |
| `rating` | 評価 |
| `delivery_fee` | 配送料 |
| `estimated_delivery_time` | 配達予定時間 |
| `in_stock` | 現在利用可能かどうか |
| `images` | 商品画像 URL |
| `description` | 商品説明 |
| `timestamp` | 収集タイムスタンプ |

### サンプル出力

```json
[
  {
    "url": "https://www.talabat.com/product/sample-item-123456",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://talabat.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## 高度なオプション

`trigger_collection()` 関数は、データ収集を制御するためのオプションパラメータを受け付けます:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | 返すレコードの最大数 |
| `include_errors` | boolean | `true` | 結果に error report を含める |
| `notify` | string (URL) | - | snapshot の準備完了時に呼び出す webhook URL |
| `format` | string | `json` | 出力形式: `json`、`csv`、または `ndjson` |

オプション付きの例:

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.talabat.com/product/sample-item-123456"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## リソース

- 🌟 [Talabat Price Tracker - Bright Insights (Managed)](https://brightdata.jp/products/insights/price-tracker/talabat)
- 🏗️ [Build a Talabat Scraper](https://brightdata.jp/products/web-scraper/talabat)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.brightdata.jp/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://brightdata.jp/cp/scrapers)
- 🔑 [How to get an API token](https://docs.brightdata.jp/general/account/account-settings#api-token)
- 🌐 [Bright Data Homepage](https://brightdata.jp)

---

*[Bright Data](https://brightdata.jp) により構築 - 業界をリードする web データプラットフォーム。*