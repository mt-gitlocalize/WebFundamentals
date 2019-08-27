project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"
description: The Badging API is a new web platform API that allows installed web apps
  to set an application-wide badge, shown in an operating-system-specific place associated
  with the application, such as the shelf or home screen.

{# wf_published_on: 2018-12-11 #}
{# wf_updated_on: 2019-08-21 #}
{# wf_featured_image: /web/updates/images/generic/notifications.png #}
{# wf_tags:
capabilities,badging,install,progressive-web-apps,serviceworker,notifications,origintrials
#}
{# wf_featured_snippet: The Badging API is a new web platform API that allows
installed web apps to set an application-wide badge, shown in an
operating-system-specific place associated with the application, such as the
shelf or home screen. Badging makes it easy to subtly notify the user that there
is some new activity that might require their attention, or it can be used to
indicate a small amount of information, such as an unread count. #}
{# wf_blink_components: UI>Browser>WebAppInstalls #}

{# When updating this post, don't forget to update /updates/capabilities.md #}

# Badging para ícones de aplicativos {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

<div class="clearfix"></div>

<aside class="caution">
  We’re currently working on this API as part of the new
  <a href="/web/updates/capabilities">capabilities project</a>, and starting
  in Chrome 73 it is available as an <a href="#ot"><b>origin trial</b></a>.
  This post will be updated as the Badging API evolves.<br>
  <b>Last Updated:</b> August 21st, 2019
</aside>

## O que é API Badging? {: #what }

<figure class="attempt-right">
  <img src="/web/updates/images/2018/12/badges-on-windows.jpg">
  <figcaption>
    Example of Twitter with 8 notifications and another app showing a flag
    type badge.
  </figcaption>
</figure>

Badging API é uma nova API da plataforma web que permite que aplicativos web
instalados definam um badge(emblema) para toda aplicacão, mostrando em um local
específico do sistema operacional a associação com a aplicação(Como uma estante
ou a tela inicial).

A criação de um Badging facilita a notificação sutil ao usuário de que há uma
nova atividade que pode exigir sua atenção ou que pode ser usada para indicar
uma pequena quantidade de informações, como uma contagem não lida.

Badges tend to be more user-friendly than notifications, and can be updated
with a much higher frequency, since they don’t interrupt the user. And,
because they don’t interrupt the user, there’s no special permission needed
to use them.

[Leia explainer](https://github.com/WICG/badging/blob/master/explainer.md){:
.button .button-primary }

<div class="clearfix"></div>

### Suggested use cases for the Badging API {: #use-cases }

Exemplos de sites que podem usar esta API incluem:

- Chat, email and social apps, to signal that new messages have arrived, orshow
the number of unread items.
- Aplicativos de produtividade, para sinalizar que uma tarefa em segundo plano
de longa duração (como renderizar uma imagem ou vídeo) foi concluída.
- Jogos, para sinalizar que é necessária uma ação do jogador (por exemplo, no
Xadrez, quando é a vez do jogador).

## Status atual {: #status }

Etapa | Status
--- | ---
1. Criar explicador | [Completo](https://github.com/WICG/badging/blob/master/explainer.md)
2. Criar rascunho inicial de especificação | [Complete](https://wicg.github.io/badging/)
**3. Reunir feedback e iterar no design** | [**Em progresso**](#feedback)
**4. Julgamento de origem** | [**In progress**](#ot)
5. Lançamento | Not started

### See it in action

1. Using Chrome 73 or later on Windows or Mac, open the [Badging API
demo](https://badging-api.glitch.me/).
2. When prompted, click **Install** to install the app , or use the Chromemenu
to install it, then open it as an installed PWA. Note, it must berunning as an
installed PWA (in your task bar or dock).
3. Click the **Set** or **Clear** button to set or clear the badge from the
appicon. You can also provide a number for the *Badge value*.

Note: While the Badging API *in Chrome* requires an installed app
with an icon that actually can be badged, we advise against
making calls to the Badging API dependent on the install state.
The Badging API can apply to *anywhere* a browser might want to show a badge,
so developers shouldn’t make any assumptions about in what situations
the browser will make badges work. Just call the API when it exists.
If it works, it works. If not, it simply doesn’t.

## How to use the Badging API {: #use }

Starting in Chrome 73, the Badging API is available as an origin trial
for Windows (7+) and macOS.
[Origin
trials](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/README.md)
allow you to try out new features and give
feedback on usability, practicality, and effectiveness to us, and the web
standards community. For more information, see the
[Origin Trials Guide for Web
Developers](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md).

### Support for badging across platforms

The Badging API is supported (in an origin trial) on Windows and macOS.
Android is not supported because it requires you to show a notification,
though this may change in the future.
Chrome OS support is pending implementation of badging on the platform.

### Register for the origin trial {: #ot }

1. [Request a
token](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)
for your origin.
2. Add the token to your pages, there are two ways to provide this token onany
pages in your origin:
- Add an `origin-trial` `<meta>` tag to the head of any page. For
example,this may look something like:`<meta http-equiv="origin-trial"
content="TOKEN_GOES_HERE">`
- If you can configure your server, you can also provide the token on
pagesusing an `Origin-Trial` HTTP header. The resulting response header
shouldlook something like: `Origin-Trial: TOKEN_GOES_HERE`

### Alternatives to the origin trial

If you want to experiment with the Badging API locally, without an origin trial,
enable the `#enable-experimental-web-platform-features` flag in
`chrome://flags`.

### Using the Badging API during the origin trial

Dogfood: During the origin trial, the API will be available via
`window.ExperimentalBadge`. The below code is based on the current design,
and will change before it lands in the browser as a standardized API.

To use the Badging API, your web app needs to meet
[Chrome’s installability
criteria](/web/fundamentals/app-install-banners/#criteria),
and a user must add it to their home screen.

The `ExperimentalBadge` interface is a member object on `window`. It contains
two methods:

- `set([number])`: Sets the app's badge. If a value is provided, set the badgeto
the provided value otherwise, display a plain white dot (or other flag
asappropriate to the platform).
- `clear()`: Remove o badge do aplicativo.

```js
// In a web page
const unreadCount = 24;
window.ExperimentalBadge.set(unreadCount);
```

`ExperimentalBadge.set()` and `ExperimentalBadge.clear()` can be called from
a foreground page, or potentially in the future, a service worker. In either
case, it affects the whole app, not just the current page.

In some cases, the OS may not allow the exact representation of the badge,
in this case, the browser will attempt to provide the best representation for
that device. For example, while the Badging API isn’t supported on Android,
Android only ever shows a dot instead of a numeric value.

Note: Don’t assume anything about how the user agent wants to display the badge.
We expect some user agents will take a number like "4000" and rewrite it as
"99+".
If you saturate it yourself (for example to "99") then the "+" won’t appear.
No matter the actual number, just set `Badge.set(unreadCount)`
and let the user agent deal with displaying it accordingly.

## Comentários {: #feedback }

Precisamos da sua ajuda para garantir que a API Badging funcione de uma maneira
que atenda às suas necessidades e que não percamos nenhum cenário importante.

<aside class="key-point">
  <b>We need your help!</b> - Will the current design
  (allowing either an integer or a flag value) meet your needs?
  If it won’t, please file an issue in the
  <a href="https://github.com/WICG/badging/issues">WICG/badging repo</a>
  and provide as much detail as you can. In addition,
there are a number of <a
href="https://github.com/WICG/badging/blob/master/choices.md">
  open questions</a> that are still being discussed, and we’d be interested to
  hear your feedback.
</aside>

Também estamos interessados ​​em saber como você planeja usar a API Badging:

- Tem alguma ideia para caso de uso ou outra ideia em que você poderia
utilizá-la?
- Você planeja usar isso?
- Gostou e quer mostrar seu apoio?

Compartilhe suas idéias na discussão [Badging API WICG
Discourse](https://discourse.wicg.io/t/badging-api-for-showing-an-indicator-on-a-web-apps-shelf-icon/2900).

{% include "web/_shared/helpful.html" %}

## Links úteis {: #helpful }

- [Public explainer](https://github.com/WICG/badging/blob/master/explainer.md)
- [Badging API Demo](https://badging-api.glitch.me/) | [Badging API Demo
source](https://glitch.com/edit/#!/badging-api?path=demo.js)
- [Tracking bug](https://bugs.chromium.org/p/chromium/issues/detail?id=719176)
- [ChromeStatus.com
entry](https://www.chromestatus.com/features/6068482055602176)
- Request an [origin trial
token](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)
- [How to use an origin trial
token](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md#how-do-i-enable-an-experimental-feature-on-my-origin)
- Blink Component: `UI>Browser>WebAppInstalls`

{% include "web/_shared/rss-widget-updates.html" %}
