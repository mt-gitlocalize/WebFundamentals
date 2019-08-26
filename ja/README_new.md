# DevSiteのWebの基礎

[![Build Status](https://travis-ci.org/google/WebFundamentals.svg?branch=master)](https://travis-ci.org/google/WebFundamentals)

新しいWebの**基礎**へようこそ！最新のWeb開発のベストプラクティスとツールを紹介する取り組み。

### 何が変わったの？

- 現在、 [DevSite](https://developers.google.com/)インフラストラクチャを使用して[い](https://developers.google.com/)ます
    - 新しい[スタイルガイド](https://petele-scratch.appspot.com/web/resources/style-guide) 
    - 新しい[ウィジェットは](https://petele-scratch.appspot.com/web/resources/widgets) 、インラインJavaScript、共通リンク、関連ガイドなどを許可します
- ジキルは排除されました。代わりに、ページはリクエスト時にレンダリングされます
- 前件はマークダウンから削除されましたが、ファイルに[はタグの簡単なセットが](https://petele-scratch.appspot.com/web/resources/writing-an-article#yaml-front-matter)必要になりました

### 何が変わりませんか？

- GitHubは今でもコンテンツの真実の源であり、
- 私たちはあなたの貢献、PR、問題、その他何でも欲しいです！
- 最新版はhttps://web-central.appspot.com/web/で公開されています

=======
- また、このリストには別の行があります。


## リポジトリの複製

高帯域幅接続を使用している場合は、リポジトリの新しいクローンから始めることをお勧めします。

```
git clone https://github.com/google/WebFundamentals.git
```

## 設定する

新しいDevSiteインフラストラクチャは、依存関係を大幅に簡素化します。 [Node](https://nodejs.org/en/) 10以降があり、 [AppEngine SDK for Pythonが](https://cloud.google.com/appengine/downloads#Google_App_Engine_SDK_for_Python)既にインストールされていることを確認してください。

1. `npm install`実行します（ビルドプロセスに必要）

## 自動生成されたファイルをビルドする

一部のファイル（寄稿者が含まれ、更新用のページ、ショーケースなど）が自動的に生成されます。初めてリポジトリを複製して`npm install`を実行`npm install` 、これは自動的に行われます。ただし、ケーススタディや更新などを追加する場合は、次を使用してこれらのファイルを再構築する必要があります。

```
npm run build
```

## ローカルサーバーを起動

サイトをローカルで表示するには、次を実行します。

```
npm start
```

**注：**サーバーを最初に起動したときに、あなたが実行する必要があり`start-appengine.sh`とにより提供されるすべてのプロンプトに答える`dev_appserver.py` 。

## コードラボを更新する

Code Labsを更新するには、 [`claat`](https://github.com/googlecodelabs/tools/tree/master/claat)ツールと元のDocファイルへのアクセスが必要です。これはおそらくGoogle社員のみに有効です。

1. `claat`ツールをダウンロードして、 `tools`ディレクトリに配置します。
2. `tools/update-codelabs.sh`実行し`tools/update-codelabs.sh`
3. GitHubの最新の変更を確認する

## 開発サーバーを起動します

1. `npm start`実行します

## PRを送信する前に変更をテストする

PRを送信する前に、npmテストで変更を実行してください。テストでは、DevSiteで問題を引き起こす可能性のあるものを探し、コンテンツの一貫性を維持しようとします。これは展開プロセスの一部であるため、エラーがあるとPRは失敗します！走る：

```
npm test
```
