---
title: Flutter 앱 테스트하기
---

앱의 기능이 많아질수록, 수동으로 테스트하기는 더 어려워집니다.
자동 테스트는 기능과 버그 개선 속도를 유지하여 앱을 출시하기 전에 
정상적으로 작동할 수 있도록 보장해드립니다.

자동 테스트는 아래의 몇 가지 카테고리로 나뉩니다:

* A [_unit test_](#unit-tests) tests a single function, method, or class.
* A [_widget test_](#widget-tests) (in other UI frameworks referred to
  as _component test_) tests a single widget.
* An [_integration test_](#integration-tests)
  tests a complete app or a large part of an app.

Generally speaking, a well-tested app has many unit and widget tests, tracked by
[code coverage](https://en.wikipedia.org/wiki/Code_coverage), plus enough
integration tests to cover all the important use cases. This advice is based on
the fact that there are trade-offs between different kinds of testing, seen
below.

|                      | Unit   | Widget | Integration |
|----------------------|--------|--------|-------------|
| **Confidence**       | Low    | Higher | Highest     |
| **Maintenance cost** | Low    | Higher | Highest     |
| **Dependencies**     | Few    | More   | Most        |
| **Execution speed**  | Quick  | Quick  | Slow        |
{:.table.table-striped}


## 단위 테스트

_단위 테스트_ 는 단위 기능, 방법 또는 클래스를 테스트합니다. 단위 테스트의 목적은 다양한 조건 아래에서 로직 단위의 정확성을 검증하는 것입니다.
일반적으로 테스트 되는 단위 로직의 외부 의존성은 모의 객체로 대체[(mocked out)](/cookbook/testing/mocking)합니다.
일반적으로 단위 테스트는 테스트가 실행되고 있는 동안 저장소를 읽거나 쓰지 않으며, 스크린 렌더링을 하지 않고, 외부에서 사용자 액션을 받지 않습니다.

### 사용 방법

{% include testing_toc.md type='unit' %}

## 위젯 테스트

_위젯 테스트_(다른 UI 프레임워크에서는 _컴포넌트 테스트_ 라 불립니다) 는 단일 위젯을 테스트합니다. 위젯 테스트의 목적은 위젯 UI가 예상한대로 보이고 상호 작용하는지 검증하는 것입니다.
위젯 테스트는 다수의 클래스를 포함하며 적합한 위젯 생명주기 컨텍스트를 제공하는 테스트 환경을 요구합니다.

For example, the Widget being tested should be able to receive and
respond to user actions and events, perform layout, and instantiate child
widgets. A widget test is therefore more comprehensive than a unit test.
However, like a unit test, a widget test's environment is replaced with
an implementation much simpler than a full-blown UI system.

### 사용 방법

{% include testing_toc.md type='widget' %}

## 통합 테스트

_통합 테스트_는 완성된 앱이나 앱의 상당 부분을 테스트합니다. 통합 테스트의 목표는 테스트는 앱이 전체적으로 올바르게 작동하는지, 모든 위젯이 예상대로 서로 통합되어있는지 확인하는 것입니다.
더불어 통합 테스트를 앱의 성능 검증을 위해 사용할 수 있습니다.

일반적으로 _통합 테스트_는 실제 장치나 
iOS 시뮬레이터와 Android 에뮬레이터와 같은 OS 에뮬레이터에서 실행됩니다.
테스트 대상 앱은 일반적으로 결과가 왜곡되지 않도록 테스트 드라이버 코드에서 격리됩니다.

### 사용 방법

{% include testing_toc.md type='integration' %}

## Continuous integration services

연속 통합 (CI) 서비스는 새로운 코드 변경을 푸시할 때 당신의 테스트를 자동적으로 시행하는 것을 허용합니다.
이는 코드 변경이 예상대로 작동하는지, 버그가 생기지 않는지에 대한 여부를 때에 맞춰 피드백합니다.

For information on running tests on various continuous integration services,
see the following:

* [Continuous delivery using fastlane with
  Flutter](/docs/deployment/cd#fastlane)
* [Test Flutter apps on
  Travis]({{site.flutter-medium}}/test-flutter-apps-on-travis-3fd5142ecd8c)
* [Test Flutter apps on Cirrus](https://cirrus-ci.org/examples/#flutter)
* [Codemagic CI/CD for Flutter](https://blog.codemagic.io/getting-started-with-codemagic/)
* [Flutter CI/CD with Bitrise](https://devcenter.bitrise.io/getting-started/getting-started-with-flutter-apps/)
