project_path: "/web/fundamentals/_project.yaml"
book_path: "/web/fundamentals/_book.yaml"
description: Web Paymentsエコシステムに誰が関与しているか、どのように相互作用し、どのように関与できるかを学びます。

{# wf_published_on: 2018-09-10 #} {# wf_updated_on: 2019-02-22 #} {# wf_blink_components: Blink>Payments #}

# 支払いエコシステムの仕組み{: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/dgash.html" %}

新しい支払いエコシステムがWebペイメントとどのように機能するかを見てみましょう。

## Webペイメントの構造

*Webペイメント*は複数のWeb標準で構成されています。

- **支払い要求API：** [支払い要求API](https://www.w3.org/TR/payment-request/)は、ネイティブブラウザーUIを介して迅速かつ簡単なチェックアウトを可能にします。一貫したチェックアウトフローを提供すると同時に、ユーザーがチェックアウトごとに配送および支払い情報を入力する必要性を減らします。どのように高レベルで[機能する](/web/fundamentals/payments/basics/how-payment-request-api-works)かについては[、支払いリクエストAPIの](/web/fundamentals/payments/merchant-guide/deep-dive-into-payment-request)仕組みをご覧ください。詳細は[、支払いリクエストAPIの](/web/fundamentals/payments/merchant-guide/deep-dive-into-payment-request)詳細をご覧ください。
- **ペイメントハンドラーAPI：** [ペイメントハンドラーAPI](https://w3c.github.io/payment-handler/)は、標準のペイメントリクエストAPIを介して、Webベースのペイメントアプリケーションがマーチャントウェブサイトのペイメントメソッドとして機能できるようにすることで、ペイメントプロバイダーにエコシステムを開きます。
- **支払い方法識別子：** [支払い方法識別子](https://w3c.github.io/payment-method-id/)は、文字列（ `basic-card` 、 `https://google.com/pay`など）を使用して支払い方法を識別する[方法を](https://w3c.github.io/payment-method-id/)定義します。標準化された支払い方法識別子に加えて、URLベースの支払い方法識別子を使用して、誰でも独自の支払い方法を定義できます。詳しくは、 [支払い方法の基本を](/web/fundamentals/payments/basics/payment-method-basics)ご覧ください。
- **支払い方法マニフェスト：** [支払い方法マニフェスト](https://w3c.github.io/payment-method-manifest/)は、 [支払い方法マニフェスト](https://w3c.github.io/payment-method-manifest/)と呼ばれる機械可読マニフェストファイルを定義し、支払い方法が支払いエコシステムに参加する方法と、そのようなファイルの使用方法を記述します。

## 支払い要求プロセスの仕組み

通常、オンライントランザクションには4人の参加者がいます。

<table>
  <tr>
   <th style="width:30%;">プレイヤー</th>
   <th style="width:50%;">説明</th>
   <th style="width:20%;">APIの使用</th>
  </tr>
  <tr>
   <td>お客さま</td>
   <td>チェックアウトフローを経てオンラインでアイテムを購入するユーザー。</td>
   <td>なし</td>
  </tr>
  <tr>
   <td>商人</td>
   <td>ウェブサイトで製品を販売する企業。</td>
   <td>支払い要求API</td>
  </tr>
  <tr>
   <td>決済サービスプロバイダー（PSP）</td>
   <td>実際に支払いを処理するサードパーティ企業。これには、顧客への請求と加盟店へのクレジットが含まれます。支払いゲートウェイまたは支払い処理者とも呼ばれます。</td>
   <td>支払い要求API</td>
  </tr>
  <tr>
   <td>支払いハンドラ</td>
   <td>通常、顧客の支払い資格情報を保存し、その承認に基づいて取引を処理するために商人に提供するアプリケーションを提供するサードパーティ企業。</td>
   <td>支払いハンドラーAPI</td>
  </tr>
</table>

<figure>
  <img src="../../images/payment-ecosystem/payment-interactions.png" alt="">
  <figcaption><i>Webでのクレジットカード支払いの処理における典型的な一連のイベント</i></figcaption>
</figure>

1. 顧客は販売者のWebサイトにアクセスし、アイテムをショッピングカートに追加して、チェックアウトフローを開始します。
2. 商人は、取引を処理するために顧客の支払い資格情報を必要とします。 [**Payment Request API**](/web/fundamentals/payments/basics/how-payment-request-api-works)を使用して、顧客に支払い要求UIを提示し[**ます**](/web/fundamentals/payments/basics/how-payment-request-api-works) 。 UIは[**、Payment Method Identifiersで**](/web/fundamentals/payments/basics/payment-method-basics)指定され[**た**](/web/fundamentals/payments/basics/payment-method-basics)さまざまな支払い方法をリストします。支払い方法には、ブラウザに保存されたクレジットカード番号、またはGoogle Pay、Samsung Payなどの支払いハンドラーを含めることができます。商人は、オプションで顧客の配送先住所と連絡先情報を要求できます。
3. 顧客がGoogle Payなどの支払い方法を選択すると、Chromeはプラットフォーム固有の支払いアプリまたはウェブベースの支払いアプリのいずれかを起動します。この手順は**、Payment Method Manifestに**基づいて**、支払い**ハンドラの実装次第です。顧客が支払いを承認した後、支払いハンドラーは支払い要求APIに応答を返し、APIはそれをマーチャントサイトに中継します。 （支払いが銀行振込、暗号通貨などのプッシュ型の場合、支払いは商人が応答を受け取ったときにすでに処理されています。）
4. マーチャントサイトは、支払いを処理し、資金移動を開始するために、支払い資格情報をPSPに送信します。通常、サーバー側で支払いを確認することも必要です。
5. PSPは支払いを処理し、顧客の銀行またはクレジットカード発行会社からマーチャントへの資金移動を安全に要求し、成功または失敗の結果をマーチャントWebサイトに返します。
6. 商人のウェブサイトは、取引の成功または失敗を顧客に通知し、次のステップ、たとえば購入した商品の発送を表示します。

![](../../images/payment-ecosystem/payment-transaction-process.png)

## 警告：PSP Reliance

あなたがマーチャントであり、クレジットカード支払いを受け入れたい場合、PSPは支払い処理チェーンの重要なリンクです。 Payment Request APIを実装しても、PSPが不要になるわけではありません。

商人は通常、サードパーティのPSPに依存して、利便性と費用の理由で支払い処理を実行します。これは主に、ほとんどのPSPがカード所有者データの安全性を規制する情報セキュリティ標準である[PCI DSS](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard)に準拠しているためです。

厳密なPCI DSSコンプライアンスの達成と維持は高価で困難な場合があるため、ほとんどの加盟店は、準拠するPSPに依存することで、認証プロセスを経ることを避けられることに気付きます。ただし、一部の大規模で経済的に堅牢な企業は、そのようなサードパーティへの依存を避けるために、独自のPCI DSS認定を取得しています。

プライマリアカウント番号（PAN）を支払い資格情報、つまりカードに刻印された番号として返す`basic-card`支払い方法を使用している場合は、特に重要です。 JavaScriptで処理するには、 [PCI SAQ A-EPに](https://www.pcisecuritystandards.org/documents/PCI-DSS-v3_2-SAQ-A_EP.pdf)準拠する必要があります。

したがって、支払い処理をPCI DSS準拠のPSPに委任すると、マーチャントサイトの要件が簡素化され、顧客の支払い情報の整合性が確保されます。

支払い要求APIを使用したいが、PCI SAQ A-EPに準拠する帯域幅がない場合、一部のPSPは支払い要求APIを使用するSDKを提供します。

### サポートされている支払いゲートウェイのリスト

- [ストライプ](https://stripe.com/docs/stripe-js/elements/payment-request-button)
- [Braintree](https://developers.braintreepayments.com/guides/payment-request/overview)
- [Bluesnap](https://developers.bluesnap.com/v8976-Basics/docs/payment-request-api)
- [我々が支払います](https://developer.wepay.com/docs/mobile/payment-request-api)

注：お支払いゲートウェイが、Payment Request APIを含むSDKを提供しているが、ここにリストされていない場合は、 [プルリクエスト](https://github.com/google/WebFundamentals/pulls)を送信してください。

## 次の

Payment Request APIの[仕組み](/web/fundamentals/payments/basics/how-payment-request-api-works)については、Payment Request APIのフィールドとメソッドをご覧ください。

## フィードバック{: #feedback }

{% include "web/_shared/helpful.html" %}
