# DevSite의 웹 기초

[![Build Status](https://travis-ci.org/google/WebFundamentals.svg?branch=master)](https://travis-ci.org/google/WebFundamentals)

새로운 웹 **기초에** 오신 것을 환영합니다! 최신 웹 개발을위한 모범 사례 및 도구를 소개하려는 노력.

### 무엇이 바뀌 었습니까?

- 우리는 이제 [DevSite](https://developers.google.com/) 인프라를 사용하고 있습니다 
    -  새로운 [스타일 가이드](https://petele-scratch.appspot.com/web/resources/style-guide) 
    -  새로운 [위젯으로](https://petele-scratch.appspot.com/web/resources/widgets) 인라인 JavaScript, 공통 링크, 관련 안내서 등 
- 지킬이 제거되었습니다. 대신 요청시 페이지가 렌더링됩니다.
- 앞면 문제가 마크 다운에서 제거되었지만 이제 파일에는 [간단한 태그 세트가](https://petele-scratch.appspot.com/web/resources/writing-an-article#yaml-front-matter) 필요 [합니다.](https://petele-scratch.appspot.com/web/resources/writing-an-article#yaml-front-matter)

### 무엇이 동일하게 유지됩니까?

- GitHub는 여전히 콘텐츠의 진실 소스입니다.
- 우리는 당신의 기여, PR, 이슈 등을 원합니다!
- 최신은 https://web-central.appspot.com/web/에서 준비됩니다.
- 또한-이 목록에 다른 줄이 있습니다.

## 레포 복제

고 대역폭 연결이있는 경우 새로운 리포지토리 복제본으로 시작하는 것이 좋습니다.

```
git clone https://github.com/google/WebFundamentals.git
```

## 설정하기

새로운 DevSite 인프라는 종속성을 크게 단순화합니다. [Node](https://nodejs.org/en/) 10 이상이 있고 [Python 용 AppEngine SDK가](https://cloud.google.com/appengine/downloads#Google_App_Engine_SDK_for_Python) 이미 설치되어 있는지 확인하십시오.

1. `npm install` 실행 (빌드 프로세스에 필요)

## 자동 생성 파일 작성

일부 파일 (기고자 포함, 업데이트를위한 일부 페이지, 쇼케이스 등)이 자동으로 생성됩니다. 처음으로 repo를 복제하고 `npm install` 실행하면이 작업이 완료됩니다. 그러나 사례 연구, 업데이트 등을 추가 할 때는 다음을 사용하여 해당 파일을 다시 빌드해야합니다.

```
npm run build
```

## 로컬 서버 시작

사이트를 로컬로 보려면 다음을 실행하십시오.

```
npm start
```

**참고 :** 서버를 처음 시작할 때, 당신은 실행해야 할 수도 있습니다 `start-appengine.sh` 과에서 제공하는 프롬프트에 응답 `dev_appserver.py` .

## 코드 랩 업데이트

Code Lab을 업데이트하려면 [`claat`](https://github.com/googlecodelabs/tools/tree/master/claat) 도구가 필요하고 원본 문서 파일에 액세스해야합니다. 이것은 Google 직원에게만 해당됩니다.

1. `claat` 툴을 다운로드하여 `tools` 디렉토리에 배치하십시오.
2. `tools/update-codelabs.sh` 실행하십시오.
3. GitHub의 최신 변경 사항 확인

## 개발 서버를 시작하십시오

1. `npm start` 실행

## PR을 제출하기 전에 변경 사항을 테스트하십시오.

PR을 제출하기 전에 npm 테스트를 통해 변경 사항을 실행하십시오. 이 테스트는 DevSite에 문제를 일으킬 수있는 것을 찾고 콘텐츠의 일관성을 유지하려고합니다. 배포 프로세스의 일부이므로 오류가 있으면 PR이 실패합니다! 실행하려면

```
npm test
```
