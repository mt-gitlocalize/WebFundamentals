project_path: "/web/fundamentals/_project.yaml"
book_path: "/web/fundamentals/_book.yaml"
description: Web Payments 생태계에 참여하는 사람, 서로 상호 작용하는 방법 및 참여 방법에 대해 학습하십시오.

{# wf_published_on: 2018-09-10 #} {# wf_updated_on: 2019-02-22 #} {# wf_blink_components: Blink>Payments #}

# 지불 생태계 작동 방식 {: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/dgash.html" %}

새로운 결제 에코 시스템이 웹 결제와 어떻게 작동하는지 봅시다.

## 웹 지불의 해부학

*웹 결제* 는 여러 웹 표준으로 구성됩니다.

- **결제 요청 API :** [결제 요청 API를](https://www.w3.org/TR/payment-request/) 사용하면 기본 브라우저 UI를 통해 빠르고 쉽게 결제 할 수 있습니다. 일관된 결제 흐름을 제공하는 동시에 사용자가 모든 결제시 배송 및 결제 정보를 입력 할 필요가 없습니다. [결제 요청 API 작동 방식](/web/fundamentals/payments/basics/how-payment-request-api-works) 또는 [결제 요청 API에 대한 심층 분석](/web/fundamentals/payments/merchant-guide/deep-dive-into-payment-request) 에서 세부 사항이 [어떻게](/web/fundamentals/payments/basics/how-payment-request-api-works) 작동하는지 학습하십시오.
- **지불 처리기 API :** [지불 처리기 API](https://w3c.github.io/payment-handler/) 는 웹 기반 지불 응용 프로그램이 표준 지불 요청 API를 통해 판매자 웹 사이트에서 지불 방법으로 작동 할 수 있도록하여 지불 공급자에게 에코 시스템을 엽니 다.
- **결제 수단 식별자 :** [결제 수단 식별자](https://w3c.github.io/payment-method-id/) 는 결제 수단을 식별하기 위해 문자열 ( `basic-card` , `https://google.com/pay` 등)을 사용하는 방법을 정의합니다. 표준화 된 결제 방법 식별자와 함께 누구나 URL 기반 결제 방법 식별자를 사용하여 자체 결제 방법을 정의 할 수 있습니다. [결제 수단 기본 사항](/web/fundamentals/payments/basics/payment-method-basics) 에서 자세히 알아보세요.
- **지불 방법 매니페스트 (Payment Method Manifest) : 지불 방법 매니페스트** ( [Payment Method Manifest)](https://w3c.github.io/payment-method-manifest/) 는 지불 방법 매니페스트 (payment method manifest)라고하는 기계 판독 가능 매니페스트 파일을 정의하고, 지불 방법이 지불 생태계에 참여하는 방법 및 이러한 파일이 사용되는 방법을 설명합니다.

## 결제 요청 프로세스 작동 방식

온라인 거래에는 일반적으로 네 명의 참가자가 있습니다.

<table>
  <tr>
   <th style="width:30%;">선수</th>
   <th style="width:50%;">기술</th>
   <th style="width:20%;">API 사용법</th>
  </tr>
  <tr>
   <td>고객</td>
   <td>결제 과정을 거친 사용자가 온라인으로 품목을 구매합니다.</td>
   <td>해당 없음</td>
  </tr>
  <tr>
   <td>가맹점</td>
   <td>웹 사이트에서 제품을 판매하는 업체</td>
   <td>결제 요청 API</td>
  </tr>
  <tr>
   <td>결제 서비스 제공 업체 (PSP)</td>
   <td>실제로 결제를 처리하는 타사 회사로, 고객에게 청구하고 가맹점을 청구합니다. 지불 게이트웨이 또는 지불 프로세서라고도합니다.</td>
   <td>결제 요청 API</td>
  </tr>
  <tr>
   <td>결제 처리기</td>
   <td>일반적으로 고객의 지불 자격 증명을 저장하는 응용 프로그램을 제공하고 권한을 부여받은 타사 회사는 거래를 처리하기 위해 판매자에게 제공합니다.</td>
   <td>결제 처리기 API</td>
  </tr>
</table>

<figure>
  <img src="../../images/payment-ecosystem/payment-interactions.png" alt="">
  <figcaption><i>웹에서 신용 카드 결제를 처리 할 때 발생하는 일반적인 일련의 이벤트</i></figcaption>
</figure>

1. 고객이 판매자의 웹 사이트를 방문하여 장바구니에 품목을 추가 한 후 결제 흐름을 시작합니다.
2. 판매자는 거래를 처리하기 위해 고객의 지불 자격 증명이 필요합니다. [**결제 요청 API를**](/web/fundamentals/payments/basics/how-payment-request-api-works) 사용 [**하여**](/web/fundamentals/payments/basics/how-payment-request-api-works) 고객에게 결제 요청 UI를 제공합니다. UI에는 [**지불 방법 식별자에**](/web/fundamentals/payments/basics/payment-method-basics) 의해 지정된 다양한 지불 방법이 나열됩니다. 결제 방법에는 브라우저에 저장된 신용 카드 번호 또는 Google Pay, Samsung Pay 등과 같은 결제 처리기가 포함될 수 있습니다. 판매자는 고객의 배송 주소 및 연락처 정보를 선택적으로 요청할 수 있습니다.
3. 고객이 Google Pay와 같은 결제 방법을 선택하면 Chrome에서 플랫폼 기본 결제 앱 또는 웹 기반 결제 앱을 시작합니다. 이 단계는 **Payment Method Manifest를** 기반으로 지불 처리기의 구현에 전적으로 달려 있습니다. 고객이 지불을 승인 한 후 지불 핸들러는 지불 요청 API에 대한 응답을 리턴하여 판매자 사이트로 릴레이합니다. (지급이 은행 송금, 암호 화폐와 같은 푸시 유형 인 경우 판매자가 응답을받을 때 지불이 이미 처리됩니다.)
4. 판매자 사이트는 지불 자격 증명을 PSP에 보내 지불을 처리하고 자금 이체를 시작합니다. 일반적으로 서버 측에서 지불을 확인하는 것도 필요합니다.
5. PSP는 결제를 처리하여 고객의 은행 또는 신용 카드 발급 기관에서 판매자에게 자금 이체를 안전하게 요청한 다음 성공 또는 실패 결과를 판매자 웹 사이트로 반환합니다.
6. 판매자 웹 사이트는 고객에게 거래의 성공 또는 실패를 알리고 다음 단계 (예 : 구매 한 품목 배송)를 표시합니다.

![](../../images/payment-ecosystem/payment-transaction-process.png)

## 주의 사항 : PSP 의존

판매자이고 신용 카드 결제를 수락하려는 경우 PSP는 결제 처리 체인에서 중요한 링크입니다. 지불 요청 API를 구현한다고해서 PSP가 필요하지는 않습니다.

판매자는 일반적으로 편의 및 비용상의 이유로 지불 처리를 수행하기 위해 타사 PSP에 의존합니다. 이는 대부분의 PSP가 카드 소지자 데이터의 안전을 규제하는 정보 보안 표준 인 [PCI DSS를](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard) 준수하기 때문입니다.

엄격한 PCI DSS 준수를 달성하고 유지하는 것은 비용이 많이 들고 어려울 수 있기 때문에 대부분의 판매자는 준수 PSP에 의존하면 인증 프로세스 자체를 거치지 않아도됩니다. 그러나 재정적으로 탄탄한 일부 대기업은 타사의 의존을 피하기 위해 자체 PCI DSS 인증을받습니다.

`basic-card` 결제 방법 (PAN)을 결제 자격 증명으로 반환하는 기본 `basic-card` 결제 방법, 즉 카드에 엠보싱 된 번호를 사용하는 경우 특히 중요합니다. JavaScript로 처리하려면 [PCI SAQ A-EP](https://www.pcisecuritystandards.org/documents/PCI-DSS-v3_2-SAQ-A_EP.pdf) 준수가 필요합니다.

따라서 지불 처리를 PCI DSS 호환 PSP에 위임하면 판매자 사이트의 요구 사항이 단순 해지고 고객의 지불 정보 무결성이 보장됩니다.

지불 요청 API를 사용하고 싶지만 PCI SAQ A-EP와 호환되는 대역폭이없는 경우 일부 PSP는 지불 요청 API를 사용하는 SDK를 제공합니다.

### 지원 결제 게이트웨이 목록

- [줄무늬](https://stripe.com/docs/stripe-js/elements/payment-request-button)
- [브레인 트리](https://developers.braintreepayments.com/guides/payment-request/overview)
- [블루 스냅](https://developers.bluesnap.com/v8976-Basics/docs/payment-request-api)
- [우리는 지불한다](https://developer.wepay.com/docs/mobile/payment-request-api)

참고 : 지불 게이트웨이가 지불 요청 API를 포함하지만 여기에 나열되지 않은 SDK를 제공하는 경우 [풀 요청](https://github.com/google/WebFundamentals/pulls) 을 보내주십시오.

## 다음

지불 요청 API의 [작동](/web/fundamentals/payments/basics/how-payment-request-api-works) 방식에서 지불 요청 API의 필드 및 방법에 대해 학습하십시오.

## 피드백 {: #feedback }

{% include "web/_shared/helpful.html" %}
