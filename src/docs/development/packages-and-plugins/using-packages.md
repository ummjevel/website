---
title: 패키지 사용하기
description: Flutter 앱에서 패키지를 사용하는 방법을 학습합니다.
---

Flutter는 다른 개발자들이 Flutter와 Dart 에코시스템에 제공한 공유 패키지들을 
사용할 수 있도록 지원합니다. 이 패키지를 이용하면 
처음부터 모든 것을 개발하지 않아도 되기 때문에 빠르게 앱을 개발할 수 있습니다. 

이미 만들어진 패키지들을 이용하면 많은 것을 할 수 있습니다.
예를 들어서 네트워크 요청 ([`http`][]),
사용자 지정 탐색/경로 관리 ([`fluro`][]),
장비 API 통합
([`url_launcher`][]와 [`battery`][])가 있고, 
또한 Firebase
([FlutterFire][]) 같은 서드파티 플랫폼 SDK를 사용할 수 있습니다.

새로운 패키지 개발에 대해서는, [패키지 개발하기][]를 참조하세요.
파일이나 패키지에 있는 리소스, 이미지 또는 글꼴을 추가하는 방법에 대해서는,
[리소스 & 이미지 파일][]를 참조하세요.

## 패키지 사용하기

아래 섹션은 이미 만들어져 배포된 패키지를 사용하는 방법을 설명합니다.

### 패키지 검색하기

패키지들은 [pub.dev][]에 게시됩니다.

pub.dev의 [Flutter landing page][]에서 
Flutter와 호환되는 인기있는 패키지를 확인할 수 있습니다
(일반적으로 Flutter 호환 가능하다고 종속성을 선언한 패키지).
또한, 배포된 모든 패키지를 패키지를 검색할 수도 있습니다.

### 앱에 패키지 추가하기

`css_colors` 패키지를 앱에 추가하는 방법은 아래와 같습니다.

1. 의존성 추가하기
   * 앱 폴더 하위에 있는 `pubspec.yaml` 파일을 열고, 
     `dependencies` 아래에 `css_colors:`를 추가합니다.

1. 설치하기
   * 터미널: `flutter pub get` 명령어를 실행합니다<br/>
   **또는**
   * Android Studio/IntelliJ: `pubspec.yaml` 파일을 열고, 
     상단에서 **Packages get** 버튼을 클릭합니다
   * VS Code: `pubspec.yaml` 파일을 열고, 
     우측 상단에서 **Get Packages** 버튼을 클릭합니다

1. 불러오기 
   * 해당하는 `import` 구문을 Dart 코드에 추가합니다.

1. 필요시 앱을 중지하고 재시작하기
   * 만약에 패키지가 특정 플랫폼의 코드를 사용한다면 (Android: Java/Kotlin, 
     iOS: Swift/Object-C), 꼭 미리 빌드가 되어야 합니다. 
     Hot reload와 hot restart는 Dart 코드에 대해서만 동작하기 때문에, 
     패키지를 사용하면서 `MissingPluginException` 에러를 피하기 위해서는 
     앱을 완전히 재시작해야 합니다. 

Pub 사이트에 있는 각 패키지의 [Installing 탭][]에서는 
패키지를 설치하는 이런 과정에 대해서 간단히 설명해주고 있습니다.

전체 예제를 원하신다면, 아래에 있는 [css_colors example][]을 보시면 됩니다.

### Conflict resolution

Suppose you want to use `some_package` and `another_package` in an app,
and both of these depend on `url_launcher`, but in different versions.
That causes a potential conflict.
The best way to avoid this is for package authors to use
[version ranges][]
rather than specific versions when specifying dependencies.

```yaml
dependencies:
  url_launcher: ^0.4.2    # Good, any 0.4.x version where x >= 2 works.
  image_picker: '0.1.1'   # Not so good, only version 0.1.1 works.
```

If `some_package` declares the dependencies above and `another_package`
declares a compatible `url_launcher` dependency like `'0.4.5'` or
`^0.4.0`, Pub resolves the issue automatically.
Platform-specific dependencies on [Gradle modules][] and/or [CocoaPods][]
are solved in a similar way.

Even if `some_package` and `another_package` declare incompatible versions
for `url_launcher`, they might actually use `url_launcher` in
compatible ways. In this situation, the conflict can be resolved by adding
a dependency override declaration to the app's `pubspec.yaml` file,
forcing the use of a particular version.

To force the use of `url_launcher` version `0.4.3`,
make the following changes to the app's `pubspec.yaml` file:

```yaml
dependencies:
  some_package:
  another_package:
dependency_overrides:
  url_launcher: '0.4.3'
```

If the conflicting dependency is not itself a package,
but an Android-specific library like `guava`, the dependency override
declaration must be added to Gradle build logic instead.

To force the use of `guava` version `23.0`, make the following
changes to the app's `android/build.gradle` file:

```groovy
configurations.all {
    resolutionStrategy {
        force 'com.google.guava:guava:23.0-android'
    }
}
```

CocoaPods does not currently offer dependency override functionality.

## 새로운 패키지 개발하기

여러분에게 딱 맞는 패키지를 찾을 수 없다면,
[새로운 패키지를 개발][]할 수도 있습니다.

## 패키지 의존성과 버전 관리

버전 충돌의 위험을 최소화할 수 있도록,
pubspec 파일의 버전을 지정해주세요.

### 패키지 버전

모든 패키지들은 버전 번호를 가지고 있고, `pubspec.yaml` 파일에 명시되어 있습니다.
패키지의 현재 버전은 패키지 이름 다음에 표시되는데 
(예: [`url_launcher`][] 패키지), as
전체 버전 리스트도 확인 가능합니다
([url_launcher 버전 리스트][]).

패키지가 `pubspec.yaml` 에 추가되었을 때 `plugin1:`과 
같이 줄여서 표현하면, 사용 가능한 plugin1 패키지 중 아무 버전이나 설치해도 좋다는 뜻입니다.
하지만 패키지가 업데이트 되었을 때 앱에서 문제가 발생하지 않도록 하기 위해서, 
패키지가 사용될 수 있는 버전의 범위를 아래와 같이 명시해 주는 것이 좋습니다:

* 범위 설정 조건: 최소 버전과 최대 버전을 명시. 예시:

  ```yaml
  dependencies:
    url_launcher: '>=0.1.2 <0.2.0'
  ```

* [*캐럿 문법*][]을
  이용해도 기존 범위 설정 조건과 유사하게 표현할 수 있습니다:

  ```yaml
  dependencies:
    collection: '^0.1.2'
  ```

추가적인 정보는, [package versioning guide][]를 참조해주세요.

### 패키지 의존성 업데이트

처음 패키지를 추가하고 `flutter pub get` (IntelliJ에서는 **Packages Get**)을 실행하면 
Flutter는 `pubspec.lock` 
[lockfile][]에 
구체적인 버전 정보를 저장합니다. 이 정보를 이용해서 나중에 여러분 또는 
여러분의 팀이 `flutter pub get`을 
실행하면 동일한 버전을 가져올 수 있습니다. 

사용하는 패키지의 새로운 기능을 사용하고 싶거나 또는 
다른 이유로 패키지의 버전을 업그레이드 하기 원하신다면 
`flutter pub upgrade` (IntelliJ에서는 **Upgrade dependencies**)를 
실행하면 됩니다. 이 명령어를 실행하면 `pubspec.yaml`에 명시된 버전 범위에서 
사용 가능한 가장 높은 버전을 가져옵니다.

### 게시되지 않은 패키지의 종속성

Pub에 게시되지 않은 패키지도 사용 가능합니다. 
추가적인 의존성 옵션을 통해서, 일반 공개를 원하지 않는 개인적인 플러그인이나   
아직 게시할 준비가 되지 않은 패키지를 사용할 수 있습니다. 

* **경로** 종속성: Flutter 앱은 파일 시스템 `path:` 를 이용해서 플러그인에 
  종속될 수 있습니다. 경로는 상대 경로, 절대 경로 모두 가능합니다. 
  예를 들어서 앱의 하위 디렉토리에 있는 'plugin1' 플러그인의 종속성을 추가하려면 
  아래 구문을 사용하시면 됩니다.
   
  ```
  dependencies:
    plugin1:
      path: ../plugin1/
  ```

* **Git** 종속성: Git 리포지토리에 저장 되어 있는 패키지에 대한 종속을 사용할 수도 
  있습니다. 패키지가 최상위 리포지토리에 있다면 아래 구문을 사용하시면 됩니다. 
  ```
  dependencies:
    plugin1:
      git:
        url: git://github.com/flutter/plugin1.git
  ```

* 패키지의 폴더에 있는 **Git** 종속성: 기본적으로 Pub은 
  패키지가 Git 리포지토리의 최상위 경로에 있다고 가정합니다. 만약 이 경우가 아니라면, 
  `path` 인자를 이용해서 경로를 직접 지정해 줄 수 있습니다. 예: 
  
  ```yaml
  dependencies:
    package1:
      git:
        url: git://github.com/flutter/packages.git
        path: packages/package1
  ```

  마지막으로, `ref` 인수를 사용하면 특정 git 커밋, 브랜치, 또는 태그에 의존성을 고정할 수 
  있습니다. 자세한 내용은, 
  [Package dependencies][]에서 확인하실 수 있습니다. 

## 예제

아래 예제는 패키지를 사용할 때 꼭 필요한 단계에 대해 다룹니다.

### 예제: css_colors 패키지 사용하기 {#css-example}

wherever the Flutter framework expects the `Color` type.
[`css_colors`][] 패키지는 CSS 색을 
상수로 정의해서, Flutter 프레임워크에서 `Color` 타입을 필요로 하는 곳에 
사용할 수 있도록 합니다. 

패키지 사용 방법:

1. `cssdemo` 이름의 새로운 프로젝트를 생성하세요

1. `pubspec.yaml` 파일을 열고, `css_colors`를 추가하세요:

   ```yaml
   dependencies:
     flutter:
       sdk: flutter
     css_colors: ^1.0.0
   ```

1. 터미널에서 `flutter pub get` 을 실행하거나, 
   IntelliJ에서 **Packages get** 을 클릭하세요.

1. `lib/main.dart` 파일을 열고 아래 내용으로 모두 변경하세요.
 
    ```dart
    import 'package:css_colors/css_colors.dart';
    import 'package:flutter/material.dart';

    void main() {
      runApp(MyApp());
    }

    class MyApp extends StatelessWidget {
      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          home: DemoPage(),
        );
      }
    }

    class DemoPage extends StatelessWidget {
      @override
      Widget build(BuildContext context) {
        return Scaffold(body: Container(color: CSSColors.orange));
      }
    }
    ```

1. 앱을 실행하세요. 앱의 배경이 노란색으로 바뀌어야 합니다.


### 예제: url_launcher 패키지를 사용해서 브라우저 실행하기 {#url-example}

[url_launcher][] 플러그인 패키지를 사용하면 주어진 
URL을 모바일 플랫폼의 기본 브라우저를 이용해서 표시할 수 있습니다. 
이는 플랫폼별 코드가 패키지에 포함될 수 있다는 것을 보여줍니다 
(이런 패키지를 _플러그인_ 이라고 부릅니다). Android와 iOS 모두 지원됩니다.

패키지 사용 방법:

1. 'launchdemo'라는 이름의 새로운 프로젝트를 생성합니다

1. `pubspec.yaml` 파일을 열고, `url_launcher`를 추가하세요:

   ```yaml
   dependencies:
     flutter:
       sdk: flutter
     url_launcher: ^0.4.1
   ```

1. 터미널에서 `flutter pub get` 을 실행하거나, 
   IntelliJ에서 **Packages get** 을 클릭하세요.

1. `lib/main.dart` 파일을 열고 아래 내용으로 모두 변경하세요.

    ```dart
    import 'package:flutter/material.dart';
    import 'package:url_launcher/url_launcher.dart';

    void main() {
      runApp(MyApp());
    }

    class MyApp extends StatelessWidget {
      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          home: DemoPage(),
        );
      }
    }

    class DemoPage extends StatelessWidget {
      launchURL() {
        launch('https://flutter.dev');
      }

      @override
      Widget build(BuildContext context) {
        return Scaffold(
          body: Center(
            child: RaisedButton(
              onPressed: launchURL,
              child: Text('Show Flutter homepage'),
            ),
          ),
        );
      }
    }
    ```

1. 앱을 실행하세요 (플러그인을 추가하기 전부터 앱을 실행중이었다면, 멈추고 재시작합니다). 
   **Show Flutter homepage**를 클릭하면, 핸드폰의 기본 브라우저를 
   이용해서 Flutter 홈페이지가 나타나는 것을 확인할 수 있습니다.


[리소스 & 이미지 파일]: /docs/development/ui/assets-and-images
[`battery`]: {{site.pub-pkg}}/battery
[*캐럿 문법*]: {{site.dart-site}}/tools/pub/dependencies#caret-syntax
[CocoaPods]: https://guides.cocoapods.org/syntax/podspec.html#dependency
[`css_colors`]: {{site.pub-pkg}}/css_colors
[css_colors example]: #css-example
[새로운 패키지를 개발]: /docs/development/packages-and-plugins/developing-packages
[패키지 개발하기]: /docs/development/packages-and-plugins/developing-packages
[`fluro`]: {{site.pub-pkg}}/fluro
[Flutter landing page]: {{site.pub}}/flutter
[FlutterFire]: {{site.github}}/flutter/plugins/blob/master/FlutterFire.md
[Gradle modules]: https://docs.gradle.org/current/userguide/introduction_dependency_management.html
[`http`]: /docs/cookbook/networking/fetch-data
[Installing 탭]: {{site.pub-pkg}}/css_colors#-installing-tab-
[lockfile]: {{site.dart-site}}/tools/pub/glossary#lockfile
[Package dependencies]: {{site.dart-site}}/tools/pub/dependencies
[package versioning guide]: {{site.dart-site}}/tools/pub/versioning
[pub.dev]: {{site.pub}}
[`url_launcher`]: {{site.pub-pkg}}/url_launcher
[url_launcher 버전 리스트]: {{site.pub-pkg}}/url_launcher#-versions-tab-
[version ranges]: {{site.dart-site}}/tools/pub/dependencies#version-constraints
