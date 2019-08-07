---
title: Flutter 앱 디버깅하기
description: DevTools 수트를 사용하여 앱을 디버깅하는 방법 소개.
---

Flutter 어플리케이션을 디버깅하기 위해 다양한 도구와 기능이 준비되어있습니다. 그 중에서 몇 가지를 소개합니다.

* [DevTools][], 브라우저 상에서 작동하는 퍼포먼스 체크와 프로파일링을 위한 도구들입니다.
* [Android Studio/IntelliJ][], 그리고 [VS Code][]
  (Flutter와 Dart 플러그인들을 활성화한 경우)
  값 검사와, 코드 진행,
  브레이크 포인트를 설정할 수 있는
  소스 레벨의 디버거를 지원합니다.
* [Flutter inspector][], DevTools에서 위젯 인스펙터를 사용할 수 있으며,
  (Flutter 플러그인이 활성화된) 안드로이드 스튜디오와 인텔리제이에서
  바로 사용할 수 있습니다.
  인스펙터를 통해 위젯 트리를 눈으로 확인할 수 있으며,
  개별 위젯과 각각의 프로퍼티 값들,
  퍼포먼스 오버레이 허용등의
  기능을 사용할 수 있습니다.

## DevTools

DevTools는 여러분이 앱을 디버깅하고,
프로필링하기 위해서 가장 처음 사용할 수 있는 도구입니다.
DevTools는 브라우저상에서 다양한 기능을 지원합니다.

* 소스 레벨 디버거
* 위젯 트리를 시각화해 보여주고,
  트리 구조에서 위젯을 드릴 다운하여
  앱상의 위젯을 선택할 수 있는 "widget select" 모드를
  제공하는 위젯 인스펙터
* 메모리 프로파일러
* 트레이싱과 트레이스 정보의 내보내기와 불러오기를
  제공하는 타임라인 뷰
* 로깅 뷰

여러분의 어플리케이션이 [debug mode][] 이나 [profile mode][]
에서 실행되고 있다면, 브라우저에서 앱에 연결하여
DevTools를 실행할 수 있습니다.
[release mode][]로 컴파일한 경우에는
디버깅과 프로파일링 정보가 제거되기 때문에
DevTools가 제대로 동작하지 않습니다.

DevTools를 프로파일링을 위해 사용하려면,
[profile mode][]로 어플리케이션을 실행해야 합니다. 그렇지 않으면,
프로파일링에는 프레임워크의 다양한 불변성을
([Debug mode assertions](#debug-mode-assertions)j을 살펴보세요)
검사하는 디버그 assert가 주로 남게 됩니다.

![DevTools의 기능을 소개하는 GIF]({% asset tools/devtools/inspector.gif @path %}){:width="100%"}

더 많은 정보가 필요하시면,
[DevTools][] 문서를 살펴보세요.

## Setting breakpoints

[Android Studio/IntelliJ][]나 [VS Code][] 같은
IDE/편집기에서 [DevTools debugger][]로
직접 브레이크 포인트를 설정하거나,
[프로그래밍 방식][[breakpoints]으로 설정할 수 있습니다.

## The Dart Analyzer

[Flutter enabled IDE/editor](/docs/get-started/editor/) 를 사용한다면, 
개발자가 별도로 실행하지 않아도 IDE가 자동으로 실행하여 실수를 찾도록 돕습니다.

커멘드 라인을 사용하고 있다면 
`flutter analyze` 를 통해 테스트하세요.. 

The Dart Analyzer는 문제를 추적하기 위해 여러분이 코드에 넣어둔 타입 어노테이션을 
이용합니다. 문제를 추적하기 위한 가장 빠르고 덜 고통스러운 방법이니만큼 모든 곳에서
`var` 선언,  타입 없는 인수, 타입 없는 배열 문법 등의 사용을 자제하는 것이 좋습니다.

더 많은 정보가 필요하시면, [Using the Dart analyzer][]를 살펴보세요.

## Logging

로깅도 유용한 디버깅 도구입니다.
[프로그래밍 방식으로][logging] 로깅을 설정하고
DevTools의 [logging view][]나, 콘솔에서
출력을 확인할 수 있습니다.

## Debugging application layers

Flutter는 위젯, 렌더링, 그리고 페인팅 레이어를 포함하는 레이어드 아키텍처로
구현되어 있습니다. 더 많은 정보와 비디오에 대한 링크는 [GitHub wiki][]의
[The Framework architecture][], 그리고 플러터 커뮤니티의
[The Layer Cake][] 기사를 살펴보세요.

Flutter 위젯 인스펙터는
위젯 트리의 시각적 표현을 제공하지만,
더 상세한 수준을 원하거나, 위젯, 레이어,
혹은 렌더 트리에 대한 텍스트 기반의 덤프를 원하신다면,
[Debugging Flutter apps programatically][] 페이지의
[Debug flags: application layers][]를 살펴보세요.

## Debug mode assertions

개발 중에는 Flutter의 [debug mode][]를 사용하시는 것을
적극 권장합니다. 안드로이드 스튜디오에서 벌레 모양 아이콘을 사용하거나,
커맨드 라인에서 `flutter run` 명령어를 사용하실 경우 기본 모드 입니다.
몇 가지 도구는 커맨드 라인 플래그인 `--enable-asserts`을 통해 assert
구문을 지원합니다.

이 모드에서는 Dart assert 구문을 사용할 수 있고,
Flutter 프레임워크가 실행 도중에
만나는 assert 구문을 평가하여
결과가 false인 경우에는 예외를 던지게 됩니다.
개발자가 불변 검사에 대해
활성화 혹은 비활성화 할 수 있으며,
성능 저하는 디버깅 세션일 동안만 발생합니다.

불변성을 위반한 경우에는,
문제의 원인을 찾아갈 수 있도록
일부 상황 정보를 콘솔에 출력합니다.

더 많은 정보를 원하신다면, [Dart language tour][]에
있는 [Assert][]를 살펴보세요.

 ## 애니메이션 디버깅하기

애니메이션을 디버그하는 가장 쉬운 방법은 동작을 느리게 하는 것입니다.
[Flutter inspector][]는 **Slow Animations**을 제공하고 있으며,
[slow the animations programmatically][]을 하셔도 좋습니다.

응답이 없거나, 늦어지는 어플리케이션을 디버깅하는 방법에 대해서
더 알고 싶으시다면 [Flutter performance profiling][]를 살펴보세요.

## 앱 시작 시간 측정

Flutter app을 시작하는데 들어가는 시간에 대한 정보를 알고 싶다면,
`flutter run` 명령어에 `trace-startup`
그리고 `profile` 옵션을 추가해서 실행할 수 있습니다.

```
$ flutter run --trace-startup --profile
```

추적한 결과는 Flutter 프로젝트의 `build` 디렉토리안에
 `start_up_info.json` JSON 파일로 저장됩니다.
출력은 microsecond 단위로 앱의 시작 시점부터 각 추적 이벤트까지의
경과시간을 리스트로 출력합니다.

+ Flutter engine 코드까지 진입한 시간.
+ 앱의 첫 프레임을 렌더링하는데 걸리는 시간.
+ Flutter 프레임워크를 초기화 하기까지 걸리는 시간.
+ Flutter 프레임워크의 초기화를 완료하기 까지 걸리는 시간.

예:

```
{
  "engineEnterTimestampMicros": 96025565262,
  "timeToFirstFrameMicros": 2171978,
  "timeToFrameworkInitMicros": 514585,
  "timeAfterFrameworkInitMicros": 1657393
}
```

## Tracing Dart code

To perform a performance trace,
you can use the DevTools [Timeline view][].
The Timeline view also supports importing
and exporting trace files. For more
information, see the [Timeline view][] docs.

You can also
[perform traces programmatically][],
though these traces can't be imported
into DevTool's Timeline view.

Be sure to use run your app in [profile mode][]
before tracing to ensure that the runtime performance
characteristics closely matches that of your final product.

## Performance overlay

To get a graphical view of the performance of your application,
turn on the performance overlay. You can do this in the
by clicking the **Performance Overlay** button in the
[Flutter inspector][].

You can also turn on the overlay [programmatically][overlay].

For information on how to interpret the graphs in the overlay,
see [The performance overlay][] in
the [Flutter performance profiling][] guide.

## Debug flags

In most cases, you won't need to use the debug flags
directly, as you'll find the most useful debugging
functionality in the [DevTools][] suite. But if you
prefer to use the debug flags directly, see
[Debug flags: performance][] in the
[Debugging Flutter apps programmatically][] page.

## Common problems

The following is a problem that some have encountered on MacOS.

### "Too many open files" exception (MacOS)

The default limit for Mac OS on how many files it can have open at a
time is rather low.  If you run into this limit,
increase the number of available
file handlers using the `ulimit` command:

```
ulimit -S -n 2048
```

If you use Travis or Cirrus for testing, increase the number of
available file handlers that they can open by adding the same line to
flutter/.travis.yml, or flutter/.cirrus.yml, respectively.

## Other resources

You might find the following docs useful:

* [Performance best practices][]
* [Flutter performance profiling][]
* [Using an OEM debugger][]
* [Flutter's modes][]
* [Debugging Flutter apps programatically][]
* [DevTools][]
* [Android Studio/IntelliJ][]
* [VS Code][]


[Flutter enabled IDE/editor]: /docs/get-started/editor

[Debugging Flutter apps programatically]: /docs/testing/code-debugging
[perform traces programmatically]: /docs/testing/code-debugging#tracing-dart-code-performance
[Debug flags: application layers]: /docs/testing/code-debugging#debug-flags-application-layers
[Debug flags: performance]: /docs/testing/code-debugging#debug-flags-performance
[slow the animations programmatically]: /docs/testing/code-debugging#debugging-animations
[breakpoints]: /docs/testing/code-debugging#setting-breakpoints
[logging]: /docs/testing/code-debugging#logging
[Flutter's modes]: /docs/testing/build-modes
[Flutter performance profiling]: /docs/testing/ui-performance
[Performance best practices]: /docs/testing/best-practices
[Using an OEM debugger]: /docs/testing/oem-debuggers

[The Layer Cake]: https://medium.com/flutter-community/the-layer-cake-widgets-elements-renderobjects-7644c3142401

[GitHub wiki]: {{site.github}}/flutter/flutter/wiki/
[Using the Dart analyzer]: {{site.github}}/flutter/flutter/wiki/Using-the-Dart-analyzer
[The Framework architecture]: {{site.github}}/flutter/flutter/wiki/The-Framework-architecture

[Android Studio/IntelliJ]: /docs/development/tools/android-studio#run-app-with-breakpoints
[VS Code]: /docs/development/tools/vs-code#run-app-with-breakpoints
[DevTools]: /docs/development/tools/devtools
[Flutter inspector]: /docs/development/tools/devtools/inspector
[DevTools debugger]: /docs/development/tools/devtools/debugger
[logging view]: /docs/development/tools/devtools/logging
[Timeline view]: /docs/development/tools/devtools/timeline
[The performance overlay]: /docs/testing/ui-performance#the-performance-overlay
[Flutter performance profiling]: /docs/testing/ui-performance
[overlay]: /docs/testing/code-debugging#performance-overlay
[debug mode]: /docs/testing/build-modes#debug
[profile mode]: /docs/testing/build-modes#profile
[release mode]: /docs/testing/build-modes#release

[Assert]: {{site.dart-site}}/guides/language/language-tour#assert
[Dart language tour]: {{site.dart-site}}/guides/language/language-tour
