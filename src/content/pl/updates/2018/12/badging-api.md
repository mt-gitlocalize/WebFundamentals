project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"
description: Interfejs API Badging to nowy interfejs API platformy internetowej, który
  pozwala zainstalowanym aplikacjom internetowym ustawić znaczek dla całej aplikacji,
  wyświetlany w specyficznym dla systemu operacyjnego miejscu powiązanym z aplikacją,
  takim jak półka lub ekran główny.

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

# Oznaczenia ikon aplikacji {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

<div class="clearfix"></div>

<aside class="caution">Obecnie pracujemy nad tym interfejsem API w ramach
projektu dotyczącego nowych <a href="/web/updates/capabilities">możliwości</a> ,
a od Chrome 73 jest on dostępny jako wersja <a href="#ot"><b>oryginalna</b></a>
. Ten post będzie aktualizowany w miarę ewolucji interfejsu API Badging. <br>
<b>Ostatnia aktualizacja:</b> 21 sierpnia 2019 r</aside>

## Co to jest interfejs API Badging? {: #what }

<figure class="attempt-right">
  <img src="/web/updates/images/2018/12/badges-on-windows.jpg">
<figcaption>Przykład Twittera z 8 powiadomieniami i inną aplikacją z plakietką
typu flagi.</figcaption>
</figure>

Interfejs API Badging to nowy interfejs API platformy internetowej, który
pozwala zainstalowanym aplikacjom internetowym ustawić znaczek dla całej
aplikacji, wyświetlany w specyficznym dla systemu operacyjnego miejscu
powiązanym z aplikacją (takim jak półka lub ekran główny).

Odznaka ułatwia subtelne powiadomienie użytkownika, że istnieje jakaś nowa
aktywność, która może wymagać jego uwagi, lub może być wykorzystana do wskazania
niewielkiej ilości informacji, na przykład nieprzeczytanej liczby.

Odznaki są zwykle bardziej przyjazne dla użytkownika niż powiadomienia i mogą
być aktualizowane z dużo większą częstotliwością, ponieważ nie przeszkadzają
użytkownikowi. A ponieważ nie przeszkadzają użytkownikowi, nie jest wymagane
specjalne pozwolenie na ich użycie.

[Przeczytaj
wyjaśnienie](https://github.com/WICG/badging/blob/master/explainer.md) {:
.button .button-primary }

<div class="clearfix"></div>

### Sugerowane przypadki użycia interfejsu API Badging {: #use-cases }

Przykłady witryn, które mogą korzystać z tego interfejsu API:

- Czat, e-mail i aplikacje społecznościowe, aby zasygnalizować, że nadeszły nowe
wiadomości lub pokazać liczbę nieprzeczytanych elementów.
- Aplikacje zwiększające produktywność, sygnalizujące zakończenie długotrwałego
zadania w tle (takiego jak renderowanie obrazu lub wideo).
- Gry, aby zasygnalizować, że wymagana jest akcja gracza (np. W szachach, kiedy
nadchodzi kolej gracza).

## Obecny status {: #status }

Krok | Status
--- | ---
1. Utwórz objaśnienie | [Kompletny](https://github.com/WICG/badging/blob/master/explainer.md)
2. Utwórz wstępny projekt specyfikacji | [Kompletny](https://wicg.github.io/badging/)
**3. Zbierz opinie i powtórz projekt** | [**W trakcie**](#feedback)
**4. Próbka pochodzenia** | [**W trakcie**](#ot)
5. Uruchom | Nie rozpoczął

### Zobacz to w akcji

1. W przeglądarce Chrome 73 lub nowszej w systemie Windows lub Mac otwórz [demo
Badging API](https://badging-api.glitch.me/) .
2. Po wyświetleniu monitu kliknij opcję **Instaluj,** aby zainstalować aplikację
lub użyj menu Chrome, aby ją zainstalować, a następnie otwórz ją jako
zainstalowany PWA. Uwaga: musi być uruchomiony jako zainstalowany PWA (na pasku
zadań lub pasku zadań).
3. Kliknij przycisk **Ustaw** lub **Wyczyść** , aby ustawić lub usunąć znaczek z
ikony aplikacji. Możesz także podać numer *wartości odznaki* .

Uwaga: Chociaż interfejs API Badging *w Chrome* wymaga zainstalowanej aplikacji
z ikoną, która może zostać oznaczona, odradzamy uzależnianie wywołań interfejsu
API Badging od stanu instalacji. Interfejs API Badging może być stosowany
*wszędzie* tam, *gdzie* przeglądarka może chcieć wyświetlać odznakę, więc
programiści nie powinni przyjmować żadnych założeń dotyczących tego, w jakich
sytuacjach przeglądarka sprawi, że odznaki będą działać. Wystarczy wywołać
interfejs API, jeśli istnieje. Jeśli to działa, to działa. Jeśli nie, to po
prostu nie.

## Jak korzystać z interfejsu API Badging {: #use }

Począwszy od Chrome 73, interfejs API Badging jest dostępny jako wersja próbna
dla systemu Windows (7+) i macOS. [Testy
Origin](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/README.md)
pozwalają wypróbować nowe funkcje i przekazać nam informacje zwrotne na temat
użyteczności, praktyczności i skuteczności oraz społeczności internetowej. Aby
uzyskać więcej informacji, zobacz [Przewodnik próbny Origin dla programistów
internetowych](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md)
.

### Wsparcie dla znaczków na różnych platformach

Interfejs API Badging jest obsługiwany (w wersji źródłowej) w systemach Windows
i macOS. Android nie jest obsługiwany, ponieważ wymaga powiadomienia, ale może
się to zmienić w przyszłości. Obsługa Chrome OS oczekuje na wdrożenie znaczków
na platformie.

### Zarejestruj się na próbę pochodzenia {: #ot }

1. [Poproś o
token](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)
swojego pochodzenia.
2. Dodaj token do swoich stron, istnieją dwa sposoby umieszczenia tego tokena na
dowolnych stronach w twoim pochodzeniu:
-  Dodaj tag `origin-trial` `<meta>` do nagłówka dowolnej strony. Na
przykład może to wyglądać `<meta http-equiv="origin-trial"
content="TOKEN_GOES_HERE">` tak: `<meta http-equiv="origin-trial"
content="TOKEN_GOES_HERE">`
-  Jeśli możesz skonfigurować serwer, możesz także podać token na stronach
przy użyciu nagłówka HTTP `Origin-Trial` . Wynikowy nagłówek odpowiedzi powinien
wyglądać `Origin-Trial: TOKEN_GOES_HERE` tak: `Origin-Trial: TOKEN_GOES_HERE`

### Alternatywy dla próby pochodzenia

Jeśli chcesz eksperymentować z interfejsem API Badging lokalnie, bez wersji
źródłowej, włącz flagę `#enable-experimental-web-platform-features` w
`chrome://flags` .

### Korzystanie z interfejsu API Badging podczas wersji źródłowej

Dogfood: Podczas wersji próbnej interfejs API będzie dostępny przez
`window.ExperimentalBadge` . Poniższy kod jest oparty na bieżącym projekcie i
zmieni się, zanim wyląduje w przeglądarce jako standardowy interfejs API.

Aby korzystać z interfejsu API Badging, Twoja aplikacja internetowa musi
spełniać [kryteria instalacyjne
Chrome](/web/fundamentals/app-install-banners/#criteria) , a użytkownik musi
dodać ją do ekranu głównego.

Interfejs `ExperimentalBadge` to obiekt członkowski w `window` . Zawiera dwie
metody:

- `set([number])` : Ustawia odznakę aplikacji. Jeśli podano wartość, ustaw
odznakę na podaną wartość, w przeciwnym razie wyświetl zwykłą białą kropkę (lub
inną flagę odpowiednią dla platformy).
- `clear()` : usuwa znaczek aplikacji.

```js
// In a web page
const unreadCount = 24;
window.ExperimentalBadge.set(unreadCount);
```

`ExperimentalBadge.set()` i `ExperimentalBadge.clear()` można wywołać ze strony
na pierwszym planie, lub potencjalnie w przyszłości, pracownika serwisu. W obu
przypadkach wpływa to na całą aplikację, a nie tylko na bieżącą stronę.

W niektórych przypadkach system operacyjny może nie zezwalać na dokładne
przedstawienie plakietki, w takim przypadku przeglądarka podejmie próbę
zapewnienia najlepszej reprezentacji dla tego urządzenia. Na przykład, chociaż
interfejs API Badging nie jest obsługiwany na Androidzie, Android zawsze
pokazuje kropkę zamiast wartości liczbowej.

Uwaga: nie zakładaj niczego o tym, w jaki sposób agent użytkownika chce
wyświetlać odznakę. Oczekujemy, że niektóre programy użytkownika przyjmą liczbę
taką jak „4000” i przepisają ją jako „99+”. Jeśli sam go nasycisz (na przykład
do „99”), wówczas „+” nie pojawi się. Bez względu na faktyczną liczbę, po prostu
ustaw `Badge.set(unreadCount)` i pozwól `Badge.set(unreadCount)` użytkownika
odpowiednio go wyświetlić.

## Informacja zwrotna {: #feedback }

Potrzebujemy Twojej pomocy, aby upewnić się, że API Badging działa w sposób,
który spełnia Twoje potrzeby i że nie brakuje nam żadnych kluczowych
scenariuszy.

<aside class="key-point"><b>Potrzebujemy Twojej pomocy!</b> - Czy obecny projekt
(uwzględniający liczbę całkowitą lub wartość flagi) spełni twoje potrzeby? Jeśli
nie, zgłoś problem w <a
href="https://github.com/WICG/badging/issues">repozytorium WICG / odznakie</a> i
podaj jak najwięcej szczegółów. Ponadto istnieje wiele <a
href="https://github.com/WICG/badging/blob/master/choices.md">otwartych
pytań</a> , które wciąż są dyskutowane, a my chcielibyśmy poznać Twoją
opinię.</aside>

Zależy nam również na tym, jak planujesz używać interfejsu API Badging:

- Masz pomysł na przypadek użycia lub pomysł, gdzie go użyjesz?
- Czy planujesz to wykorzystać?
- Podoba ci się i chcesz pokazać swoje wsparcie?

Podziel się swoimi przemyśleniami na temat dyskusji [Dyskurs WICG API Badging
API](https://discourse.wicg.io/t/badging-api-for-showing-an-indicator-on-a-web-apps-shelf-icon/2900)
.

{% include "web/_shared/helpful.html" %}

## Pomocne linki {: #helpful }

- [Publiczny tłumacz](https://github.com/WICG/badging/blob/master/explainer.md)
- [Badging API Demo](https://badging-api.glitch.me/) | [Źródło demonstracyjne
API Badging](https://glitch.com/edit/#!/badging-api?path=demo.js)
- [Błąd śledzenia](https://bugs.chromium.org/p/chromium/issues/detail?id=719176)
- [Wpis
ChromeStatus.com](https://www.chromestatus.com/features/6068482055602176)
- Poproś o [token próbny
pochodzenia](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)
- [Jak korzystać z tokenu próbnego
pochodzenia](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md#how-do-i-enable-an-experimental-feature-on-my-origin)
- Komponent Blink: `UI>Browser>WebAppInstalls`

{% include "web/_shared/rss-widget-updates.html" %}
