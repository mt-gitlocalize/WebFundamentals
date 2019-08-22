project_path: "/web/_project.yaml"
book_path: "/web/resources/_book.yaml"
description: Если вы хотите перевести основы Интернета на другие языки, каждый может
  внести свой вклад.

{# wf_updated_on: 2019-03-04 #} {# wf_published_on: 2016-09-13 #} {# wf_blink_components: N/A #}

# Переводы {: .page-title }

Предупреждение: мы больше не используем GitLocalize, эти инструкции устарели и скоро будут обновлены.

Новый абзац для тестирования.

Мы экспериментируем с инструментом перевода под названием [GitLocalize](https://gitlocalize.com/) . Следуйте инструкциям, чтобы начать с вашего вклада:

1. Перейдите в [репозиторий GitLocalize Web Fundamentals](https://gitlocalize.com/repo/107) .
2. Зарегистрируйтесь, используя свою учетную запись GitHub.
3. Найдите документ, который вы собираетесь перевести.
4. Начните переводить.
5. Когда вы закончите, отправьте перевод для отзывов.
6. Рассмотренный перевод будет отправлен как запрос на извлечение модератором в сообществе.

Чтобы узнать больше о том, как работает GitLocalize, посетите [их страницу помощи](https://docs.gitlocalize.com/) .

Если вы обнаружите какие-либо проблемы или пожелания, отправьте их в [систему отслеживания ошибок GitLocalize](https://github.com/gitlocalize/feedback/issues) .

Чтобы пообщаться с другими участниками перевода, [зарегистрируйтесь на ChromiumDev Slack](https://join.slack.com/t/chromiumdev/shared_invite/enQtMzM3NjYwNjI0MDM4LTk2NjEyYTIxODk1MDYxMmNjNWYzMGMxZGVhMDNhY2I1ZjBhMjdlYTg0MTg4ZGE0OTQ0ZmYwNTRiMGJlYzVjOTE) и присоединитесь к каналу # l10n.

Примечание. Важно, чтобы вы нашли кого-то, чтобы просмотреть ваш перевод. Попробуйте найти рецензента на канале Slack # l10n, если еще [никто не назначен модератором языка](https://gitlocalize.com/repo/107/roles) . Если вы заинтересованы в том, чтобы стать модератором, пинг @agektmr в # l10n.

Поддерживаемые языки: арабский (AR), немецкий (DE), испанский (ES), французский (FR), иврит (HE), индонезийский (ID), итальянский (IT), японский (JA), корейский (KO) Голландский (NL), польский (PL), португальский (PT-BR), русский (RU), турецкий (TR), китайский традиционный (ZH-TW) и китайский упрощенный (ZH-CN).

## кредит

Мы хотим убедиться, что вы получите кредит на статьи, которые вы переводите.

Добавьте свои данные в `src/data/_contributors.yaml` и добавьте `- translator` в атрибут `role` . Мы используем эту информацию для заполнения [страницы](/web/resources/contributors) наших [участников,](/web/resources/contributors) а также для добавления вашего имени к каждой статье. Например:

```
paulkinlan:
  name:
    given: Paul
    family: Kinlan
  org:
    name: Google
      unit: Developer Relations
  country: UK
    role:
    - author
    - engineer
    - translator
  homepage: http://paul.kinlan.me
  twitter: paul_kinlan
  email: paulkinlan@google.com
  description:
    en: "Paul is a Developer Advocate"
```

Кроме того, не забудьте добавить свой кредит в нижней части статьи. Поскольку в GitLocalize вы не можете добавить новый раздел, добавьте свой кредит после нескольких пустых строк в существующем последнем разделе, как на следующем снимке экрана:

## Лицензия

Весь наш контент - Creative Commons 3.0. Вклад и переводы очень важны, однако вы должны подписать наше [Лицензионное соглашение](https://github.com/google/WebFundamentals/blob/master/CONTRIBUTING.md) для участников, чтобы код был возвращен в хранилище.
