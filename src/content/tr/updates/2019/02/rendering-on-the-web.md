project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"

{# wf_updated_on: 2019-08-27 #} {# wf_published_on: 2019-02-06 #} {# wf_tags:
fundamentals, performance, app-shell #} {# wf_featured_image:
/web/updates/images/2019/02/rendering-on-the-web/icon.png #} {#
wf_featured_snippet: Where should we implement logic and rendering in our
applications? Should we use Server Side Rendering? What about Rehydration? Let's
find some answers! #} {# wf_blink_components: N/A #}

# Web'de İşleme {: .page-title }

{% include "web/_shared/contributors/developit.html" %} {% include
"web/_shared/contributors/addyosmani.html" %}

Geliştiriciler olarak, uygulamalarımızın tüm mimarisini etkileyecek kararlarla
sık sık karşılaşıyoruz. Web geliştiricilerinin alması gereken temel kararlardan
biri, uygulamalarında mantığı ve görüntü oluşturmayı nerede
gerçekleştirecekleridir. Bir web sitesi oluşturmanın birkaç farklı yolu
olduğundan, bu zor olabilir.

Bu alandaki anlayışımız, son birkaç yıl içinde büyük sitelerle konuşan
Chrome'daki çalışmamızla bildiriliyor. Genel olarak, geliştiricilerin sunucu
oluşturma veya statik oluşturma işlemlerini tam bir rehidrasyon yaklaşımı
üzerinde düşünmelerini teşvik ederiz.

Bu kararı verirken seçtiğimiz mimarileri daha iyi anlamak için, onlar hakkında
konuşurken her bir yaklaşımı ve tutarlı bir terminolojiyi anlamamız gerekir. Bu
yaklaşımlar arasındaki farklılıklar, performans merceğinden web’de oluşturulmuş
takasların gösterilmesine yardımcı olur.

## Terminoloji {: #terminology }

**Rendering**

- **SSR:** Sunucu Tarafı Oluşturma - istemci tarafında veya evrensel bir
uygulamayı sunucudaki HTML'ye oluşturma.
- **CSR:** Client-Side Rendering - genellikle DOM kullanarak bir tarayıcıda
uygulama oluşturma.
- **Rehidrasyon:** “önyükleme” İstemcideki JavaScript görünümlerini, sunucu
tarafından oluşturulan HTML’nin DOM ağacını ve verilerini yeniden kullanacak
şekilde.
- **Ön Hazırlık:** başlangıç halini statik HTML olarak yakalamak için bir
istemci tarafı uygulamayı derleme zamanında çalıştırma.

**performans**

- **TTFB:** İlk **bayta** kadar süre - bir linke tıklanıp gelen içeriğin ilk
kısmı arasında geçen süre olarak görülür.
- **FP:** İlk Boya - herhangi bir piksel ilk aldığında kullanıcı tarafından
görülür hale gelir.
- **FCP:** First Contentful Paint - istenen içeriğin (makale gövdesi vb.)
Göründüğü süre.
- **TTI:** Interaktif Süre - bir sayfanın interaktif hale geldiği saat (olaylar
kablolu vb.).

## Sunucu Oluşturma {: #server-rendering }

*Sunucu oluşturma, navigasyona yanıt olarak sunucudaki bir sayfa için tam HTML
oluşturur. Bu, tarayıcı yanıt vermeden önce ele alındığından, istemcide veri
toplama ve ayarlamaya yönelik ek yolculuklardan kaçınır.*

Sunucu oluşturma genellikle hızlı bir [İlk
Boya](https://developers.google.com/web/fundamentals/performance/user-centric-performance-metrics#first_paint_and_first_contentful_paint)
(FP) ve [İlk İçerikli
Boya](https://developers.google.com/web/fundamentals/performance/user-centric-performance-metrics#first_paint_and_first_contentful_paint)
(FCP) üretir. Sayfa mantığını çalıştırma ve sunucuda oluşturma, istemciye çok
fazla JavaScript göndermekten kaçınmayı mümkün kılar; bu [da
Etkileşimli](https://developers.google.com/web/tools/lighthouse/audits/time-to-interactive)
(TTI)
[için](https://developers.google.com/web/tools/lighthouse/audits/time-to-interactive)
hızlı bir
[zaman](https://developers.google.com/web/tools/lighthouse/audits/time-to-interactive)
elde etmenize yardımcı olur. Bu mantıklı, çünkü sunucu oluşturma ile gerçekten
sadece kullanıcının tarayıcısına metin ve link gönderiyorsunuz. Bu yaklaşım,
geniş bir cihaz ve ağ koşulları yelpazesi için iyi çalışabilir ve akışta belge
ayrıştırma gibi ilginç tarayıcı optimizasyonları açar.

<img src="../../images/2019/02/rendering-on-the-web/server-rendering-tti.png"
alt="Diagram showing server rendering and JS execution affecting FCP and TTI"
width="350">

Sunucu oluşturma ile, kullanıcıların sitenizi kullanmadan önce CPU'ya bağlı
JavaScript'in işlenmesini beklemesine izin verilmemektedir. [Üçüncü taraf
JS’den](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/loading-third-party-javascript/)
kaçınılmasa bile, kendi birinci taraf [JS
maliyetlerinizi](https://medium.com/@addyosmani/the-cost-of-javascript-in-2018-7d8950fbb5d4)
azaltmak için sunucu oluşturma özelliğini kullanmak size geri kalanı için daha
fazla "
[bütçe](https://medium.com/@addyosmani/start-performance-budgeting-dabde04cf6a3)
" sağlayabilir. Bununla birlikte, bu yaklaşımın bir ana dezavantajı vardır:
sunucuda sayfa oluşturmak zaman alır, bu da genellikle [İlk
Bayt](https://en.wikipedia.org/wiki/Time_to_first_byte) (TTFB)
[için](https://en.wikipedia.org/wiki/Time_to_first_byte) daha yavaş bir
[Zamana](https://en.wikipedia.org/wiki/Time_to_first_byte) neden olabilir.

Sunucunuz için uygulamanın yeterli olup olmadığı, ne tür bir deneyim
geliştirdiğinize bağlıdır. Sunucu tarafı oluşturma yerine istemci tarafı
oluşturma uygulamaları üzerinde uzun zamandır devam eden bir tartışma var, ancak
bazılarında sunucu oluşturma özelliğini kullanmayı tercih edebileceğinizi
hatırlamak önemlidir. Bazı siteler başarıyla hibrit render tekniklerini
benimsemiştir.
[Netflix](https://medium.com/dev-channel/a-netflix-web-performance-case-study-c0bcde26a9d9)
sunucusu göreceli olarak statik açılış sayfalarını oluşturur,
[JS'yi](https://dev.to/addyosmani/speed-up-next-page-navigations-with-prefetching-4285)
etkileşimli ağır sayfalar için [önceden
seçerek](https://dev.to/addyosmani/speed-up-next-page-navigations-with-prefetching-4285)
, bu daha ağır müşteri tarafından oluşturulan sayfalara hızlı bir şekilde
yükleme yapma şansı verir.

Birçok modern çerçeve, kütüphane ve mimarisi, aynı uygulamanın hem müşteri hem
de sunucu üzerinde yapılmasını mümkün kılar. Bu teknikler Sunucu Oluşturma için
kullanılabilir, ancak sunucuda ***ve*** istemcide görüntü oluşturma işleminin
gerçekleştiği mimarilerin, çok farklı performans özelliklerine ve değişimlere
sahip kendi çözüm sınıfları olduğunu belirtmek önemlidir. React kullanıcıları,
sunucu oluşturma için [Next.js](https://nextjs.org) gibi
[oluşturulmuş](https://nextjs.org) [renderToString
()](https://reactjs.org/docs/react-dom-server.html) veya çözümlerini
kullanabilir. Vue kullanıcıları Vue'nin [sunucu oluşturma
kılavuzuna](https://ssr.vuejs.org) veya [Nuxt'a bakabilir](https://nuxtjs.org) .
Açısal [Evrensel vardır](https://angular.io/guide/universal) . Çoğu popüler
çözüm, yine de bir miktar hidrasyon kullanır, bu nedenle bir araç seçmeden önce
kullanılan yaklaşımın farkında olun.

## Statik İşleme {: #static-rendering }

[Statik görüntü](https://frontarm.com/articles/static-vs-server-rendering/)
oluşturma anında gerçekleşir ve müşteri tarafındaki JS miktarının sınırlı
olduğunu varsayarak hızlı bir İlk Boyama, İlk İçerikli Boyama ve Etkileşimli
(Time To Interactive) sunar. Sunucu Oluşturmanın aksine, aynı zamanda hızlı bir
şekilde Time To First Byte elde etmeyi de başarır, çünkü bir sayfanın HTML'si
anında oluşturulmak zorunda değildir. Genel olarak, statik oluşturma, her bir
URL için önceden vaktinden ayrı bir HTML dosyası oluşturmak anlamına gelir.
Önceden oluşturulan HTML yanıtlarıyla, statik önbelleklemeler, kenar önbelleğe
almanın avantajlarından yararlanmak için birden fazla CDN'ye dağıtılabilir.

<img src="../../images/2019/02/rendering-on-the-web/static-rendering-tti.png"
alt="Diagram showing static rendering and optional JS execution affecting FCP
and TTI" width="280">

Statik görüntü oluşturma çözümleri tüm şekil ve boyutlarda gelir.
[Gatsby](https://www.gatsbyjs.org) gibi araçlar, geliştiricilerin,
uygulamalarını bir oluşturma adımı olarak oluşturulduğundan ziyade dinamik bir
şekilde oluşturulduğunu hissettirmek için tasarlanmıştır.
[Jekyl](https://jekyllrb.com) ve [Metalsmith](https://metalsmith.io) gibi
[diğerleri](https://jekyllrb.com) , statik [kalıplarını](https://metalsmith.io)
kucaklar ve daha şablon odaklı bir yaklaşım sunar.

Statik görüntü oluşturma işleminin olumsuz yanlarından biri, her bir URL için
ayrı ayrı HTML dosyalarının oluşturulması gerektiğidir. Bu URL’lerin önceden ne
olacağını tahmin edemediğinizde veya çok sayıda benzersiz sayfaya sahip siteler
için bu zor olabilir ve hatta olanaksız olabilir.

Kullanıcıların tepki
[vermesi](https://nextjs.org/learn/excel/static-html-export/)
[Gatsby](https://www.gatsbyjs.org) , [Next.js statik dışa
aktarımı](https://nextjs.org/learn/excel/static-html-export/) veya [Navi
ile](https://frontarm.com/navi/)
[tanışıyor](https://nextjs.org/learn/excel/static-html-export/) olabilir -
bunların hepsi bileşenleri kullanarak
[yazmayı](https://nextjs.org/learn/excel/static-html-export/) kolaylaştırır.
Bununla birlikte, statik oluşturma ve ön işleme arasındaki farkı anlamak
önemlidir: statik olarak işlenen sayfalar, çok fazla müşteri tarafı JS
çalıştırmaya gerek kalmadan etkileşimlidir; oysa ön işleme, önyüklenmesi gereken
bir Tek Sayfa Uygulamasının İlk Boyasını veya İlk Çekişmeli Boyasını geliştirir
Sayfaların gerçekten etkileşimli olması için müşteri.

Belirli bir çözümün statik renderleme mi yoksa önleme mi olduğundan emin
değilseniz, bu testi deneyin: JavaScript'i devre dışı bırakın ve oluşturulan web
sayfalarını yükleyin. Statik olarak oluşturulmuş sayfalar için, işlevlerin çoğu
JavaScript etkinleştirilmeden devam edecektir. Önceden hazırlanmış sayfalar
için, linkler gibi bazı temel işlevler olabilir, ancak sayfanın çoğu inert olur.

Diğer bir yararlı test, Chrome DevTools'u kullanarak ağınızı yavaşlatmak ve bir
sayfa etkileşime girmeden önce ne kadar JavaScript'in indirildiğini
gözlemlemektir. Önceden oluşturma, etkileşimli olmak için genellikle daha fazla
JavaScript gerektirir ve JavaScript'in statik görüntülemenin kullandığı [Aşamalı
Geliştirme](https://developer.mozilla.org/en-US/docs/Glossary/Progressive_Enhancement)
yaklaşımından daha karmaşık olma eğilimindedir.

## Sunucu İşleme vs Statik İşleme {: #server-vs-static }

Sunucu oluşturma gümüş bir kurşun değildir - dinamik yapısı [önemli
hesaplama](https://medium.com/airbnb-engineering/operationalizing-node-js-for-server-side-rendering-c5ba718acfc9)
maliyetleri ile birlikte gelebilir. Pek çok sunucu oluşturma çözümü erken yıkama
yapmaz, TTFB'yi geciktirebilir veya gönderilen verileri ikiye katlayabilir (örn.
İstemcide JS tarafından kullanılan satır içi durumu). React'te, renderToString
() senkronize ve tek iş parçacıklı olduğu için yavaş olabilir. Sunucu oluşturma
işlemini "doğru" yapmak, [bileşen önbelleğe
alma](https://medium.com/@reactcomponentcaching/speedier-server-side-rendering-in-react-16-with-component-caching-e8aa677929b1)
, bellek tüketimini yönetme,
[notlandırma](https://speakerdeck.com/maxnajim/hastening-react-ssr-with-component-memoization-and-templatization)
tekniklerini uygulama ve diğer pek çok kaygı için bir çözüm bulma veya
oluşturmayı içerebilir. Genellikle aynı uygulamayı birden çok kez işliyor /
yeniden oluşturuyorsunuz - bir kez istemcide ve bir kez de sunucuda. Sadece
sunucu oluşturma işleminin bir şeyi daha erken gösterebilmesi, aniden yapacak
daha az işiniz olduğu anlamına gelmez.

Sunucu oluşturma, her URL için isteğe bağlı HTML üretir, ancak yalnızca statik
olarak işlenen içerik sunmaktan daha yavaş olabilir. Ek çalışma alanı
ekleyebilirseniz, sunucu oluşturma + [HTML önbelleğe alma
işlemi](https://freecontent.manning.com/caching-in-react/) sunucu oluşturma
süresini büyük ölçüde azaltabilir. Sunucunun tersine çevrilmesi, daha fazla
"canlı" veri çekme ve statik işleme ile mümkün olandan daha eksiksiz bir istek
grubuna yanıt verme yeteneğidir. Kişiselleştirme gerektiren sayfalar, statik
görüntülemeyle iyi çalışmayan istek türünün somut bir örneğidir.

Sunucu oluşturma ayrıca bir [PWA
oluştururken](https://developers.google.com/web/progressive-web-apps/) ilginç
kararlar verebilir. Tam sayfa [hizmet
çalışanını](https://developers.google.com/web/fundamentals/primers/service-workers/)
önbelleğe almak veya yalnızca içeriği tek tek işlemek için kullanmak daha mı
iyidir?

## İstemci Tarafında İşleme (CSR) {: #csr }

*İstemci tarafı oluşturma (CSR), JavaScript'i kullanarak sayfaları doğrudan
tarayıcıda oluşturma anlamına gelir. Tüm mantık, veri toplama, şablon oluşturma
ve yönlendirme sunucudan ziyade istemcide ele alınır.*

Müşteri tarafında görüntü oluşturma, mobil cihazlar için hızlı bir şekilde elde
edilebilinir. En az çalışma yapıldığında, [sıkı bir JavaScript
bütçesini](https://mobile.twitter.com/HenrikJoreteg/status/1039744716210950144)
koruyarak ve mümkün olan en az
[RTT'de](https://en.wikipedia.org/wiki/Round-trip_delay_time) değer sağlayarak,
saf sunucu oluşturma performansına yaklaşabilir. Kritik komut dosyaları ve
veriler, [HTTP / 2 Server
Push](https://www.smashingmagazine.com/2017/04/guide-http2-server-push/) veya
`<link rel=preload>` kullanılarak daha hızlı bir şekilde teslim edilebilir; bu,
ayrıştırıcının sizin için daha erken çalışmasını sağlar.
[PRPL](https://developers.google.com/web/fundamentals/performance/prpl-pattern/)
gibi
[desenler](https://developers.google.com/web/fundamentals/performance/prpl-pattern/)
, başlangıçtaki ve sonraki gezinmelerin anında hissetmesini sağlamak için
değerlendirmeye değer.

<img src="../../images/2019/02/rendering-on-the-web/client-rendering-tti.png"
alt="Diagram showing client-side rendering affecting FCP and TTI" width="500">

Müşteri Tarafındaki Rendering'in birincil dezavantajı, bir uygulama büyüdükçe
gereken JavaScript miktarının artma eğiliminde olmasıdır. Bu, özellikle işlem
gücü için rekabet eden ve bir sayfanın içeriği oluşturulmadan önce sıklıkla
işlenmesi gereken yeni JavaScript kitaplıklarının, çoklu dolguların ve üçüncü
taraf kodlarının eklenmesiyle zorlaşır. Büyük JavaScript paketlerine dayanan CSR
ile oluşturulmuş deneyimler [agresif kod
bölmeyi](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/code-splitting/)
göz önünde bulundurmalı ve JavaScript'i tembel olarak yüklemeyi sağlamalıdır -
"yalnızca ihtiyacınız olduğunda ihtiyacınız olana hizmet edin". Etkileşim az
veya hiç olmayan deneyimler için, sunucu oluşturma bu sorunlara daha
ölçeklenebilir bir çözüm sunabilir.

Tek Sayfa Uygulaması yapan kişiler için, çoğu sayfa tarafından paylaşılan
Kullanıcı Arayüzünün ana bölümlerini belirlemek, [Uygulama Kabuğu önbelleğe
alma](https://developers.google.com/web/updates/2015/11/app-shell) tekniğini
uygulayabileceğiniz anlamına gelir. Servis çalışanları ile birlikte, bu tekrar
ziyaretlerinde algılanan performansı önemli ölçüde artırabilir.

## Sunucuyu ve CSR'yi rehidrasyon yoluyla birleştirme {: #rehydration }

Çoğunlukla Evrensel Rendering veya basitçe “SSR” olarak adlandırılır, bu
yaklaşım, Müşteri Tarafı Rendering ve Sunucu Rendering arasındaki takas
işlemlerini her ikisini de yaparak düzeltmeye çalışır. Tam sayfa yükleme veya
yeniden yükleme gibi gezinme istekleri, uygulamayı HTML'ye veren bir sunucu
tarafından gerçekleştirilir, daha sonra JavaScript ve oluşturma için kullanılan
veriler elde edilen belgeye gömülür. Dikkatle uygulandığında, bu, Sunucu
Rendering gibi hızlı bir İlk İçerikli Boya elde eder, daha sonra [(yeniden)
hidrasyon](https://docs.electrode.io/guides/general/server-side-data-hydration)
adı verilen bir teknik kullanarak müşteriye tekrar işleyerek "alır". Bu yeni bir
çözüm, ancak bazı önemli performans dezavantajları olabilir.

Rehidrasyonlu SSR'nin temel olumsuz tarafı, First Paint'i geliştirse bile Time
To Interactive üzerinde önemli bir olumsuz etkiye sahip olabileceğidir. SSR'd
sayfaları genellikle aldatıcı bir şekilde yüklü ve etkileşimli görünür, ancak
istemci tarafı JS yürütülene ve olay işleyicileri ekleninceye kadar girdilere
cevap veremez. Bu, mobil cihazlarda saniyeler hatta dakikalar sürebilir.

Belki de bunu kendiniz deneyimlediniz - bir sayfanın yüklendiği göründükten bir
süre sonra, tıklamak veya tıklamak hiçbir şey yapmaz. Bu hızla sinir bozucu
*oluyor* ... *“Neden hiçbir şey olmuyor? Neden kaydırma yapamıyorum? ”*

### Bir Rehidrasyon Sorunu: İki Fiyata Bir Uygulama {: #rehydration-issues }

Rehidrasyon sorunları genellikle JS nedeniyle gecikmeli etkileşimden daha kötü
olabilir. İstemci tarafı JavaScript'in, sunucunun HTML'yi oluşturmak için
kullandığı tüm verileri yeniden istemek zorunda kalmadan sunucunun kaldığı
yerden “alabilmesi” için, mevcut SSR çözümleri genellikle bir kullanıcı
arabiriminden gelen yanıtları seri hale getirir. belgeye veri bağımlılıkları ile
kod etiketleri. Sonuçta ortaya çıkan HTML belgesi, yüksek düzeyde çoğaltma
içerir:

<img src="../../images/2019/02/rendering-on-the-web/html.png" alt="HTML document
containing serialized UI, inlined data and a bundle.js script">

Gördüğünüz gibi, sunucu bir gezinme isteğine yanıt olarak uygulamanın kullanıcı
arayüzünün bir açıklamasını döndürüyor, ancak aynı zamanda söz konusu kullanıcı
arayüzünü oluşturmak için kullanılan kaynak verileri ve daha sonra istemcide
önyüklenen kullanıcı arayüzünün tam bir kopyasını döndürüyor . Sadece bundle.js
yüklendikten ve çalıştırıldıktan sonra bu kullanıcı arayüzü etkileşimli hale
gelir.

SSR rehidrasyonunu kullanarak gerçek web sitelerinden toplanan performans
ölçütleri, kullanımının büyük ölçüde teşvik edilmesi gerektiğini göstermektedir.
Sonuçta, bu sebep Kullanıcı Deneyimi'ne bağlı: Kullanıcıları “esrarengiz bir
vadide” bırakmak son derece kolaydır.

<img src="../../images/2019/02/rendering-on-the-web/rehydration-tti.png"
alt="Diagram showing client rendering negatively affecting TTI" width="600">

Yine de rehidrasyonlu SSR için umut var. Kısa vadede, yalnızca yüksek oranda
önbelleğe alınabilen içerik için SSR kullanmak, TTFB gecikmesini azaltabilir ve
ön işleme benzer sonuçlar üretebilir.
[Artımlı](https://www.emberjs.com/blog/2017/10/10/glimmer-progress-report.html)
, aşamalı veya kısmen rehidrate olmak, bu tekniği gelecekte daha uygulanabilir
hale getirmek için anahtar olabilir.

## Akışlı sunucu oluşturma ve Aşamalı Rehidrasyon {: #progressive-rehydration }

Sunucu oluşturma, son birkaç yıl içinde bir takım gelişmeler yaşamıştır.

[Akışlı sunucu
oluşturma](https://zeit.co/blog/streaming-server-rendering-at-spectrum) ,
tarayıcının alındığı şekilde aşamalı olarak oluşturabildiği topaklarda HTML
göndermenizi sağlar. Bu, işaretleme kullanıcılara daha hızlı ulaştığında hızlı
bir İlk Boya ve İlk İçerikli Boya sağlayabilir.
[React'te](https://reactjs.org/docs/react-dom-server.html#rendertonodestream) ,
akışlar [renderToNodeStream ()
'de](https://reactjs.org/docs/react-dom-server.html#rendertonodestream) - zaman
uyumlu renderToString ile karşılaştırıldığında - zaman uyumsuzdur - geri
basıncın iyi işlendiği anlamına gelir.

İlerici rehidrasyon da göz önünde bulundurmaya değer ve React'in [araştırdığı
bir](https://github.com/facebook/react/pull/14717) şey var. Bu yaklaşımla,
sunucu tarafından oluşturulan bir uygulamanın ayrı parçaları, tüm uygulamanın
bir kerede başlatılmasının mevcut ortak yaklaşımından ziyade, zaman içinde
“önyüklenir”. Bu, sayfaları etkileşimli hale getirmek için gereken JavaScript
miktarını azaltmaya yardımcı olabilir, çünkü sayfanın düşük öncelikli
bölümlerinin istemci tarafında yükseltilmesi, ana iş parçacığının engellenmesini
önlemek için ertelenebilir. Ayrıca, sunucu tarafından oluşturulan bir DOM
ağacının imha edildiği ve ardından derhal yeniden oluşturulduğu en yaygın SSR
Rehidrasyon tuzaklarından birinin engellenmesine de yardımcı olabilir - çoğu
zaman, ilk senkronize istemci tarafı, oldukça hazır olmayan, belki de Promise
bekleyen verileri gerekli kılarken çözüm.

### Kısmi Rehidrasyon {: #partial-rehydration }

Kısmi rehidrasyonun uygulanmasının zor olduğu kanıtlanmıştır. Bu yaklaşım,
aşamalı olarak rehidre edilecek olan tek tek parçaların (bileşenler / görünümler
/ ağaçlar) analiz edildiği ve etkileşimi çok az olan veya hiç reaktivitesi
olmayanların tanımlandığı, ilerici rehidrasyon fikrinin bir uzantısıdır.
Çoğunlukla statik olan bu parçaların her biri için, karşılık gelen JavaScript
kodu daha sonra atıl referanslara ve dekoratif işlevlere dönüştürülerek, istemci
tarafı ayak izlerini sıfıra düşürür. Kısmi hidrasyon yaklaşımı kendi sorunları
ile gelir ve ödün verir. Önbellekleme için bazı ilginç zorluklar ortaya
koymaktadır ve istemci tarafı navigasyonu, uygulamanın atıl bölümleri için tam
sayfa yükü olmadan kullanıma sunulacağı için sunucu tarafından oluşturulan
HTML'yi varsaymayacağımız anlamına gelir.

### Trisomorfik Rendering {: #trisomorphic }

[Servis çalışanları
sizin](https://developers.google.com/web/fundamentals/primers/service-workers/)
için bir seçenekse, “trisomorfik” renderleme de ilgi çekici olabilir. İlk / JS
olmayan gezinmeler için akış sunucusu oluşturmayı kullanabileceğiniz ve daha
sonra servis çalışanınızın yüklendikten sonra gezinmeler için HTML sunumu
yapmasını sağlayan bir tekniktir. Bu, önbelleğe alınmış bileşenleri ve
şablonları güncel tutabilir ve aynı oturumda yeni görünümler oluşturmak için SPA
tarzı gezinmeler sağlar. Bu yaklaşım, sunucu, istemci sayfası ve servis çalışanı
arasında aynı şablonlama ve yönlendirme kodunu paylaşabileceğiniz zaman en iyi
şekilde çalışır.

<img src="../../images/2019/02/rendering-on-the-web/trisomorphic.png"
alt="Diagram of Trisomorphic rendering, showing a browser and service worker
communicating with the server">

## SEO ile ilgili önemli noktalar {: #seo }

Ekipler, web’de oluşturulacak bir strateji seçerken, SEO’nun etkisinde
genellikle etkilidir. Sunucu oluşturma genellikle tarayıcıların kolaylıkla
yorumlayabileceği "tam görünümlü" bir deneyim sunmak için seçilir. Tarayıcılar
[JavaScript'i anlayabilir](https://web.dev/discoverable/how-search-works) ,
ancak genellikle nasıl işlediklerinin farkında olma konusunda bazı
[sınırlamalar](/search/docs/guides/rendering) vardır. Müşteri tarafı oluşturma,
ek testler ve ayak işleri olmadan çalışabilir ancak çoğu zaman çalışamaz. Daha
yakın zamanda, [dinamik görüntü
oluşturma](/search/docs/guides/dynamic-rendering) , mimarinizin ağır bir şekilde
istemci tarafı JavaScript tarafından yönlendirilip yönlendirilmediğini dikkate
almaya değer bir seçenek haline geldi.

Şüphe duyduğunuzda, [Mobil Dostu
Test](https://search.google.com/test/mobile-friendly) aracı seçtiğiniz
yaklaşımın beklediğiniz şeyi yaptığını test etmek için paha biçilmezdir.
Herhangi bir sayfanın Google’ın tarayıcısına nasıl göründüğünün, serileştirilmiş
HTML içeriğinin (JavaScript çalıştırıldıktan sonra) ve oluşturma sırasında
karşılaşılan hataların görsel bir önizlemesini gösterir.

<img src="../../images/2019/02/rendering-on-the-web/mobile-friendly-test.png"
alt="Screenshot of the Mobile Friendly Test UI">

## Tamamlanıyor ... {: #wrapup }

Bir yaklaşıma karar verirken, darboğazlarınızın ne olduğunu ölçün ve anlayın.
Statik görüntü oluşturma veya sunucu oluşturma işleminin size buradaki yoldan%
90'ını getirip getiremeyeceğini düşünün. Etkileşimli bir deneyim elde etmek için
HTML'yi en az JS ile birlikte göndermek son derece iyidir. İşte sunucu-müşteri
spektrumunu gösteren kullanışlı bir infografik:

<img src="../../images/2019/02/rendering-on-the-web/infographic.png"
alt="Infographic showing the spectrum of options described in this article">

## Krediler {: #credits }

Yorum ve ilhamlarından dolayı herkese teşekkürler:

Jeffrey Posnick, Houssein Djirdeh, Shubhie Panicker, Chris Harrelson ve
Sebastian Markbåge

<div class="clearfix"></div>

{% include "web/_shared/helpful.html" %}

{% include "web/_shared/rss-widget-updates.html" %}
