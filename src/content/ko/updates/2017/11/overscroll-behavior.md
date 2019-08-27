project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"
description: CSS 오버 스크롤 동작 속성 소개

{# wf_updated_on: 2019-08-26 #} {# wf_published_on: 2017-11-14 #}

{# wf_tags: chrome63,css,overscroll,scroll #} {# wf_blink_components: Blink>CSS #} {# wf_featured_image:/web/updates/images/2017/11/overscroll-behavior/card.png #} {# wf_featured_snippet: The CSS overscroll-behavior property allows developers to override the browser's overflow scroll effects when reaching the top/bottom of content. It can be used to customize or prevent the mobile pull-to-refresh action. #}

# 스크롤 제어 : 풀-리-프레쉬 및 오버 플로우 효과 사용자 정의 {: .page-title }

{% include "web/_shared/contributors/ericbidelman.html" %} {% include "web/_shared/contributors/majidvp.html" %} {% include "web/_shared/contributors/sunyunjia.html" %}

<style>
figure {
  text-align: center;
}
figcaption {
  font-size: 14px;
  font-style: italic;
}
.border {
  border: 1px solid #ccc;
}
.centered {
  display: flex;
  justify-content: center;
}
</style>

### TL; DR {: #tldr .hide-from-toc}

[CSS](https://wicg.github.io/overscroll-behavior/) 오버 스크롤 [`overscroll-behavior`](https://wicg.github.io/overscroll-behavior/) 속성을 통해 개발자는 컨텐츠의 상단 / 하단에 도달 할 때 브라우저의 기본 오버 플로우 스크롤 동작을 무시할 수 있습니다. 사용 사례에는 모바일에서 새로 고침 풀 기능을 비활성화하고 오버 스크롤 글로우 및 고무 밴딩 효과를 제거하며 페이지 콘텐츠가 모달 / 오버레이 아래에서 스크롤되는 것을 방지하는 것이 포함됩니다.

`overscroll-behavior` 에는 Chrome `overscroll-behavior` 필요합니다. 다른 브라우저에서 개발 중이거나 고려 중입니다. 자세한 내용은 [chromestatus.com](https://www.chromestatus.com/feature/5734614437986304) 을 참조하십시오. {: .caution }

## 배경

### 스크롤 경계 및 스크롤 체인 {: #scrollchaining }

<figure class="attempt-right">
  <a href="/web/updates/images/2017/11/overscroll-behavior/drawer-scroll.mp4" target="_blank">
    <video src="/web/updates/images/2017/11/overscroll-behavior/drawer-scroll.mp4" autoplay loop muted alt="Drawer demo" height="300"></video>
  </a>
  <figcaption>Chrome Android에서 스크롤 체인</figcaption>
</figure>

스크롤은 페이지와 상호 작용하는 가장 기본적인 방법 중 하나이지만 브라우저의 기발한 기본 동작으로 인해 특정 UX 패턴을 다루기가 까다로울 수 있습니다. 예를 들어, 사용자가 스크롤해야 할 항목이 많은 앱 드로어를 사용하십시오. 하단에 도달하면 더 이상 사용할 콘텐츠가 없으므로 오버플로 컨테이너가 스크롤을 중지합니다. 다시 말해, 사용자는 "스크롤 경계"에 도달한다. 그러나 사용자가 계속 스크롤하면 어떻게되는지 확인하십시오. **서랍 *뒤* 의 내용이 스크롤되기 시작합니다** ! 부모 컨테이너가 스크롤을 넘깁니다. 예제에서 메인 페이지 자체.

이 동작을 **스크롤 체인** 이라고 **합니다** . 컨텐츠를 스크롤 할 때 브라우저의 기본 동작 종종 기본값은 꽤 좋지만 때로는 바람직하지 않거나 예기치 않은 경우도 있습니다. 특정 앱은 사용자가 스크롤 경계를 칠 때 다른 사용자 경험을 제공하고자 할 수 있습니다.

### 새로 고침 풀 효과 {: #p2r }

Pull-to-Refresh는 Facebook 및 Twitter와 같은 모바일 앱에서 널리 사용되는 직관적 인 제스처입니다. 소셜 피드를 풀고 해제하면 최신 게시물을로드 할 수있는 새로운 공간이 만들어집니다. 실제로이 특정 UX는 *인기* 가 높아져 Android의 Chrome과 같은 모바일 브라우저에서도 동일한 효과를 채택했습니다. 페이지 상단에서 아래로 스 와이프하면 전체 페이지가 새로 고쳐집니다.

<div class="clearfix centered">
  <figure class="attempt-left">
    <a href="/web/updates/images/2017/11/overscroll-behavior/twitter.mp4" target="_blank">
       <video src="/web/updates/images/2017/11/overscroll-behavior/twitter.mp4" autoplay muted loop height="350" class="border"></video>
    </a>
    <figcaption>트위터의 커스텀 Pull-to-Refresh <br> PWA에서 피드를 새로 고침 할 때</figcaption>
  </figure>
  <figure class="attempt-right">
    <a href="/web/updates/images/2017/11/overscroll-behavior/mobilep2r.mp4" target="_blank">
       <video src="/web/updates/images/2017/11/overscroll-behavior/mobilep2r.mp4" autoplay muted loop height="350" class="border"></video>
    </a>
    <figcaption>Chrome Android의 기본 Pull-to-Refresh 액션 <br> 전체 페이지를 새로 고칩니다.</figcaption>
  </figure>
</div>

Twitter [PWA](/web/progressive-web-apps/) 와 같은 상황에서는 기본 끌어 오기 새로 고침 작업을 비활성화하는 것이 좋습니다. 왜? 이 앱에서는 사용자가 실수로 페이지를 새로 고치지 않기를 원할 것입니다. 이중 새로 고침 애니메이션을 볼 가능성도 있습니다! 또는 브라우저의 작업을 사이트 브랜딩에보다 가깝게 정렬하여 브라우저 작업을 사용자 정의하는 것이 더 좋을 수도 있습니다. 불행한 부분은 이러한 유형의 사용자 정의가 까다로워 졌다는 것입니다. 개발자는 불필요한 JavaScript를 작성하거나 [패시브 방식이 아닌](/web/tools/lighthouse/audits/passive-event-listeners) 터치 리스너를 추가하거나 (스크롤을 차단) 100vw / vh `<div>` 페이지 전체를 고정시킵니다 (페이지 넘침 방지). 이러한 해결 방법에는 스크롤 성능에 대한 [문서화 된](https://wicg.github.io/overscroll-behavior/#intro) 부정적인 영향이 있습니다.

우리는 더 잘할 수 있습니다!

## `overscroll-behavior` {: #intro } 소개

`overscroll-behavior` [속성](https://wicg.github.io/overscroll-behavior/) 은 컨테이너를 오버 스크롤 할 때 발생하는 동작 (페이지 자체 포함)을 제어하는 새로운 CSS 기능입니다. 이를 사용하여 스크롤 체인을 취소하고, 새로 고침 풀 액션을 비활성화 / 사용자 정의하고, iOS에서 Safari가 `overscroll-behavior` 구현할 때 `overscroll-behavior` 효과를 비활성화 할 수 있습니다. <strong data-md-type="double_emphasis">`overscroll-behavior`</strong> 을 **사용** 하면 소개에서 언급 한 해킹과 같이 **페이지 성능에 부정적인 영향을 미치지 않습니다** !

이 속성에는 세 가지 가능한 값이 있습니다.

1. **자동** -기본값. 요소에서 시작된 스크롤은 상위 요소로 전파 될 수 있습니다.

- **포함** -스크롤 체인을 방지합니다. 스크롤은 조상에게 전파되지 않지만 노드 내의 로컬 효과가 표시됩니다. 예를 들어, Android의 오버 스크롤 글로우 효과 또는 iOS의 고무 밴딩 효과는 스크롤 경계에 도달하면 사용자에게 알립니다. **참고** : `overscroll-behavior: contain` `html` 요소에 `overscroll-behavior: contain` 을 사용하면 초과 스크롤 탐색 작업을 방지 할 수 있습니다.
- **none-** `contain` 과 동일하지만 노드 자체 내에서의 오버 스크롤 효과를 방지합니다 (예 : Android 오버 스크롤 글로우 또는 iOS 고무 밴딩).

참고 : `overscroll-behavior` 동작은 특정 축에 대한 동작 만 정의하려는 경우 `overscroll-behavior-y` `overscroll-behavior-x` 및 `overscroll-behavior-y` `overscroll-behavior-x` 속기를 지원합니다.

`overscroll-behavior` 사용 방법을 알아보기 위해 몇 가지 예를 살펴 보겠습니다.

## 스크롤이 고정 위치 요소 {: #fixedpos }을 (를) 빠져 나가지 못하도록 방지

### 대화 상자 시나리오 {: #chat }

<figure class="attempt-right">
  <a href="/web/updates/images/2017/11/overscroll-behavior/chatbox-chaining.mp4" target="_blank">
    <video src="/web/updates/images/2017/11/overscroll-behavior/chatbox-chaining.mp4" autoplay muted loop alt="Chatbox demo" height="350" class="border"></video>
  </a>
  <figcaption>채팅 창 아래의 내용도 스크롤됩니다. (</figcaption>
</figure>

페이지 하단에 고정 된 위치 지정된 대화 상자를 고려하십시오. 대화 상자는 자체 포함 된 구성 요소이며 대화 상자의 내용과 별도로 스크롤됩니다. 그러나 스크롤 체인으로 인해 사용자가 채팅 기록의 마지막 메시지에 도달하자마자 문서가 스크롤되기 시작합니다.

이 응용 프로그램의 경우 채팅 상자 내에서 시작된 스크롤을 채팅 내에 유지하는 것이 더 적합합니다. 채팅 메시지를 보유하는 요소에 `overscroll-behavior: contain` 를 추가하여 `overscroll-behavior: contain` 할 수 있습니다.

```css
#chat .msgs {
  overflow: auto;
  overscroll-behavior: contain;
  height: 300px;
}
```

기본적으로 대화 상자의 스크롤 컨텍스트와 기본 페이지를 논리적으로 구분합니다. 결과적으로 사용자가 채팅 기록의 맨 위 / 아래에 도달하면 기본 페이지가 그대로 유지됩니다. 대화 상자에서 시작하는 스크롤이 전파되지 않습니다.

### 페이지 오버레이 시나리오 {: #overlay }

"언더 스크롤"시나리오의 또 다른 변형은 **고정 된 위치 오버레이** 뒤에서 내용이 스크롤되는 경우입니다. 죽은 공짜 `overscroll-behavior` 은 순서대로입니다! 브라우저가 도움이 되려고하지만 사이트를 버그가있는 것처럼 보이게합니다.

**예** - `overscroll-behavior: contain` 없는 모달 `overscroll-behavior: contain` :

<figure class="clearfix centered">
  <div class="attempt-left">
    <a href="/web/updates/images/2017/11/overscroll-behavior/modal-off.mp4" target="_blank">
      <video src="/web/updates/images/2017/11/overscroll-behavior/modal-off.mp4" autoplay muted loop height="290"></video>
    </a>
    <figcaption><b>Before</b> : 페이지 내용이 오버레이 아래로 스크롤됩니다.</figcaption>
  </div>
  <div class="attempt-right">
    <a href="/web/updates/images/2017/11/overscroll-behavior/modal-on.mp4" target="_blank">
      <video src="/web/updates/images/2017/11/overscroll-behavior/modal-on.mp4" autoplay muted loop height="290"></video>
    </a>
    <figcaption><b>After</b> : 페이지 내용이 오버레이 아래로 스크롤되지 않습니다.</figcaption>
  </div>
</figure>

## Pull-to-Refresh {: #disablp2r } 비활성화

**Pull-to-Refresh 액션을 끄는 것은 CSS 한 줄입니다** . 전체 뷰포트 정의 요소에서 스크롤 체인을 방지하십시오. 대부분의 경우 `<html>` 또는 `<body>` .

```css
body {
  /* Disables pull-to-refresh but allows overscroll glow effects. */
  overscroll-behavior-y: contain;
}
```

이 간단한 추가 기능을 사용하여 [chatbox 데모](https://ebidel.github.io/demos/chatbox.html) 에서 이중 새로 고침 애니메이션을 수정하고 대신 깔끔한 로딩 애니메이션을 사용하는 사용자 지정 효과를 구현할 수 있습니다. 받은 편지함을 새로 고치면 전체받은 편지함도 흐리게 표시됩니다.

<figure class="clearfix centered">
  <div class="attempt-left">
    <video src="/web/updates/images/2017/11/overscroll-behavior/chatbox-double-refresh.mp4" autoplay muted loop height="225"></video>
    <figcaption>전에</figcaption>
  </div>
  <div class="attempt-right">
    <video src="/web/updates/images/2017/11/overscroll-behavior/chatbox-double-refresh-fix.mp4" autoplay muted loop height="225"></video>
    <figcaption>후</figcaption>
  </div>
</figure>

다음은 [전체 코드](https://github.com/ebidel/demos/blob/master/chatbox.html) 의 스 니펫입니다.

```html
<style>
  body.refreshing #inbox {
    filter: blur(1px);
    touch-action: none; /* prevent scrolling */
  }
  body.refreshing .refresher {
    transform: translate3d(0,150%,0) scale(1);
    z-index: 1;
  }
  .refresher {
    --refresh-width: 55px;
    pointer-events: none;
    width: var(--refresh-width);
    height: var(--refresh-width);
    border-radius: 50%; 
    position: absolute;
    transition: all 300ms cubic-bezier(0,0,0.2,1);
    will-change: transform, opacity;
    ...
  }
</style>

<div class="refresher">
  <div class="loading-bar"></div>
  <div class="loading-bar"></div>
  <div class="loading-bar"></div>
  <div class="loading-bar"></div>
</div>

<section id="inbox"><!-- msgs --></section>

<script>
  let _startY;
  const inbox = document.querySelector('#inbox');

  inbox.addEventListener('touchstart', e => {
    _startY = e.touches[0].pageY;
  }, {passive: true});

  inbox.addEventListener('touchmove', e => {
    const y = e.touches[0].pageY;
    // Activate custom pull-to-refresh effects when at the top of the container
    // and user is scrolling up.
    if (document.scrollingElement.scrollTop === 0 && y > _startY &&
        !document.body.classList.contains('refreshing')) {
      // refresh inbox.
    }
  }, {passive: true});
</script>
```

## 오버 스크롤 글로우 및 러버 밴딩 효과 비활성화 {: #disableglow }

스크롤 경계를 칠 때 바운스 효과를 비활성화하려면 `overscroll-behavior-y: none` .

```css
body {
  /* Disables pull-to-refresh and overscroll glow effect.
     Still keeps swipe navigations. */
  overscroll-behavior-y: none;
}
```

<figure class="clearfix centered">
  <div class="attempt-left">
    <video src="/web/updates/images/2017/11/overscroll-behavior/drawer-glow.mp4" autoplay muted loop height="300" class="border"></video>
    <figcaption><b>Before</b> : 스크롤 경계를 두드리면 빛이납니다.</figcaption>
  </div>
  <div class="attempt-right">
    <video src="/web/updates/images/2017/11/overscroll-behavior/drawer-noglow.mp4" autoplay muted loop height="300" class="border"></video>
    <figcaption><b>후</b> : 글로우 비활성화.</figcaption>
  </div>
</figure>

참고 : 여전히 왼쪽 / 오른쪽 스 와이프 탐색 기능이 유지됩니다. 탐색을 방지하기 위해 `overscroll-behavior-x: none` 사용할 수 있습니다. 그러나 이것은 [여전히](https://crbug.com/762023) Chrome [에서 구현](https://crbug.com/762023) 되고 있습니다.

## 전체 데모 {: #demo }

전체 `overscroll-behavior` [데모](https://ebidel.github.io/demos/chatbox.html) 는 `overscroll-behavior` 을 사용하여 사용자 정의 새로 고침 풀업 애니메이션을 만들고 스크롤이 채팅 상자 위젯에서 빠져 나오지 못하도록합니다. 이는 CSS `overscroll-behavior` 없이 달성하기 까다로운 최적의 사용자 경험을 제공합니다.

<figure>
  <a href="https://ebidel.github.io/demos/chatbox.html" target="_blank">
    <video src="/web/updates/images/2017/11/overscroll-behavior/chatbox-fixed.mp4" autoplay muted loop alt="Chatbox demo" height="600"></video>
  </a>
  <figcaption><a href="https://ebidel.github.io/demos/chatbox.html" target="_blank">데모보기</a> | <a href="https://github.com/ebidel/demos/blob/master/chatbox.html" target="_blank">출처</a></figcaption>
</figure>

<br>
