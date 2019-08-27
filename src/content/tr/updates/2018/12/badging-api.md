project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"
description: Badging API, yüklü web uygulamalarının, raf veya ana ekran gibi uygulamaya
  bağlı işletim sistemine özgü bir yerde gösterilen, uygulama genelinde bir rozet
  belirlemesini sağlayan yeni bir web platformu API'sidir.

{# wf_published_on: 2018-12-11 #} {# wf_updated_on: 2019-08-21 #} {#
wf_featured_image: /web/updates/images/generic/notifications.png #} {# wf_tags:
capabilities,badging,install,progressive-web-apps,serviceworker,notifications,origintrials
#} {# wf_featured_snippet: The Badging API is a new web platform API that allows
installed web apps to set an application-wide badge, shown in an
operating-system-specific place associated with the application, such as the
shelf or home screen. Badging makes it easy to subtly notify the user that there
is some new activity that might require their attention, or it can be used to
indicate a small amount of information, such as an unread count. #} {#
wf_blink_components: UI>Browser>WebAppInstalls #}

{# When updating this post, don't forget to update /updates/capabilities.md #}

# Uygulama Simgeleri İçin Rozet {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

<div class="clearfix"></div>

<aside class="caution">Şu anda bu API üzerinde yeni <a
href="/web/updates/capabilities">yetenekler projesinin bir</a> parçası olarak
çalışıyoruz ve Chrome 73'ten başlayarak bir <a href="#ot"><b>deneme
sürümü</b></a> olarak mevcut. Bu yayın Badging API'sı geliştikçe
güncellenecektir. <br> <b>Son Güncelleme:</b> 21 Ağustos 2019</aside>

## Rozet API nedir? {: #what }

<figure class="attempt-right">
  <img src="/web/updates/images/2018/12/badges-on-windows.jpg">
<figcaption>8 bildirime sahip bir Twitter örneği ve bir bayrak tipi rozeti
gösteren başka bir uygulama.</figcaption>
</figure>

Badging API, yüklü web uygulamalarının, uygulama ile ilişkili işletim sistemine
özgü bir yerde (raf veya ana ekran gibi) gösterilen, uygulama genelinde bir
rozet ayarlamasına izin veren yeni bir web platformu API'sidir.

Rozetleme, kullanıcının dikkatini gerektirebilecek yeni bir etkinlik olduğunu
dikkatlice bildirmeyi kolaylaştırır veya okunmamış bir sayı gibi az miktarda
bilgiyi belirtmek için kullanılabilir.

Rozetler, bildirimlerden daha kullanıcı dostu olma eğilimindedir ve kullanıcıyı
kesintiye uğratmadıkları için çok daha yüksek bir sıklıkta güncellenebilir. Ve
kullanıcıyı rahatsız etmedikleri için, onları kullanmak için özel bir izin
gerekli değildir.

[Açıklayıcıyı oku](https://github.com/WICG/badging/blob/master/explainer.md) {:
.button .button-primary }

<div class="clearfix"></div>

### Badging API'sı için önerilen kullanım durumları {: #use-cases }

Bu API'yi kullanabilecek sitelere örnekler:

- Sohbet, e-posta ve sosyal uygulamalar, yeni mesajların geldiğini bildirmek
veya okunmamış öğelerin sayısını göstermek için.
- Verimlilik uygulamaları, uzun süredir devam eden bir arka plan görevinin
(görüntü veya video oluşturma gibi) tamamlandığını bildirmek için.
- Oyunlar, bir oyuncunun harekete geçmesi gerektiğini işaret etmek için
(örneğin, satranç sırası, oyuncunun sırası olduğunda).

## Mevcut durum {: #status }

Adım | durum
--- | ---
1. Açıklayıcı oluşturun | [Tamamlayınız](https://github.com/WICG/badging/blob/master/explainer.md)
2. İlk spesifikasyon taslağını hazırlayın | [Tamamlayınız](https://wicg.github.io/badging/)
**3. geribildirim toplamak ve tasarım üzerinde iterate** | [**Devam etmekte**](#feedback)
**4. Menşe deneme** | [**Devam etmekte**](#ot)
5. Başlat | Başlatılmadı

### İşlemde görün

1. Chrome 73 veya üstünü Windows veya Mac'te kullanarak [Badging API demosunu
açın](https://badging-api.glitch.me/) .
2. İstendiğinde, uygulamayı yüklemek için **Yükle'yi** tıklayın veya yüklemek
için Chrome menüsünü kullanın, ardından yüklü bir PWA'yı açın. Not, yüklü bir
PWA olarak çalışıyor olması gerekir (görev çubuğunuzda veya dock'ta).
3. Rozeti uygulama simgesinden ayarlamak veya silmek için **Ayarla** veya
**Temizle** düğmesini tıklayın. *Rozet değeri* için bir sayı da
sağlayabilirsiniz.

Not: *Chrome'daki* Badging API'sı *,* gerçekte kötüleştirilebilen bir simgeye
sahip yüklü bir uygulama gerektiriyor olsa da, Badging API'sini arama durumuna
bağlı olarak çağırmamanızı öneririz. Badging API, bir tarayıcının bir rozet
göstermek isteyebileceği *herhangi* bir *yere* uygulanabilir, bu nedenle
geliştiriciler tarayıcının hangi durumlarda rozetleri çalıştıracağı konusunda
herhangi bir varsayımda bulunmamalıdır. Sadece mevcut olduğunda API'yi arayın.
Çalışırsa çalışır. Olmazsa, basitçe yapmaz.

## Rozet API'si nasıl kullanılır {: #use }

Chrome 73'ten başlayarak, Badging API, Windows (7+) ve macOS için kaynak bir
deneme olarak kullanılabilir. [Menşe
denemeleri](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/README.md)
, yeni özellikleri denemenizi ve bize, web standartları topluluğundaki
kullanılabilirlik, pratiklik ve etkinlik hakkında geri bildirimde bulunmanızı
sağlar. Daha fazla bilgi [için, Web Geliştiricileri için Kaynak Deneme
Kılavuzuna
bakın](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md)
.

### Platformlar arasında yakalama desteği

Badging API, Windows ve macOS'ta (başlangıç sürümünde) desteklenir. Gelecekte
değişebilmesine rağmen, bir bildirim göstermenizi gerektirdiğinden Android
desteklenmiyor. Chrome OS desteği, platformda rozet uygulaması beklemektedir.

### Kaynak deneme için kaydolun {: #ot }

1.
[Menşeiniz](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)
için [bir simge
isteyin](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)
.
2. Belirteci sayfanıza ekleyin, bu belirteci kökeninizdeki herhangi bir sayfada
belirtmenin iki yolu vardır:
-  Herhangi bir sayfanın başına bir `origin-trial` `<meta>` etiketi ekleyin.
Örneğin, şuna benzer bir şey olabilir: `<meta http-equiv="origin-trial"
content="TOKEN_GOES_HERE">`
-  Sunucunuzu yapılandırabilirseniz, `Origin-Trial` HTTP üstbilgisi
kullanarak belirteci sayfalarda da sağlayabilirsiniz. Sonuçta ortaya çıkan yanıt
başlığı aşağıdaki gibi görünmelidir: `Origin-Trial: TOKEN_GOES_HERE`

### Kökeni deneme alternatifleri

Rozet API'sini yerel olarak denemek istiyorsanız, orijinal deneme olmadan,
`#enable-experimental-web-platform-features` bayrağını `chrome://flags` içinde
`#enable-experimental-web-platform-features` .

### Rozet deneme sırasında Rozet API'sini kullanma

Dogfood: Kökeni deneme sırasında API, `window.ExperimentalBadge` üzerinden
`window.ExperimentalBadge` . Aşağıdaki kod mevcut tasarıma dayanmaktadır ve
tarayıcıya standart bir API olarak gelmeden önce değişecektir.

Badging API'sini kullanmak için web uygulamanızın [Chrome'un yüklenebilirlik
ölçütlerini](/web/fundamentals/app-install-banners/#criteria) karşılaması ve bir
kullanıcının giriş ekranına eklemesi gerekir.

`ExperimentalBadge` arabirimi, `window` bir üye nesnesidir. İki yöntem içerir:

- `set([number])` : Uygulamanın rozetini ayarlar. Bir değer verilirse, rozeti
verilen değere ayarlayın, düz beyaz bir nokta (veya platforma uygun başka bir
bayrak) gösterin.
- `clear()` : Uygulamanın rozetini kaldırır.

```js
// In a web page
const unreadCount = 24;
window.ExperimentalBadge.set(unreadCount);
```

`ExperimentalBadge.set()` ve `ExperimentalBadge.clear()` ön plan sayfasından
veya gelecekte potansiyel olarak bir servis çalışanı olarak adlandırılabilir.
Her iki durumda da, sadece geçerli sayfayı değil, tüm uygulamayı etkiler.

Bazı durumlarda, işletim sistemi rozetin tam temsiline izin vermeyebilir, bu
durumda tarayıcı bu aygıt için en iyi gösterimi sağlamaya çalışacaktır. Örneğin,
Badging API Android'de desteklenmiyorken, Android yalnızca sayısal bir değer
yerine yalnızca bir nokta gösterir.

Not: Kullanıcı aracısının rozeti nasıl göstermek istediği hakkında hiçbir şey
düşünmeyin. Bazı kullanıcı temsilcilerinin "4000" gibi bir sayı almasını ve
"99+" olarak yeniden yazmasını bekliyoruz. Kendiniz doyurursanız (örneğin "99"
a), "+" görünmez. Gerçek sayı ne olursa olsun, sadece `Badge.set(unreadCount)`
ve kullanıcı temsilcisinin uygun şekilde göstermesi için uğraşmasına izin verin.

## Geribildirim {: #feedback }

Badging API'sinin gereksinimlerinizi karşılayacak şekilde çalıştığından ve
önemli senaryoları kaçırmadığımızdan emin olmak için yardımınıza ihtiyacımız
var.

<aside class="key-point"><b>Yardımına ihtiyacımız var!</b> - Mevcut tasarım (bir
tamsayı veya bayrak değerine izin veren) ihtiyaçlarınızı karşılayacak mı? <a
href="https://github.com/WICG/badging/issues">Olmazsa</a> , lütfen <a
href="https://github.com/WICG/badging/issues">WICG / badging repo'da</a> bir
sorun <a href="https://github.com/WICG/badging/issues">belirtin</a> ve mümkün
olduğunca ayrıntılı bilgi verin. Ek olarak, halen tartışılmakta olan bir dizi <a
href="https://github.com/WICG/badging/blob/master/choices.md">açık soru</a> var
ve görüşlerinizi duymak isteriz.</aside>

Badging API'sini nasıl kullanmayı planladığınızı da duymak isteriz:

- Bir kullanım durumu hakkında bir fikriniz veya nerede kullanacağınız hakkında
bir fikriniz mi var?
- Bunu kullanmayı düşünüyor musun?
- Beğen ve desteğini göstermek ister misin?

[Badging API WICG
Discourse](https://discourse.wicg.io/t/badging-api-for-showing-an-indicator-on-a-web-apps-shelf-icon/2900)
tartışması hakkındaki düşüncelerinizi paylaşın.

{% include "web/_shared/helpful.html" %}

## Faydalı Linkler {: #helpful }

- [Genel açıklayıcı](https://github.com/WICG/badging/blob/master/explainer.md)
- [Rozet API Demo](https://badging-api.glitch.me/) | [Rozet API Demo
kaynağı](https://glitch.com/edit/#!/badging-api?path=demo.js)
- [İzleme hata](https://bugs.chromium.org/p/chromium/issues/detail?id=719176)
- [ChromeStatus.com
girişi](https://www.chromestatus.com/features/6068482055602176)
- [Kökeni deneme
belirteci](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)
iste
- [Kökeni deneme belirteci nasıl
kullanılır](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md#how-do-i-enable-an-experimental-feature-on-my-origin)
- Blink Bileşen: `UI>Browser>WebAppInstalls`

{% include "web/_shared/rss-widget-updates.html" %}
