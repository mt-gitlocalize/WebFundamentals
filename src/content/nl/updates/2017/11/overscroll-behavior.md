project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"
description: Inleiding tot de eigenschap CSS overscroll-gedrag.

{# wf_updated_on: 2019-08-26 #} {# wf_published_on: 2017-11-14 #}

{# wf_tags: chrome63,css,overscroll,scroll #} {# wf_blink_components: Blink>CSS
#} {# wf_featured_image:/web/updates/images/2017/11/overscroll-behavior/card.png
#} {# wf_featured_snippet: The CSS overscroll-behavior property allows
developers to override the browser's overflow scroll effects when reaching the
top/bottom of content. It can be used to customize or prevent the mobile
pull-to-refresh action. #}

# Neem de controle over uw scroll: aanpassen van pull-to-refresh- en overflow-effecten {: .page-title }

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

Met de [`overscroll-behavior` CSS
`overscroll-behavior`](https://wicg.github.io/overscroll-behavior/) kunnen
ontwikkelaars het standaard overflow
[`overscroll-behavior`](https://wicg.github.io/overscroll-behavior/) de browser
overschrijven bij het bereiken van de boven- / onderkant van inhoud.
Gebruikscasussen omvatten het uitschakelen van de pull-to-refresh-functie op
mobiel, het verwijderen van overscroll glow en rubberbanding effecten, en
voorkomen dat pagina-inhoud verschuift wanneer deze zich onder een modale /
overlay bevindt.

`overscroll-behavior` vereist Chrome 63+. Het is in ontwikkeling of wordt
overwogen door andere browsers. Zie
[chromestatus.com](https://www.chromestatus.com/feature/5734614437986304) voor
meer informatie. {: .caution }

## Achtergrond

### Scroll grenzen en scroll chaining {: #scrollchaining }

<figure class="attempt-right">
<a href="/web/updates/images/2017/11/overscroll-behavior/drawer-scroll.mp4"
target="_blank">
<video
src="/web/updates/images/2017/11/overscroll-behavior/drawer-scroll.mp4" autoplay
loop muted alt="Drawer demo" height="300"></video>
  </a>
  <figcaption>Scroll chaining op Chrome Android.</figcaption>
</figure>

Scrollen is een van de meest fundamentele manieren om met een pagina te
communiceren, maar bepaalde UX-patronen kunnen lastig zijn om mee om te gaan
vanwege het eigenzinnige standaardgedrag van de browser. Neem bijvoorbeeld een
app-lade met een groot aantal items waar de gebruiker doorheen moet bladeren.
Wanneer ze de bodem bereiken, stopt de overloopcontainer met scrollen omdat er
geen inhoud meer is om te consumeren. Met andere woorden, de gebruiker bereikt
een "schuifgrens". Maar let op wat er gebeurt als de gebruiker blijft scrollen.
**De inhoud *achter* de lade begint te scrollen** ! Scrollen wordt overgenomen
door de bovenliggende container; de hoofdpagina zelf in het voorbeeld.

Blijkt dat dit gedrag **scroll chaining** wordt genoemd; het standaardgedrag van
de browser bij het bladeren door inhoud. Vaak is de standaardwaarde vrij aardig,
maar soms is het niet wenselijk of zelfs onverwacht. Bepaalde apps willen
mogelijk een andere gebruikerservaring bieden wanneer de gebruiker een
schuifgrens raakt.

### Het pull-to-refresh-effect {: #p2r }

Pull-to-refresh is een intuïtief gebaar dat populair wordt gemaakt door mobiele
apps zoals Facebook en Twitter. Als je een sociale feed gebruikt en loslaat,
ontstaat er nieuwe ruimte voor het laden van recentere berichten. Deze
specifieke UX is zelfs *zo populair geworden* dat mobiele browsers zoals Chrome
op Android hetzelfde effect hebben. Als u boven aan de pagina naar beneden
veegt, wordt de hele pagina vernieuwd:

<div class="clearfix centered">
  <figure class="attempt-left">
<a href="/web/updates/images/2017/11/overscroll-behavior/twitter.mp4"
target="_blank">
<video src="/web/updates/images/2017/11/overscroll-behavior/twitter.mp4"
autoplay muted loop height="350" class="border"></video>
    </a>
<figcaption>Twitter's aangepaste pull-to-refresh <br> bij het vernieuwen van
een feed in hun PWA.</figcaption>
  </figure>
  <figure class="attempt-right">
<a href="/web/updates/images/2017/11/overscroll-behavior/mobilep2r.mp4"
target="_blank">
<video
src="/web/updates/images/2017/11/overscroll-behavior/mobilep2r.mp4" autoplay
muted loop height="350" class="border"></video>
    </a>
<figcaption>De native pull-to-refresh-actie van Chrome Android <br>
vernieuwt de hele pagina.</figcaption>
  </figure>
</div>

Voor situaties zoals de Twitter [PWA](/web/progressive-web-apps/) kan het zinvol
zijn om de native pull-to-refresh-actie uit te schakelen. Waarom? In deze app
wilt u waarschijnlijk niet dat de gebruiker per ongeluk de pagina vernieuwt. Er
is ook het potentieel om een dubbele verversingsanimatie te zien! Als
alternatief is het misschien leuker om de actie van de browser aan te passen en
deze beter af te stemmen op de branding van de site. Het ongelukkige deel is dat
dit soort aanpassingen lastig te realiseren is geweest. Ontwikkelaars schrijven
uiteindelijk onnodige JavaScript, voegen
[niet-passieve](/web/tools/lighthouse/audits/passive-event-listeners)
luisteraars toe (die scrollen blokkeren) of steken de hele pagina in een 100vw /
vh `<div>` (om te voorkomen dat de pagina overloopt). Deze oplossingen hebben
[goed gedocumenteerde](https://wicg.github.io/overscroll-behavior/#intro)
negatieve effecten op de scrollprestaties.

We kunnen het beter doen!

## Introductie van `overscroll-behavior` {: #intro }

De `overscroll-behavior`
[eigenschap](https://wicg.github.io/overscroll-behavior/) is een nieuwe
CSS-functie die het gedrag van wat er gebeurt wanneer u een container (met
inbegrip van de pagina zelf) overscrollen regelt. Je kunt het gebruiken om
scroll chaining te annuleren, de pull-to-refresh-actie uit te schakelen / aan te
passen, rubberbanding-effecten op iOS uit te schakelen (wanneer Safari
`overscroll-behavior` implementeert) en meer. Het beste deel is dat het <strong
data-md-type="double_emphasis">gebruik van `overscroll-behavior` geen nadelige
invloed heeft</strong> op de <strong
data-md-type="double_emphasis">`overscroll-behavior`</strong> zoals de hacks die
in de intro worden genoemd!

De eigenschap heeft drie mogelijke waarden:

1. **auto** - Standaard. Scrolls die afkomstig zijn van het element kunnen zich
voortplanten naar voorouderelementen.

- **bevatten** - voorkomt scrollen. Scrolls worden niet doorgegeven aan
voorouders, maar lokale effecten binnen het knooppunt worden weergegeven.
Bijvoorbeeld het overscroll glow-effect op Android of het rubberbanding-effect
op iOS dat de gebruiker op de hoogte brengt wanneer hij een schuifgrens heeft
bereikt. **Opmerking** : gebruik van `overscroll-behavior: contain` op het
`html` element voorkomt overscroll-navigatie-acties.
- **geen** - hetzelfde als `contain` maar het voorkomt ook overscroll-effecten
binnen het knooppunt zelf (bijvoorbeeld Android-overscroll glow of iOS
rubberbanding).

Opmerking: `overscroll-behavior` ondersteunt ook steno voor
`overscroll-behavior-x` en `overscroll-behavior-y` als u alleen gedrag voor een
bepaalde as wilt definiëren.

Laten we enkele voorbeelden onder de `overscroll-behavior` om te kijken hoe we
`overscroll-behavior` .

## Voorkom dat rollen aan een element met vaste positie ontsnappen {: #fixedpos }

### Het chatbox-scenario {: #chat }

<figure class="attempt-right">
<a href="/web/updates/images/2017/11/overscroll-behavior/chatbox-chaining.mp4"
target="_blank">
<video
src="/web/updates/images/2017/11/overscroll-behavior/chatbox-chaining.mp4"
autoplay muted loop alt="Chatbox demo" height="350" class="border"></video>
  </a>
  <figcaption>Inhoud onder het chatvenster schuift ook :(</figcaption>
</figure>

Overweeg een vast gepositioneerde chatbox die onderaan de pagina staat. De
bedoeling is dat de chatbox een op zichzelf staand onderdeel is en dat deze
afzonderlijk van de inhoud erachter schuift. Vanwege het scrollen van de ketting
begint het document echter te scrollen zodra de gebruiker het laatste bericht in
de chatgeschiedenis krijgt.

Voor deze app is het beter om scrolls die afkomstig zijn uit de chatbox in de
chat te laten staan. We kunnen dat doen door `overscroll-behavior: contain` aan
het element dat de chatberichten bevat:

```css
#chat .msgs {
  overflow: auto;
  overscroll-behavior: contain;
  height: 300px;
}
```

In wezen creëren we een logische scheiding tussen de scrollcontext van de
chatbox en de hoofdpagina. Het eindresultaat is dat de hoofdpagina blijft staan
wanneer de gebruiker de bovenkant / onderkant van de chatgeschiedenis bereikt.
Scrolls die beginnen in de chatbox worden niet doorgegeven.

### Het pagina-overlay-scenario {: #overlay }

Een andere variant van het "underscroll" -scenario is wanneer u inhoud achter
een **vaste positie-overlay** ziet scrollen. Een dode giveaway
`overscroll-behavior` in orde is! De browser probeert behulpzaam te zijn, maar
de site ziet er buggy uit.

**Voorbeeld** - modaal met en zonder `overscroll-behavior: contain` :

<figure class="clearfix centered">
  <div class="attempt-left">
<a href="/web/updates/images/2017/11/overscroll-behavior/modal-off.mp4"
target="_blank">
<video src="/web/updates/images/2017/11/overscroll-behavior/modal-off.mp4"
autoplay muted loop height="290"></video>
    </a>
<figcaption><b>Voordien</b> : pagina-inhoud schuift onder
overlay.</figcaption>
  </div>
  <div class="attempt-right">
<a href="/web/updates/images/2017/11/overscroll-behavior/modal-on.mp4"
target="_blank">
<video src="/web/updates/images/2017/11/overscroll-behavior/modal-on.mp4"
autoplay muted loop height="290"></video>
    </a>
<figcaption><b>Na</b> : pagina-inhoud schuift niet onder de
overlay.</figcaption>
  </div>
</figure>

## Pull-to-refresh uitschakelen {: #disablp2r }

**Het uitschakelen van de pull-to-refresh-actie is een enkele regel CSS** .
Voorkom gewoon scroll chaining op het hele viewport-definiërende element. In de
meeste gevallen is dat `<html>` of `<body>` :

```css
body {
  /* Disables pull-to-refresh but allows overscroll glow effects. */
  overscroll-behavior-y: contain;
}
```

Met deze eenvoudige toevoeging repareren we de dubbele pull-to-refresh-animaties
in de [chatbox-demo](https://ebidel.github.io/demos/chatbox.html) en kunnen we
in plaats daarvan een aangepast effect implementeren dat een nettere
laadanimatie gebruikt. De hele inbox vervaagt ook als de inbox vernieuwt:

<figure class="clearfix centered">
  <div class="attempt-left">
<video
src="/web/updates/images/2017/11/overscroll-behavior/chatbox-double-refresh.mp4"
autoplay muted loop height="225"></video>
    <figcaption>Voor</figcaption>
  </div>
  <div class="attempt-right">
<video
src="/web/updates/images/2017/11/overscroll-behavior/chatbox-double-refresh-fix.mp4"
autoplay muted loop height="225"></video>
    <figcaption>Na</figcaption>
  </div>
</figure>

Hier is een fragment van de [volledige
code](https://github.com/ebidel/demos/blob/master/chatbox.html) :

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

## Overscroll glow en rubberbanding effecten uitschakelen {: #disableglow }

Gebruik `overscroll-behavior-y: none` om het bounce-effect uit te schakelen
wanneer u een scroll-grens raakt `overscroll-behavior-y: none` :

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
<figcaption><b>Voordien</b> : het raken van de scrollgrens toont een
gloed.</figcaption>
  </div>
  <div class="attempt-right">
<video
src="/web/updates/images/2017/11/overscroll-behavior/drawer-noglow.mp4" autoplay
muted loop height="300" class="border"></video>
    <figcaption><b>Na</b> : gloeien uitgeschakeld.</figcaption>
  </div>
</figure>

Opmerking: hierdoor blijven navigatie naar links / rechts vegen behouden. Om
navigatie te voorkomen, kunt u `overscroll-behavior-x: none` . Dit wordt echter
[nog steeds geïmplementeerd](https://crbug.com/762023) in Chrome.

## Volledige demo {: #demo }

Alles bij elkaar genomen, maakt de volledige
[chatbox-demo](https://ebidel.github.io/demos/chatbox.html) gebruik van
`overscroll-behavior` om een aangepaste pull-to-refresh-animatie te maken en te
voorkomen dat scrolls aan de chatbox-widget ontsnappen. Dit biedt een optimale
gebruikerservaring die lastig te bereiken zou zijn geweest zonder CSS
`overscroll-behavior` .

<figure>
  <a href="https://ebidel.github.io/demos/chatbox.html" target="_blank">
<video
src="/web/updates/images/2017/11/overscroll-behavior/chatbox-fixed.mp4" autoplay
muted loop alt="Chatbox demo" height="600"></video>
  </a>
<figcaption><a href="https://ebidel.github.io/demos/chatbox.html"
target="_blank">Bekijk demo</a> | <a
href="https://github.com/ebidel/demos/blob/master/chatbox.html"
target="_blank">Bron</a></figcaption>
</figure>

<br>
