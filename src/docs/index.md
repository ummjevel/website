---
title: Flutter 문서
short-title: 문서
description: Flutter 문서의 랜딩 페이지
---

{% for card in site.data.docs_cards -%}
  {% capture index0Modulo3 %}{{ forloop.index0 | modulo:3 }}{% endcapture %}
  {% capture indexModulo3 %}{{ forloop.index | modulo:3 }}{% endcapture %}
  {% if index0Modulo3 == '0' %}
  <div class="card-deck mb-4">
  {% endif %}
    <a class="card" href="{{card.url}}">
      <div class="card-body">
        <header class="card-title">{{card.name}}</header>
        <p class="card-text">{{card.description}}</p>
      </div>
    </a>
  {% if indexModulo3 == '0' %}
  </div>
  {% endif %}
{% endfor -%}

<a name="latest-release"></a>
## 사이트 개편 내역

To stay on top of Flutter announcements,
including breaking changes, join the
[flutter-announce][] Google group.

**Sept 10, 2019**

Flutter 1.9 is live!

더 많은 정보를 원하시면, [Flutter news from GDD China: uniting Flutter on web and mobile, and introducing Flutter
1.9](https://developers.googleblog.com/2019/09/flutter-news-from-gdd-china-flutter1.9.html?m=1)와
[1.9.1 release notes]({{site.github}}/flutter/flutter/wiki/Release-Notes-Flutter-1.9.1)를
참조하세요.

1.9 버전에선, Flutter의 웹 지원이 
메인 저장소로 통합(merged, unforked)됐습니다.
**웹 지원은 아직 베타 전 단계이고, 
프로덕션에 사용되기에는 아직 준비가 덜 됐습니다.**
원래는 Flutter 모바일 앱만을 개발하기 위해 만들어진 이 웹사이트에도 
웹과 데스크탑 지원 (곧 나올) 관련 내용이 반영될 것입니다.
일부 웹사이트 업데이트는 지금도 이용할 수 있지만 (아래 목록에 있는),
앞으로 더 많은 것들이 나올 것입니다.

이 사이트의 새롭게 개편된 문서는 아래의 업데이트를 포함합니다:  

* We've revamped the [Showcase](/showcase) page.
* The Flutter layout codelab has been rewritten and
  uses the updated DartPad, the browser-based tool for
  running Dart code. DartPad now supports Flutter!
  [Try it out](/docs/codelabs/layout-basics)
  and let us know what you think.
* A new page on [using the dart:ffi
  library](/docs/development/platform-integration/c-interop)
  to bind your app to native code (a feature currently under
  development).
* The Performance view tool, which allows you to record
  and profile a session from your Dart/Flutter application,
  has been enabled in DevTools. For more information,
  see the [Performance
  view](/docs/development/tools/devtools/performance)
  page.
* A new page on
  [building a web application](/docs/get-started/web).
* A new page on [creating responsive
  apps](/docs/development/ui/layout/responsive)
  in Flutter.
* A new page on
  [preparing a web app for release](/docs/deployment/web).
* A new [web FAQ](/docs/development/platform-integration/web).
* The [Flutter for web](/web) page is updated.

Other relevant docs:

* Error messages have been improved in SDK 1.9.
  For more information, read [Improving Flutter's
  Error Messages](https://medium.com/flutter/improving-flutters-error-messages-e098513cecf9)
  on the [Flutter Medium publication](https://medium.com/flutter).
* If you already have a web app that depends on the
  flutter_web package, the following instructions tell
  you how to migrate to the flutter package:
  [Upgrading from package:flutter_web to the Flutter
  SDK](https://github.com/flutter/flutter/wiki/Upgrading-from-package:flutter_web-to-the-Flutter-SDK).
* A new [ToggleButtons]({{site.api}}/flutter/material/ToggleButtons-class.html)
  widget, described in the API docs.
  [ToggleButtons demo]({{site.github}}/csells/flutter_toggle_buttons)
* A new [ColorFiltered]({{site.api}}/flutter/widgets/ColorFiltered-class.html)
  widget, also described in the API docs.
  [ColorFiltered demo]({{site.github}}/csells/flutter_color_filter)
* New behavior for the
  [SelectableText]({{site.api}}/flutter/material/SelectableText-class.html)
  widget.

Happy Fluttering!

[What's new archive][]

## Flutter가 처음인가요?

[시작하기]()와
 [첫 번째 앱 작성하기]()를 완료했다면, 
다음 단계가 준비되어 있습니다.

### 문서

다른 플랫폼 경험이 있으신가요? 아래 문서를 확인해보세요:
[Android][], [iOS][], [웹][], [React Native][],
[Xamarin.Forms][]

[Flutter의 레이아웃]()
: 레이아웃을 만드는 방법을 배워보세요. Flutter에서는 모든 게 위젯입니다.

[Flutter 앱에 상호작용 추가하기][]
: 상태를 저장할 수 있는 위젯을 만드는 방법을 배워보세요.

[Flutter 위젯 프레임워크 둘러보기][]
: react-style 프레임워크 Flutter에 대해 더 알아봅시다.

[자주 묻는 질문][]
: 자주 묻는 질문에 대한 답변을 살펴보세요.

### 영상

도움이 될만한 영상을 [Flutter 유튜브 채널]()을 통해 제공합니다!
특히 Flutter in Focus 시리즈를 보고, 
비디오 페이지에서 다른 시리즈를 통해 배워보세요.

First up, why use Flutter? What makes it different than other
app frameworks?

<iframe width="560" height="315" src="https://www.youtube.com/embed/l-YO9CmaSUM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe><br>
How is Flutter different for app development?

<iframe style="max-width: 100%" width="560" height="315" src="https://www.youtube.com/embed/wgTBLj7rMPM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
Flutter in Focus: Learn Flutter features in 10 minutes or less.<br>
[Flutter in Focus 재생 목록][]

Flutter에서는 "모든 것이 위젯"입니다!
Stateless과 Stateful라는 2가지 종류의 위젯을 보다 잘 이해하기 위해,
[Flutter in Focus]() 시리즈
를 확인해보세요.

<iframe style="max-width: 100%" width="560" height="315" src="https://www.youtube.com/embed/wE7khGHVkYY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> <iframe width="560" height="315" src="https://www.youtube.com/embed/AqCMFXEmf3w" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 한 단계 더 나아가고 싶나요?

엔지니어가 코드를 작성하고, 실수하고, 고치는 모습을 보면서 배우길 원한다면
[Boring Flutter Show]()
영상 시리즈를 확인하세요:

<iframe style="max-width: 100%" width="560" height="315" src="https://www.youtube.com/embed/vqPG1tU6-c0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
[Boring Flutter Show 재생 목록][]

아래 문서들도 유용할 것입니다:

* [Using packages][]
* [Adding assets and images][]
* [Navigation and routing][]
* [State management][]
* [Animations][]


[A tour of the Flutter widget framework]: /docs/development/ui/widgets-intro
[리소스 및 이미지 추가하기]: /docs/development/ui/assets-and-images
[Adding interactivity to your Flutter app]: /docs/development/ui/interactive
[Android]: /docs/get-started/flutter-for/android-devs
[애니메이션]: /docs/development/ui/animations
[Boring Flutter Show]: https://www.youtube.com/watch?v=vqPG1tU6-c0&list=PLjxrf2q8roU28W3pXbISJbVA5REsA41Sx&index=3&t=9s
[Boring Flutter Show playlist]: https://www.youtube.com/watch?v=vqPG1tU6-c0&list=PLjxrf2q8roU28W3pXbISJbVA5REsA41Sx&index=3&t=9s
[Building layouts]: /docs/development/ui/layout
[FAQ]: /docs/resources/faq
[flutter-announce]: https://groups.google.com/forum/#!forum/flutter-announce
[Flutter in Focus]: https://www.youtube.com/playlist?list=PLjxrf2q8roU2HdJQDjJzOeO6J3FoFLWr2
[Flutter in Focus playlist]: https://www.youtube.com/playlist?list=PLjxrf2q8roU2HdJQDjJzOeO6J3FoFLWr2
[Flutter Youtube channel]: {{site.social.youtube}}
[Get Started]: /docs/get-started/install
[iOS]: /docs/get-started/flutter-for/ios-devs
[내비게이션 및 라우팅]: /docs/development/ui/navigation
[React Native]: /docs/get-started/flutter-for/react-native-devs
[상태 관리]: /docs/development/data-and-backend/state-mgmt/intro
[패키지 사용하기]: /docs/development/packages-and-plugins/using-packages
[videos]: /docs/resources/videos
[Web]: /docs/get-started/flutter-for/web-devs
[What's new archive]: /docs/whats-new-archive
[Write Your First Flutter App,]: /docs/get-started/codelab
[Xamarin.Forms]: /docs/get-started/flutter-for/xamarin-forms-devs

