project_path: "/web/_project.yaml"
book_path: "/web/resources/_book.yaml"
description: 웹 기초를 다른 언어로 번역하려는 경우 누구나 기여할 수 있습니다.

{# wf_updated_on: 2019-03-04 #} {# wf_published_on: 2016-09-13 #} {# wf_blink_components: N/A #}

# 번역 {: .page-title }

경고 : 더 이상 GitLocalize를 사용하지 않습니다.이 지침은 오래되었으며 곧 업데이트 될 예정입니다.

우리는 [GitLocalize](https://gitlocalize.com/) 라는 번역 도구를 실험하고 있습니다. 기여를 시작하려면 다음 단계를 따르십시오.

1. [GitLocalize의 Web Fundamentals 저장소로 이동하십시오](https://gitlocalize.com/repo/107) .
2. GitHub 계정을 사용하여 가입하십시오.
3. 번역 할 문서를 찾으십시오.
4. 번역을 시작하십시오.
5. 완료되면 검토를 위해 번역을 보냅니다.
6. 검토 된 번역은 커뮤니티의 언어 중재자가 풀 요청으로 전송합니다.

GitLocalize의 작동 방식에 대한 자세한 내용은 [도움말 페이지를](https://docs.gitlocalize.com/) 방문 [하십시오](https://docs.gitlocalize.com/) .

이슈 나 기능 요청이 있으면 [GitLocalize의 이슈 트래커](https://github.com/gitlocalize/feedback/issues) 에 [제출](https://github.com/gitlocalize/feedback/issues) 하십시오.

다른 번역 제공 업체와 채팅하려면 [ChromiumDev Slack에 등록하고](https://join.slack.com/t/chromiumdev/shared_invite/enQtMzM3NjYwNjI0MDM4LTk2NjEyYTIxODk1MDYxMmNjNWYzMGMxZGVhMDNhY2I1ZjBhMjdlYTg0MTg4ZGE0OTQ0ZmYwNTRiMGJlYzVjOTE) # l10n 채널에 가입하십시오.

참고 : 번역을 검토 할 사람을 찾는 것이 중요합니다. [아직 언어 중재자로 지정된 사람이 없으면](https://gitlocalize.com/repo/107/roles) Slack의 # l10n 채널에서 검토자를 찾으십시오. 중재자가되고 싶다면 # l10n에 @agektmr을 핑 (ping)하십시오.

지원되는 언어는 아랍어 (AR), 독일어 (DE), 스페인어 (ES), 프랑스어 (FR), 히브리어 (HE), 바 하사 인도네시아 (ID), 이탈리아어 (IT), 일본어 (JA), 한국어 (KO)입니다. 네덜란드어 (NL), 폴란드어 (PL), 포르투갈어 (PT-BR), 러시아어 (RU), 터키어 (TR), 중국어 번체 (ZH-TW) 및 중국어 간체 (ZH-CN)

## 신용

번역 한 기사에 대한 크레딧을 얻으려고합니다.

`src/data/_contributors.yaml` 세부 사항을 추가하고 `role` 속성에 `- translator` 를 추가하십시오. 이 정보를 사용하여 [기고자 페이지](/web/resources/contributors) 를 채우고 각 기사에 귀하의 이름을 첨부합니다. 예를 들면 다음과 같습니다.

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

또한 기사 하단에 크레딧을 추가하십시오. GitLocalize에서는 새 섹션을 추가 할 수 없으므로 다음 스크린 샷과 같이 기존 마지막 섹션에서 여러 개의 빈 줄 뒤에 크레딧을 추가하십시오.

## 특허

우리의 모든 내용은 Creative Commons 3.0입니다. 기고 및 번역은 대단히 감사하지만 코드를 저장소로 가져 오려면 [기고자 라이센스 계약](https://github.com/google/WebFundamentals/blob/master/CONTRIBUTING.md) 에 서명해야합니다.
