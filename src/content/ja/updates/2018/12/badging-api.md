project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"
description: Badging APIは、インストールされたWebアプリがアプリケーション全体のバッジを設定できる新しいWebプラットフォームAPIで、シェルフやホーム画面など、アプリケーションに関連付けられたオペレーティングシステム固有の場所に表示されます。

{# wf_published_on: 2018-12-11 #} {# wf_updated_on: 2019-08-21 #} {# wf_featured_image: /web/updates/images/generic/notifications.png #} {# wf_tags: capabilities,badging,install,progressive-web-apps,serviceworker,notifications,origintrials #} {# wf_featured_snippet: The Badging API is a new web platform API that allows installed web apps to set an application-wide badge, shown in an operating-system-specific place associated with the application, such as the shelf or home screen. Badging makes it easy to subtly notify the user that there is some new activity that might require their attention, or it can be used to indicate a small amount of information, such as an unread count. #} {# wf_blink_components: UI>Browser>WebAppInstalls #}

{# When updating this post, don't forget to update /updates/capabilities.md #}

# アプリアイコンのバッジ{: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

<div class="clearfix"></div>

<aside class="caution">私たちは現在、新しいAPI <a href="/web/updates/capabilities">プロジェクトの</a>一環としてこのAPIに取り組んでおり、Chrome 73からは、最初の<a href="#ot"><b>トライアル</b></a>として利用できます。この投稿は、Badging APIが進化するにつれて更新されます。 <br> <b>最終更新日：</b> 2019年8月21日</aside>

## Badging APIとは何ですか？ {: #what }

<figure class="attempt-right">
  <img src="/web/updates/images/2018/12/badges-on-windows.jpg">
  <figcaption>8つの通知とフラグ型バッジを表示する別のアプリを備えたTwitterの例。</figcaption>
</figure>

Badging APIは、インストールされたWebアプリがアプリケーション全体のバッジを設定できる新しいWebプラットフォームAPIで、アプリケーションに関連付けられたオペレーティングシステム固有の場所（シェルフやホーム画面など）に表示されます。

バッジを使用すると、注意を必要とする可能性のある新しいアクティビティがあることをユーザーに簡単に通知したり、未読カウントなどの少量の情報を示すために使用したりできます。

バッジは、通知よりもユーザーフレンドリーである傾向があり、ユーザーを中断しないため、はるかに高い頻度で更新できます。そして、それらはユーザーを中断しないので、それらを使用するのに必要な特別な許可はありません。

[説明者を読む](https://github.com/WICG/badging/blob/master/explainer.md) {: .button .button-primary }

<div class="clearfix"></div>

### Badging APIの推奨ユースケース{: #use-cases }

このAPIを使用できるサイトの例は次のとおりです。

- チャット、メール、ソーシャルアプリ。新しいメッセージが届いたことを通知するか、未読アイテムの数を表示します。
- 生産性アプリ。長時間実行されるバックグラウンドタスク（イメージやビデオのレンダリングなど）が完了したことを通知します。
- ゲーム。プレーヤーのアクションが必要であることを通知するため（たとえば、チェスの場合、プレーヤーの番です）。

## 現在のステータス{: #status }

ステップ | 状態
--- | ---
1.説明者を作成する | [コンプリート](https://github.com/WICG/badging/blob/master/explainer.md)
2.仕様の初期ドラフトを作成する | [コンプリート](https://wicg.github.io/badging/)
**3.フィードバックを収集し、設計について反復する** | [**進行中**](#feedback)
**4. Originトライアル** | [**進行中**](#ot)
5.起動 | 始まっていない

### 実際にご覧ください

1. WindowsまたはMacでChrome 73以降を使用して、 [Badging APIデモを](https://badging-api.glitch.me/)開き[ます](https://badging-api.glitch.me/) 。
2. プロンプトが表示されたら、[ **インストール** ] **を**クリックしてアプリをインストールするか、Chromeメニューを使用してインストールし、インストール済みのPWAとして開きます。注、インストールされたPWAとして（タスクバーまたはドックで）実行されている必要があります。
3. [ **設定]**または[ **クリア** ]ボタンをクリックして、アプリアイコンからバッジを設定またはクリアします。 *バッジの値に*番号を指定することもでき*ます* 。

注： *Chrome*のBadging API *に*は、実際にバッジを付けることができるアイコン付きのインストール済みアプリが必要ですが、インストール状態に応じてBadging APIを呼び出すことはお勧めしません。 Badging APIは、ブラウザがバッジを表示する可能性のある*あらゆる場所に*適用できるため、開発者は、ブラウザがどのような状況でバッジを機能させるかについて仮定するべきではありません。存在するときにAPIを呼び出すだけです。動作する場合、動作します。そうでない場合は、単にそうではありません。

## Badging APIの使用方法{: #use }

Chrome 73以降、Badging APIはWindows（7以降）およびmacOSのオリジントライアルとして利用可能です。 [Originのトライアルでは](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/README.md) 、新しい機能を試して、ユーザビリティ、実用性、有効性に関するフィードバックをGoogleおよびWeb標準コミュニティに提供できます。詳細については、「 [Web開発者向けOriginトライアルガイド」を](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md)参照してください。

### プラットフォーム間のバッジのサポート

Badging APIは、WindowsおよびmacOSでサポートされています（オリジントライアルで）。 Androidは通知を表示する必要があるためサポートされていませんが、今後変更される可能性があります。 Chrome OSのサポートは、プラットフォームでのバッジの実装が保留中です。

### 元のトライアルに登録する{: #ot }

1. オリジンの[トークン](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)を[リクエストします](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481) 。
2. ページにトークンを追加します。オリジンのページでこのトークンを提供する方法は2つあります。 
    -  `origin-trial` `<meta>`タグを任意のページのヘッドに追加します。たとえば、次のようになります。 `<meta http-equiv="origin-trial" content="TOKEN_GOES_HERE">` 
    - サーバーを構成できる場合は、 `Origin-Trial` HTTPヘッダーを使用してページにトークンを提供することもできます。結果の応答ヘッダーは次のようになります`Origin-Trial: TOKEN_GOES_HERE` 

### 元の試験の代替

オリジントライアルなしでローカルにBadging APIを試してみたい場合は、 `chrome://flags` `#enable-experimental-web-platform-features`フラグを`#enable-experimental-web-platform-features` 。

### オリジンのトライアル中にBadging APIを使用する

Dogfood：オリジンのトライアル中、APIは`window.ExperimentalBadge`から利用できます。以下のコードは現在の設計に基づいており、標準化されたAPIとしてブラウザに表示される前に変更されます。

Badging APIを使用するには、Webアプリが[Chromeのインストール可能性基準](/web/fundamentals/app-install-banners/#criteria)を満たしている必要があり、ユーザーはそれをホーム画面に追加する必要があります。

`ExperimentalBadge`インターフェースは、 `window`メンバーオブジェクトです。次の2つのメソッドが含まれています。

- `set([number])` ：アプリのバッジを設定します。値が提供されている場合は、バッジを提供されている値に設定し、それ以外の場合は、プレーンな白いドット（またはプラットフォームに応じて他のフラグ）を表示します。
- `clear()` ：アプリのバッジを削除します。

```js
// In a web page
const unreadCount = 24;
window.ExperimentalBadge.set(unreadCount);
```

`ExperimentalBadge.set()`および`ExperimentalBadge.clear()`は、フォアグラウンドページから、または潜在的にはサービスワーカーから呼び出すことができます。いずれの場合も、現在のページだけでなく、アプリ全体に影響します。

場合によっては、OSはバッジの正確な表現を許可しないことがあります。この場合、ブラウザーはそのデバイスに最適な表現を提供しようとします。たとえば、AndroidではBadging APIはサポートされていませんが、Androidでは数値ではなくドットのみが表示されます。

注：ユーザーエージェントがバッジを表示する方法については何も想定しないでください。一部のユーザーエージェントは「4000」などの番号を取り、「99+」に書き換える予定です。自分で飽和させた場合（たとえば「99」）、「+」は表示されません。実際の数に関係なく、 `Badge.set(unreadCount)`を設定するだけで、ユーザーエージェントはそれに応じて表示を処理できます。

## フィードバック{: #feedback }

Badging APIがお客様のニーズを満たす方法で機能し、主要なシナリオを見逃さないようにするために、あなたの助けが必要です。

<aside class="key-point"><b>君の力が必要なんだ！</b> -現在の設計（整数またはフラグ値のいずれかを許可）がニーズを満たしますか？うまくいかない場合は、 <a href="https://github.com/WICG/badging/issues">WICG /バッジレポジトリに</a>問題を<a href="https://github.com/WICG/badging/issues">報告</a>し、できるだけ詳細な情報を提供してください。さらに、まだ議論されている多くの<a href="https://github.com/WICG/badging/blob/master/choices.md">未解決の質問</a>がありますので、フィードバックをお待ちしております。</aside>

また、Badging APIの使用方法についても興味があります。

- ユースケースのアイデアや、それを使用する場所のアイデアをお持ちですか？
- これを使用する予定はありますか？
- それが好きで、あなたのサポートを見せたいですか？

[Badging API WICG談話の](https://discourse.wicg.io/t/badging-api-for-showing-an-indicator-on-a-web-apps-shelf-icon/2900)議論についての考えを共有してください。

{% include "web/_shared/helpful.html" %}

## 役立つリンク{: #helpful }

- [公開説明者](https://github.com/WICG/badging/blob/master/explainer.md)
- [バッジAPIデモ](https://badging-api.glitch.me/) | [バッジAPIデモソース](https://glitch.com/edit/#!/badging-api?path=demo.js)
- [追跡バグ](https://bugs.chromium.org/p/chromium/issues/detail?id=719176)
- [ChromeStatus.comエントリ](https://www.chromestatus.com/features/6068482055602176)
- [オリジントライアルトークンを](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)リクエストする
- [オリジントライアルトークンの使用方法](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md#how-do-i-enable-an-experimental-feature-on-my-origin)
- 点滅コンポーネント： `UI>Browser>WebAppInstalls`

{% include "web/_shared/rss-widget-updates.html" %}
