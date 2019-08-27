project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"
description: L'API di Badging è una nuova API della piattaforma Web che consente alle
  app Web installate di impostare un badge a livello di applicazione, mostrato in
  un luogo specifico del sistema operativo associato all'applicazione, come lo scaffale
  o la schermata principale.

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

# Badge per le icone delle app {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

<div class="clearfix"></div>

<aside class="caution">Attualmente stiamo lavorando su questa API come parte del
nuovo <a href="/web/updates/capabilities">progetto di funzionalità</a> e, a
partire da Chrome 73, è disponibile come versione di <a href="#ot"><b>prova di
origine</b></a> . Questo post verrà aggiornato con l'evolversi dell'API di
Badging. <br> <b>Ultimo aggiornamento:</b> 21 agosto 2019</aside>

## Cos'è l'API Badging? {: #what }

<figure class="attempt-right">
  <img src="/web/updates/images/2018/12/badges-on-windows.jpg">
<figcaption>Esempio di Twitter con 8 notifiche e un'altra app che mostra un
badge di tipo bandiera.</figcaption>
</figure>

L'API di Badging è una nuova API della piattaforma Web che consente alle app Web
installate di impostare un badge a livello di applicazione, mostrato in un luogo
specifico del sistema operativo associato all'applicazione (come lo scaffale o
la schermata principale).

Il badge consente di notificare sottilmente all'utente la presenza di alcune
nuove attività che potrebbero richiedere la sua attenzione, oppure può essere
utilizzato per indicare una piccola quantità di informazioni, come un conteggio
non letto.

I badge tendono ad essere più intuitivi delle notifiche e possono essere
aggiornati con una frequenza molto più elevata, poiché non interrompono
l'utente. E, poiché non interrompono l'utente, non è necessaria alcuna
autorizzazione speciale per utilizzarli.

[Leggi spiegazione](https://github.com/WICG/badging/blob/master/explainer.md) {:
.button .button-primary }

<div class="clearfix"></div>

### Casi d'uso suggeriti per l'API Badging {: #use-cases }

Esempi di siti che possono utilizzare questa API includono:

- Chat, e-mail e app social, per segnalare l'arrivo di nuovi messaggi o mostrare
il numero di elementi non letti.
- App di produttività, per segnalare che un'attività in background di lunga
durata (come il rendering di un'immagine o video) è stata completata.
- Giochi, per segnalare che è richiesta un'azione di un giocatore (ad esempio,
negli scacchi, quando è il turno del giocatore).

## Stato attuale {: #status }

Passo | Stato
--- | ---
1. Crea spiegatore | [Completare](https://github.com/WICG/badging/blob/master/explainer.md)
2. Creare una bozza iniziale di specifica | [Completare](https://wicg.github.io/badging/)
**3. Raccogli feedback e ripeti sul design** | [**In corso**](#feedback)
**4. Prova di origine** | [**In corso**](#ot)
5. Avvia | Non iniziato

### Guardalo in azione

1. Utilizzando Chrome 73 o versioni successive su Windows o Mac, apri la [demo
API Badging](https://badging-api.glitch.me/) .
2. Quando richiesto, fai clic su **Installa** per installare l'app o utilizza il
menu Chrome per installarlo, quindi aprilo come PWA installato. Nota, deve
essere in esecuzione come PWA installato (nella barra delle applicazioni o nel
dock).
3. Fai clic sul pulsante **Imposta** o **Cancella** per impostare o cancellare
il badge dall'icona dell'app. Puoi anche fornire un numero per il *valore Badge*
.

Nota: sebbene l'API Badging *in Chrome* richieda un'app installata con un'icona
che può essere effettivamente contrassegnata, si sconsiglia di effettuare
chiamate all'API Badging in base allo stato di installazione. L'API di Badging
può applicarsi *ovunque* un browser potrebbe voler mostrare un badge, quindi gli
sviluppatori non dovrebbero fare ipotesi su quali situazioni il browser farà
funzionare i badge. Chiama l'API quando esiste. Se funziona, funziona.
Altrimenti, semplicemente non lo fa.

## Come utilizzare l'API Badging {: #use }

A partire da Chrome 73, l'API Badging è disponibile come versione di prova di
origine per Windows (7+) e macOS. [Le prove di
Origin](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/README.md) ti
consentono di provare nuove funzionalità e fornire feedback su usabilità,
praticità ed efficacia per noi e per la comunità degli standard web. Per
ulteriori informazioni, consultare la [Guida alle prove di Origin per
sviluppatori
Web](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md)
.

### Supporto per badge su più piattaforme

L'API Badging è supportata (in una prova di origine) su Windows e macOS. Android
non è supportato perché richiede di mostrare una notifica, sebbene ciò possa
cambiare in futuro. Il supporto di Chrome OS è in attesa di implementazione del
badge sulla piattaforma.

### Registrati per la prova di origine {: #ot }

1. [Richiedi un
token](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)
per la tua origine.
2. Aggiungi il token alle tue pagine, ci sono due modi per fornire questo token
su qualsiasi pagina nella tua origine:
-  Aggiungi un tag `<meta>` `origin-trial` all'inizio di qualsiasi pagina.
Ad esempio, potrebbe essere simile a: `<meta http-equiv="origin-trial"
content="TOKEN_GOES_HERE">`
-  Se puoi configurare il tuo server, puoi anche fornire il token nelle
pagine usando un'intestazione HTTP `Origin-Trial` . L'intestazione della
risposta risultante dovrebbe essere simile a: `Origin-Trial: TOKEN_GOES_HERE`

### Alternative alla prova di origine

Se vuoi sperimentare localmente l'API di Badging, senza una prova di origine,
abilita il flag `#enable-experimental-web-platform-features` in `chrome://flags`
.

### Utilizzo dell'API Badging durante la prova di origine

Dogfood: durante la prova di origine, l'API sarà disponibile tramite
`window.ExperimentalBadge` . Il codice seguente si basa sul design attuale e
cambierà prima che arrivi nel browser come API standardizzata.

Per utilizzare l'API Badging, l'app Web deve soddisfare [i criteri di
installazione di Chrome](/web/fundamentals/app-install-banners/#criteria) e un
utente deve aggiungerla alla schermata principale.

L'interfaccia `ExperimentalBadge` è un oggetto membro nella `window` . Contiene
due metodi:

- `set([number])` : imposta il badge dell'app. Se viene fornito un valore,
impostare il badge sul valore fornito altrimenti, visualizzare un semplice punto
bianco (o altra bandiera a seconda della piattaforma).
- `clear()` : rimuove il badge dell'app.

```js
// In a web page
const unreadCount = 24;
window.ExperimentalBadge.set(unreadCount);
```

`ExperimentalBadge.set()` e `ExperimentalBadge.clear()` possono essere chiamati
da una pagina in primo piano o, potenzialmente, in futuro, un operatore del
servizio. In entrambi i casi, interessa l'intera app, non solo la pagina
corrente.

In alcuni casi, il sistema operativo potrebbe non consentire l'esatta
rappresentazione del badge, in questo caso il browser tenterà di fornire la
migliore rappresentazione per quel dispositivo. Ad esempio, mentre l'API Badging
non è supportata su Android, Android mostra sempre solo un punto anziché un
valore numerico.

Nota: non dare per scontato in che modo l'agente utente desidera visualizzare il
badge. Ci aspettiamo che alcuni programmi utente prendano un numero come "4000"
e lo riscrivano come "99+". Se lo saturi da solo (ad esempio a "99"), il "+" non
verrà visualizzato. Indipendentemente dal numero effettivo, imposta
`Badge.set(unreadCount)` e lascia che l'agente utente si occupi di visualizzarlo
di conseguenza.

## Feedback {: #feedback }

Abbiamo bisogno del tuo aiuto per garantire che l'API Badging funzioni in modo
tale da soddisfare le tue esigenze e che non ci manchino scenari chiave.

<aside class="key-point"><b>Abbiamo bisogno del tuo aiuto!</b> - Il design
attuale (che consente un valore intero o un flag) soddisferà le tue esigenze? In
caso contrario, si prega di presentare un problema nel <a
href="https://github.com/WICG/badging/issues">repository WICG / badging</a> e
fornire quanti più dettagli possibile. Inoltre, ci sono un certo numero di <a
href="https://github.com/WICG/badging/blob/master/choices.md">domande aperte</a>
che sono ancora in discussione e saremmo interessati a ricevere il tuo
feedback.</aside>

Siamo anche interessati a sapere come prevedi di utilizzare l'API Badging:

- Hai un'idea per un caso d'uso o un'idea su dove lo useresti?
- Pensi di usarlo?
- Ti piace e vuoi mostrare il tuo supporto?

Condividi le tue opinioni sulla discussione sul [Discorso WICG sull'API di
Badging](https://discourse.wicg.io/t/badging-api-for-showing-an-indicator-on-a-web-apps-shelf-icon/2900)
.

{% include "web/_shared/helpful.html" %}

## Link utili {: #helpful }

- [Spiegatore
pubblico](https://github.com/WICG/badging/blob/master/explainer.md)
- [Demo API Badging](https://badging-api.glitch.me/) | [Fonte demo API
Badging](https://glitch.com/edit/#!/badging-api?path=demo.js)
- [Bug di
tracciamento](https://bugs.chromium.org/p/chromium/issues/detail?id=719176)
- [Voce
ChromeStatus.com](https://www.chromestatus.com/features/6068482055602176)
- Richiedi un [token di prova di
origine](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)
- [Come utilizzare un token di prova di
origine](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md#how-do-i-enable-an-experimental-feature-on-my-origin)
- Componente lampeggiante: `UI>Browser>WebAppInstalls`

{% include "web/_shared/rss-widget-updates.html" %}
