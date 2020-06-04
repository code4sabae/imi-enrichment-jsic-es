# 産業分類候補生成パッケージ一式 imi-enrichment-jsic ES

与えられた自由文 (string) に対して、所与のコードリストから適切なコードを推薦するESモジュールです。
コードには `説明` が付与されているものとします。
この実装ではコードリストとして日本標準産業分類を整備・使用します。

[![esmodules](https://taisukef.github.com/denolib/esmodulesbadge.svg)](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Modules)
[![deno](https://taisukef.github.com/denolib/denobadge.svg)](https://deno.land/)

# 利用者向け情報

## API

モジュール `imi-enrichment-jsic` は以下のような API の関数を提供します。

```
import IMIEnrichmentJSIC from "https://code4sabae.github.io/imi-enrichment-jsic-es/IMIEnrichmentJSIC.mjs";
console.log(IMIEnrichmentJSIC("インターネットサービスプロバイダ"));
```

- 入力 (input) : 入力テキストとする String
- 出力 : 推薦結果の JSON ※ 推薦は同期で行うため Promise でないことに注意

以下のように HTML で読み込み、使用することができます。

```
<script type="module">
import IMIEnrichmentJSIC from "https://code4sabae.github.io/imi-enrichment-jsic-es/IMIEnrichmentJSIC.mjs";
console.log(IMIEnrichmentJSIC("インターネットサービスプロバイダ"));
</script>
```

**codelist.json**

```codelist.json
{
  "@context": "https://imi.go.jp/ns/core/context.jsonld",
  "@graph": [
    {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-A",
      "@type": "コード型",
      "識別値": "A",
      "表記": "農業，林業",
      "説明": "..."
    },
    {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-A01",
      "@type": "コード型",
      "識別値": "A01",
      "表記": "農業",
      "説明": "...",
      "上位コード" : "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-A"
    },
    {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-A010",
      "@type": "コード型",
      "識別値": "A010",
      "表記": "管理，補助的経済活動を行う事業所（01農業）",
      "説明": "...",
      "上位コード" : "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-A01"
    },
    ...
  ]
}
```

注) コードリストは [e-Stat 日本標準産業分類](https://www.e-stat.go.jp/classifications/terms/10) でダウンロードできる CSV データを加工して作成します。コードの `@id` には統計 LOD で整備されている URI を付与しています。

**input.txt**

```input.txt
インターネットとは、インターネット・プロトコル・スイートを使用し、
複数のコンピュータネットワークを相互接続した、
グローバルなネットワーク（地球規模の情報通信網）のことである。
```

注) この例文は [Wikipedia:インターネット](https://ja.wikipedia.org/wiki/%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%8D%E3%83%83%E3%83%88) の概要文をコピーしたものです。

**output.json**

```output.json
[
  {
    "value": {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-G",
      "@type": "コード型",
      "コード種別": "http://data.e-stat.go.jp/lod/data/ontology/crossDomain/code/IndustryClassification2013ConceptScheme",
      "識別値": "G",
      "表記": "情報通信業",
      "説明": "..."
    },
    "score": 16
  },
  {
    "value": {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-E",
      "@type": "コード型",
      "コード種別": "http://data.e-stat.go.jp/lod/data/ontology/crossDomain/code/IndustryClassification2013ConceptScheme",
      "識別値": "E",
      "表記": "製造業",
      "説明": "..."
    },
    "score": 14
  },
  {
    "value": {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-A",
      "@type": "コード型",
      "コード種別": "http://data.e-stat.go.jp/lod/data/ontology/crossDomain/code/IndustryClassification2013ConceptScheme",
      "識別値": "A",
      "表記": "農業，林業",
      "説明": "..."
    },
    "score": 13
  },
  {
    "value": {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-G4011",
      "@type": "コード型",
      "コード種別": "http://data.e-stat.go.jp/lod/data/ontology/crossDomain/code/IndustryClassification2013ConceptScheme",
      "識別値": "G4011",
      "表記": "ポータルサイト・サーバ運営業",
      "説明": "...",
      "上位コード": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-G401"
    },
    "score": 11
  },
  {
    "value": {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-G40",
      "@type": "コード型",
      "コード種別": "http://data.e-stat.go.jp/lod/data/ontology/crossDomain/code/IndustryClassification2013ConceptScheme",
      "識別値": "G40",
      "表記": "インターネット附随サービス業",
      "説明": "...",
      "上位コード": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-G"
    },
    "score": 10
  },
  {
    "value": {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-G4012",
      "@type": "コード型",
      "コード種別": "http://data.e-stat.go.jp/lod/data/ontology/crossDomain/code/IndustryClassification2013ConceptScheme",
      "識別値": "G4012",
      "表記": "アプリケーション・サービス・コンテンツ・プロバイダ",
      "説明": "...",
      "上位コード": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-G401"
    },
    "score": 10
  },
  {
    "value": {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-E2842",
      "@type": "コード型",
      "コード種別": "http://data.e-stat.go.jp/lod/data/ontology/crossDomain/code/IndustryClassification2013ConceptScheme",
      "識別値": "E2842",
      "表記": "電子回路実装基板製造業",
      "説明": "...",
      "上位コード": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-E284"
    },
    "score": 10
  },
  {
    "value": {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-L7311",
      "@type": "コード型",
      "コード種別": "http://data.e-stat.go.jp/lod/data/ontology/crossDomain/code/IndustryClassification2013ConceptScheme",
      "識別値": "L7311",
      "表記": "広告業",
      "説明": "...",
      "上位コード": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-L731"
    },
    "score": 10
  },
  {
    "value": {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-B",
      "@type": "コード型",
      "コード種別": "http://data.e-stat.go.jp/lod/data/ontology/crossDomain/code/IndustryClassification2013ConceptScheme",
      "識別値": "B",
      "表記": "漁業",
      "説明": "...",
    },
    "score": 9
  },
  {
    "value": {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-E3031",
      "@type": "コード型",
      "コード種別": "http://data.e-stat.go.jp/lod/data/ontology/crossDomain/code/IndustryClassification2013ConceptScheme",
      "識別値": "E3031",
      "表記": "電子計算機製造業（パーソナルコンピュータを除く）",
      "説明": "...",
      "上位コード": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-E303"
    },
    "score": 9
  }
]
```

- 与えられた文字列に対して output.json を生成します
- output.json のルートは配列、配下に `value` と `score` を持つ JSON Object を最大10件保持します
- `value` には codelist.json に含まれる コード型のコピーが保持されます
- `score` には評価値が保持されます (大きいほど評価が高い)
- JSON Object は `score` の値が大きい順にソートされているものとします
- 与えられた文字列に対して推薦できるコードが存在しない場合には空の配列が返ります

# 開発者向け情報

## 環境構築

以下の手順で環境を構築します。

```
$ git clone https://github.com/code4sabae/imi-enrichment-jsic-es.git
```

## コマンドラインインターフェイス

コマンドラインインターフェイスのファイルの実体は `bin/cli.mjs` です。

```
$ cd bin

# ヘルプの表示
$ deno run cli.mjs -h

# テキストファイルからの推薦
$ deno run --allow-read cli.mjs input.txt > output.json

# 標準入力のテキストからの推薦
$ cat input.json | deno run cli.mjs > output.json

# 文字列からの推薦
$ deno run cli.mjs -s インターネットサービスプロバイダ > output.json

```

## Web API

Web API を提供するサーバプログラムが同梱されています。

### サーバの起動方法

`bin/server.mjs` がサーバの実体です。
以下のように `bin/server.mjs` を実行することで起動できます。

```
$ cd bin
$ deno run -A server.mjs
Usage: deno run -A server.mjs [port number]

$ deno run -A server.mjs 8080
imi-enrichment-jsic-server is running on port 8080
```

なお、実行時にはポート番号の指定が必要です。指定しなかった場合にはエラーが表示されて終了します。
サーバを停止するには `Ctrl-C` を入力してください。

### 利用方法

WebAPI は POST されたテキストを入力として JSON を返します。

```
$ curl -X POST -H 'Content-Type: text/plain' -d 'インターネットサービスプロバイダ' localhost:8080
[
  {
    "value": {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-G4012",
      "@type": "コード型",
      "コード種別": "http://data.e-stat.go.jp/lod/data/ontology/crossDomain/code/IndustryClassification2013ConceptScheme",
      "識別値": "G4012",
      "表記": "アプリケーション・サービス・コンテンツ・プロバイダ",
      "説明": "...",
      "上位コード": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-G401"
    },
    "score": 13
  },
  {
    "value": {
      "@id": "http://data.e-stat.go.jp/lod/ontology/crossDomain/code/industryClassification2013-G",
      "@type": "コード型",
      "コード種別": "http://data.e-stat.go.jp/lod/data/ontology/crossDomain/code/IndustryClassification2013ConceptScheme",
      "識別値": "G",
      "表記": "情報通信業",
      "説明": "..."
    },
    "score": 10
  },
  ...
]
```

- WebAPI の URL に GET メソッドでアクセスした場合には HTML ページが表示され、WebAPI の動作を確認することができます
- POST,GET 以外のメソッドでアクセスした場合には `405 Method Not Allowed` エラーが返されます
- POST Body を入力テキストとして扱い、推薦結果のコード群からなる JSON を返します

## データ生成

コードリストは [e-Stat 日本標準産業分類](https://www.e-stat.go.jp/classifications/terms/10) でダウンロードできる CSV データ(data/FEK_download.utf8.csv)を、tools/csv2json.mjsで加工して作成します。コードの `@id` には統計 LOD で整備されている URI を付与しています。

```
$ cd tools
$ deno run --allow-read tools/csv2json.mjs data/FEK_download.utf8.csv > lib/jsic.mjs
```

## テスト

以下の手順でテストを実行します

```
$ deno test --allow-read
```

## 依存関係

なし

## 出典

本ライブラリは IMI 情報共有基盤 コンポーネントツール <https://info.gbiz.go.jp/tools/imi_tools/> の「産業分類候補生成パッケージ一式」をESモジュール対応したものです。

## 関連記事

Deno対応ESモジュール対応、IMIコンポーネントツールx4とDenoバッジ  
https://fukuno.jig.jp/2866  

日本政府発のJavaScriptライブラリを勝手にweb標準化するプロジェクト、全角-半角統一コンポーネントのESモジュール/Deno対応版公開  
https://fukuno.jig.jp/2865  
