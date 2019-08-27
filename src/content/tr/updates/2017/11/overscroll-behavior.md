project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"
description: CSS aşırı kaydırma davranışı özelliğine giriş.

{# wf_updated_on: 2019-08-26 #} {# wf_published_on: 2017-11-14 #}

{# wf_tags: chrome63,css,overscroll,scroll #} {# wf_blink_components: Blink>CSS
#} {# wf_featured_image:/web/updates/images/2017/11/overscroll-behavior/card.png
#} {# wf_featured_snippet: The CSS overscroll-behavior property allows
developers to override the browser's overflow scroll effects when reaching the
top/bottom of content. It can be used to customize or prevent the mobile
pull-to-refresh action. #}

# Kaydırma kontrolünüzü elinize alın: yenileme ve taşma efektlerini özelleştirme {: .page-title }

{% include "web/_shared/contributors/ericbidelman.html" %} {% include
"web/_shared/contributors/majidvp.html" %} {% include
"web/_shared/contributors/sunyunjia.html" %}

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

[CSS `overscroll-behavior`](https://wicg.github.io/overscroll-behavior/)
özelliği, geliştiricilerin, içeriğin tepesine / altına ulaştığında tarayıcının
varsayılan taşma kaydırma davranışını geçersiz kılmalarını sağlar. Kullanım
örnekleri arasında, mobil cihazda yenilemek için çekme özelliğinin devre dışı
bırakılması, aşırı kaydırılan ışıma ve lastik bantlama efektlerinin kaldırılması
ve sayfa içeriğinin bir modal / kaplamanın altındayken kaydırılmasını önleme
bulunur.

aşırı `overscroll-behavior` Chrome 63+ gerektirir. Geliştirilmekte veya diğer
tarayıcılar tarafından değerlendirilmektedir. Daha fazla bilgi için
[chromestatus.com
adresine](https://www.chromestatus.com/feature/5734614437986304) bakın. {:
.caution }

## Arka fon

### Kaydırma sınırları ve kaydırma zincirleme {: #scrollchaining }

<figure class="attempt-right">
<a href="/web/updates/images/2017/11/overscroll-behavior/drawer-scroll.mp4"
target="_blank">
<video
src="/web/updates/images/2017/11/overscroll-behavior/drawer-scroll.mp4" autoplay
loop muted alt="Drawer demo" height="300"></video>
  </a>
  <figcaption>Chrome Android'de zincirleme kaydırın.</figcaption>
</figure>

Kaydırma, bir sayfa ile etkileşime girmenin en temel yollarından biridir, ancak
tarayıcının ilginç varsayılan davranışları nedeniyle belirli UX kalıplarının
üstesinden gelmek zor olabilir. Örnek olarak, kullanıcının kaydırması
gerekebilecek çok sayıda öğeye sahip bir uygulama çekmecesini alın. Altlarına
geldiklerinde taşma kabı kaydırmayı durdurur çünkü tüketilecek başka içerik
yoktur. Başka bir deyişle, kullanıcı bir "kaydırma sınırına" ulaşır. Ancak,
kullanıcı kaydırma yapmaya devam ederse ne olacağını fark edin. **Çekmecenin
*arkasındaki* içerik hareket etmeye başlar** ! Kaydırma, ana kap tarafından
devralınır; örnekte ana sayfanın kendisi.

Bu davranışa **kaydırma zincirleme** denir; içeriği kaydırırken tarayıcının
varsayılan davranışı. Çoğu zaman, varsayılan değer oldukça iyidir, ancak bazen
bu istenmez ve hatta beklenmeyen bir durum değildir. Bazı uygulamalar, kullanıcı
bir kaydırma sınırına ulaştığında farklı bir kullanıcı deneyimi sağlamak
isteyebilir.

### Yenilemek için çekme efekti {: #p2r }

Yenile-çek, Facebook ve Twitter gibi mobil uygulamalar tarafından popüler olan
sezgisel bir jest. Bir sosyal yayını aşağı çekmek ve bırakmak, daha yeni
yayınların yüklenmesi için yeni bir alan yaratır. Aslında, bu belirli UX o
*kadar popüler* hale geldi *ki* , Android'deki Chrome gibi mobil tarayıcılar da
aynı etkiye sahipti. Sayfanın üst kısmından aşağı doğru kaydırmak tüm sayfayı
yeniler:

<div class="clearfix centered">
  <figure class="attempt-left">
<a href="/web/updates/images/2017/11/overscroll-behavior/twitter.mp4"
target="_blank">
<video src="/web/updates/images/2017/11/overscroll-behavior/twitter.mp4"
autoplay muted loop height="350" class="border"></video>
    </a>
<figcaption>Twitter'ın özel yenileme-yenileme <br> PWA'larında bir beslemeyi
yenilerken.</figcaption>
  </figure>
  <figure class="attempt-right">
<a href="/web/updates/images/2017/11/overscroll-behavior/mobilep2r.mp4"
target="_blank">
<video
src="/web/updates/images/2017/11/overscroll-behavior/mobilep2r.mp4" autoplay
muted loop height="350" class="border"></video>
    </a>
<figcaption>Chrome Android'in yerel yenileme-yenileme eylemi <br> tüm
sayfayı yeniler.</figcaption>
  </figure>
</div>

Twitter [PWA](/web/progressive-web-apps/) gibi durumlar için, yerel yenileme
için yenileme eylemini devre dışı bırakmak mantıklı olabilir. Niye ya? Bu
uygulamada, kullanıcının yanlışlıkla sayfayı yenilemesini istemiyorsunuz. Çift
yenileme animasyonu görme potansiyeli de var! Alternatif olarak, tarayıcının
eylemini özelleştirmek, sitenin markalamasına daha yakından hizalamak daha iyi
olabilir. Talihsiz kısım, bu tür bir özelleştirmenin kaldırılmasının zor olduğu.
Geliştiriciler gereksiz JavaScript yazıyor, [pasif
olmayan](/web/tools/lighthouse/audits/passive-event-listeners) dokunmatik
dinleyiciler ekliyor (kaydırmayı engelleyen) veya tüm sayfayı 100vw / vh `<div>`
(sayfanın taşmasını önlemek için) içine yapıştırıyorlar. Bu geçici çözümlerin
kaydırma performansı üzerinde [iyi
belgelenmiş](https://wicg.github.io/overscroll-behavior/#intro) olumsuz etkileri
vardır.

Daha iyisini yapabiliriz!

## `overscroll-behavior` {: #intro }

Aşırı `overscroll-behavior`
[özelliği](https://wicg.github.io/overscroll-behavior/) , bir kabı fazla
`overscroll-behavior` ne olacağının davranışını kontrol eden yeni bir CSS
özelliğidir (sayfanın kendisi de dahil). Kaydırma zincirini iptal etmek,
yenilemek için çekme işlemini devre dışı bırakmak / özelleştirmek, iOS
üzerindeki lastik bantlama efektlerini devre dışı bırakmak (Safari
`overscroll-behavior` uyguladığında) ve daha fazlasını kullanabilirsiniz. En iyi
bölüm, <strong data-md-type="double_emphasis">`overscroll-behavior`
kullanmanın</strong> , giriş bölümünde belirtilen saldırılar gibi **sayfa
performansını olumsuz yönde etkilememesidir** !

Özellik üç olası değer alır:

1. **otomatik** - Varsayılan. Öğeden kaynaklanan kaydırmalar ata öğelerine
yayılabilir.

- **içerir** - kaydırma zincirini önler. Scroll'lar atalara yayılmaz, ancak
düğüm içindeki yerel efektler gösterilir. Örneğin, Android'de aşırı kaydırma
parlama efekti veya bir kaydırma sınırına ulaştığında kullanıcıyı uyaran
iOS'taki lastik bantlama efekti. **Not** : `overscroll-behavior: contain` `html`
öğesinde bulunan, overcroll gezinme eylemlerini önler.
- **hiçbiri** - `contain` aynıdır ancak aynı zamanda düğümün içindeki aşırı
kaydırma etkilerini de önler (örneğin, Android aşırı kaydırma parlaklığı veya
iOS lastik bantlaması).

Not: `overscroll-behavior` , yalnızca belirli bir eksen için davranış tanımlamak
istiyorsanız, `overscroll-behavior-x` ve `overscroll-behavior-y` kısayollarını
da destekler.

`overscroll-behavior` nasıl kullanılacağını görmek için bazı örneklere
`overscroll-behavior` .

## Kaydırmaların {: #fixedpos } sabit konumlu bir öğeden kaçmasını önleme

### Sohbet kutusu senaryosu {: #chat }

<figure class="attempt-right">
<a href="/web/updates/images/2017/11/overscroll-behavior/chatbox-chaining.mp4"
target="_blank">
<video
src="/web/updates/images/2017/11/overscroll-behavior/chatbox-chaining.mp4"
autoplay muted loop alt="Chatbox demo" height="350" class="border"></video>
  </a>
  <figcaption>Sohbet penceresinin altındaki içerik de kaydırılır :(</figcaption>
</figure>

Sayfanın altında yer alan sabit konumlu bir sohbet kutusu düşünün. Amaç, sohbet
kutusunun kendi kendine yeten bir bileşen olması ve arkasındaki içerikten ayrı
olarak kaydırılmasıdır. Ancak, kaydırma zincirlemesi nedeniyle, kullanıcı sohbet
geçmişindeki son mesaja ulaşır ulaşmaz belge kaydırmaya başlar.

Bu uygulama için, sohbet kutusundan çıkan kaydırmaların sohbet içinde kalması
daha uygun olur. Bunu, `overscroll-behavior: contain` ekleyerek sohbet
mesajlarını tutan öğeye ekleyerek yapabiliriz:

```css
#chat .msgs {
  overflow: auto;
  overscroll-behavior: contain;
  height: 300px;
}
```

Temel olarak, sohbet kutusunun kaydırma içeriği ve ana sayfa arasında mantıksal
bir ayrım yaratıyoruz. Sonuçta, kullanıcı sohbet geçmişinin en üstüne / altına
ulaştığında ana sayfanın sabit kalması sağlanır. Sohbet kutusunda başlayan
kaydırmalar yayılmaz.

### Sayfa yer paylaşımı senaryosu {: #overlay }

"Alt çizgi" senaryosunun bir başka varyasyonu, içeriğin **sabit bir konum
bindirmesinin** ardında kaydırıldığını görmenizdir. Ölü bir hediye
`overscroll-behavior` uygun! Tarayıcı yardımcı olmaya çalışıyor, ancak sitenin
buggy görünmesini sağlıyor.

**Örnek** - `overscroll-behavior: contain` olan ve olmayan modal
`overscroll-behavior: contain` :

<figure class="clearfix centered">
  <div class="attempt-left">
<a href="/web/updates/images/2017/11/overscroll-behavior/modal-off.mp4"
target="_blank">
<video src="/web/updates/images/2017/11/overscroll-behavior/modal-off.mp4"
autoplay muted loop height="290"></video>
    </a>
<figcaption><b>Önce</b> : sayfa içeriği kaplamanın altına
kayar.</figcaption>
  </div>
  <div class="attempt-right">
<a href="/web/updates/images/2017/11/overscroll-behavior/modal-on.mp4"
target="_blank">
<video src="/web/updates/images/2017/11/overscroll-behavior/modal-on.mp4"
autoplay muted loop height="290"></video>
    </a>
<figcaption><b>Sonra</b> : sayfa içeriği kaplamanın altına
kaydırılmıyor.</figcaption>
  </div>
</figure>

## Yenileme süresine devre dışı bırakma {: #disablp2r }

**Yenilemek için çekme eylemini kapatmak, tek bir CSS satırıdır** . Tüm görünüm
tanımlayıcı öğesinde kaydırma zincirini engellemeniz yeterli. Çoğu durumda, bu
`<html>` veya `<body>` :

```css
body {
  /* Disables pull-to-refresh but allows overscroll glow effects. */
  overscroll-behavior-y: contain;
}
```

Bu basit [eklemeyle](https://ebidel.github.io/demos/chatbox.html) ,
[sohbet](https://ebidel.github.io/demos/chatbox.html) kutusu
[demosundaki](https://ebidel.github.io/demos/chatbox.html) çift yenileme için
yenile animasyonları düzeltiriz ve bunun yerine daha düzgün yükleme animasyonu
kullanan özel bir efekt uygulayabiliriz. Gelen kutusunun tamamı, gelen kutunun
yenilenmesiyle de bulanıklaşır:

<figure class="clearfix centered">
  <div class="attempt-left">
<video
src="/web/updates/images/2017/11/overscroll-behavior/chatbox-double-refresh.mp4"
autoplay muted loop height="225"></video>
    <figcaption>Önce</figcaption>
  </div>
  <div class="attempt-right">
<video
src="/web/updates/images/2017/11/overscroll-behavior/chatbox-double-refresh-fix.mp4"
autoplay muted loop height="225"></video>
    <figcaption>Sonra</figcaption>
  </div>
</figure>

İşte [tam kodun](https://github.com/ebidel/demos/blob/master/chatbox.html) bir
parçası:

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

## Overcroll kızdırma ve lastik bantlama efektlerini devre dışı bırakma {: #disableglow }

Kaydırma sınırına çarptığında hemen çıkma efektini devre dışı bırakmak için
`overscroll-behavior-y: none` :

```css
body {
  /* Disables pull-to-refresh and overscroll glow effect.
     Still keeps swipe navigations. */
  overscroll-behavior-y: none;
}
```

<figure class="clearfix centered">
  <div class="attempt-left">
<video src="/web/updates/images/2017/11/overscroll-behavior/drawer-glow.mp4"
autoplay muted loop height="300" class="border"></video>
<figcaption><b>Önce</b> : kaydırma sınırına vurmak bir parlama
gösterir.</figcaption>
  </div>
  <div class="attempt-right">
<video
src="/web/updates/images/2017/11/overscroll-behavior/drawer-noglow.mp4" autoplay
muted loop height="300" class="border"></video>
    <figcaption><b>Sonra</b> : parlama devre dışı.</figcaption>
  </div>
</figure>

Not: Bu hala sol / sağ kaydırma hareketlerini koruyacaktır.
`overscroll-behavior-x: none` önlemek için `overscroll-behavior-x: none` .
Ancak, bu [hala](https://crbug.com/762023) Chrome'da
[uygulanmaktadır](https://crbug.com/762023) .

## Tam demo {: #demo }

Tümünü bir araya getirmek için tam [sohbet kutusu
demosu](https://ebidel.github.io/demos/chatbox.html) , `overscroll-behavior`
özel bir çekme animasyonu oluşturmak için `overscroll-behavior` kullanır ve
`overscroll-behavior` kutusu widget'ından kaçmasını engeller. Bu, CSS
`overscroll-behavior` olmadan elde edilmesi zor olabilecek en uygun kullanıcı
deneyimini sağlar.

<figure>
  <a href="https://ebidel.github.io/demos/chatbox.html" target="_blank">
<video
src="/web/updates/images/2017/11/overscroll-behavior/chatbox-fixed.mp4" autoplay
muted loop alt="Chatbox demo" height="600"></video>
  </a>
<figcaption><a href="https://ebidel.github.io/demos/chatbox.html"
target="_blank">Demoyu görüntüle</a> | <a
href="https://github.com/ebidel/demos/blob/master/chatbox.html"
target="_blank">Kaynak</a></figcaption>
</figure>

<br>
