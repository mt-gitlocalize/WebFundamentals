project_path: "/web/_project.yaml"
book_path: "/web/resources/_book.yaml"
description: Web Fundamentalsを他の言語に翻訳したい場合は、誰でも貢献できます。

{# wf_updated_on: 2019-03-04 #} {# wf_published_on: 2016-09-13 #} {# wf_blink_components: N/A #}

# 翻訳{: .page-title }

警告：GitLocalizeは現在使用していません。これらの手順は古く、まもなく更新されます。

[GitLocalize](https://gitlocalize.com/)と呼ばれる翻訳ツールを[試してい](https://gitlocalize.com/)ます。次の手順に従って、貢献を開始してください。

1. [GitLocalizeのWeb Fundamentalsリポジトリに](https://gitlocalize.com/repo/107)移動し[ます](https://gitlocalize.com/repo/107) 。
2. GitHubアカウントを使用してサインアップします。
3. 翻訳するドキュメントを見つけます。
4. 翻訳を開始します。
5. 完了したら、レビュー用の翻訳を送信します。
6. レビューされた翻訳は、コミュニティの言語モデレーターによってプルリクエストとして送信されます。

GitLocalizeの仕組みの詳細については[、ヘルプページをご覧ください](https://docs.gitlocalize.com/) 。

問題や機能のリクエストを見つけた場合は、 [GitLocalizeの問題トラッカーに](https://github.com/gitlocalize/feedback/issues)それらを提出してください。

他の翻訳貢献者とチャットする[には、ChromiumDev Slackに登録して](https://join.slack.com/t/chromiumdev/shared_invite/enQtMzM3NjYwNjI0MDM4LTk2NjEyYTIxODk1MDYxMmNjNWYzMGMxZGVhMDNhY2I1ZjBhMjdlYTg0MTg4ZGE0OTQ0ZmYwNTRiMGJlYzVjOTE) ＃l10nチャンネルに参加してください。

注：翻訳をレビューする人を見つけることが重要です。 [言語モデレーターとしてまだ誰も割り当てられていない](https://gitlocalize.com/repo/107/roles)場合は、Slackの＃l10nチャネルでレビュアーを見つけてみてください。モデレーターになることに興味がある場合は、＃l10nで@agektmrにpingを送信します。

サポートされている言語：アラビア語（AR）、ドイツ語（DE）、スペイン語（ES）、フランス語（FR）、ヘブライ語（HE）、インドネシア語（ID）、イタリア語（IT）、日本語（JA）、韓国語（KO） 、オランダ語（NL）、ポーランド語（PL）、ポルトガル語（PT-BR）、ロシア語（RU）、トルコ語（TR）、繁体字中国語（ZH-TW）、簡体字中国語（ZH-CN）。

## クレジット

翻訳する記事のクレジットを確実に取得する必要があります。

`src/data/_contributors.yaml`詳細を追加し、 `role`属性に`- translator` `src/data/_contributors.yaml`を追加します。この情報を使用して、 [寄稿者のページにデータ](/web/resources/contributors)を入力したり、各記事に名前を付けたりします。例えば：

```
paulkinlan:
  name:
    given: Paul
    family: Kinlan
  org:
    name: Google
      unit: Developer Relations
  country: UK
    role:
    - author
    - engineer
    - translator
  homepage: http://paul.kinlan.me
  twitter: paul_kinlan
  email: paulkinlan@google.com
  description:
    en: "Paul is a Developer Advocate"
```

また、記事の最後にクレジットを追加してください。 GitLocalizeでは新しいセクションを追加できないため、次のスクリーンショットのように、既存の最後のセクションの複数の空行の後にクレジットを追加します。

## ライセンス

すべてのコンテンツはクリエイティブコモンズ3.0です。貢献と翻訳は大歓迎ですが、コードをリポジトリに引き戻すには、 [寄稿者ライセンス契約に](https://github.com/google/WebFundamentals/blob/master/CONTRIBUTING.md)署名する必要があります。
