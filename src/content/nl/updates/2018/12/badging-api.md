project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"
description: De Badging-API is een nieuwe webplatform-API waarmee geïnstalleerde web-apps
  een badge voor de hele applicatie kunnen instellen, weergegeven op een besturingssysteemspecifieke
  plaats die is gekoppeld aan de applicatie, zoals het schap of het startscherm.

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

# Badges voor app-pictogrammen {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

<div class="clearfix"></div>

<aside class="caution">We werken momenteel aan deze API als onderdeel van het
nieuwe <a href="/web/updates/capabilities">mogelijkhedenproject</a> en vanaf
Chrome 73 is het beschikbaar als een <a href="#ot"><b>proefversie van
oorsprong</b></a> . Dit bericht wordt bijgewerkt naarmate de Badging-API zich
ontwikkelt. <br> <b>Laatst bijgewerkt:</b> 21 augustus 2019</aside>

## Wat is de Badging-API? {: #what }

<figure class="attempt-right">
  <img src="/web/updates/images/2018/12/badges-on-windows.jpg">
<figcaption>Voorbeeld van Twitter met 8 meldingen en een andere app met een
vlag-type badge.</figcaption>
</figure>

De Badging-API is een nieuwe webplatform-API waarmee geïnstalleerde web-apps een
badge voor de hele applicatie kunnen instellen, weergegeven op een
besturingssysteemspecifieke plaats die is gekoppeld aan de applicatie (zoals het
schap of het startscherm).

Badging maakt het gemakkelijk om de gebruiker subtiel op de hoogte te brengen
dat er een nieuwe activiteit is die mogelijk zijn aandacht vereist, of deze kan
worden gebruikt om een kleine hoeveelheid informatie aan te geven, zoals een
ongelezen telling.

Badges zijn meestal gebruikersvriendelijker dan meldingen en kunnen met een veel
hogere frequentie worden bijgewerkt, omdat ze de gebruiker niet onderbreken. En
omdat ze de gebruiker niet onderbreken, is er geen speciale toestemming nodig om
ze te gebruiken.

[Lees uitleg](https://github.com/WICG/badging/blob/master/explainer.md) {:
.button .button-primary }

<div class="clearfix"></div>

### Aanbevolen gebruikstoepassingen voor de Badging-API {: #use-cases }

Voorbeelden van sites die deze API kunnen gebruiken, zijn onder meer:

- Chat-, e-mail- en sociale apps om aan te geven dat er nieuwe berichten zijn
binnengekomen of om het aantal ongelezen items weer te geven.
- Productiviteitsapps om aan te geven dat een langlopende achtergrondtaak (zoals
het weergeven van een afbeelding of video) is voltooid.
- Games, om aan te geven dat actie van een speler vereist is (bijvoorbeeld in
schaken, wanneer de speler aan de beurt is).

## Huidige status {: #status }

Stap | staat
--- | ---
1. Maak uitleg | [Compleet](https://github.com/WICG/badging/blob/master/explainer.md)
2. Maak een eerste conceptontwerp van de specificatie | [Compleet](https://wicg.github.io/badging/)
**3. Verzamel feedback en herhaal het ontwerp** | [**Bezig**](#feedback)
**4. Oorsprong proef** | [**Bezig**](#ot)
5. Start | Niet begonnen

### Zie het in actie

1. Gebruik Chrome 73 of hoger op Windows of Mac en open de [demo van Badging
API](https://badging-api.glitch.me/) .
2. Klik wanneer daarom wordt gevraagd op **Installeren** om de app te
installeren of gebruik het Chrome-menu om deze te installeren en open de app
vervolgens als een geïnstalleerde PWA. Let op, het moet worden uitgevoerd als
een geïnstalleerde PWA (in uw taakbalk of dock).
3. Klik op de knop **Instellen** of **Wissen** om de badge van het app-pictogram
in te stellen of te wissen. U kunt ook een nummer *opgeven* voor de *waarde
Badge* .

Opmerking: Hoewel de Badging-API *in Chrome* een geïnstalleerde app vereist met
een pictogram dat daadwerkelijk kan worden gebadged, raden we af om oproepen
naar de Badging-API afhankelijk te maken van de installatiestatus. De
Badging-API kan van toepassing zijn op *elke plaats waar* een browser mogelijk
een badge wil laten zien, dus ontwikkelaars moeten geen aannames doen over in
welke situaties de browser badges zal laten werken. Roep gewoon de API aan als
deze bestaat. Als het werkt, werkt het. Zo niet, dan doet het dat gewoon niet.

## Hoe de Badging API te gebruiken {: #use }

Vanaf Chrome 73 is de Badging-API beschikbaar als proefversie voor Windows (7+)
en macOS. [Met
Origin-proeven](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/README.md)
kunt u nieuwe functies uitproberen en feedback geven over bruikbaarheid,
bruikbaarheid en effectiviteit voor ons en de community voor webstandaarden. Zie
de [Origin Trials Guide voor
webontwikkelaars](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md)
voor meer informatie.

### Ondersteuning voor badging op verschillende platforms

De Badging API wordt ondersteund (in een proefversie) op Windows en macOS.
Android wordt niet ondersteund omdat hiervoor een melding moet worden
weergegeven, hoewel dit in de toekomst kan veranderen. Ondersteuning voor Chrome
OS is in afwachting van implementatie van badging op het platform.

### Registreer u voor de proefversie {: #ot }

1. [Vraag een token
aan](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)
voor uw afkomst.
2. Voeg het token toe aan uw pagina's, er zijn twee manieren om dit token aan te
bieden op alle pagina's van uw oorsprong:
-  Voeg een `origin-trial` `<meta>` -tag toe aan het hoofd van een pagina.
Dit kan er bijvoorbeeld zo uitzien: `<meta http-equiv="origin-trial"
content="TOKEN_GOES_HERE">`
-  Als u uw server kunt configureren, kunt u het token ook op pagina's
leveren met behulp van een `Origin-Trial` HTTP-header. De resulterende
antwoordkop moet er ongeveer zo uitzien: `Origin-Trial: TOKEN_GOES_HERE`

### Alternatieven voor de herkomstproef

Als u lokaal wilt experimenteren met de Badging API, zonder een proefversie van
oorsprong, schakelt u de vlag `#enable-experimental-web-platform-features` in
`chrome://flags` .

### De Badging-API gebruiken tijdens de oorspronkelijke proefperiode

Hondenvoer: tijdens de proefperiode van oorsprong is de API beschikbaar via
`window.ExperimentalBadge` . De onderstaande code is gebaseerd op het huidige
ontwerp en wordt gewijzigd voordat deze als gestandaardiseerde API in de browser
wordt geplaatst.

Als u de Badging-API wilt gebruiken, moet uw web-app voldoen aan [de
installeerbaarheidscriteria van
Chrome](/web/fundamentals/app-install-banners/#criteria) en moet een gebruiker
deze toevoegen aan zijn startscherm.

De `ExperimentalBadge` interface is een lidobject in het `window` . Het bevat
twee methoden:

- `set([number])` : hiermee stelt u de badge van de app in. Als een waarde wordt
opgegeven, stelt u de badge anders in op de opgegeven waarde, geeft u een effen
witte stip weer (of een andere vlag die geschikt is voor het platform).
- `clear()` : verwijdert de badge van de app.

```js
// In a web page
const unreadCount = 24;
window.ExperimentalBadge.set(unreadCount);
```

`ExperimentalBadge.set()` en `ExperimentalBadge.clear()` kunnen worden
opgeroepen vanaf een voorgrondpagina of mogelijk in de toekomst door een
servicemedewerker. In beide gevallen heeft het invloed op de hele app, niet
alleen op de huidige pagina.

In sommige gevallen staat het besturingssysteem mogelijk niet de exacte weergave
van de badge toe, in dit geval probeert de browser de beste weergave voor dat
apparaat te bieden. Hoewel de Badging-API bijvoorbeeld niet wordt ondersteund op
Android, toont Android alleen een stip in plaats van een numerieke waarde.

Opmerking: ga niet uit van hoe de user-agent de badge wil weergeven. We
verwachten dat sommige user-agents een getal als "4000" gebruiken en het
herschrijven als "99+". Als u het zelf verzadigt (bijvoorbeeld tot "99"),
verschijnt de "+" niet. Ongeacht het werkelijke aantal, stel
`Badge.set(unreadCount)` en laat de user-agent het dienovereenkomstig weergeven.

## Feedback {: #feedback }

We hebben uw hulp nodig om ervoor te zorgen dat de Badging API werkt op een
manier die aan uw behoeften voldoet en dat we geen belangrijke scenario's
missen.

<aside class="key-point"><b>We hebben je hulp nodig!</b> - Zal het huidige
ontwerp (waardoor een geheel getal of een vlagwaarde mogelijk is) aan uw
behoeften voldoen? Als dit niet het geval is, dient u een probleem in de <a
href="https://github.com/WICG/badging/issues">WICG / badging-repo in</a> en
geeft u zo veel mogelijk details. Daarnaast zijn er nog een aantal <a
href="https://github.com/WICG/badging/blob/master/choices.md">open vragen</a>
die nog worden besproken en we zouden graag uw feedback horen.</aside>

We zijn ook geïnteresseerd in hoe u van plan bent de Badging-API te gebruiken:

- Heb je een idee voor een use case of een idee waar je het zou gebruiken?
- Ben je van plan dit te gebruiken?
- Vind je het leuk en wil je je steun tonen?

Deel uw mening over de [Badging API WICG
Discourse-](https://discourse.wicg.io/t/badging-api-for-showing-an-indicator-on-a-web-apps-shelf-icon/2900)
discussie.

{% include "web/_shared/helpful.html" %}

## Handige links {: #helpful }

- [Openbare uitleg](https://github.com/WICG/badging/blob/master/explainer.md)
- [Badging API-demo](https://badging-api.glitch.me/) | [Bron van
badging-API-demo](https://glitch.com/edit/#!/badging-api?path=demo.js)
- [Tracking bug](https://bugs.chromium.org/p/chromium/issues/detail?id=719176)
-
[ChromeStatus.com-vermelding](https://www.chromestatus.com/features/6068482055602176)
- Vraag een [oorsprong-proef-token
aan](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)
- [Hoe een oorsprong-proef-token te
gebruiken](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md#how-do-i-enable-an-experimental-feature-on-my-origin)
- `UI>Browser>WebAppInstalls` : `UI>Browser>WebAppInstalls`

{% include "web/_shared/rss-widget-updates.html" %}
