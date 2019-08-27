project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"
description: CSS overscroll-behavior屬性簡介。

{# wf_updated_on: 2019-08-26 #} {# wf_published_on: 2017-11-14 #}

{# wf_tags: chrome63,css,overscroll,scroll #} {# wf_blink_components: Blink>CSS #} {# wf_featured_image:/web/updates/images/2017/11/overscroll-behavior/card.png #} {# wf_featured_snippet: The CSS overscroll-behavior property allows developers to override the browser's overflow scroll effects when reaching the top/bottom of content. It can be used to customize or prevent the mobile pull-to-refresh action. #}

# 控制滾動:自定義拉到刷新和溢出效果{: .page-title }

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

[CSS `overscroll-behavior`](https://wicg.github.io/overscroll-behavior/)屬性允許開發人員在到達內容的頂部/底部時覆蓋瀏覽器的默認溢出滾動行為。用例包括禁用移動設備上的“拉出 - 刷新”功能，刪除過度滾動發光和橡皮帶效果，以及防止頁面內容在模態/疊加層下方滾動。

`overscroll-behavior`需要Chrome 63+.它正在開發中或被其他瀏覽器考慮.有關更多信息,請參閱[chromestatus.com](https://www.chromestatus.com/feature/5734614437986304) . {: .caution }

## 背景

### 滾動邊界和滾動鏈接{: #scrollchaining }

<figure class="attempt-right">
  <a href="/web/updates/images/2017/11/overscroll-behavior/drawer-scroll.mp4" target="_blank">
    <video src="/web/updates/images/2017/11/overscroll-behavior/drawer-scroll.mp4" autoplay loop muted alt="Drawer demo" height="300"></video>
  </a>
  <figcaption>滾動鏈接Chrome Android。</figcaption>
</figure>

滾動是與頁面交互的最基本方式之一，但由於瀏覽器的古怪默認行為，某些UX模式可能很難處理。例如，使用具有大量項目的app抽屜，用戶可能需要滾動瀏覽。當它們到達底部時，溢出容器停止滾動，因為沒有更多的內容要消耗。換句話說，用戶到達“滾動邊界”。但請注意，如果用戶繼續滾動會發生什麼。 **抽屜*後面*的內容開始滾動** ！滾動由父容器接管;示例中的主頁面本身。

原來這種行為稱為**滾動鏈接** ;滾動內容時瀏覽器的默認行為。通常情況下，默認值非常好，但有時候這是不可取的甚至是意外的。某些應用可能希望在用戶點擊滾動邊界時提供不同的用戶體驗。

### 拉動刷新效果{: #p2r }

Pull-to-refresh是一種直觀的手勢，由Facebook和Twitter等移動應用推廣。下拉社交訂閱源並發布會為最近加載的帖子創建新空間。事實上，這個特定的用戶體驗已經變得*如此受歡迎* ，以至於Android上的Chrome等移動瀏覽器採用了同樣的效果。向下滑動頁面頂部會刷新整個頁面：

<div class="clearfix centered">
  <figure class="attempt-left">
    <a href="/web/updates/images/2017/11/overscroll-behavior/twitter.mp4" target="_blank">
       <video src="/web/updates/images/2017/11/overscroll-behavior/twitter.mp4" autoplay muted loop height="350" class="border"></video>
    </a>
    <figcaption>Twitter的定制拉動刷新<br>在他們的PWA中刷新飼料時。</figcaption>
  </figure>
  <figure class="attempt-right">
    <a href="/web/updates/images/2017/11/overscroll-behavior/mobilep2r.mp4" target="_blank">
       <video src="/web/updates/images/2017/11/overscroll-behavior/mobilep2r.mp4" autoplay muted loop height="350" class="border"></video>
    </a>
    <figcaption>Chrome Android的原生拉動式動作<br>刷新整個頁面。</figcaption>
  </figure>
</div>

對於像Twitter [PWA](/web/progressive-web-apps/)這樣的情況，禁用本機pull-to-refresh操作可能是有意義的。為什麼？在此應用程序中，您可能不希望用戶意外刷新頁面。還有可能看到雙重刷新動畫！或者，定制瀏覽器的操作可能更好，使其與網站的品牌更緊密地對齊。不幸的是，這種類型的定制很難實現。開發人員最終編寫不必要的JavaScript，添加[非被動](/web/tools/lighthouse/audits/passive-event-listeners)觸摸偵聽器（阻止滾動），或將整個頁面粘貼在100vw / vh `<div>` （以防止頁面溢出）。這些變通方法對滾動性能有[很好的](https://wicg.github.io/overscroll-behavior/#intro)負面影響。

我們可以做得更好！

## 引入`overscroll-behavior` {: #intro }

`overscroll-behavior` [屬性](https://wicg.github.io/overscroll-behavior/)是一個新的CSS功能，它控制當您過度滾動容器（包括頁面本身）時發生的行為。您可以使用它來取消滾動鏈接，禁用/自定義拉動刷新操作，禁用iOS上的橡皮筋效果（當Safari實現`overscroll-behavior` ）等等。最好的部分是<strong data-md-type="double_emphasis">使用`overscroll-behavior`不會</strong>像介紹中提到的黑客那樣**對頁面性能產生負面影響** ！

該屬性有三個可能的值：

1. **自動** - 默認。源自元素的滾動可以傳播到祖先元素。

- **包含** - 防止滾動鏈接。滾動不會傳播到祖先，但會顯示節點內的局部效果。例如，Android上的過度滾動發光效果或iOS上的橡皮筋效果會在用戶達到滾動邊界時通知用戶。 **注意** ：使用`overscroll-behavior: contain`在`html`元素上可以防止過度滾動導航操作。
- **無** -一樣`contain` ，但它也可以防止節點本身內反彈時的效果（例如Android的反彈時發光或iOS橡皮）。

注意：如果您只想定義某個軸的行為，則`overscroll-behavior`還支持`overscroll-behavior-x`和`overscroll-behavior-y` 。

讓我們深入研究一些例子，看看如何使用`overscroll-behavior` 。

## 防止滾動轉義固定位置元素{: #fixedpos }

### 聊天框場景{: #chat }

<figure class="attempt-right">
  <a href="/web/updates/images/2017/11/overscroll-behavior/chatbox-chaining.mp4" target="_blank">
    <video src="/web/updates/images/2017/11/overscroll-behavior/chatbox-chaining.mp4" autoplay muted loop alt="Chatbox demo" height="350" class="border"></video>
  </a>
  <figcaption>聊天窗口下方的內容也會滾動:(</figcaption>
</figure>

考慮位於頁面底部的固定定位聊天框。目的是聊天框是一個獨立的組件，它與其背後的內容分開滾動。但是，由於滾動鏈接，一旦用戶點擊聊天歷史記錄中的最後一條消息，文檔就會開始滾動。

對於這個應用程序，使聊天框內的滾動保持在聊天內更合適。我們可以通過添加`overscroll-behavior: contain`到包含聊天消息的元素：

```css
#chat .msgs {
  overflow: auto;
  overscroll-behavior: contain;
  height: 300px;
}
```

基本上，我們在聊天框的滾動上下文和主頁面之間創建了邏輯分隔。最終結果是當用戶到達聊天記錄的頂部/底部時，主頁面保持不變。從聊天框開始的滾動不會傳播出去。

### 頁面疊加方案{: #overlay }

“下劃線”場景的另一種變體是當您看到內容在**固定位置疊加層**後面滾動時。一個死的贈品`overscroll-behavior`是有序的！瀏覽器試圖提供幫助，但它最終會讓網站看起來很麻煩。

**示例** - 包含和不`overscroll-behavior: contain` ：

<figure class="clearfix centered">
  <div class="attempt-left">
    <a href="/web/updates/images/2017/11/overscroll-behavior/modal-off.mp4" target="_blank">
      <video src="/web/updates/images/2017/11/overscroll-behavior/modal-off.mp4" autoplay muted loop height="290"></video>
    </a>
    <figcaption><b>之前</b> ：頁面內容在疊加下滾動。</figcaption>
  </div>
  <div class="attempt-right">
    <a href="/web/updates/images/2017/11/overscroll-behavior/modal-on.mp4" target="_blank">
      <video src="/web/updates/images/2017/11/overscroll-behavior/modal-on.mp4" autoplay muted loop height="290"></video>
    </a>
    <figcaption><b>之後</b> ：頁面內容不會在疊加下滾動。</figcaption>
  </div>
</figure>

## 禁用pull-to-refresh {: #disablp2r }

**關閉pull-to-refresh動作只需一行CSS** 。只是阻止整個視口定義元素上的滾動鏈接。在大多數情況下，那是`<html>`或`<body>` ：

```css
body {
  /* Disables pull-to-refresh but allows overscroll glow effects. */
  overscroll-behavior-y: contain;
}
```

通過這個簡單的添加，我們修復了[聊天室演示中](https://ebidel.github.io/demos/chatbox.html)的雙拉動畫，並且可以實現使用整潔加載動畫的自定義效果。收件箱刷新後整個收件箱也會模糊：

<figure class="clearfix centered">
  <div class="attempt-left">
    <video src="/web/updates/images/2017/11/overscroll-behavior/chatbox-double-refresh.mp4" autoplay muted loop height="225"></video>
    <figcaption>之前</figcaption>
  </div>
  <div class="attempt-right">
    <video src="/web/updates/images/2017/11/overscroll-behavior/chatbox-double-refresh-fix.mp4" autoplay muted loop height="225"></video>
    <figcaption>後</figcaption>
  </div>
</figure>

這是[完整代碼](https://github.com/ebidel/demos/blob/master/chatbox.html)的片段：

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

## 禁用過捲髮光和橡皮筋效果{: #disableglow }

要在按下滾動邊界時禁用反彈效果，請使用`overscroll-behavior-y: none` ：

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
    <figcaption><b>之前</b> ：擊中滾動邊界顯示發光。</figcaption>
  </div>
  <div class="attempt-right">
    <video src="/web/updates/images/2017/11/overscroll-behavior/drawer-noglow.mp4" autoplay muted loop height="300" class="border"></video>
    <figcaption><b>之後</b> ：發光禁用。</figcaption>
  </div>
</figure>

注意：這仍將保留左/右滑動導航。要阻止導航，可以使用`overscroll-behavior-x: none` 。但是，這[仍然](https://crbug.com/762023)在Chrome [中實現](https://crbug.com/762023) 。

## 完整演示{: #demo }

將所有內容放在一起，完整的`overscroll-behavior` [演示](https://ebidel.github.io/demos/chatbox.html)使用`overscroll-behavior`來創建自定義的拉動 - 刷新動畫，並禁止滾動來轉義`overscroll-behavior`小部件。這提供了最佳的用戶體驗，如果沒有CSS `overscroll-behavior` ，這將是很難實現的。

<figure>
  <a href="https://ebidel.github.io/demos/chatbox.html" target="_blank">
    <video src="/web/updates/images/2017/11/overscroll-behavior/chatbox-fixed.mp4" autoplay muted loop alt="Chatbox demo" height="600"></video>
  </a>
  <figcaption><a href="https://ebidel.github.io/demos/chatbox.html" target="_blank">查看演示</a> | <a href="https://github.com/ebidel/demos/blob/master/chatbox.html" target="_blank">資源</a></figcaption>
</figure>

<br>
