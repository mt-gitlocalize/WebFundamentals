project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"

{# wf_updated_on: 2019-08-27 #} {# wf_published_on: 2019-02-06 #} {# wf_tags: fundamentals, performance, app-shell #} {# wf_featured_image: /web/updates/images/2019/02/rendering-on-the-web/icon.png #} {# wf_featured_snippet: Where should we implement logic and rendering in our applications? Should we use Server Side Rendering? What about Rehydration? Let's find some answers! #} {# wf_blink_components: N/A #}

# 在網上渲染{: .page-title }

{% include "web/_shared/contributors/developit.html" %} {% include "web/_shared/contributors/addyosmani.html" %}

作為開發人員，我們經常面臨影響應用程序整個架構的決策。 Web開發人員必須做出的核心決策之一是在應用程序中實現邏輯和呈現的位置。這可能很困難，因為有許多不同的方法來構建網站。

我們對這一領域的理解來自於我們在過去幾年中與大型網站交流的Chrome工作。從廣義上講，我們鼓勵開發人員考慮通過完全補液方法進行服務器渲染或靜態渲染。

為了更好地理解我們在做出這個決定時所選擇的架構，我們需要對每種方法和在談論它們時使用的一致術語有充分的理解。這些方法之間的差異有助於說明通過性能鏡頭在Web上呈現的權衡。

## 術語{: #terminology }

**渲染**

- **SSR：**服務器端呈現 - 將客戶端或通用應用程序呈現為服務器上的HTML。
- **CSR：**客戶端渲染 - 通常使用DOM在瀏覽器中呈現應用程序。
- **補液：**在客戶端上“啟動”JavaScript視圖，以便它們重用服務器呈現的HTML的DOM樹和數據。
- **預渲染：**運行在編譯的時候一個客戶端應用程序，以獲取其初始狀態為靜態HTML。

**性能**

- **TTFB：**第一個字節的時間 - 被視為點擊鏈接和第一個內容之間的時間。
- **FP：** First Paint  - 第一次任何像素變得對用戶可見。
- **FCP：** First Contentful Paint  - 請求內容（文章正文等）變得可見的時間。
- **TTI：**交互時間 - 頁面變為交互的時間（連接的事件等）。

## 服務器渲染{: #server-rendering }

*服務器呈現為服務器上的頁面生成完整的HTML以響應導航。這避免了在客戶端上進行數據獲取和模板化的額外往返，因為它是在瀏覽器獲得響應之前處理的。*

服務器渲染通常會產生快速的[First Paint](https://developers.google.com/web/fundamentals/performance/user-centric-performance-metrics#first_paint_and_first_contentful_paint) （FP）和[First Contentful Paint](https://developers.google.com/web/fundamentals/performance/user-centric-performance-metrics#first_paint_and_first_contentful_paint) （FCP）。在服務器上運行頁面邏輯和呈現可以避免向客戶端發送大量JavaScript，這有助於實現快速[交互時間](https://developers.google.com/web/tools/lighthouse/audits/time-to-interactive) （TTI）。這是有道理的，因為通過服務器渲染，您實際上只是向用戶的瀏覽器發送文本和鏈接。這種方法適用於大範圍的設備和網絡條件，並開啟了有趣的瀏覽器優化，如流文檔解析。

<img src="../../images/2019/02/rendering-on-the-web/server-rendering-tti.png" alt="Diagram showing server rendering and JS execution affecting FCP and TTI" width="350">

使用服務器呈現，用戶不太可能等待CPU綁定的JavaScript處理，然後才能使用您的站點。即使[第三方JS](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/loading-third-party-javascript/)無法避免，使用服務器渲染來減少您自己的第一方[JS成本](https://medium.com/@addyosmani/the-cost-of-javascript-in-2018-7d8950fbb5d4)也可以為您提供更多的“ [預算](https://medium.com/@addyosmani/start-performance-budgeting-dabde04cf6a3) ”。但是，這種方法有一個主要缺點：在服務器上生成頁面需要時間，這通常會導致較慢的[第一個字節時間](https://en.wikipedia.org/wiki/Time_to_first_byte) （TTFB）。

服務器渲染是否足以滿足您的應用程序在很大程度上取決於您正在構建的體驗類型。關於服務器呈現與客戶端呈現的正確應用存在長期爭論，但重要的是要記住，您可以選擇對某些頁面而不是其他頁面使用服務器呈現。一些網站已成功採用混合渲染技術。 [Netflix](https://medium.com/dev-channel/a-netflix-web-performance-case-study-c0bcde26a9d9)服務器渲染其相對靜態的登陸頁面，同時為交互繁重的頁面[預取](https://dev.to/addyosmani/speed-up-next-page-navigations-with-prefetching-4285) JS，使這些較重的客戶端呈現頁面更快地加載。

許多現代框架，庫和體系結構使得在客戶端和服務器上呈現相同的應用程序成為可能。這些技術可用於服務器渲染，但重要的是要注意，在服務器***和***客戶端上進行渲染的體系結構都是他們自己的解決方案類，具有非常不同的性能特徵和權衡。 React用戶可以使用[renderToString（）](https://reactjs.org/docs/react-dom-server.html)或在其上構建的解決方案，如[Next.js，](https://nextjs.org)用於服務器呈現。 Vue用戶可以查看Vue的[服務器渲染指南](https://ssr.vuejs.org)或[Nuxt](https://nuxtjs.org) 。 Angular有[Universal](https://angular.io/guide/universal) 。最流行的解決方案採用某種形式的水合作用，因此在選擇工具之前要注意使用的方法。

## 靜態渲染{: #static-rendering }

[靜態渲染](https://frontarm.com/articles/static-vs-server-rendering/)在構建時發生，並提供快速的First Paint，First Contentful Paint和Time To Interactive  - 假設客戶端JS的數量有限。與服務器渲染不同，它還設法實現始終如一的快速首字節時間，因為頁面的HTML不必動態生成。通常，靜態呈現意味著提前為每個URL生成單獨的HTML文件。通過預先生成HTML響應，可以將靜態呈現部署到多個CDN以利用邊緣緩存。

<img src="../../images/2019/02/rendering-on-the-web/static-rendering-tti.png" alt="Diagram showing static rendering and optional JS execution affecting FCP
and TTI" width="280">

靜態渲染的解決方案有各種形狀和大小。像[Gatsby](https://www.gatsbyjs.org)這樣的工具旨在讓開發人員感覺他們的應用程序是動態呈現的，而不是作為構建步驟生成的。像[Jekyl](https://jekyllrb.com)和[Metalsmith這樣的人](https://metalsmith.io)擁抱他們的靜態性質，提供更多模板驅動的方法。

靜態渲染的一個缺點是必須為每個可能的URL生成單獨的HTML文件。如果您無法提前預測這些URL的內容，或者對於具有大量唯一頁面的網站，這可能具有挑戰性甚至是不可行的。

React用戶可能熟悉[Gatsby](https://www.gatsbyjs.org) ， [Next.js靜態導出](https://nextjs.org/learn/excel/static-html-export/)或[Navi](https://frontarm.com/navi/) - 所有這些都可以方便作者使用組件。但是，了解靜態呈現和預呈現之間的區別非常重要：靜態呈現頁面是交互式的，無需執行太多客戶端JS，而預呈現改進了必須啟動的單頁應用程序的First Paint或First Contentful Paint客戶端，以使頁面真正具有交互性。

如果您不確定給定的解決方案是靜態呈現還是預呈現，請嘗試此測試：禁用JavaScript並加載創建的網頁。對於靜態呈現的頁面，如果不啟用JavaScript，大多數功能仍將存在。對於預渲染頁面，可能仍然存在一些基本功能，如鍊接，但大多數頁面都是惰性的。

另一個有用的測試是使用Chrome DevTools減慢網絡速度，並觀察在頁面變為交互之前已下載了多少JavaScript。預渲染通常需要更多的JavaScript來實現交互，並且JavaScript往往比靜態渲染使用的[漸進增強](https://developer.mozilla.org/en-US/docs/Glossary/Progressive_Enhancement)方法更複雜。

## 服務器渲染與靜態渲染{: #server-vs-static }

服務器渲染不是一個靈丹妙藥 - 它的動態特性會帶來[巨大的計算開銷](https://medium.com/airbnb-engineering/operationalizing-node-js-for-server-side-rendering-c5ba718acfc9)成本。許多服務器渲染解決方案不會提前刷新，可以延遲TTFB或將發送的數據加倍（例如，JS在客戶端上使用的內聯狀態）。在React中，renderToString（）可能很慢，因為它是同步和單線程的。使服務器呈現“正確”可能涉及查找或構建[組件緩存](https://medium.com/@reactcomponentcaching/speedier-server-side-rendering-in-react-16-with-component-caching-e8aa677929b1)解決方案，管理內存消耗，應用[memoization](https://speakerdeck.com/maxnajim/hastening-react-ssr-with-component-memoization-and-templatization)技術以及許多其他問題。您通常多次處理/重建同一個應用程序 - 一次在客戶端上，一次在服務器中。僅僅因為服務器渲染可以使某些東西更快地顯示出來並不會突然意味著您可以減少工作量。

服務器呈現為每個URL按需生成HTML，但速度可能比僅提供靜態呈現內容慢。如果您可以進行額外的工作，服務器呈現+ [HTML緩存](https://freecontent.manning.com/caching-in-react/)可以大大減少服務器渲染時間。服務器渲染的優勢在於能夠提取更多“實時”數據並響應比靜態渲染更完整的請求集。需要個性化的頁面是請求類型的具體示例，該類型不適用於靜態呈現。

在構建[PWA](https://developers.google.com/web/progressive-web-apps/)時，服務器呈現也可以提出有趣的決策。使用整頁[服務工作者](https://developers.google.com/web/fundamentals/primers/service-workers/)緩存或僅僅是服務器呈現單個內容片段更好嗎？

## 客戶端呈現（CSR）{: #csr }

*客戶端呈現（CSR）意味著使用JavaScript直接在瀏覽器中呈現頁面。所有邏輯，數據獲取，模板和路由都在客戶端而不是服務器上處理。*

客戶端渲染可能很難獲得併且移動速度很快。如果做最少的工作，保持[嚴格的JavaScript預算](https://mobile.twitter.com/HenrikJoreteg/status/1039744716210950144)並在盡可能少的[RTT中](https://en.wikipedia.org/wiki/Round-trip_delay_time)提供價值，它可以接近純服務器渲染的性能。使用[HTTP / 2服務器推送](https://www.smashingmagazine.com/2017/04/guide-http2-server-push/)或`<link rel=preload>`可以更快地提供關鍵腳本和數據，這將使解析器更快地為您服務。像[PRPL](https://developers.google.com/web/fundamentals/performance/prpl-pattern/)這樣的模式值得評估，以確保初始和後續導航感覺即時。

<img src="../../images/2019/02/rendering-on-the-web/client-rendering-tti.png" alt="Diagram showing client-side rendering affecting FCP and TTI" width="500">

客戶端渲染的主要缺點是隨著應用程序的增長，所需的JavaScript數量會增加。添加新的JavaScript庫，polyfill和第三方代碼會變得特別困難，這些代碼會競爭處理能力，並且必須經常在呈現頁面內容之前進行處理。使用依賴於大型JavaScript捆綁包的CSR構建的體驗應該考慮[積極的代碼分割](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/code-splitting/) ，並確保延遲加載JavaScript  - “只在您需要時提供所需內容”。對於很少或沒有交互性的體驗，服務器呈現可以代表這些問題的更具可擴展性的解決方案。

對於構建單頁應用程序的人來說，識別大多數頁面共享的用戶界面的核心部分意味著您可以應用[Application Shell緩存](https://developers.google.com/web/updates/2015/11/app-shell)技術。與服務工作者相結合，這可以顯著提高重複訪問的感知性能。

## 通過補液將服務器渲染與CSR相結合{: #rehydration }

這種方法通常被稱為通用渲染或簡稱為“SSR”，它試圖通過兩者兼顧來平滑客戶端渲染和服務器渲染之間的權衡。整頁加載或重新加載等導航請求由將應用程序呈現為HTML的服務器處理，然後用於呈現的JavaScript和數據嵌入到生成的文檔中。當仔細實施時，這就像服務器渲染一樣實現了快速的First Contentful Paint，然後通過使用稱為[（重新）水合](https://docs.electrode.io/guides/general/server-side-data-hydration)的技術在客戶端上再次渲染來“拾取”。這是一種新穎的解決方案，但它可能具有一些相當大的性能缺陷。

補水的SSR的主要缺點是它會對Time To Interactive產生顯著的負面影響，即使它改善了First Paint。 SSR頁面通常看起來具有欺騙性加載和交互性，但在執行客戶端JS並附加事件處理程序之前，實際上無法響應輸入。移動設備可能需要幾秒甚至幾分鐘。

也許你自己也經歷過這種情況 - 在看起來頁面已經加載後的一段時間內，點擊或點擊什麼都不做。這很快變得令人沮喪...... *“為什麼沒有發生什麼？為什麼我不能滾動？“*

### 補液問題:兩個價格的一個應用程序{: #rehydration-issues }

由於JS，補液問題往往比延遲交互更糟糕。為了使客戶端JavaScript能夠準確地“拾取”服務器停止的位置而不必重新請求服務器用於呈現其HTML的所有數據，當前的SSR解決方案通常將UI的響應序列化。作為腳本標記的數據依賴關係到文檔中。生成的HTML文檔包含高級別的重複：

<img src="../../images/2019/02/rendering-on-the-web/html.png" alt="HTML document
containing serialized UI, inlined data and a bundle.js script">

正如您所看到的，服務器返回應用程序UI的描述以響應導航請求，但它也返回用於組成該UI的源數據，以及UI實現的完整副本，然後在客戶端上啟動。只有在bundle.js完成加載和執行後，此UI才會變為交互式。

使用SSR補液從真實網站收集的性能指標表明應該嚴格禁止使用它。歸根結底，原因歸結為用戶體驗：最終讓用戶處於“不可思議的山谷”中非常容易。

<img src="../../images/2019/02/rendering-on-the-web/rehydration-tti.png" alt="Diagram showing client rendering negatively affecting TTI" width="600">

不過，SSR有補液的希望。在短期內，僅將SSR用於高度可緩存的內容可以減少TTFB延遲，從而產生與預渲染類似的結果。 [逐步](https://www.emberjs.com/blog/2017/10/10/glimmer-progress-report.html) ，逐步或部分再水化可能是使該技術在未來更可行的關鍵。

## 流服務器呈現和漸進式補液{: #progressive-rehydration }

服務器渲染在過去幾年中有了許多發展。

[流式服務器呈現](https://zeit.co/blog/streaming-server-rendering-at-spectrum)允許您以塊的形式發送HTML，瀏覽器可以在接收時逐步呈現。這可以提供快速的First Paint和First Contentful Paint，因為標記更快地到達用戶。在React中，在[renderToNodeStream（）中](https://reactjs.org/docs/react-dom-server.html#rendertonodestream)異步的流 - 與同步renderToString相比 - 意味著可以很好地處理背壓。

進步補液也值得關注，React一直在[探索](https://github.com/facebook/react/pull/14717) 。使用這種方法，服務器呈現的應用程序的各個部分隨著時間的推移而被“啟動”，而不是一次初始化整個應用程序的當前常用方法。這可以幫助減少使頁面交互所需的JavaScript量，因為可以延遲頁面的低優先級部分的客戶端升級以防止阻塞主線程。它還可以幫助避免最常見的SSR Rehydration陷阱之一，其中服務器呈現的DOM樹被破壞然後立即重建 - 通常是因為初始同步客戶端渲染所需的數據還沒有完全準備好，可能還在等待Promise解析度。

### 部分補液{: #partial-rehydration }

部分補液已證明難以實施。該方法是漸進式再水合的概念的擴展，其中分析逐漸再水化的各個部分（組分/視圖/樹）並且識別具有很小交互性或無反應性的那些。對於這些大多數靜態部分中的每一個，相應的JavaScript代碼然後被轉換為惰性引用和裝飾功能，將其客戶端佔用空間減少到接近零。部分補水方法伴隨著自身的問題和妥協。它為緩存帶來了一些有趣的挑戰，而客戶端導航意味著我們無法假設應用程序的惰性部分的服務器呈現的HTML將在沒有完整頁面加載的情況下可用。

### 三次渲染{: #trisomorphic }

如果[服務工作者](https://developers.google.com/web/fundamentals/primers/service-workers/)是您的選擇，“三體”渲染也可能是有意義的。這是一種技術，您可以將流服務器呈現用於初始/非JS導航，然後讓您的服務工作者在安裝後為其導航呈現HTML。這可以使緩存的組件和模板保持最新，並啟用SPA樣式導航以在同一會話中呈現新視圖。當您可以在服務器，客戶端頁面和服務工作者之間共享相同的模板和路由代碼時，此方法最有效。

<img src="../../images/2019/02/rendering-on-the-web/trisomorphic.png" alt="Diagram of Trisomorphic rendering, showing a browser and service worker
communicating with the server">

## SEO注意事項{: #seo }

在選擇在網絡上呈現的策略時，團隊通常會考慮SEO的影響。通常選擇服務器呈現來提供爬蟲可以輕鬆解釋的“完整外觀”體驗。爬蟲[可能會理解JavaScript](https://web.dev/discoverable/how-search-works) ，但是在呈現它們的方式方面通常存在值得注意的[限制](/search/docs/guides/rendering) 。客戶端渲染可以工作，但往往沒有額外的測試和腿部工作。如果您的架構受客戶端JavaScript的嚴重驅動，最近[動態渲染](/search/docs/guides/dynamic-rendering)也成為值得考慮的選擇。

如果有疑問， [移動友好測試](https://search.google.com/test/mobile-friendly)工具對於測試您所選擇的方法是否符合您的預期非常寶貴。它顯示了Google抓取工具顯示任何頁面的方式預覽，找到的序列化HTML內容（執行JavaScript後）以及渲染過程中遇到的任何錯誤。

<img src="../../images/2019/02/rendering-on-the-web/mobile-friendly-test.png" alt="Screenshot of the Mobile Friendly Test UI">

## 結束...... {: #wrapup }

在決定渲染方法時，測量並了解您的瓶頸是什麼。考慮靜態渲染或服務器渲染是否可以使您獲得90％的方式。主要使用最少的JS發布HTML以獲得交互式體驗是完全可以的。這是一個方便的信息圖，顯示了服務器 - 客戶端頻譜：

<img src="../../images/2019/02/rendering-on-the-web/infographic.png" alt="Infographic showing the spectrum of options described in this article">

## 積分{: #credits }

感謝大家的評論和靈感：

Jeffrey Posnick，Houssein Djirdeh，Shubhie Panicker，Chris Harrelson和SebastianMarkbåge

<div class="clearfix"></div>

{% include "web/_shared/helpful.html" %}

{% include "web/_shared/rss-widget-updates.html" %}
