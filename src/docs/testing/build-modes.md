---
title: Flutter의 빌드 모드
description: Flutter의 빌드 모드를 설명하고 언제 디버그, 릴리스, 프로파일 모드를 사용해야하는지 설명합니다.
---

Flutter 도구는 앱을 컴파일 할 떄 3가지 모드를 제공하고, 
테스트를 위한 headless 모드를 제공합니다.
개발 주기에서 어느 정도에 위치해 있느냐에 따라 컴파일 모드를 선택합니다.
코드를 디버깅 중인가요? 프로파일링 정보가 필요한가요?
앱을 배포할 준비가 됐나요?

아래에 언제 어떤 모드를 사용해야 하는지에 관한 요약이 있습니다:

* 개발하면서 [hot reload][]를 사용하고 싶다면, 
  [디버그](#디버그) 모드를 사용하세요.
* [DevTools][]을 사용해서 성능을 분석하고 싶다면,
  [프로파일](#프로파일) 모드를 사용하세요.
* 앱을 출시할 준비가 됐다면,
  [릴리스](#릴리스) 모드를 사용하세요.

남은 지면에서는 이 모드에 대해 더 자세히 설명합니다.
headless 테스팅에 대해 더 자세한 정보를 원한다면, [Flutter wiki][]를 참조하세요.

## 디버그

_디버그 모드_ 에서는 물리적 기기 혹은 에뮬레이터, 시뮬레이터에서 앱이 디버깅 
할 수 있는 상태로 준비가 됩니다. 디버그 모드는 아래와 같은 의미가 있습니다:

* [Assertions][]이 활성화됩니다.
* 서비스 익스텐션이 활성화됩니다.
* 빠른 개발과 실행 사이클에 최적화된 상태로 컴파일됩니다 
  (실행 속도가 빠르다거나, 바이너리 사이즈가 작다거나, 배포가 빠르다는 뜻은 아닙니다).
* 디버깅이 활성화되고, ([DevTools][]과 같은) 소스 레벨의 디버깅을 지원하는 툴을 
  연결하여 작업할 수 있습니다.

기본적으로, `flutter run`은 디버그 모드로 컴파일합니다.
IDE는 이 모드를 지원합니다. 예를 들어 Android Studio는
**Run > Debug...** 메뉴를 제공하고, 프로젝트 페이지에 있는
작은 삼각형과 겹쳐져 있는 초록색 버그 아이콘을 통해서도
실행할 수 있습니다.

{{site.alert.note}}
  * Hot reload는 디버그 모드에서 _만_ 작동합니다.
  * 에뮬레이터와 시뮬레이터는 디버그 모드에서 _만_ 실행할 수 있습니다.
  * 앱의 성능은 디버그 모드에서 정상이 아닐 수 있습니다.
    실제 기기로 [프로파일](#프로파일) 모드에서 
    성능을 측정하세요.
{{site.alert.end}}

## 릴리스

최대한 최적화하고 앱의 사이즈를 가장 작게하기를 원한다면, 
앱을 배포할 때 _릴리스 모드_ 를 사용하세요. 릴리스 모드는 시뮬레이터와 에뮬레이터에서 
지원되지 않습니다. 릴리스 모드는 아래와 같은 의미가 있습니다:

* Assertions이 불가능합니다.
* 디버깅 정보는 제거됩니다.
* 디버깅이 불가능합니다.
* 빠른 시작, 빠른 실행, 작은 앱 사이즈에 최적화히여 컴파일 합니다.
* 서비스 익스텐션을 사용할 수 없습니다. 

`flutter run --release` 명령은 릴리스 모드로 컴파일합니다.
IDE는 이 모드를 지원합니다. 예를 들어 Android Studio는 
**Run > Run...** 메뉴를 제공하고, 프로젝트 페이지에 있는 
작은 삼각형의 초록색 아이콘을 통해서도 실행할 수 있습니다.

더 많은 정보를 원하시면,
[iOS][]와 [Android][] 앱 출시에 관한 문서를 참조하세요.

## 프로파일

_프로파일 모드_ 에서는, 일부 디버깅 기능이 유지됩니다&mdash;앱의 성능을 측정할 만큼. 
실제 성능을 표현할 수 없기 때문에,
에뮬레이터나 시뮬레이터에서는 프로파일 모드를 사용할 수 없습니다. 
프로파일 모드는 아래 차이를 제외하면 릴리스 모드와 유사합니다:

* performance overlay 등 일부 서비스 익스텐션이 활성화됩니다.
* 추적이 활성화되고, (DevTools과 같은) 소스 레벨의 
  디버깅을 지원하는 툴을 연결하여 작업할 수 있습니다.

IDE는 이 모드를 지원합니다. 
예를 들어 Android Studio는 **Run > Profile...** 메뉴를 제공합니다.
`flutter run --profile` 명령은 프로파일 모드로 컴파일을 실행합니다.

{{site.alert.note}}
  [DevTools][] 제품군을 사용하여 앱의 성능을 프로파일링하십시오.
{{site.alert.end}}

빌드 모드에 대한 더 많은 정보를 원한다면, 
[Flutter's build modes][]를 참조하세요.


[Flutter wiki]: {{site.github}}/flutter/flutter/wiki/Flutter's-modes
[Assertions]: {{site.dart-site}}/guides/language/language-tour#assert
[iOS]:  /docs/deployment/ios
[Android]: /docs/deployment/android
[hot reload]: /docs/development/tools/hot-reload
[DevTools]: /docs/development/tools/devtools
[Flutter's build modes]: ({{site.github}}/flutter/flutter/wiki/Flutter%27s-modes)
