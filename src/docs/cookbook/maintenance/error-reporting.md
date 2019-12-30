---
title: 서비스에 에러 보고하기
prev:
  title: 큰 사이즈의 리스트 다루기
  path: /docs/cookbook/lists/long-lists
next:
  title: 스크린간 위젯 애니메이션
  path: /docs/cookbook/navigation/hero-animations
---

While one always tries to create apps that are free of bugs,
they're sure to crop up from time to time.
Since buggy apps lead to unhappy users and customers,
it's important to understand how often your users
experience bugs and where those bugs occur.
That way, you can prioritize the bugs with the
highest impact and work to fix them.

How can you determine how often your users experiences bugs?
Whenever an error occurs, create a report containing the
error that occurred and the associated stacktrace.
You can then send the report to an error tracking
service, such as Sentry, Fabric, or [Rollbar][].

에러 추적 서비스는 사용자가 경험한 모든 에러들을 모아 그룹화합니다. 
이를 통해 얼마나 자주 에러가 발생하고 어디서 사용자가 어려움을 겪는지 알 수 있게 됩니다.

In this recipe, learn how to report errors to the
[Sentry][] crash reporting service using
the following steps:

## 진행 단계

  1. Sentry로부터 DSN 얻기.
  2. Sentry 패키지 import.
  3. `SentryClient` 생성.
  4. 에러 리포팅을 위한 함수 생성.
  5. Dart 에러 catch하고 리포팅하기.
  6. Flutter 에러 catch하고 리포팅하기.

## 1. Sentry로부터 DSN 얻기

Sentry에 에러 리포팅을 하기 전에 Sentry.io 서비스에서 
여러분의 앱을 식별할 수 있는 "DSN"이 필요합니다.

  1. [Create an account with Sentry][].
  2. Log in to the account.
  3. Create a new app.
  4. Copy the DSN.

  1. [Sentry 계정 생성](https://sentry.io/signup/).
  2. 생성한 계정으로 로그인.
  3. 새로운 앱 생성.
  4. DSN 복사.

Import the [`sentry`][] package into the app.
The sentry package makes it easier to send
error reports to the Sentry error tracking service.

```yaml
dependencies:
  sentry: <latest_version>
```

## 3. `SentryClient` 생성

`SentryClient`를 생성하세요. `SentryClient`는 Sentry 
서비스에 에러 리포트를 보내기 위해 사용합니다.

<!-- skip -->
```dart
final SentryClient _sentry = SentryClient(dsn: "App DSN goes Here");
```

## 4. 에러 리포팅을 위한 함수 생성

Sentry 설정이 완료되면 이제 에러 리포팅을 할 수 있습니다. 
개발 단계에서는 에러 리포팅을 할 필요가 없기 때문에, 우선 디버그 모드인지 프로덕션 모드인지 구분할 수 있는 함수를 만드세요.

<!-- skip -->
```dart
bool get isInDebugMode {
  // 기본적으로 프로덕션 모드라고 가정합니다.
  bool inDebugMode = false;

  // Assert 표현식은 개발 단계에서만 사용되며, 프로덕션 모드에서는 무시됩니다.
  // 그러므로 이 코드는 개발 단계에서만 'inDebugMode'를 true로 설정합니다.
  assert(inDebugMode = true);

  return inDebugMode;
}
```

그런 다음 프로덕션 모드일 때는 에러를 리포팅할 수 있도록 `SentryClient`를 사용합니다.

<!-- skip -->
```dart
Future<void> _reportError(dynamic error, dynamic stackTrace) async {
  // exception을 콘솔에 출력합니다.
  print('Caught error: $error');
  if (isInDebugMode) {
    // 디버그 모드에서는 전체 stacktrace를 출력합니다.
    print(stackTrace);
    return;
  } else {
    // 프로덕션 모드에서는 exception과 stacktrace를 Sentry로 보냅니다.
    _sentry.captureException(
      exception: error,
      stackTrace: stackTrace,
    );
  }
}
```

## 5. Dart 에러 catch하고 리포팅하기

개발 환경에 따라 에러 리포팅하는 함수를 작성했으니 이제 Dart 에러를 catch할 방법이 필요합니다.

For this task, run your app inside a custom [`Zone`][].
Zones establish an execution context for the code.
This provides a convenient way to capture all errors
that occur within that context by providing an `onError()`
function.

아래 코드에서는 앱을 새로운 `Zone`에서 실행하고 
`onError()` 콜백을 통해 모든 에러를 잡습니다.

<!-- skip -->
```dart
runZoned<Future<void>>(() async {
  runApp(CrashyApp());
}, onError: (error, stackTrace) {
  // 에러가 발생할 때 마다, `_reportError` 함수를 호출합니다.
  // 발생한 Dart 에러는 개발 환경에 따라 콘솔에 출력되거나 Sentry로 보내집니다.
  _reportError(error, stackTrace);
});
```

## 6. Flutter 에러 catch하고 리포팅하기

Dart 에러 뿐만 아니라, Flutter도 native 코드를 
호출할 때 발생할 수 있는 플랫폼 예외와 같은 에러들을 던질 수 있는데,
이러한 유형의 에러들 역시 반드시 리포팅해야 합니다.

To capture Flutter errors,
override the [`FlutterError.onError`][] property.
If you're in debug mode, use a convenience function
from Flutter to properly format the error.
If you're in production mode, send the error to the
`onError` callback defined in the previous step.

<!-- skip -->
```dart
// Flutter 프레임워크에 의해 보고된 에러를 포착합니다.
FlutterError.onError = (FlutterErrorDetails details) {
  if (isInDebugMode) {
    // 개발 모드에서는 단순히 콘솔에 출력합니다.
    FlutterError.dumpErrorToConsole(details);
  } else {
    // 프로덕션 모드에서는 Sentry에 보고하기 위해 application zone에 보고 합니다.
    Zone.current.handleUncaughtError(details.exception, details.stack);
  }
};
```

## 전체 예제

To view a working example,
see the [Crashy][] example app.


[Crashy]: {{site.github}}/flutter/crashy
[Create an account with Sentry]: https://sentry.io/signup/
[`FlutterError.onError`]: {{site.api}}/flutter/foundation/FlutterError/onError.html
[Rollbar]: https://rollbar.com/
[Sentry]: https://sentry.io/welcome/
[`sentry`]: {{site.pub-pkg}}/sentry
[`Zone`]: {{site.api}}/flutter/dart-async/Zone-class.html
