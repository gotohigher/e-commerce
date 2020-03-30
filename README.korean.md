[✔]: assets/images/checkbox-small-blue.png

# Node.js 모범 사례

<h1 align="center">
  <img src="assets/images/banner-2.jpg" alt="Node.js Best Practices">
</h1>

<br/>

<div align="center">
  <img src="https://img.shields.io/badge/⚙%20Item%20count%20-%2086%20Best%20Practices-blue.svg" alt="86 items"> <img src="https://img.shields.io/badge/%F0%9F%93%85%20Last%20update%20-%20March%2012%202020-green.svg" alt="Last update: March, 2020"> <img src="https://img.shields.io/badge/ %E2%9C%94%20Updated%20For%20Version%20-%20Node%2012.12.0-brightgreen.svg" alt="Updated for Node 13.1.0">
</div>

<br/>

[![nodepractices](/assets/images/twitter-s.png)](https://twitter.com/nodepractices/) **트위터에서 팔로우 하세요!** [**@nodepractices**](https://twitter.com/nodepractices/)

<br/>

[:green_book: 포괄적인 Node.js 테스트 & 품질 모범사례 강의](https://testjavascript.com/)

<br/>

다른 언어로 읽기: [![CN](/assets/flags/CN.png)**CN**](/README.chinese.md), [![BR](/assets/flags/BR.png)**BR**](/README.brazilian-portuguese.md), [![RU](/assets/flags/RU.png)**RU**](/README.russian.md) [(![ES](/assets/flags/ES.png)**ES**, ![FR](/assets/flags/FR.png)**FR**, ![HE](/assets/flags/HE.png)**HE**, ![KR](/assets/flags/KR.png)**KR** and ![TR](/assets/flags/TR.png)**TR** 은 작업중입니다!)](#translations)

<br/>

###### [운영 위원회](#운영-위원회)와 [협력자분들](#협력자)이 구축하고 유지하고 있습니다

# 최근 모범사례와 뉴스

- **:tada: Node.js 모범사례가 4만 별에 도달했습니다**: Thank you to each and every contributor who helped turning this project into what it is today! We've got lots of plans for the time ahead, as we expand our ever-growing list of Node.js best practices even further.

- **:rocket: 새로운 모범사례 둘 추가**: We've been working hard on two new best practices, a section about [using npm ci](https://github.com/goldbergyoni/nodebestpractices#-519-install-your-packages-with-npm-ci) to preview the dependency state in production environments and another on [testing your middlewares in isolation](https://github.com/goldbergyoni/nodebestpractices#-413-test-your-middlewares-in-isolation)

- **:whale: Node.js + Docker 모범사례**: We've opened a [call for ideas](https://github.com/goldbergyoni/nodebestpractices/issues/620) to collect best practices related to running dockerized Node.js applications. If you've got any further best practices, don't hesitate to join the conversation!

<br/><br/>

# 어서오세요! 먼저 이 3가지를 알아두세요:

**1. 이 문서를 읽는 것은 베스트 Node.js 문서 수십개를 읽는 것과 같습니다. -** 이 문서는 Node.js 의 가장 일반적인 Best Practice 모범사례들을 모은 요약집 및 큐레이션입니다.

**2. 가장 큰 모음집이며, 매주 성장하고 있습니다. -** 현재 80개 이상의 모범사례들과 스타일 가이드 및 아키텍처 관련 팁들을 제공하고 있습니다. 이 문서를 계속 갱신하는 새로운 이슈들과 PR들이 매일 나오고 있습니다. 이 문서의 잘못된 코드를 고치거나 새로운 아이디어들을 제안하는 것은 매우 환영합니다. [마일스톤 보러가기](https://github.com/i0natan/nodebestpractices/milestones?direction=asc&sort=due_date&state=open)

**3. 항목 대부분은 추가적인 정보가 있습니다 -** 항목 옆쪽에 존재하는 **🔗자세히 보기** 링크에서 코드 예제, 참조 블로그 또는 기타 정보들을 확인 할 수 있습니다.

<br/><br/>

## 목차

1. [프로젝트 구조 설계 (5)](#1-프로젝트-구조-설계)
2. [에러 처리 방법 (11)](#2-에러-처리-방법)
3. [코드 스타일 (12) ](#3-코드-스타일)
4. [테스트 및 전체 품질 관리 (13) ](#4-테스트-및-전체-품질-관리)
5. [운영 환경으로 전환하기 (19) ](#5-운영-환경으로-전환하기)
6. [보안 (25)](#6-보안)
7. [성능 (2) (현재진행형 ✍️)](#7-초안-성능)

<br/><br/>

# `1. 프로젝트 구조 설계`

## ![✔] 1.1 컴포넌트 기반으로 설계하라

**핵심요약:** 큰 프로젝트에서 빠지기 쉬운 최악의 함정은 수백개의 의존성을 가진 커다란 소스코드를 유지보수하는 것이다. 그렇게 단일로 통째로 짜여진 monolith 코드는 개발자가 새로운 기능들을 추가하는 속도를 느려지게 한다. 그 대신에 코드를 컴포넌트로 나누고, 각각의 컴포넌트가 자신의 폴더 혹은 할당된 코드베이스를 안에서 각 단위가 작고 간단하게 유지되도록 해라. 아래의 '자세히 보기'를 눌러 올바른 프로젝트 구조의 예시를 확인해라.

**그렇게 하지 않을 경우:** 새로운 기능을 작성하는 개발자가 변경사항이 어떻게 영향을 미치는지 알기가 힘들어 의존하고 있는 다른 컴포넌트를 망칠까봐 두려워 하면 배포는 느려지고 더 위험해진다. 비지니스 단위가 나눠져 있지 않으면 확장(scale-out)하기도 쉽지 않다.

🔗 [**자세히 보기: 컴포넌트로 구조화하기**](/sections/projectstructre/breakintcomponents.korean.md)

<br/><br/>

## ![✔] 1.2 컴포넌트를 계층화(layer)하고, Express를 그 경계 안에 둬라.

**핵심요약:** 각각의 컴포넌트는 웹, 로직, 데이터 접근 코드을 위한 객체인 '계층'을 포함해야 한다. 이것은 우려할 만한 요소들을 깨끗하게 분리할 뿐만 아니라 모의 객체(mock)를 만들어 테스트하기 굉장히 쉽게 만든다. 이것이 굉장히 일반적인 패턴임에도, API 개발자들은 웹 계층의 객체 (Express req, res)를 비지니스 로직과 데이터 계층으로 보내서 계층을 뒤섞어버리는 경향이 있다. 그렇게 하는것은 당신의 어플리케이션에 의존성을 만들고 Express에서만 접근 가능하도록 만든다.

**그렇게 하지 않을 경우:** 웹 객체와 다른 계층을 뒤섞은 앱은 테스트 코드, CRON 작업이나 Express가 아닌 다른 곳에서의 접근을 불가능하게 한다.

🔗 [**자세히 보기: 앱을 계층화하기**](/sections/projectstructre/createlayers.korean.md)

<br/><br/>

## ![✔] 1.3 유틸리티들을 NPM 패키지로 감싸라(wrap)

**핵심요약:** 커다란 코드 기반으로 구성되어있는 커다란 앱에서는 로깅, 암호화 같은 횡단 관심사(cross-cutting-concern)가 있는 유틸의 경우 당신이 쓴 코드로 감싸진 private NPM package의 형태로 노출이 되어야 한다. 이것은 여러 코드 기반과 프로젝트들에게 그것들을 공유할 수 있도록 해준다.

**그렇게 하지 않을 경우:** 당신 자신만의 배포 및 의존성 바퀴(wheel)를 새로 발명해야 할 것이다.

🔗 [**자세히 보기: 기능으로 구조화 하기**](/sections/projectstructre/wraputilities.korean.md)

<br/><br/>

## ![✔] 1.4 Express의 app과 server를 분리하라

**핵심요약:** [Express](https://expressjs.com/)앱을 통째로 하나의 큰 파일에 정의하는 나쁜 습관은 피해라 - 'Express' 정의를 최소한 둘로는 나누자: API 선언(app.js)과 네트워크 부분(WWW). 더 좋은 구조는 API 선언을 컴포넌트 안에 놓는 것이다.

**그렇게 하지 않을 경우:** HTTP 요청으로만 API 테스트가 가능해진다 (커버리지 보고서를 생성하기가 더 느려지고 훨씬 힘들어진다). 수백줄의 코드를 하나의 파일에서 관리하는 것이 크게 즐겁지는 않을 것이다.

🔗 [**자세히 보기: Express를 'app'과 'server'로 분리하기**](/sections/projectstructre/separateexpress.korean.md)

<br/><br/>

## ![✔] 1.5 환경을 인식하는, 보안적인, 계층적인 설정을 사용하라

**핵심요약:** 완벽하고 결점이 없는 구성 설정은 (a) 파일과 환경 변수 모두에서 키 값을 읽을 수 있어야하고 (b) 보안 값들은 커밋된 코드 밖에서 관리되어야하고 (c) 설정은 좀 더 쉽게 찾을 수 있도록 계층적으로 관리해야 한다. [rc](https://www.npmjs.com/package/rc), [nconf](https://www.npmjs.com/package/nconf), [config](https://www.npmjs.com/package/config)와 같이 이러한 요구사항을 동작하게 해주는 몇가지 패키지가 존재한다.

**그렇게 하지 않을 경우:** 위의 구성 요구사항 중 하나라도 만족시키지 못한다면 개발팀 혹은 데브옵스팀을 늪으로 몰아갈 수 있다. 십중팔구 둘 다.

🔗 [**자세히 보기: 구성 모범 사례**](/sections/projectstructre/configguide.korean.md)

<br/><br/><br/>

<p align="right"><a href="#목차">⬆ 목차로 돌아가기</a></p>

# `2. 에러 처리 방법`

## ![✔] 2.1 비동기 에러 처리시에는 async-await 혹은 promise를 사용하라

**핵심요약:** 비동기 에러를 콜백 스타일로 처리하는 것은 지옥으로 가는 급행열차나 마찬가지 (혹은 파멸의 피라미드). 당신이 코드에 줄 수 있는 가장 큰 선물은 평판이 좋은 promise 라이브러리를 사용하거나 훨신 작고 친숙한 코드 문법인 try-catch를 사용하게 해주는 async-await를 사용하는 것이다.

**그렇게 하지 않을 경우:** Node.js 콜백 스타일인 function(err, response)는 에러 처리와 일반 코드의 혼합, 코드의 과도한 중첩, 이상한 코딩 패턴 때문에 유지보수가 불가능한 코드로가는 확실한 길이다.

🔗 [**자세히 보기: 콜백 피하기**](/sections/errorhandling/asyncerrorhandling.korean.md)

<br/><br/>

## ![✔] 2.2 내장된 Error 객체만 사용하라

**핵심요약:** 많은 사람들이 문자열이나 사용자가 임의로 정의한 타입으로 에러를 던진다(throw). 이것은 에러처리 로직과 모듈 사이의 상호운영성을 복잡하게 한다. 당신이 promise를 거부(reject)하든, 예외를 던지든, 에러를 냈건 내장된 Error 객체를 이용하는 것은 균일성을 향상하고 정보의 손실을 방지하게 만들것이다.

**그렇게 하지 않을 경우:** 일부 컴포넌트를 호출할때 어떤 에러의 타입이 반환될지 불확실해져서 적절한 에러처리가 매우 어려워 질것이다. 게다가 임의적인 타입으로 에러를 나타내는 것은 스택 정보(stack trace)와 같은 중요한 에러 관련 정보 손실을 일으킬 수 있다!

🔗 [**자세히 보기: 내장된 Error 객체 사용하기**](/sections/errorhandling/useonlythebuiltinerror.korean.md)

<br/><br/>

## ![✔] 2.3 동작상의 에러와 프로그래머 에러를 구분하라

**핵심요약:** API에서 잘못된 입력을 받는 것과 같은 동작상의 에러는 에러의 영향을 완전히 이해할수 있고 신중하게 처리 할수있는 알려진 경우를 의미한다. 반면에, 정의되지 않은 변수를 읽는 것과 같은 프로그래머 에러는 어플리케이션을 안정적으로 다시 시작하게 만드는 알수 없는 코드 에러를 의미한다.

**그렇게 하지 않을 경우:** 에러가 날 때마다 어플리케이션을 다시 시작할수도 있지만, 왜 사소하고 예측가능한 동작상의 오류때문에 5000명의 온라인 사용자를 다운시키는 것인가? 그 반대도 이상적이지 않다. 알수없는 이슈 (프로그래머 에러)가 났는데 어플리케이션을 그대로 두는 것은 예측이 불가능한 반응을 일으킬 수 있다. 두 가지를 구별하면 요령있는 처신과 주어진 상황에 따른 균형잡힌 접근이 가능하다.

🔗 [**자세히 보기: 동작상의 에러와 프로그래머 에러**](/sections/errorhandling/operationalvsprogrammererror.korean.md)

<br/><br/>

## ![✔] 2.4 에러를 Express 중간에서 처리하지 말고 한군데에서 집중적으로 처리해라

**핵심요약:** 관리자에게 메일을 보내거나 로깅을 하는 것과 같은 에러 처리는 에러가 발생할 때 모든 엔드포인트 (예를 들어 Express 미들웨어, cron 작업, 단위 테스트 등)가 호출하는 에러전용 중앙집중 객체로 캡슐화 되어야한다.

**그렇게 하지 않을 경우:** 한 곳에서 에러를 처리하지 않는 것은 코드 중복과 부적절한 에러처리로 이어진다.

🔗 [**자세히 보기: 중앙집중적으로 에러 처리하기**](/sections/errorhandling/centralizedhandling.korean.md)

<br/><br/>

## ![✔] 2.5 Swagger를 이용해 API 에러를 문서화하라

**핵심요약:** API를 호출자들에게 어떤 에러가 돌아올 수 있는지 미리 알려주어서 에러를 충돌없이 신중하게 처리 할 수 있게 해주어라. RESTful API같은 경우엔 Swagger 같은 API 문서화 프레임워크를 통해 이루어진다. GraphQL의 경우엔 개요(schema)와 주석을 활용할 수 있다.

**그렇게 하지 않을 경우:** API 클라이언트는 알수 없는 에러로 인한 충돌 후에 재시작을 결정할수도 있을 것이다. 참고로 당신의 API를 호출한 사람이 당신 자신 일수도 있다 (마이크로서비스 환경에서는 아주 일반적이다).

🔗 [**자세히 보기: Swagger에서 에러 문서화하기**](/sections/errorhandling/documentingusingswagger.korean.md)

<br/><br/>

## ![✔] 2.6 이상한 것이 들어왔을때 프로세스를 정상적으로 중단하라

**핵심요약:** 알수 없는 에러(프로그래머 에러, 모범사례 2.3번 참조)가 발생하면 어플리케이션의 건강상태가 불확실해진다. 일반적인 방법은 [Forever](https://www.npmjs.com/package/forever)나 [PM2](http://pm2.keymetrics.io/) 같은 '재시작' 도구로 프로세스를 다시 시작하는 것이다.

**그렇게 하지 않을 경우:** 익숙치 않은 예외가 발생하면 일부 객체가 오류 상태 (예를 들어 전역적으로 사용되지만 내부 오류로 인해 이벤트를 더이상 내보내지 않는 event emitter)라서 향후의 모든 요청을 실패시키거나 미친듯이 작동할 수 있다.

🔗 [**자세히 보기: 프로세스 중단하기**](/sections/errorhandling/shuttingtheprocess.korean.md)

<br/><br/>

## ![✔] 2.7 에러 확인을 용이하게 해주는 안정된 로거를 사용하라

**핵심요약:** Winston, Bunyan 혹은 Log4J와 같이 자리를 잡은 로깅 도구들은 에러를 발견하고 이해하는 속도를 높여준다. 그러니 console.log은 잊어버려라.

**그렇게 하지 않을 경우:** 로그 검색 도구나 제대로 된 로그 뷰어 없이 console.log을 훑어보거나 복잡하게 꼬인 텍스트 파일을 일일이 읽어 보는 것은 야근을 부를 수 있다.

🔗 [**자세히 보기: 발전된 로거를 사용하기**](/sections/errorhandling/usematurelogger.korean.md)

<br/><br/>

## ![✔] 2.8 당신이 선호하는 테스트 프레임워크로 에러 흐름을 테스트하라

**핵심요약:** 전문 자동화 QA든 일반 수동 개발자 테스트든 당신의 코드가 긍정적인 상황에서 잘 동작할 뿐만 아니라 올바른 에러를 처리하고 반환하는지 확실히 하라. Mocha & Chai와 같은 테스트 프레임워크는 이것을 쉽게 처리 할수 있다("Gist popup"안의 코드 예제를 확인하라).

**그렇게 하지 않을 경우:** 자동이든 수동이든 테스트가 없다면 당신은 당신의 코드가 올바른 에러를 반환하는지 믿지 못할 것이다. 의미가 있는 에러가 없다면 에러 처리는 없는 것이다.

🔗 [**자세히 보기: 에러 흐름 테스트하기**](/sections/errorhandling/testingerrorflows.korean.md)

<br/><br/>

## ![✔] 2.9 APM 제품을 사용하여 에러와 다운타임을 확인하라

**핵심요약:** APM이라고 불리는 모니터링 및 성능 제품은 미리 알아서 코드베이스와 API를 측정하고 자동적으로 당신이 놓친 에러, 충돌, 느린부분을 강조 표시해준다.

**그렇게 하지 않을 경우:** API의 성능과 다운타임을 측정하기위해 많은 노력을 들여야 할지도 모른다. 아마 당신은 실제 상황에서 어떤 코드 부분이 가장 느린지, 그것이 UX에 어떻게 영향을 미칠지 절대 알수없을 것이다.

🔗 [**자세히 보기: APM 제품 사용하기**](/sections/errorhandling/apmproducts.korean.md)

<br/><br/>

## ![✔] 2.10 처리되지 않은 promise 거부(unhandled promise rejection)를 잡아라

**핵심요약:** promise안에서 발생한 예외는 개발자가 명시적으로 처리하는 것을 잊게되면 삼켜지고 버려지게 된다. 당신의 코드가 process.uncaughtException 이벤트를 구독하고 있다고해도 말이다! 이것을 극복하기위해 process.unhandledRejection 이벤트를 등록하라.

**그렇게 하지 않을 경우:** 당신의 에러는 삼켜지고 어떤 흔적도 남기지 않을 것이다. 걱정할 것이 없긴 하다.

🔗 [**자세히 보기: 처리되지 않은 promise 거부 잡기**](/sections/errorhandling/catchunhandledpromiserejection.korean.md)

<br/><br/>

## ![✔] 2.11 전용 라이브러리를 이용해 인자값이 유효한지 검사하여 빠르게 실패하라(fail fast)

**핵심요약:** 나중에 처리하기가 더 힘들어지는 지저분한 버그를 피하기 위해 Assert API입력은 당신의 Express 모범사례가 되어야 한다. 당신이 Joi와 같은 유용한 헬퍼 라이브러리를 사용하지 않는 이상 유효성 검사 코드는 일반적으로 지루하다.

**그렇게 하지 않을 경우:** 이런 상황을 생각해보자. 당신의 함수가 "Discount"라는 숫자를 받아야하는데 요청하는 사람이 넘겨주는 것을 깜빡했다. 그 후에 당신의 코드는 Discount!=0인지 아닌지 체크한다(사실 허용된 Discount의 값은 0보다 커야 한다). 그러면 사용자가 할인을 받게될 것이다. 보이는가? 엄청나게 지저분한 버그이다.

🔗 [**자세히 보기: 빠르게 실패하기**](/sections/errorhandling/failfast.korean.md)

<br/><br/><br/>

<p align="right"><a href="#목차">⬆ 목차로 돌아가기</a></p>

# `3. 코드 스타일`

## ![✔] 3.1 ESLint를 사용하라

**핵심요약:** [ESLint](https://eslint.org)는 발생 가능한 코드 에러를 체크하고 껄끄러운 간격(spacing)문제를 식별하는 것부터 프로그래머가 분별없이 에러를 던지는 것과 같은 코드의 심각한 안티 패턴을 감지하여 코드 스타일을 바꾸는 것에 대한 사실상의 표준이다. ESLint도 자동으로 코드스타일을 고칠 수 있지만 [prettier](https://www.npmjs.com/package/prettier)와 [beautify](https://www.npmjs.com/package/js-beautify)같은 수정 부분의 포맷을 맞춰주는 강력한 툴이 있고 ESLint와 함께 작동된다.

**그렇게 하지 않을 경우:** 프로그래머가 쓸데없는 간격과 한줄의 길이(line-width) 문제에 대해서 집중해야하고 프로젝트의 코드스타일에 대해 과도하게 생각하느라 시간을 낭비해야할 수도 있다.

<br/><br/>

## ![✔] 3.2 Node.js에 특화된 플러그인들

**핵심요약:** vanlla JS만 지원하는 ESLinst의 표준 규칙 위에 [eslint-plugin-node](https://www.npmjs.com/package/eslint-plugin-node), [eslint-plugin-mocha](https://www.npmjs.com/package/eslint-plugin-mocha), [eslint-plugin-node-security](https://www.npmjs.com/package/eslint-plugin-security)와 같은 Node에 특화된 플러그인을 같이 사용하라.

**그렇게 하지 않을 경우:** 많은 결함이 있는 Node.js 코드 패턴들이 레이더에서 벗어날 수 있다. 예를 들어 프로그래머는 변수로된 파일경로를 이용해 `require(파일경로변수)`로 파일을 가져올수 있다. 이것은 공격자들이 어떤 JS script도 실행시킬 수 있게 한다. Node.js linter는 그러한 패턴을 감지하고 미리 알려준다.

<br/><br/>

## ![✔] 3.3 코드 블록의 중괄호를 같은 줄에서 시작하라

**핵심요약:** 블록에서 중괄호를 여는 부분은 코드를 여는 문장과 같은 줄에 있어야 한다.

### 코드 예제

```javascript
// Do
function someFunction() {
  // code block
}

// Avoid
function someFunction()
{
  // code block
}
```

**그렇게 하지 않을 경우:** 이 모범사례를 적용하지 않는 것은 아래의 Stackoverflow 스레드에서 보는 바와 같이 예기치못한 결과로 이어질 수 있다.

🔗 [**자세히 보기:** "왜 결과가 중괄호의 위치에 따라 달라지는 거죠?" (Stackoverflow)](https://stackoverflow.com/questions/3641519/why-does-a-results-vary-based-on-curly-brace-placement)

<br/><br/>

## ![✔] 3.4 세미콜론을 잊지 마라

**핵심요약:** 만장일치로 동의하지는 않겠지만 각 문장의 끝에 세미콜론을 붙이는 것은 여전히 권장사항이다. 이것은 당신의 코드를 읽는 다른 프로그래머가 좀더 잘 읽게하고 명확하게 할것이다.

**그렇게 하지 않을 경우:** 이전 섹션에서 본것처럼 자바스크립트의 인터프리터는 세미콜론이 없으면 의도되지 않은 결과를 야기할수 있기에 자동으로 문장의 끝에 세미콜론을 붙인다.

<br/><br/>

## ![✔] 3.5 함수에 이름을 붙여라

**핵심요약:** 클로저와 콜백을 포함한 모든 함수에 이름을 붙여라. 익명함수를 피해라. 이것은 노드 앱을 프로파일링 할때 특히 유용하다. 모든 함수를 명명하는 것은 당신이 메모리 스냅샷을 확인할때 당신이 보고있는 것이 무엇인지 쉽게 이해 할수있도록 해준다.

**그렇게 하지 않을 경우:**
당신이 익명함수에서 메모리 소비가 많다는 것을 확인 했을 때 코어 덤프(메모리 스냅샷)을 이용해 프로덕션 문제를 디버깅하는 것이 어려울 수도 있습니다.

<br/><br/>

## ![✔] 3.6 변수, 상수, 함수, 클래스의 명명 규칙(naming convention)

**핵심요약:** 상수와 변수 함수를 명명할때는 **_lowerCamelCase_** 를 사용하고 클래스를 명명 할때는 **_UpperCamelCase_**(첫 글자 대문자)를 사용하라. 이것은 일반 변수/함수와 인스턴스로 만들어야 하는 클래스를 구분하는데 도움을  것이다. 설명이 포함된 이름을 사용하되 이름을 짧게 유지하도록 해라.

**그렇게 하지 않을 경우:** 자바스크립트는 먼저 인스턴스로 만들지 않고 직접 생성자("Class")를 호출할 수 있는 세계 유일의 언어이다. 그러므로 클래스와 함수생성자는 UpperCamelCase를 통해 구분된다.

### 코드예제

```javascript
// 클래스명은 UpperCamelCase 사용
class SomeClassExample {}

// 상수명은 const 키워드와 lowerCamelCase 사용
const config = {
  key: 'value'
};

// 변수와 함수 이름은 lowerCamelCase 사용
let someVariableExample = 'value';
function doSomething() {}
```

<br/><br/>

## ![✔] 3.7 let보다는 const를 사용하라. var는 갖다버려라

**핵심요약:** `const`를 사용한다는 것은 변수에 한번 값이 할당되면 다시 할당할 수 없다는 것을 의미한다. `const`를 선호하는 것은 같은 변수를 다른 용도로 사용하는 것을 방지하고 당신의 코드를 더 깔끔하게 만드는데 도움을 준다. for루프처럼 변수가 재할당 되어야 할 필요가 있으면 `let`을 사용하여 선언하라. `let`의 또 다른 중요한 부분은 선언된 변수를 사용하는 것이 변수가 정의된 블록범위(block scope) 안에서만 가능하다는 것이다. `var`는 블록범위가 아니라 함수범위(function scope)이며 이제 대신할 수 있는 const와 let이 있으므로 [ES6에서는 사용하면 안된다](https://hackernoon.com/why-you-shouldnt-use-var-anymore-f109a58b9b70).

**그렇게 하지 않을 경우:** 자주 변경되는 변수를 따라가려면 디버깅이 훨씬 더 번거로워 진다.

🔗 [**자세히 보기: JavaScript ES6+: var 혹은 let 혹은 const?**](https://medium.com/javascript-scene/javascript-es6-var-let-or-const-ba58b8dcde75)

<br/><br/>

## ![✔] 3.8 require는 맨 처음에 오게하고 함수 안에서의 사용은 피하라

**핵심요약:** 모듈을 각 파일의 시작이나 모든 함수의 앞부분 혹은 밖에서 require하라. 이 간단한 모범사례는 파일의 의존성을 맨 위에서 쉽고 빠르게 구분 할수 있게 해줄 뿐만 아니라 여러 잠재적인 문제를 피하게 해준다.

**그렇게 하지 않을 경우:** require는 Node.js에서 동기로 실행된다. 함수 안에서 호출되면 다른 요청들을 더 중요한 시간에 처리되지 못하도록 막을 수 있다. 또한 require된 모듈이나 그것의 의존 모듈이 에러를 뱉거나 서버를 다운시키면, 함수 안에서 그 모듈이 require된 것이 아닌지 가능한 아주 빠르게 찾아야 할 것이다.

<br/><br/>

## ![✔] 3.9 require는 파일에 직접하지말고 폴더에 하라

**핵심요약:** 폴더에서 모듈과 라이브러리를 개발할 때 모든 소비자가 그것을 거치도록  모듈의 내부를 노출하는 index.js 파일을 둬라. 이것은 모듈의 '인터페이스'역할을 하며 계약을 위반하지 않으면서 미래의 변경사항에 대해 유연하게 대처하도록 해준다.

**그렇게 하지 않을 경우:** 파일 내부의 구조 혹은 서명을 변경하면 클라이언트와의 인터페이스가 손상될 수 있다.

### 코드 예제

```javascript
// 이렇게 하라
module.exports.SMSProvider = require('./SMSProvider');
module.exports.SMSNumberResolver = require('./SMSNumberResolver');

// 피하라
module.exports.SMSProvider = require('./SMSProvider/SMSProvider.js');
module.exports.SMSNumberResolver = require('./SMSNumberResolver/SMSNumberResolver.js');
```

<br/><br/>

## ![✔] 3.10 `===` 연산자를 사용하라

**핵심요약:** 약하고 추상적인 같음연산자 `==` 보다 엄격한 항등연산자 `===`를 선호한다. `==`는 두 변수를 공용 타입으로 변환한 후에 비교한다. `===`에는 타입 변환이 없고 두 변수가 같으려면 타입도 같아야 한다.

**그렇게 하지 않을 경우:** `==`으로 비교하는 경우 같지 않은 변수가 true로 반환 될 수있다.

### 코드 예제

```javascript
'' == '0'           // false
0 == ''             // true
0 == '0'            // true

false == 'false'    // false
false == '0'        // true

false == undefined  // false
false == null       // false
null == undefined   // true

' \t\r\n ' == 0     // true
```

위의 모든 문장은 `===`를 사용했다면 false를 반환 했을것이다.

<br/><br/>

## ![✔] 3.11 async-await을 사용하고 콜백을 피하라

**핵심요약:** Node 8의 LTS 버전은 현재 async-await을 완전히 지원한다. 이것은 콜백과 promise를 대체하여 비동기 코드를 다루는 새로운 방법이다. async-await은 비차단적(non-blocking)이고 비동기 코드를 동기 코드처럼 보이게 만든다. 당신의 코드에게 줄수 있는 최고의 선물은 try-catch와 같은 더 작고 친숙한 코드 구문을 제공하는 async-await을 사용하는 것이다.

**그렇게 하지 않을 경우:** 콜백 스타일로 비동기 에러를 처리하는 것은 아마도 지옥으로 가는 가장 빠른 방법일것이다. 이런 스타일은 에러를 전부 확인하게 하고 어색한 코드 중첩을 다루게하며 코드 흐름을 추론하기 어렵게 만든다.

🔗[**자세히 보기: async-await 1.0 가이드**](https://github.com/yortus/asyncawait)

<br/><br/>

## ![✔] 3.12 두꺼운(=>) 화살표 함수를 사용하라

**핵심요약:** async-await을 사용하고 함수 인자를 사용하는 것을 피하는 것이 권장되지만 promise와 콜백을 받는 예전 API를 다룰 때는 화살표 함수가 코드 구조를 더 작게해주고 루트 함수의 어휘적 맥락(lexical context)을 유지시켜 준다. (예를 들어 'this')

**그렇게 하지 않을 경우:** 더 긴 코드(ES5의 function)은 버그에 더 취약하고 읽기가 번거롭다.

🔗 [**Read mode: 화살표 함수를 받아들일 시간이다**](https://medium.com/javascript-scene/familiarity-bias-is-holding-you-back-its-time-to-embrace-arrow-functions-3d37e1a9bb75)

<br/><br/><br/>

<p align="right"><a href="#목차">⬆ 목차로 돌아가기</a></p>

# `4. 테스트 및 전체 품질 관리`

## ![✔] 4.1 다른건 못해도 최소한 API (컴포넌트) 테스트는 써라

**핵심요약:** 대부분의 프로젝트는 짧은 일정으로 인해 자동화 된 테스트가 없거나 후일 통제를 벗어난 이른바 "시험용 프로젝트" 라는 이유로 버려진다. 그러므로, 쓰기에도 제일 쉽고 유닛 테스트보다 더 넓게 커버할 수 있는 API 테스트를 우선으로 시작해라. ([포스트맨](https://www.getpostman.com/) 같은 도구를 이용하지 않고도 API 테스트를 쓸 수 있다.) 나중에 시간과 자원이 더 나면 그때 유닛테스트, DB 테스트, 성능 테스트 등의 상급 테스트 종류를 계속 더해라.

**그렇게 하지 않을 경우:** 유닛 테스트 쓰는데 시간을 몇 날 며칠을 쓰고선 시스템 커버리지가 20% 밖에 안된다는걸 깨닫게 될 수 있다.

<br/><br/>

## ![✔] 4.2 테스트 이름에 이 3가지를 포함해라

**핵심요약:** 코드 내부에 익숙하지 않은 QA 엔지니어들과 개발자들에게 따로 설명이 필요 없도록 requirement 계층에서 이미 자명하도록 해라. 무엇을 (unit under test), 어떤 환경에서, 어떤 결과를 예상하고 테스트 하는 것인지 테스트 이름에 명시해라.


**그렇게 하지 않을 경우:** 배포에 실패하였습니다, “프로덕트 추가” 라는 테스트가 실패했습니다. 이런 메시지를 보고 정확히 뭐가 잘못되었는지 알 수 있는가?

🔗 [**자세히 보기: Include 3 parts in each test name**](/sections/testingandquality/3-parts-in-name.md)

<br/><br/>

## ![✔] 4.3 AAA 패턴을 따라 테스트를 구축해라

**핵심요약:** 테스트를 다음 세가지 부분으로 명확히 나누어라: 준비 (arrange), 실행 (act), 표명 (assert). 제일 먼저 테스트를 준비하고, 그 다음 테스트 단위(unit under test)를 실행하고, 마지막은 확인 단계다. 이 구조를 따르는 것은 읽는이가 힘들여 머리를 쓰지 않고도 테스트 설계를 이해할 수 있도록 보장한다.


**그렇게 하지 않을 경우:** 매일 하루종일 주요 코드를 읽는데 시간을 오래 쓰는 것도 모자라 간단해야하는 테스트 부분에도 열심히 머리를 써야 할 것이다.


🔗 [**Read More: Structure tests by the AAA pattern**](/sections/testingandquality/aaa.md)

<br/><br/>

## ![✔] 4.4 린터를 사용해서 코드의 문제점을 찾아내라

**핵심요약:** 코드 린터를 사용해서 기본적인 품질을 확인하고 안티패턴을 초기에 찾아내라. 테스트 하기도 전에 먼저 돌리고, pre-commit git-hook으로 추가해서 문제를 검토하고 정정하는 시간을 최소화해라.  [3문단](#3-코드-스타일)의 코드 스타일 관례들도 확인해라.

**그렇게 하지 않을 경우:** 안티패턴과 취약한 코드가 상용환경에 넘어갈 수도 있다.

<br/><br/>

## ![✔] 4.5 고정적인 공용 테스트 데이터나 seeds는 피하고, 테스트별로 데이타를 붙여라

**핵심요약:** 테스트간의 간섭과 결합도(coupling)를 최소화하고 테스트 흐름을 추론하기 쉽도록 테스트들은 각자 자신만의 DB 행 (row) 집합을 만들어 써야 한다. 테스트가 DB 데이터를 가져오거나 데이터가 존재한다고 간주해야 할 때마다 다른 레코드를 변형시키지 않도록 그 데이터를 직접 추가하여야 한다.


**그렇게 하지 않을 경우:** 실패하는 테스트때문에 전개(deployment)가 중단되었다고 가정해 보자. 이제 팀원들은 소중한 시간을 조사하는데 소모한 뒤 슬픈 결론을 내릴 것이다: 시스템은 잘 작동하지만, 테스트간의 상호 간섭으로 빌드 실패.

🔗 [**자세히 보기: Avoid global test fixtures**](/sections/testingandquality/avoid-global-test-fixture.md)

<br/><br/>

## ![✔] 4.6 끊임없이 취약한 dependency를 점검해라

**핵심요약:** Express같은 네임드 dependency도 알려진 취약점이 있다. 이건CI에서 매 빌드마다 호출할 수 있는 🔗 [npm audit](https://docs.npmjs.com/cli/audit)나 🔗 [snyk.io](https://snyk.io)같은 커뮤니티 혹은 상업용 도구를 사용하면 쉽게 해결할 수 있다.

**그렇게 하지 않을 경우:** 전용 도구없이 코드를 취약점 없이 깨끗하게 유지하려면 온라인 출판물들을 항상 유심히 찾아읽어야 한다. 꽤나 성가신 일이다.

<br/><br/>

## ![✔] 4.7 테스트를 태그하라 (#테스트)

**핵심요약:** 다른 종류의 테스트는 서로 다른 시나리오를 바탕으로 실행해야한다: 빌드 성공유무 테스트(quick smoke)나 IO 입출력이 없는 테스트는 개발자가 파일을 저장하거나 commit 할 때마다 실행하고, 더 포괄적인 단대단 (end-to-end 테스트는 보통 풀리퀘스트를 제출 할 때마다 실행한다. 이건 테스트를 #cold #api #sanity 와 같은 키워드로 태그해서 원하는 테스트들만 부분적으로 grep 으로 test harness를 검색해서 실행하면 된다. 예를 들면, [모카](https://mochajs.org/)에서 sanity 테스트 그룹만 실행하고 싶다면 이렇게 하면 된다: mocha --grep 'sanity'

**그렇게 하지 않을 경우:** 개발자가 조금씩 코드를 바꿀때마다 DB 쿼리를 한다스씩 보내는 테스까지 포함해서 전부 실행하면 개발 속도도 느려지고 개발자들도 점점 테스트를 실행하는걸 꺼리게 된다

<br/><br/>

## ![✔] 4.8 테스트 범위를 확인하여 안좋은 테스트 패턴을 잡아내라

**핵심요약:** [이스탄불](https://github.com/istanbuljs/istanbuljs)/[NYC](https://github.com/istanbuljs/nyc)같은 코드 커버리지 도구가 좋은 이유는 세가지가 있다: 무료이고 (거져먹는거다), 테스트 범위가 줄어드는것을 잡아내주고, 마지막으로 테스트 부조화를 하이라이트한다: 색으로 나타낸 코드 커버리지 리포트를 보다보면 catch 절 같이 테스트 하지 않는 부분들을 알아채기 시작할것이다. (테스트는 보통 경로만 테스트하기에 앱이 에러가 날 경우에는 어떻게 반응하는지는...) 커버리지가 일정 기준 이하로 떨어지면 빌드가 자동으로 실패하게 해라.

**그렇게 하지 않을 경우:** 코드의 상당한 범위가 테스트로 커버되지 않더라도 자동적으로 측정하여 알 길이 없다.

<br/><br/>

## ![✔] 4.9 오래되어 뒤떨어진 패키지는 없는지 검사해라

**핵심요약:** 설치된 패키지중 outdated 된 패키지는 없는지 선호하는 도 (예: 'npm outdated'나 [npm-check-updates](https://www.npmjs.com/package/npm-check-updates))를 써서 확인하고, 심할 경우 빌드가 실패하도록 CI 경로에 이 체크를 주입해라. 예를 들면, 설치된 패키지가 패치 commit 5개 이상 뒤쳐졌거나 (예: 로컬은 1.3.1버젼인데 repository 버젼은 1.3.8이라던가) 제작자가 deprecated 되었다고 태그하면 빌드를 죽이고 이 버젼을 배포하지 못하게 막아라.

**그렇게 하지 않을 경우:** 제작자가 직접 불안정하다고 태그한 패키지가 프로덕션에서 놀아날 수 있다

<br/><br/>

## ![✔] 4.10 프로덕션과 비슷한 환경에서 e2e 테스트를 실행해라

**핵심요약:** 실제 데이터를 쓰는 end to end 테스트는 DB같은 여러 묵직한 서비스에 의존하기에 CI 프로세스의 취약점이었다. 가능한 한 프로덕션과는 최대한 동떨어진 환경을 써라.

**그렇게 하지 않을 경우:** docker-compose 없이는 팀들이 테스트 환경별로 (각 개발자의 컴퓨터 포함) 테스트 DB를 유지하고, 환경에 따라 테스트 결과가 다르게 나오지 않도록 이 모든 DB들을 동기화해야한다.

<br/><br/>

## ![✔] 4.11 정적분석도구를 이용해서 refactor를 정기적으로 해라

**핵심요약:** 정적분석도구(static analysis tool)는 코드의 품질을 객관적으로 개선하고 코드 유지를 쉽게 해준다. 코드스멜을 감지하면 CI 빌드가 실패하도록 정적분석도구를 넣어주면 된다. 이게 단순한 린보팅다 나은 주된 이유로는 여러 파일에 걸친 맥락에서 품질을 점검할 수 있다는 점 (예: 중복된 코드 감지), 더 발달된 분석을 할 수 있다는 점 (예: 코드 복잡도), 코드 문제의 전적과 진전을 따라 볼 수 있다는 점이 있다. 쓸만한 도구의 예를 두가지를 들자면 [Sonarqube](https://www.sonarqube.org/) (2,600+ [stars](https://github.com/SonarSource/sonarqube)) 와 [Code Climate](https://codeclimate.com/) (1,500+ [stars](https://github.com/codeclimate/codeclimate))가 있다.

**그렇게 하지 않을 경우:** 제아무리 반짝이는 새로나온 라이브러리나 최첨단 기능을 써봤자 코드 품질이 불량하면 버그와 성능은 못고친다

🔗 [**자세히 보기: Refactoring!**](/sections/testingandquality/refactoring.md)

<br/><br/>

## ![✔] 4.12 CI 플랫폼은 신중하게 선택해라 (Jenkins vs CircleCI vs Travis vs 나머지)

**핵심요약:** 지속적 통합 플랫폼(CICD)은 품질 관리 도구(예: 테스트, 린트)들을 돌릴 수 있게 플러그인 생태계가 활발해야 한다. 예전에는 대부분의 프로젝트들이 배우기는 어려워도 커뮤니티도 제일 크고 강력한 플랫폼을 가진 [Jenkins](https://jenkins.io/)를 기본으로 썼다. 요즘엔 [CircleCI](https://circleci.com)등의 SaaS 해결책을 쓰는게 훨씬 더 쉬워졌다. 이런 도구들은 인프라 전체를 관리하는 부담 없이도 유연한 CI 경로를 만들 수 있게 해준다. 결국에는 안전성과 빠름의 상호 절충이다 - 조심해서 선택해라.

**그렇게 하지 않을 경우:** 잘 알려지지 않은 중소 솔루션 업체를 쓰다간 흔치 않은 고급 설정을 써야할 때 막혀버릴 수도 있다. 하지만 반대로, Jenkins를 택하면 인프라를 수축하는데 소중한 시간을 다 빼앗길 수도 있다.

🔗 [**자세히 보기: Choosing CI platform**](/sections/testingandquality/citools.korean.md)

## ![✔] 4.13 미들웨어들은 따로 테스트해라

**핵심요약:** 여러 요청에 걸친 막대한 로직을 미들웨어가 수용하는 경우, 웹 프레임워크 전체를 깨우지 않고 따로 테스트 할만한 가치가 있다. {req, res, next} 객체들을 스텁(stub)하여 염탐(spy)하면 쉽게 달성할 수 있다.

**그렇게 하지 않을 경우:**  Express 미들웨어의 버그 === 거의 모든 요정의 버그

🔗 [**자세히 보기: Test middlewares in isolation**](/sections/testingandquality/test-middlewares.md)

<br/><br/><br/>

<p align="right"><a href="#목차">⬆ 목차로 돌아가기</a></p>

# `5. 운영환경으로 전환하기`

## ![✔] 5.1. 모니터링

**핵심요약:** 모니터링은 고객이 문제를 발견하기 전에 먼저 발견하는 게임이다. 모니터링에는 분명히 전례가없는 중요성을 부여해야한다. 솔루션에 너무 많은 기능들이 들어가 있을 가능성이 있으므로 확인해야만하는 기본 항목을 내부적으로 정의하고 나서 추가적인 기능들을 살펴보고 필요한 기능들이 모두 들어있는 솔루션을 선택하라. 아래의 'gist'를 클릭하면 솔루션 개요를 볼 수 있다

**그렇게 하지 않을 경우:** 오류 === 고객의 실망. 간단하다

🔗 [**자세히 보기: 모니터링!**](/sections/production/monitoring.md)

<br/><br/>

## ![✔] 5.2. 똑똑한 로깅을 통해 투명성을 높여라

**핵심요약:** 로그는 디버그 텍스트가 모여있는 의미없는 창고일 수도 있고 앱의 상태를 대시보드로 볼수 있도록 만들어 줄 수 있는 유용한 데이터일 수도 있다. 첫날부터 로그를 어떻게 수집, 저장 및 분석하는지 계획하여 원하는 정보(예: 오류율, 서비스 및 서버를 통한 전체 트랜잭션 수행)를 실제로 추출 할지 확실히 하라

**그렇게 하지 않을 경우:** 당신은 결국 추측하기 힘든 블랙박스에 도달하게고, 필요한 정보를 추가하기 위해 모든 로그를 다시 작성하게 될것이다

🔗 [**자세히 보기: Increase transparency using smart logging**](/sections/production/smartlogging.md)

<br/><br/>

## ![✔] 5.3. 가능한 모든 것들(예: gzip, SSL)을 reverse proxy에 위임하라

**핵심요약:** Node는 gzip이나 SSL Termination과 같이 CPU 집약적인 작업에 약하다. 이런게 필요할 경우 '실제' 미들웨어 서비스인 nginx, HAproxy 혹은 클라우드 서비스를 사용해야한다

**그렇게 하지 않을 경우:** 불쌍한 싱글 스레드는 어플리케이션의 코어를 처리하는 대신 인프라 작업을 수행하는 것에 더 바쁘게되고 성능은 저하될 것이다

🔗 [**자세히 보기: 가능한 모든 것들(예: gzip, SSL)을 reverse proxy에 위임하라**](/sections/production/delegatetoproxy.md)

<br/><br/>

## ![✔] 5.4. 의존성을 잠궈라(Lock dependencies)

**핵심요약:** 코드는 모든 환경에서 동일해야하지만 놀랍게도 npm을 사용하면 기본적으로 환경간에 종속성이 달라질 수 있다. 다양한 환경에서 패키지를 설치하면 패키지의 최신 패치 버전을 가져온다. npm config 파일 인 .npmrc를 사용하여이 문제를 극복하라. 각 환경에 각 패키지의 최신 버전이 아닌 정확한 버전을 저장하도록 알려준다. 또는 세밀하게 제어하려면`npm shrinkwrap`을 사용하라. \* 업데이트 : NPM5 현재 기본적으로 종속성이 잠겨 있다. 새로운 패키지 관리자인 Yarn도 기본적으로 잠겨 있다.

**그렇게 하지 않을 경우:** QA팀은 코드를 철저히 테스트했지만 테스트 환경과는 다르게 작동하는 버전을 승인할 것이다. 심지어 더 나쁜 것은 같은 프로덕션 클러스터의 여러 서버가 서로 다른 코드를 실행할 수도 있다는 것이다.

🔗 [**자세히 보기: Lock dependencies**](/sections/production/lockdependencies.md)

<br/><br/>

## ![✔] 5.5. 올바른 도구를 이용해 프로세스가 가동되는 시간을 보호하라

**핵심요약:** 프로세스는 계속 진행되어야하며 실패시 다시 시작해야한다. 간단한 시나리오의 경우 PM2와 같은 프로세스 관리 도구로도 충분하지만 오늘날 '도커가 사용되는' 세계에서는 클러스터 관리 도구도 고려해야한다

**그렇게 하지 않을 경우:** 명확한 전략없이 수십 개의 인스턴스와 너무 많은 도구 (클러스터 관리, 도커, PM2)를 실행하면 개발자가 혼란을 겪을 수 있다

🔗 [**자세히 보기: Guard process uptime using the right tool**](/sections/production/guardprocess.md)

<br/><br/>

## ![✔] 5.6. 모든 CPU를 활용하라

**핵심요약:** 기본적으로 Node 어플리케이션은 하나의 CPU 코어에서 실행되고 다른 CPU는 동작하지 않는다. 노드 프로세스를 복제하여 모든 CPU를 활용하는 것은 당신의 몫이다. 중소형 어플리케이션의 경우 노드 클러스터 또는 PM2를 사용하면 된다. 더 큰 앱의 경우 Docker 클러스터(예: K8S, ECS) 또는 Linux 초기화 시스템(예: systemd)을 기반으로하는 배포 스크립트를 사용하여 프로세스를 복제하는 것이 좋다

**그렇게 하지 않을 경우:** 사용 가능한 리소스의 25%, 혹은 훨씬 적게 활용할 것이다. 일반적인 서버는 CPU 코어가 4개 이상인 점을 감안할 때, Node를 아무 생각없이 배포하게 되면 그 중 단 1 개만 사용하게 될것이다. AWS beanstalk와 같은 PaaS 서비스 사용하더라도 말이다

🔗 [**자세히 보기: 모든 CPU를 활용하라**](/sections/production/utilizecpu.md)

<br/><br/>

## ![✔] 5.7. ‘유지 관리용 엔드 포인트’를 따로 만들어라

**핵심요약:** 보안적으로 안전한 API에서 메모리 사용 및 REPL 등과 같은 시스템 관련 정보를 노출하라. 표준 및 실전 테스트(battle-tests) 도구를 사용하는 것이 좋기는 하지만 몇몇 유용한 정보와 작업은 코드를 통해 쉽게 수행 할 수 있다

**그렇게 하지 않을 경우:** 단지 서버 진단을 목적으로 일부 정보를 추출하기 위하여 상용서버에 코드를 배포하는 "진단용 배포"를 자주 수행하고 있게 될 것이다

🔗 [**자세히 보기: Create a ‘maintenance endpoint’**](/sections/production/createmaintenanceendpoint.md)

<br/><br/>

## ![✔] 5.8. APM을 사용하여 오류 및 가동 중지 시간을 발견하라

**핵심요약:** 어플리케이션 성능 관리 제품(Application Performance Management, APM)은 코드베이스 및 API를 사전에 측정하여 기존 모니터링 시스템을 뛰어 넘어 서비스 및 계층 전반에서 사용자 경험을 측정한다. 예를 들어 일부 APM 제품은 최종 사용자 측면에서 느리게 로드되는 트랜잭션을 강조 표시 하면서 근본 원인을 제시할 수 있다

**그렇게 하지 않을 경우:** API 성능 및 가동 중지 시간을 측정하는 데 많은 노력을 기울이게 될 수 있다. 실제 상황에서 가장 느린 코드가 무엇인지, 그리고 이것이 UX에 미치는 영향을 알지 못할 것이다

🔗 [**자세히 보기: Discover errors and downtime using APM products**](/sections/production/apmproducts.md)

<br/><br/>

## ![✔] 5.9. 당신의 코드는 언제든 상용에 배포될수 있다

**핵심요약:** 첫날부터 배포를 염두에두고 코드를 작성하라. 다소 모호하게 들릴것 같아서 상용 유지 보수와 밀접하게 관련된 몇 가지 개발 팁을 컴파일해 두었다 (아래의 gist를 클릭하라)

**그렇게 하지 않을 경우:** 세계 최고의 IT/DevOps 전문가도 잘못 작성된 코드로 이루어진 시스템은 구하지 못한다

🔗 [**자세히 보기: Make your code production-ready**](/sections/production/productioncode.md)

<br/><br/>

## ![✔] 5.10. 메모리 사용량 측정 및 보호

**핵심요약:** Node.js는 메모리와 관련하여 논란의 여지가 있다. v8 엔진은 메모리 사용량 (1.4GB)에 대한 제한이 있으며 노드의 코드에서 메모리 누수가 발생하는 알려진 방법이 존재하므로 노드의 프로세스 메모리를 관찰하는 것이 필수적이다. 작은 응용 프로그램에서는 Shell 명령을 사용하여 주기적으로 메모리를 측정 할 수 있지만 중대형 어플리케이션에서는 강력한 모니터링 시스템을 통해 메모리를 감시하는 것을 고려하라

**그렇게 하지 않을 경우:** [월마트](https://www.joyent.com/blog/walmart-node-js-memory-leak)에서 일어났던 것처럼 메모리가 하루에 수백 MB씩 누수 될 수 있다

🔗 [**자세히 보기: 메모리 사용량 측정 및 보호**](/sections/production/measurememory.md)

<br/><br/>

## ![✔] 5.11. Node.js에서 프론트 엔드 파일 가져오기

**핵심요약:** 단일 스레드 모델로 인해 정적 파일을 많이 처리 할 때 Node.js 성능이 실제로 손상되기 때문에 전용 미들웨어(nginx, S3, CDN 등)를 사용하여 프론트 엔드 컨텐츠를 제공하는게 좋다

**그렇게 하지 않을 경우:** 단일 노드 스레드는 동적 컨텐츠를 전달하는 작업에 리소스를 할당하는 대신 수백 개의 html/images/angular/react 파일을 스트리밍 하느라 분주할 것이다

🔗 [**자세히 보기: Get your frontend assets out of Node**](/sections/production/frontendout.md)

<br/><br/>

## ![✔] 5.12. 무상태(stateless)로 운영하고, 거의 매일 서버를 재부팅하라

**핵심요약:** 어떤 유형의 데이터(예: 유저 세션, 캐시, 업로드된 파일)든 외부 데이터 저장소에 저장하라. 서버를 주기적으로 재부팅/교체하는 것을 고려하거나 명시적으로 무상태로 운영하게 만드는 Serverless 플랫폼(예: AWS Lambda)을 사용하는 것을 고려하라

**그렇게 하지 않을 경우:** 해당서버에 오류가 발생하면 해당 서버만 제거하면 되는 것이 아니라 어플리케이션의 다운타임이 발생하게된다. 게다가 특정 서버에 의존하기 때문에 수평적 확장이 힘들어질 것이다

🔗 [**자세히 보기: Be stateless, kill your Servers almost every day**](/sections/production/bestateless.md)

<br/><br/>

## ![✔] 5.13. 취약점을 자동으로 탐지하는 도구 사용

**핵심요약:** Express와 같은 가장 신뢰할만한 모듈조차도 시스템을 위험에 빠뜨릴 수있는 알려진 취약점이 존재한다. 이는 취약성을 지속적으로 확인하고(로컬 또는 GitHub에서) 경고하는 커뮤니티 및 상용 도구를 사용하여 쉽게 길들여질 수 있으며 일부는 즉시 패치 할 수도 있다

**그렇게 하지 않을 경우:** 전용 도구없이 취약점으로부터 코드를 깨끗하게 유지하려면 새로운 취약점에 대한 데이터를 지속적으로 확인해야 할것이다

🔗 [**자세히 보기: Use tools that automatically detect vulnerabilities**](/sections/production/detectvulnerabilities.md)

<br/><br/>

## ![✔] 5.14. 로깅을 할때 트랜잭션 ID를 할당하라

**핵심요약:** 하나의 요청 내에서 각 로그에 동일한 식별자(transaction-id: { some value })를 할당하라. 그렇게하면 로그를 분석할때 에러 전과 후에 어떤 일이 생겼는지 쉽게 알수있다. 비동기적인 특성때문에 Node.js에서 구현하기 쉽지는 않다. 아래의 예제를 확인하라

**그렇게 하지 않을 경우:** 이전에 어떤일이 일어났는지에 대한 컨텍스트 없이 에러 로그를 확인하는 것은 문제를 해결하는 것을 더 어렵고 느리게 만든다

🔗 [**자세히 보기: Assign ‘TransactionId’ to each log statement**](/sections/production/assigntransactionid.md)

<br/><br/>

## ![✔] 5.15. `NODE_ENV=production`로 설정하라

**핵심요약:** 상용 최적화가 활성화 되어야하는지 아닌지를 표시하기 위해 `NODE_ENV`를 'production' 혹은 'development'로 설정하라. 많은 npm 패키지가 현재 환경을 확인하고 최적화한다

**그렇게 하지 않을 경우:** 이 단순한 속성을 빠뜨리면 성능이 크게 저하된다. 예를 들어 Express에서 서버 사이드 렌더링(Server Side Rendering, SSP)을 사용할때 `NODE_ENV`를 빠뜨리면 3배 느려진다

🔗 [**자세히 보기: Set NODE_ENV=production**](/sections/production/setnodeenv.md)

<br/><br/>

## ![✔] 5.16. 원자성의 자동화된 무중단 배포를 설계하라

**핵심요약:** 연구 결과에 따르면 자주 배포를 하는 팀이 상용버전에서 심각한 에러가 발생할 가능성을 낮춘다고 한다. 위험이 따르는 수동적인 과정과 서비스의 중지시간이 필요하지 않은 빠르고 자동화된 배포는 배포 프로세스를 크게 향상시킨다. 간소화된 배포의 표준이 된 Docker와 CI 도구를 결합하여 이를 달성할 수 있다.

**그렇게 하지 않을 경우:** 오래걸리는 배포 작업 -> 상용버전 중지시간 및 사람에 의한 에러 -> 배포를 하는 것에 자신감이 없어진 팀 -> 더 적은 배포와 기능들

<br/><br/>

## ![✔] 5.17. Node.js의 LTS 릴리즈 버전 사용

**핵심요약:** LTS 버전의 Node.js를 사용하여 중요한 버그 수정, 보안 업데이트 및 성능 향상을 받아라

**그렇게 하지 않을 경우:** 새로 발견된 버그나 취약점이 상용에서 운영중인 어플리케이션을 악용하는데 사용될 수 있으며, 다양한 모듈들에서 지원을 하지 않게 되고 유지보수하는 것이 힘들어 지게될것이다

🔗 [**자세히 보기: Use an LTS release of Node.js**](/sections/production/LTSrelease.md)

<br/><br/>

## ![✔] 5.18. 앱 내에서 로그를 라우팅하지 말라

**핵심요약:** 로그의 목적지는 개발자가 어플리케이션 코드에 하드코딩해서는 안되고 프로그램이 실행되는 실행환경에서 정의되어야 한다. 개발자는 로거 유틸리티를 이용하여 로그를 `stdout`에 작성하고 상용환경(컨테이너, 서버 등)이 해당 `stdout`스트림을 적절한 목적지로 파이프해야한다

**그렇게 하지 않을 경우:** 어플리케이션 로그 라우팅 처리 === 확장성 저하, 로그 유실, 관심사의 분리 실패(Separation of Concerns, SoC)

🔗 [**자세히 보기: Log Routing**](/sections/production/logrouting.md)

<br/><br/>

## ![✔] 5.19. `npm ci`로 패키지를 설치해라

**핵심요약** 상용환경에서 쓰는 코드가 테스트할떄 쓴 패키지 버젼과 동일하다는 것을 반드시 보장해야 한다. `npm ci`를 써서 의존하는 패키지를 모두 package.json과 package-lock.json만들 따른 클린설치를 해라.

**그렇게 하지 않을 경우:** QA팀이 코드를 승인하기 전에 철저히 테스트해도 상용환경에서는 다르게 작동할것이다. 게다가 같은 프로덕션 클러스터의 다른 서버들이 서로 다른 코드를 실행할 수도 있다.

🔗 [**자세히 보기: npm ci 쓰기**](/sections/production/installpackageswithnpmci.md)

<br/><br/><br/>

<p align="right"><a href="#목차">⬆ 목차로 돌아가기</a></p>

# `6. 보안`

<div align="center">
<img src="https://img.shields.io/badge/OWASP%20Threats-Top%2010-green.svg" alt="54 items"/>
</div>

## ![✔] 6.1. linter 보안 규칙 수용

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20XSS%20-green.svg" alt=""/></a>

**핵심요약:** 이왕이면 코드를 작성하면서, 가능한한 빨리 eslint-plugin-security와 같은 보안 관련 linter 플러그인을 사용하여 보안 취약점을 잡으십시오. 이것은 eval, 자식 프로세스 호출, string iteral을 이용한 모듈 import (예를 들면 유저 인풋) 같은 보안 취약점을 잡는데 도움이 될 수 있다. 보안 linter가 잡는 코드를 보려면, 아래의 'Read more'을 클릭하십시오.

**그렇게 하지 않을 경우:** 개발 중에 직접 보안 취약점이 도리 수 있었던 것이 프로덕션에서 주요한 이슈가 된다. 또, 프로젝트가 일관된 보안 프렉티스를 따르지 않아, 취약점이 노출되거나 민감한 정보가 원격 저장소에 유출될 수 있다.

🔗 [**자세히 보기: Lint rules**](/sections/security/lintrules.md)

<br/><br/>

## ![✔] 6.2. Limit concurrent requests using a middleware

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**핵심요약:** DOS attacks are very popular and relatively easy to conduct. Implement rate limiting using an external service such as cloud load balancers, cloud firewalls, nginx, [rate-limiter-flexible](https://www.npmjs.com/package/rate-limiter-flexible) package, or (for smaller and less critical apps) a rate-limiting middleware (e.g. [express-rate-limit](https://www.npmjs.com/package/express-rate-limit))

**그렇게 하지 않을 경우:** An application could be subject to an attack resulting in a denial of service where real users receive a degraded or unavailable service.

🔗 [**자세히 보기: Implement rate limiting**](/sections/security/limitrequests.md)

<br/><br/>

## ![✔] 6.3 Extract secrets from config files or use packages to encrypt them

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A3-Sensitive_Data_Exposure" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A3:Sensitive%20Data%20Exposure%20-green.svg" alt=""/></a>

**핵심요약:** Never store plain-text secrets in configuration files or source code. Instead, make use of secret-management systems like Vault products, Kubernetes/Docker Secrets, or using environment variables. As a last resort, secrets stored in source control must be encrypted and managed (rolling keys, expiring, auditing, etc). Make use of pre-commit/push hooks to prevent committing secrets accidentally

**그렇게 하지 않을 경우:** Source control, even for private repositories, can mistakenly be made public, at which point all secrets are exposed. Access to source control for an external party will inadvertently provide access to related systems (databases, apis, services, etc).

🔗 [**자세히 보기: Secret management**](/sections/security/secretmanagement.md)

<br/><br/>

## ![✔] 6.4. Prevent query injection vulnerabilities with ORM/ODM libraries

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a>

**핵심요약:** To prevent SQL/NoSQL injection and other malicious attacks, always make use of an ORM/ODM or a database library that escapes data or supports named or indexed parameterized queries, and takes care of validating user input for expected types. Never just use JavaScript template strings or string concatenation to inject values into queries as this opens your application to a wide spectrum of vulnerabilities. All the reputable Node.js data access libraries (e.g. [Sequelize](https://github.com/sequelize/sequelize), [Knex](https://github.com/tgriesser/knex), [mongoose](https://github.com/Automattic/mongoose)) have built-in protection against injection attacks.

**그렇게 하지 않을 경우:** Unvalidated or unsanitized user input could lead to operator injection when working with MongoDB for NoSQL, and not using a proper sanitization system or ORM will easily allow SQL injection attacks, creating a giant vulnerability.

🔗 [**자세히 보기: Query injection prevention using ORM/ODM libraries**](/sections/security/ormodmusage.md)

<br/><br/>

## ![✔] 6.5. Collection of generic security best practices

**핵심요약:** This is a collection of security advice that is not related directly to Node.js - the Node implementation is not much different than any other language. Click read more to skim through.

🔗 [**자세히 보기: Common security best practices**](/sections/security/commonsecuritybestpractices.md)

<br/><br/>

## ![✔] 6.6. Adjust the HTTP response headers for enhanced security

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**핵심요약:** Your application should be using secure headers to prevent attackers from using common attacks like cross-site scripting (XSS), clickjacking and other malicious attacks. These can be configured easily using modules like [helmet](https://www.npmjs.com/package/helmet).

**그렇게 하지 않을 경우:** Attackers could perform direct attacks on your application's users, leading to huge security vulnerabilities

🔗 [**자세히 보기: Using secure headers in your application**](/sections/security/secureheaders.md)

<br/><br/>

## ![✔] 6.7. Constantly and automatically inspect for vulnerable dependencies

<a href="https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Known%20Vulnerabilities%20-green.svg" alt=""/></a>

**핵심요약:** With the npm ecosystem it is common to have many dependencies for a project. Dependencies should always be kept in check as new vulnerabilities are found. Use tools like [npm audit](https://docs.npmjs.com/cli/audit) or [snyk](https://snyk.io/) to track, monitor and patch vulnerable dependencies. Integrate these tools with your CI setup so you catch a vulnerable dependency before it makes it to production.

**그렇게 하지 않을 경우:** An attacker could detect your web framework and attack all its known vulnerabilities.

🔗 [**자세히 보기: Dependency security**](/sections/security/dependencysecurity.md)

<br/><br/>

## ![✔] 6.8. Avoid using the Node.js crypto library for handling passwords, use Bcrypt

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**핵심요약:** Passwords or secrets (API keys) should be stored using a secure hash + salt function like `bcrypt`, that should be a preferred choice over its JavaScript implementation due to performance and security reasons.

**그렇게 하지 않을 경우:** Passwords or secrets that are persisted without using a secure function are vulnerable to brute forcing and dictionary attacks that will lead to their disclosure eventually.

🔗 [**자세히 보기: Use Bcrypt**](/sections/security/bcryptpasswords.md)

<br/><br/>

## ![✔] 6.9. Escape HTML, JS and CSS output

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a>

**핵심요약:** Untrusted data that is sent down to the browser might get executed instead of just being displayed, this is commonly referred as a cross-site-scripting (XSS) attack. Mitigate this by using dedicated libraries that explicitly mark the data as pure content that should never get executed (i.e. encoding, escaping)

**그렇게 하지 않을 경우:** An attacker might store malicious JavaScript code in your DB which will then be sent as-is to the poor clients

🔗 [**자세히 보기: Escape output**](/sections/security/escape-output.md)

<br/><br/>

## ![✔] 6.10. Validate incoming JSON schemas

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7: XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A8-Insecure_Deserialization" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A8:Insecured%20Deserialization%20-green.svg" alt=""/></a>

**핵심요약:** Validate the incoming requests' body payload and ensure it meets expectations, fail fast if it doesn't. To avoid tedious validation coding within each route you may use lightweight JSON-based validation schemas such as [jsonschema](https://www.npmjs.com/package/jsonschema) or [joi](https://www.npmjs.com/package/joi)

**그렇게 하지 않을 경우:** Your generosity and permissive approach greatly increases the attack surface and encourages the attacker to try out many inputs until they find some combination to crash the application

🔗 [**자세히 보기: Validate incoming JSON schemas**](/sections/security/validation.md)

<br/><br/>

## ![✔] 6.11. Support blacklisting JWTs

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**핵심요약:** When using JSON Web Tokens (for example, with [Passport.js](https://github.com/jaredhanson/passport)), by default there's no mechanism to revoke access from issued tokens. Once you discover some malicious user activity, there's no way to stop them from accessing the system as long as they hold a valid token. Mitigate this by implementing a blacklist of untrusted tokens that are validated on each request.

**그렇게 하지 않을 경우:** Expired, or misplaced tokens could be used maliciously by a third party to access an application and impersonate the owner of the token.

🔗 [**자세히 보기: Blacklist JSON Web Tokens**](/sections/security/expirejwt.md)

<br/><br/>

## ![✔] 6.12. Prevent brute-force attacks against authorization

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**핵심요약:** A simple and powerful technique is to limit authorization attempts using two metrics:
           
1. The first is number of consecutive failed attempts by the same user unique ID/name and IP address.
2. The second is number of failed attempts from an IP address over some long period of time. For example, block an IP address if it makes 100 failed attempts in one day.

**그렇게 하지 않을 경우:** An attacker can issue unlimited automated password attempts to gain access to privileged accounts on an application

🔗 [**자세히 보기: Login rate limiting**](/sections/security/login-rate-limit.md)

<br/><br/>

## ![✔] 6.13. Run Node.js as non-root user

<a href="https://www.owasp.org/index.php/Top_10-2017_A5-Broken_Access_Control" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A5:Broken%20Access%20Access%20Control-green.svg" alt=""/></a>

**핵심요약:** There is a common scenario where Node.js runs as a root user with unlimited permissions. For example, this is the default behaviour in Docker containers. It's recommended to create a non-root user and either bake it into the Docker image (examples given below) or run the process on this user's behalf by invoking the container with the flag "-u username"

**그렇게 하지 않을 경우:** An attacker who manages to run a script on the server gets unlimited power over the local machine (e.g. change iptable and re-route traffic to his server)

🔗 [**자세히 보기: Run Node.js as non-root user**](/sections/security/non-root-user.md)

<br/><br/>

## ![✔] 6.14. Limit payload size using a reverse-proxy or a middleware

<a href="https://www.owasp.org/index.php/Top_10-2017_A8-Insecure_Deserialization" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A8:Insecured%20Deserialization%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**핵심요약:** The bigger the body payload is, the harder your single thread works in processing it. This is an opportunity for attackers to bring servers to their knees without tremendous amount of requests (DOS/DDOS attacks). Mitigate this limiting the body size of incoming requests on the edge (e.g. firewall, ELB) or by configuring [express body parser](https://github.com/expressjs/body-parser) to accept only small-size payloads

**그렇게 하지 않을 경우:** Your application will have to deal with large requests, unable to process the other important work it has to accomplish, leading to performance implications and vulnerability towards DOS attacks

🔗 [**자세히 보기: Limit payload size**](/sections/security/requestpayloadsizelimit.md)

<br/><br/>

## ![✔] 6.15. Avoid JavaScript eval statements

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**핵심요약:** `eval` is evil as it allows executing custom JavaScript code during run time. This is not just a performance concern but also an important security concern due to malicious JavaScript code that may be sourced from user input. Another language feature that should be avoided is `new Function` constructor. `setTimeout` and `setInterval` should never be passed dynamic JavaScript code either.

**그렇게 하지 않을 경우:** Malicious JavaScript code finds a way into text passed into `eval` or other real-time evaluating JavaScript language functions, and will gain complete access to JavaScript permissions on the page. This vulnerability is often manifested as an XSS attack.

🔗 [**자세히 보기: Avoid JavaScript eval statements**](/sections/security/avoideval.md)

<br/><br/>

## ![✔] 6.16. Prevent evil RegEx from overloading your single thread execution

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**핵심요약:** Regular Expressions, while being handy, pose a real threat to JavaScript applications at large, and the Node.js platform in particular. A user input for text to match might require an outstanding amount of CPU cycles to process. RegEx processing might be inefficient to an extent that a single request that validates 10 words can block the entire event loop for 6 seconds and set the CPU on 🔥. For that reason, prefer third-party validation packages like [validator.js](https://github.com/chriso/validator.js) instead of writing your own Regex patterns, or make use of [safe-regex](https://github.com/substack/safe-regex) to detect vulnerable regex patterns

**그렇게 하지 않을 경우:** Poorly written regexes could be susceptible to Regular Expression DoS attacks that will block the event loop completely. For example, the popular `moment` package was found vulnerable with malicious RegEx usage in November of 2017

🔗 [**자세히 보기: Prevent malicious RegEx**](/sections/security/regex.md)

<br/><br/>

## ![✔] 6.17. Avoid module loading using a variable

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**핵심요약:** Avoid requiring/importing another file with a path that was given as parameter due to the concern that it could have originated from user input. This rule can be extended for accessing files in general (i.e. `fs.readFile()`) or other sensitive resource access with dynamic variables originating from user input. [Eslint-plugin-security](https://www.npmjs.com/package/eslint-plugin-security) linter can catch such patterns and warn early enough

**그렇게 하지 않을 경우:** Malicious user input could find its way to a parameter that is used to require tampered files, for example, a previously uploaded file on the filesystem, or access already existing system files.

🔗 [**자세히 보기: Safe module loading**](/sections/security/safemoduleloading.md)

<br/><br/>

## ![✔] 6.18. Run unsafe code in a sandbox

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**핵심요약:** When tasked to run external code that is given at run-time (e.g. plugin), use any sort of 'sandbox' execution environment that isolates and guards the main code against the plugin. This can be achieved using a dedicated process (e.g. `cluster.fork()`), serverless environment or dedicated npm packages that act as a sandbox

**그렇게 하지 않을 경우:** A plugin can attack through an endless variety of options like infinite loops, memory overloading, and access to sensitive process environment variables

🔗 [**자세히 보기: Run unsafe code in a sandbox**](/sections/security/sandbox.md)

<br/><br/>

## ![✔] 6.19. Take extra care when working with child processes

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**핵심요약:** Avoid using child processes when possible and validate and sanitize input to mitigate shell injection attacks if you still have to. Prefer using `child_process.execFile` which by definition will only execute a single command with a set of attributes and will not allow shell parameter expansion.

**그렇게 하지 않을 경우:** Naive use of child processes could result in remote command execution or shell injection attacks due to malicious user input passed to an unsanitized system command.

🔗 [**자세히 보기: Be cautious when working with child processes**](/sections/security/childprocesses.md)

<br/><br/>

## ![✔] 6.20. Hide error details from clients

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**핵심요약:** An integrated express error handler hides the error details by default. However, great are the chances that you implement your own error handling logic with custom Error objects (considered by many as a best practice). If you do so, ensure not to return the entire Error object to the client, which might contain some sensitive application details

**그렇게 하지 않을 경우:** Sensitive application details such as server file paths, third party modules in use, and other internal workflows of the application which could be exploited by an attacker, could be leaked from information found in a stack trace

🔗 [**자세히 보기: Hide error details from client**](/sections/security/hideerrors.md)

<br/><br/>

## ![✔] 6.21. Configure 2FA for npm or Yarn

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**핵심요약:** Any step in the development chain should be protected with MFA (multi-factor authentication), npm/Yarn are a sweet opportunity for attackers who can get their hands on some developer's password. Using developer credentials, attackers can inject malicious code into libraries that are widely installed across projects and services. Maybe even across the web if published in public. Enabling 2-factor-authentication in npm leaves almost zero chances for attackers to alter your package code.

**그렇게 하지 않을 경우:** [Have you heard about the eslint developer who's password was hijacked?](https://medium.com/@oprearocks/eslint-backdoor-what-it-is-and-how-to-fix-the-issue-221f58f1a8c8)

<br/><br/>

## ![✔] 6.22. Modify session middleware settings

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**핵심요약:** Each web framework and technology has its known weaknesses - telling an attacker which web framework we use is a great help for them. Using the default settings for session middlewares can expose your app to module- and framework-specific hijacking attacks in a similar way to the `X-Powered-By` header. Try hiding anything that identifies and reveals your tech stack (E.g. Node.js, express)

**그렇게 하지 않을 경우:** Cookies could be sent over insecure connections, and an attacker might use session identification to identify the underlying framework of the web application, as well as module-specific vulnerabilities

🔗 [**자세히 보기: Cookie and session security**](/sections/security/sessions.md)

<br/><br/>

## ![✔] 6.23. Avoid DOS attacks by explicitly setting when a process should crash

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**핵심요약:** The Node process will crash when errors are not handled. Many best practices even recommend to exit even though an error was caught and got handled. Express, for example, will crash on any asynchronous error - unless you wrap routes with a catch clause. This opens a very sweet attack spot for attackers who recognize what input makes the process crash and repeatedly send the same request. There's no instant remedy for this but a few techniques can mitigate the pain: Alert with critical severity anytime a process crashes due to an unhandled error, validate the input and avoid crashing the process due to invalid user input, wrap all routes with a catch and consider not to crash when an error originated within a request (as opposed to what happens globally)

**그렇게 하지 않을 경우:** This is just an educated guess: given many Node.js applications, if we try passing an empty JSON body to all POST requests - a handful of applications will crash. At that point, we can just repeat sending the same request to take down the applications with ease

<br/><br/>

## ![✔] 6.24. Prevent unsafe redirects

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a>

**핵심요약:** Redirects that do not validate user input can enable attackers to launch phishing scams, steal user credentials, and perform other malicious actions.

**그렇게 하지 않을 경우:** If an attacker discovers that you are not validating external, user-supplied input, they may exploit this vulnerability by posting specially-crafted links on forums, social media, and other public places to get users to click it.

🔗 [**자세히 보기: Prevent unsafe redirects**](/sections/security/saferedirects.md)

<br/><br/>

## ![✔] 6.25. Avoid publishing secrets to the npm registry

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**핵심요약:** Precautions should be taken to avoid the risk of accidentally publishing secrets to public npm registries. An `.npmignore` file can be used to blacklist specific files or folders, or the `files` array in `package.json` can act as a whitelist.

**그렇게 하지 않을 경우:** Your project's API keys, passwords or other secrets are open to be abused by anyone who comes across them, which may result in financial loss, impersonation, and other risks.

🔗 [**자세히 보기: Avoid publishing secrets**](/sections/security/avoid_publishing_secrets.md)
<br/><br/><br/>

<p align="right"><a href="#목차">⬆ Return to top</a></p>

# `7. 초안: 성능`

## 협력자들이 현재 작업중입니다. [함꼐 하시겠습니까?](https://github.com/i0natan/nodebestpractices/issues/256)

<br/><br/>

## ![✔] 7.1. Don't block the event loop

**핵심요약:** Avoid CPU intensive tasks as they will block the mostly single-threaded Event Loop and offload those to a dedicated thread, process or even a different technology based on the context.

**그렇게 하지 않을 경우:** As the Event Loop is blocked, Node.js will be unable to handle other request thus causing delays for concurrent users. **3000 users are waiting for a response, the content is ready to be served, but one single request blocks the server from dispatching the results back**

🔗 [**자세히 보기: Do not block the event loop**](/sections/performance/block-loop.md)

<br /><br /><br />

## ![✔] 7.2. Prefer native JS methods over user-land utils like Lodash

**핵심요약:** It's often more penalising to use utility libraries like `lodash` and `underscore` over native methods as it leads to unneeded dependencies and slower performance.
Bear in mind that with the introduction of the new V8 engine alongside the new ES standards, native methods were improved in such a way that it's now about 50% more performant than utility libraries.

**그렇게 하지 않을 경우:** You'll have to maintain less performant projects where you could have simply used what was **already** available or dealt with a few more lines in exchange of a few more files.

🔗 [**자세히 보기: Native over user land utils**](/sections/performance/nativeoverutil.md)

<br/><br/><br/>

# 마일스톤

이 가이드를 관리하고 최신 버전을 유지하기 위해, 우리는 지속해서 가이드라인과 모범 사례들을 커뮤니티의 도움으로 업데이트하고 개선해 나가고 있습니다. [마일스톤](https://github.com/i0natan/nodebestpractices/milestones)을 확인하시고 이 프로젝트에 기여하고 싶다면 작업중인 그룹에 참여하세요!

<br/>

## 번역

모든 번역은 커뮤니티에 의해 기여되고 있습니다. 이미 완성된 번역이나, 진행중, 새로운 번역에 대한 도움은 언제나 환영합니다!

### 번역 작업 완료

- ![BR](/assets/flags/BR.png) [Brazilian Portuguese](./README.brazilian-portuguese.md) - Courtesy of [Marcelo Melo](https://github.com/marcelosdm)
- ![CN](/assets/flags/CN.png) [Chinese](./README.chinese.md) - Courtesy of [Matt Jin](https://github.com/mattjin)
- ![RU](/assets/flags/RU.png) [Russian](./README.russian.md) - Courtesy of [Alex Ivanov](https://github.com/contributorpw)

### 번역 작업중

- ![FR](/assets/flags/FR.png) [French](https://github.com/gaspaonrocks/nodebestpractices/blob/french-translation/README.french.md) ([Discussion](https://github.com/i0natan/nodebestpractices/issues/129))
- ![HE](/assets/flags/HE.png) Hebrew ([Discussion](https://github.com/i0natan/nodebestpractices/issues/156))
- ![KR](/assets/flags/KR.png) [Korean](README.korean.md) - Courtesy of [Sangbeom Han](https://github.com/uronly14me) ([Discussion](https://github.com/i0natan/nodebestpractices/issues/94))
- ![ES](/assets/flags/ES.png) [Spanish](https://github.com/i0natan/nodebestpractices/blob/spanish-translation/README.spanish.md) ([Discussion](https://github.com/i0natan/nodebestpractices/issues/95))
- ![TR](/assets/flags/TR.png) Turkish ([Discussion](https://github.com/i0natan/nodebestpractices/issues/139))

<br/><br/>

## 운영 위원회

Meet the steering committee members - the people who work together to provide guidance and future direction to the project. In addition, each member of the committee leads a project tracked under our [Github projects](https://github.com/i0natan/nodebestpractices/projects).

<img align="left" width="100" height="100" src="assets/images/members/yoni.png">

[Yoni Goldberg](https://github.com/i0natan)
<a href="https://twitter.com/goldbergyoni"><img src="assets/images/twitter-s.png" width="16" height="16"></img></a>
<a href="https://goldbergyoni.com"><img src="assets/images/www.png" width="16" height="16"></img></a>

Independent Node.js consultant who works with customers in the USA, Europe, and Israel on building large-scale Node.js applications. Many of the best practices above were first published at [goldbergyoni.com](https://goldbergyoni.com). Reach Yoni at [@goldbergyoni](https://github.com/goldbergyoni) or [me@goldbergyoni.com](mailto:me@goldbergyoni.com)

<br/>

<img align="left" width="100" height="100" src="assets/images/members/bruno.png">

[Bruno Scheufler](https://github.com/BrunoScheufler)
<a href="https://brunoscheufler.com/"><img src="assets/images/www.png" width="16" height="16"></img></a>

💻 full-stack web engineer, Node.js & GraphQL enthusiast

<br/>

<img align="left" width="100" height="100" src="assets/images/members/kyle.png">

[Kyle Martin](https://github.com/js-kyle)
<a href="https://twitter.com/kylemartin_93"><img src="assets/images/twitter-s.png" width="16" height="16"></img></a>
<a href="https://www.linkedin.com/in/kylemartinnz"><img src="assets/images/linkedin.png" width="16" height="16"></img></a>

Full Stack Developer & Site Reliability Engineer based in New Zealand, interested in web application security, and architecting and building Node.js applications to perform at global scale.

<br/>

<img align="left" width="100" height="100" src="assets/images/members/sagir.png">

[Sagir Khan](https://github.com/sagirk)
<a href="https://twitter.com/sagir_k"><img src="assets/images/twitter-s.png" width="16" height="16"></img></a>
<a href="https://sagirk.com"><img src="assets/images/www.png" width="16" height="16"></img></a>
<a href="https://linkedin.com/in/sagirk"><img src="assets/images/linkedin.png" width="16" height="16"></img></a>

Deep specialist in JavaScript and its ecosystem — React, Node.js, MongoDB, pretty much anything that involves using JavaScript/JSON in any layer of the system — building products using the web platform for the world’s most recognized brands. Individual Member of the Node.js Foundation, collaborating on the Community Committee's Website Redesign Initiative.

<br/>

## 협력자

Thank you to all our collaborators! 🙏

Our collaborators are members who are contributing to the repository on a regular basis, through suggesting new best practices, triaging issues, reviewing pull requests and more. If you are interested in helping us guide thousands of people to craft better Node.js applications, please read our [contributor guidelines](/.operations/CONTRIBUTING.md) 🎉

| <a href="https://github.com/idori" target="_blank"><img src="assets/images/members/ido.png" width="75" height="75"></a> | <a href="https://github.com/TheHollidayInn" target="_blank"><img src="assets/images/members/keith.png" width="75" height="75"></a> |<a href="https://github.com/kevynb" target="_blank"><img src="assets/images/members/kevyn.png" width="59" height="59"></a> |
| :---------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------: |:--------------------------------------------------------------------------------------------------------------------------------: |
|                                    [Ido Richter (Founder)](https://github.com/idori)                                    |                                        [Keith Holliday](https://github.com/TheHollidayInn)                                         |                                       [Kevyn Bruyere](https://github.com/kevynb)                                         |

### Past collaborators

| <a href="https://github.com/refack" target="_blank"><img src="assets/images/members/refael.png" width="50" height="50"></a> |
| :-------------------------------------------------------------------------------------------------------------------------: |
|                                        [Refael Ackermann](https://github.com/refack)                                        |

<br/>

## Contributing
If you've ever wanted to contribute to open source, now is your chance! See the [contributing docs](.operations/CONTRIBUTING.md) for more information.

## Contributors ✨

Thanks goes to these wonderful people who have contributed to this repository!

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://github.com/kevinrambaud"><img src="https://avatars1.githubusercontent.com/u/7501477?v=4" width="100px;" alt=""/><br /><sub><b>Kevin Rambaud</b></sub></a><br /><a href="#content-kevinrambaud" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/mfine15"><img src="https://avatars1.githubusercontent.com/u/1286554?v=4" width="100px;" alt=""/><br /><sub><b>Michael Fine</b></sub></a><br /><a href="#content-mfine15" title="Content">🖋</a></td>
    <td align="center"><a href="http://squgeim.github.io"><img src="https://avatars0.githubusercontent.com/u/4996818?v=4" width="100px;" alt=""/><br /><sub><b>Shreya Dahal</b></sub></a><br /><a href="#content-squgeim" title="Content">🖋</a></td>
    <td align="center"><a href="http://matheusrocha89.com"><img src="https://avatars1.githubusercontent.com/u/3718366?v=4" width="100px;" alt=""/><br /><sub><b>Matheus Cruz Rocha</b></sub></a><br /><a href="#content-matheusrocha89" title="Content">🖋</a></td>
    <td align="center"><a href="https://bityog.github.io/Portfolio/"><img src="https://avatars2.githubusercontent.com/u/28219178?v=4" width="100px;" alt=""/><br /><sub><b>Yog Mehta</b></sub></a><br /><a href="#content-BitYog" title="Content">🖋</a></td>
    <td align="center"><a href="http://kudapara.co.zw"><img src="https://avatars3.githubusercontent.com/u/13519184?v=4" width="100px;" alt=""/><br /><sub><b>Kudakwashe Paradzayi</b></sub></a><br /><a href="#content-kudapara" title="Content">🖋</a></td>
    <td align="center"><a href="https://www.t1st3.com/"><img src="https://avatars1.githubusercontent.com/u/1469638?v=4" width="100px;" alt=""/><br /><sub><b>t1st3</b></sub></a><br /><a href="#content-t1st3" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/mulijordan1976"><img src="https://avatars0.githubusercontent.com/u/33382022?v=4" width="100px;" alt=""/><br /><sub><b>mulijordan1976</b></sub></a><br /><a href="#content-mulijordan1976" title="Content">🖋</a></td>
    <td align="center"><a href="https://twitter.com/matchai"><img src="https://avatars0.githubusercontent.com/u/4658208?v=4" width="100px;" alt=""/><br /><sub><b>Matan Kushner</b></sub></a><br /><a href="#content-matchai" title="Content">🖋</a></td>
    <td align="center"><a href="https://fabiothiroki.github.io"><img src="https://avatars2.githubusercontent.com/u/670057?v=4" width="100px;" alt=""/><br /><sub><b>Fabio Hiroki</b></sub></a><br /><a href="#content-fabiothiroki" title="Content">🖋</a></td>
    <td align="center"><a href="http://james.sumners.info/"><img src="https://avatars1.githubusercontent.com/u/321201?v=4" width="100px;" alt=""/><br /><sub><b>James Sumners</b></sub></a><br /><a href="#content-jsumners" title="Content">🖋</a></td>
    <td align="center"><a href="https://twitter.com/_DanGamble"><img src="https://avatars2.githubusercontent.com/u/7152041?v=4" width="100px;" alt=""/><br /><sub><b>Dan Gamble</b></sub></a><br /><a href="#content-dan-gamble" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/trainorpj"><img src="https://avatars3.githubusercontent.com/u/13276704?v=4" width="100px;" alt=""/><br /><sub><b>PJ Trainor</b></sub></a><br /><a href="#content-trainorpj" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/reod"><img src="https://avatars0.githubusercontent.com/u/3164299?v=4" width="100px;" alt=""/><br /><sub><b>Remek Ambroziak</b></sub></a><br /><a href="#content-reod" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://ca.non.co.il"><img src="https://avatars0.githubusercontent.com/u/1829789?v=4" width="100px;" alt=""/><br /><sub><b>Yoni Jah</b></sub></a><br /><a href="#content-yonjah" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/hazolsky"><img src="https://avatars1.githubusercontent.com/u/1270790?v=4" width="100px;" alt=""/><br /><sub><b>Misha Khokhlov</b></sub></a><br /><a href="#content-hazolsky" title="Content">🖋</a></td>
    <td align="center"><a href="https://plus.google.com/+ЕвгенийОрехов/"><img src="https://avatars3.githubusercontent.com/u/8045060?v=4" width="100px;" alt=""/><br /><sub><b>Evgeny Orekhov</b></sub></a><br /><a href="#content-EvgenyOrekhov" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/gediminasml"><img src="https://avatars3.githubusercontent.com/u/19854105?v=4" width="100px;" alt=""/><br /><sub><b>-</b></sub></a><br /><a href="#content-gediminasml" title="Content">🖋</a></td>
    <td align="center"><a href="http://hisaac.net"><img src="https://avatars3.githubusercontent.com/u/923876?v=4" width="100px;" alt=""/><br /><sub><b>Isaac Halvorson</b></sub></a><br /><a href="#content-hisaac" title="Content">🖋</a></td>
    <td align="center"><a href="http://www.vedrankaracic.com"><img src="https://avatars3.githubusercontent.com/u/2808092?v=4" width="100px;" alt=""/><br /><sub><b>Vedran Karačić</b></sub></a><br /><a href="#content-vkaracic" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/lallenlowe"><img src="https://avatars3.githubusercontent.com/u/10761165?v=4" width="100px;" alt=""/><br /><sub><b>lallenlowe</b></sub></a><br /><a href="#content-lallenlowe" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/nwwells"><img src="https://avatars2.githubusercontent.com/u/1039473?v=4" width="100px;" alt=""/><br /><sub><b>Nathan Wells</b></sub></a><br /><a href="#content-nwwells" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/paulovitin"><img src="https://avatars0.githubusercontent.com/u/125503?v=4" width="100px;" alt=""/><br /><sub><b>Paulo Reis</b></sub></a><br /><a href="#content-paulovitin" title="Content">🖋</a></td>
    <td align="center"><a href="https://snap.simpego.ch"><img src="https://avatars2.githubusercontent.com/u/1989646?v=4" width="100px;" alt=""/><br /><sub><b>syzer</b></sub></a><br /><a href="#content-syzer" title="Content">🖋</a></td>
    <td align="center"><a href="http://sancho.dev"><img src="https://avatars0.githubusercontent.com/u/3763599?v=4" width="100px;" alt=""/><br /><sub><b>David Sancho</b></sub></a><br /><a href="#content-davesnx" title="Content">🖋</a></td>
    <td align="center"><a href="https://apiforge.it"><img src="https://avatars0.githubusercontent.com/u/4929965?v=4" width="100px;" alt=""/><br /><sub><b>Robert Manolea</b></sub></a><br /><a href="#content-pupix" title="Content">🖋</a></td>
    <td align="center"><a href="https://jumptoglide.com"><img src="https://avatars2.githubusercontent.com/u/708395?v=4" width="100px;" alt=""/><br /><sub><b>Xavier Ho</b></sub></a><br /><a href="#content-spaxe" title="Content">🖋</a></td>
    <td align="center"><a href="http://www.ocular-rhythm.io"><img src="https://avatars0.githubusercontent.com/u/2738518?v=4" width="100px;" alt=""/><br /><sub><b>Aaron</b></sub></a><br /><a href="#content-ocularrhythm" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://septa97.me"><img src="https://avatars2.githubusercontent.com/u/13742634?v=4" width="100px;" alt=""/><br /><sub><b>Jan Charles Maghirang Adona</b></sub></a><br /><a href="#content-septa97" title="Content">🖋</a></td>
    <td align="center"><a href="https://www.cakeresume.com/allenfang"><img src="https://avatars2.githubusercontent.com/u/5351390?v=4" width="100px;" alt=""/><br /><sub><b>Allen</b></sub></a><br /><a href="#content-AllenFang" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/leonardovillela"><img src="https://avatars3.githubusercontent.com/u/8650543?v=4" width="100px;" alt=""/><br /><sub><b>Leonardo Villela</b></sub></a><br /><a href="#content-leonardovillela" title="Content">🖋</a></td>
    <td align="center"><a href="https://michalzalecki.com"><img src="https://avatars1.githubusercontent.com/u/3136577?v=4" width="100px;" alt=""/><br /><sub><b>Michał Załęcki</b></sub></a><br /><a href="#content-MichalZalecki" title="Content">🖋</a></td>
    <td align="center"><a href="http://www.wealthbar.com"><img src="https://avatars1.githubusercontent.com/u/156449?v=4" width="100px;" alt=""/><br /><sub><b>Chris Nicola</b></sub></a><br /><a href="#content-chrisnicola" title="Content">🖋</a></td>
    <td align="center"><a href="https://twitter.com/aecorredor"><img src="https://avatars3.githubusercontent.com/u/9114987?v=4" width="100px;" alt=""/><br /><sub><b>Alejandro Corredor</b></sub></a><br /><a href="#content-aecorredor" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/cwar"><img src="https://avatars3.githubusercontent.com/u/272843?v=4" width="100px;" alt=""/><br /><sub><b>cwar</b></sub></a><br /><a href="#content-cwar" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/keyfoxth"><img src="https://avatars3.githubusercontent.com/u/10647132?v=4" width="100px;" alt=""/><br /><sub><b>Yuwei</b></sub></a><br /><a href="#content-keyfoxth" title="Content">🖋</a></td>
    <td align="center"><a href="https://bigcodenerd.org"><img src="https://avatars3.githubusercontent.com/u/10895594?v=4" width="100px;" alt=""/><br /><sub><b>Utkarsh Bhatt</b></sub></a><br /><a href="#content-utkarshbhatt12" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/duartemendes"><img src="https://avatars2.githubusercontent.com/u/12852058?v=4" width="100px;" alt=""/><br /><sub><b>Duarte Mendes</b></sub></a><br /><a href="#content-duartemendes" title="Content">🖋</a></td>
    <td align="center"><a href="http://jasonkim.ca"><img src="https://avatars2.githubusercontent.com/u/103456?v=4" width="100px;" alt=""/><br /><sub><b>Jason Kim</b></sub></a><br /><a href="#content-serv" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/Max101"><img src="https://avatars2.githubusercontent.com/u/2124249?v=4" width="100px;" alt=""/><br /><sub><b>Mitja O.</b></sub></a><br /><a href="#content-Max101" title="Content">🖋</a></td>
    <td align="center"><a href="http://sandromiguel.com"><img src="https://avatars0.githubusercontent.com/u/6423157?v=4" width="100px;" alt=""/><br /><sub><b>Sandro Miguel Marques</b></sub></a><br /><a href="#content-SandroMiguel" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/GabeKuslansky"><img src="https://avatars3.githubusercontent.com/u/9855482?v=4" width="100px;" alt=""/><br /><sub><b>Gabe</b></sub></a><br /><a href="#content-GabeKuslansky" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="http://ripper234.com/"><img src="https://avatars1.githubusercontent.com/u/172282?v=4" width="100px;" alt=""/><br /><sub><b>Ron Gross</b></sub></a><br /><a href="#content-ripper234" title="Content">🖋</a></td>
    <td align="center"><a href="http://www.thecodebarbarian.com"><img src="https://avatars2.githubusercontent.com/u/1620265?v=4" width="100px;" alt=""/><br /><sub><b>Valeri Karpov</b></sub></a><br /><a href="#content-vkarpov15" title="Content">🖋</a></td>
    <td align="center"><a href="https://sergiobernal.com"><img src="https://avatars3.githubusercontent.com/u/20087388?v=4" width="100px;" alt=""/><br /><sub><b>Sergio Bernal</b></sub></a><br /><a href="#content-imsergiobernal" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/ntelkedzhiev"><img src="https://avatars2.githubusercontent.com/u/7332371?v=4" width="100px;" alt=""/><br /><sub><b>Nikola Telkedzhiev</b></sub></a><br /><a href="#content-ntelkedzhiev" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/vitordagamagodoy"><img src="https://avatars0.githubusercontent.com/u/26370059?v=4" width="100px;" alt=""/><br /><sub><b>Vitor Godoy</b></sub></a><br /><a href="#content-vitordagamagodoy" title="Content">🖋</a></td>
    <td align="center"><a href="https://www.manishsaraan.com/"><img src="https://avatars2.githubusercontent.com/u/19797340?v=4" width="100px;" alt=""/><br /><sub><b>Manish Saraan</b></sub></a><br /><a href="#content-manishsaraan" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/uronly14me"><img src="https://avatars2.githubusercontent.com/u/5186814?v=4" width="100px;" alt=""/><br /><sub><b>Sangbeom Han</b></sub></a><br /><a href="#content-uronly14me" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://blackmatch.github.io"><img src="https://avatars3.githubusercontent.com/u/12443954?v=4" width="100px;" alt=""/><br /><sub><b>blackmatch</b></sub></a><br /><a href="#content-blackmatch" title="Content">🖋</a></td>
    <td align="center"><a href="https://simmsreeve.com"><img src="https://avatars3.githubusercontent.com/u/5173131?v=4" width="100px;" alt=""/><br /><sub><b>Joe Reeve</b></sub></a><br /><a href="#content-ISNIT0" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/BusbyActual"><img src="https://avatars2.githubusercontent.com/u/14985016?v=4" width="100px;" alt=""/><br /><sub><b>Ryan Busby</b></sub></a><br /><a href="#content-BusbyActual" title="Content">🖋</a></td>
    <td align="center"><a href="http://jsdecorator.com"><img src="https://avatars3.githubusercontent.com/u/4482199?v=4" width="100px;" alt=""/><br /><sub><b>Iman Mohamadi</b></sub></a><br /><a href="#content-ImanMh" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/HeeL"><img src="https://avatars1.githubusercontent.com/u/287769?v=4" width="100px;" alt=""/><br /><sub><b>Sergii Paryzhskyi</b></sub></a><br /><a href="#content-HeeL" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/kapilepatel"><img src="https://avatars3.githubusercontent.com/u/25738473?v=4" width="100px;" alt=""/><br /><sub><b>Kapil Patel</b></sub></a><br /><a href="#content-kapilepatel" title="Content">🖋</a></td>
    <td align="center"><a href="https://twitter.com/justjavac"><img src="https://avatars1.githubusercontent.com/u/359395?v=4" width="100px;" alt=""/><br /><sub><b>迷渡</b></sub></a><br /><a href="#content-justjavac" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/hozefaj"><img src="https://avatars1.githubusercontent.com/u/2084833?v=4" width="100px;" alt=""/><br /><sub><b>Hozefa</b></sub></a><br /><a href="#content-hozefaj" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/el-ethan"><img src="https://avatars3.githubusercontent.com/u/10249884?v=4" width="100px;" alt=""/><br /><sub><b>Ethan</b></sub></a><br /><a href="#content-el-ethan" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/milkdeliver"><img src="https://avatars2.githubusercontent.com/u/3108407?v=4" width="100px;" alt=""/><br /><sub><b>Sam</b></sub></a><br /><a href="#content-milkdeliver" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/ArlindXh"><img src="https://avatars0.githubusercontent.com/u/19508764?v=4" width="100px;" alt=""/><br /><sub><b>Arlind</b></sub></a><br /><a href="#content-ArlindXh" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/ttous"><img src="https://avatars0.githubusercontent.com/u/19815440?v=4" width="100px;" alt=""/><br /><sub><b>Teddy Toussaint</b></sub></a><br /><a href="#content-ttous" title="Content">🖋</a></td>
    <td align="center"><a href="http://ardern.io"><img src="https://avatars2.githubusercontent.com/u/2419690?v=4" width="100px;" alt=""/><br /><sub><b>Lewis</b></sub></a><br /><a href="#content-LewisArdern" title="Content">🖋</a></td>
    <td align="center"><a href="https://gabriellidenor.com/"><img src="https://avatars2.githubusercontent.com/u/765963?v=4" width="100px;" alt=""/><br /><sub><b>Gabriel Lidenor </b></sub></a><br /><a href="#content-GabrielLidenor" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/animir"><img src="https://avatars3.githubusercontent.com/u/4623196?v=4" width="100px;" alt=""/><br /><sub><b>Roman</b></sub></a><br /><a href="#content-animir" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/Francozeira"><img src="https://avatars1.githubusercontent.com/u/47419763?v=4" width="100px;" alt=""/><br /><sub><b>Francozeira</b></sub></a><br /><a href="#content-Francozeira" title="Content">🖋</a></td>
    <td align="center"><a href="https://twitter.com/invvard"><img src="https://avatars0.githubusercontent.com/u/7305493?v=4" width="100px;" alt=""/><br /><sub><b>Invvard</b></sub></a><br /><a href="#content-Invvard" title="Content">🖋</a></td>
    <td align="center"><a href="https://romulogarofalo.github.io/"><img src="https://avatars1.githubusercontent.com/u/18492592?v=4" width="100px;" alt=""/><br /><sub><b>Rômulo Garofalo</b></sub></a><br /><a href="#content-romulogarofalo" title="Content">🖋</a></td>
    <td align="center"><a href="http://thoqbk.github.io/"><img src="https://avatars0.githubusercontent.com/u/1491103?v=4" width="100px;" alt=""/><br /><sub><b>Tho Q Luong</b></sub></a><br /><a href="#content-thoqbk" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/Qeneke"><img src="https://avatars2.githubusercontent.com/u/20271568?v=4" width="100px;" alt=""/><br /><sub><b>Burak Shen</b></sub></a><br /><a href="#content-Qeneke" title="Content">🖋</a></td>
    <td align="center"><a href="http://www.happy-css.com"><img src="https://avatars0.githubusercontent.com/u/2950505?v=4" width="100px;" alt=""/><br /><sub><b>Martin Muzatko</b></sub></a><br /><a href="#content-MartinMuzatko" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/autoboxer"><img src="https://avatars3.githubusercontent.com/u/2757601?v=4" width="100px;" alt=""/><br /><sub><b>Jared Collier</b></sub></a><br /><a href="#content-autoboxer" title="Content">🖋</a></td>
    <td align="center"><a href="http://hiltonmeyer.com"><img src="https://avatars3.githubusercontent.com/u/4545860?v=4" width="100px;" alt=""/><br /><sub><b>Hilton Meyer</b></sub></a><br /><a href="#content-bikingbadger" title="Content">🖋</a></td>
    <td align="center"><a href="http://kr.vuejs.org"><img src="https://avatars0.githubusercontent.com/u/1451365?v=4" width="100px;" alt=""/><br /><sub><b>ChangJoo Park(박창주)</b></sub></a><br /><a href="#content-ChangJoo-Park" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/MasahiroSakaguchi"><img src="https://avatars0.githubusercontent.com/u/16427431?v=4" width="100px;" alt=""/><br /><sub><b>Masahiro Sakaguchi</b></sub></a><br /><a href="#content-MasahiroSakaguchi" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/TheHollidayInn"><img src="https://avatars1.githubusercontent.com/u/1253400?v=4" width="100px;" alt=""/><br /><sub><b>Keith Holliday</b></sub></a><br /><a href="#content-TheHollidayInn" title="Content">🖋</a></td>
    <td align="center"><a href="https://www.coreycleary.me"><img src="https://avatars3.githubusercontent.com/u/1485356?v=4" width="100px;" alt=""/><br /><sub><b>coreyc</b></sub></a><br /><a href="#content-coreyc" title="Content">🖋</a></td>
    <td align="center"><a href="http://maxcubing.wordpress.com"><img src="https://avatars0.githubusercontent.com/u/8260834?v=4" width="100px;" alt=""/><br /><sub><b>Maximilian Berkmann</b></sub></a><br /><a href="#content-Berkmann18" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/DouglasMV"><img src="https://avatars3.githubusercontent.com/u/32845487?v=4" width="100px;" alt=""/><br /><sub><b>Douglas Mariano Valero</b></sub></a><br /><a href="#content-DouglasMV" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/marcelosdm"><img src="https://avatars0.githubusercontent.com/u/18266600?v=4" width="100px;" alt=""/><br /><sub><b>Marcelo Melo</b></sub></a><br /><a href="#content-marcelosdm" title="Content">🖋</a></td>
    <td align="center"><a href="https://twitter.com/mperk_"><img src="https://avatars0.githubusercontent.com/u/3465794?v=4" width="100px;" alt=""/><br /><sub><b>Mehmet Perk</b></sub></a><br /><a href="#content-mperk" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/ryanouyang"><img src="https://avatars2.githubusercontent.com/u/360426?v=4" width="100px;" alt=""/><br /><sub><b>ryan ouyang</b></sub></a><br /><a href="#content-ryanouyang" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/shabeer-mdy"><img src="https://avatars0.githubusercontent.com/u/26842535?v=4" width="100px;" alt=""/><br /><sub><b>Shabeer</b></sub></a><br /><a href="#content-shabeer-mdy" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/halfzebra"><img src="https://avatars1.githubusercontent.com/u/3983879?v=4" width="100px;" alt=""/><br /><sub><b>Eduard Kyvenko</b></sub></a><br /><a href="#content-halfzebra" title="Content">🖋</a></td>
    <td align="center"><a href="http://deyvisonrocha.com"><img src="https://avatars2.githubusercontent.com/u/686067?v=4" width="100px;" alt=""/><br /><sub><b>Deyvison Rocha</b></sub></a><br /><a href="#content-deyvisonrocha" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="http://twitter.com/georgemamer"><img src="https://avatars1.githubusercontent.com/u/20108934?v=4" width="100px;" alt=""/><br /><sub><b>George Mamer</b></sub></a><br /><a href="#content-georgem3" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/leimonio"><img src="https://avatars0.githubusercontent.com/u/1969742?v=4" width="100px;" alt=""/><br /><sub><b>Konstantinos Leimonis</b></sub></a><br /><a href="#content-leimonio" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/Zybax"><img src="https://avatars3.githubusercontent.com/u/22094453?v=4" width="100px;" alt=""/><br /><sub><b>Oliver Lluberes</b></sub></a><br /><a href="#translation-Zybax" title="Translation">🌍</a></td>
    <td align="center"><a href="https://stackoverflow.com/story/tiendq"><img src="https://avatars2.githubusercontent.com/u/815910?v=4" width="100px;" alt=""/><br /><sub><b>Tien Do</b></sub></a><br /><a href="#content-tiendq" title="Content">🖋</a></td>
    <td align="center"><a href="http://singh1114.github.io/"><img src="https://avatars0.githubusercontent.com/u/11356398?v=4" width="100px;" alt=""/><br /><sub><b>Ranvir Singh</b></sub></a><br /><a href="#content-singh1114" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/collierrgbsitisfise"><img src="https://avatars3.githubusercontent.com/u/13496126?v=4" width="100px;" alt=""/><br /><sub><b>Vadim Nicolaev</b></sub></a><br /><a href="#content-collierrgbsitisfise" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/germangamboa95"><img src="https://avatars3.githubusercontent.com/u/28633849?v=4" width="100px;" alt=""/><br /><sub><b>German Gamboa Gonzalez</b></sub></a><br /><a href="#content-germangamboa95" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/AbdelrahmanHafez"><img src="https://avatars3.githubusercontent.com/u/19984935?v=4" width="100px;" alt=""/><br /><sub><b>Hafez</b></sub></a><br /><a href="#content-AbdelrahmanHafez" title="Content">🖋</a></td>
    <td align="center"><a href="http://linkedin.com/in/chandiran-dmc"><img src="https://avatars3.githubusercontent.com/u/42678579?v=4" width="100px;" alt=""/><br /><sub><b>Chandiran</b></sub></a><br /><a href="#content-chandiran-dmc" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/VinayaSathyanarayana"><img src="https://avatars2.githubusercontent.com/u/16976677?v=4" width="100px;" alt=""/><br /><sub><b>VinayaSathyanarayana</b></sub></a><br /><a href="#content-VinayaSathyanarayana" title="Content">🖋</a></td>
  </tr>
</table>

<!-- markdownlint-enable -->
<!-- prettier-ignore-end -->
<!-- ALL-CONTRIBUTORS-LIST:END -->
