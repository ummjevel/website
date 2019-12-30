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

**Dec 11, 2019, Flutter Interact Edition**

Flutter 1.12 is live!

For more information, see
[Flutter: the first UI platform designed for ambient computing][],
[Announcing Flutter 1.12: What a year!][] and
the [Flutter 1.12.13][] release notes.

Docs added and updated since the last announcement include:

* To accompany an updated implementation of add-to-app,
  we have added documentation on how to
  [add Flutter to an existing app][] for both iOS and Android.
* If you own plugin code, we encourage you to update to the
  new plugin APIs for Android. For more information, see
  [Migrating your plugin to the new Android APIs][].
* Web support has moved to the beta channel. For more information,
  see [Web support for Flutter][] and
  [Web support for Flutter goes beta][] on the Medium publication.
  Also, the [building a web app with Flutter][] page is updated.
* A new [write your first Flutter app on the web][] codelab
  is added to the [Get started][] docs, and includes
  instructions on setting breakpoints in DevTools!
* A new [implicit animations][] codelab is available
  featuring DartPad.
  (To run it, you don't need to download any software!)
* Alpha support for MacOS (desktop) is now available in
  release 1.13 on the master and dev channels.
  For more information, see [Desktop support for Flutter][].
* The iOS section of the [app size][] page is updated to reflect
  the inclusion of bitcode.
* An alpha release of Flutter Layout Explorer, a new feature
  (and part of the Flutter inspector) that allows you to
  explore a visual representation of your layout is available.
  For more information, see the [Flutter Layout Explorer][] docs.

Other newness:

* A brand new version of [Flutter Gallery][]. There's a 
  link to the runnable sample in the side nav under
  **Samples & Tutorials**.

Happy Fluttering!

[add Flutter to an existing app]: /docs/development/add-to-app
[Announcing Flutter 1.12: What a year!]: https://medium.com/flutter/announcing-flutter-1-12-what-a-year-22c256ba525d
[app size]: /docs/perf/app-size#ios
[building a web app with Flutter]: /docs/get-started/web
[Desktop support for Flutter]: /desktop
[Flutter: the first UI platform designed for ambient computing]: https://developers.googleblog.com/2019/12/flutter-ui-ambient-computing.html?m=1
[Flutter 1.12.13]: /docs/development/tools/sdk/release-notes/release-notes-1.12.13
[Flutter Gallery]: https://flutter.github.io/samples/#/
[Flutter Layout Explorer]: https://flutter.dev/docs/development/tools/devtools/inspector#flutter-layout-explorer
[Flutter Medium publication]: https://medium.com/flutter
[Migrating your plugin to the new Android APIs]: /docs/development/packages-and-plugins/plugin-api-migration
[implicit animations]: /docs/codelabs/implicit-animations
[Web support for Flutter]: /web
[Web support for Flutter goes beta]: https://medium.com/flutter/web-support-for-flutter-goes-beta-35b64a1217c0
[write your first Flutter app on the web]: /docs/get-started/codelab-web
[Get started]: /docs/get-started/install

## Flutter가 처음인가요?

[시작하기]()와
 [첫 번째 앱 작성하기]()를 완료했다면, 
다음 단계가 준비되어 있습니다.

### 문서

Coming from another platform? Check out:
[Android][], [iOS][], [Web][], [React Native][],
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

