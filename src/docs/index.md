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

## 사이트 개편 내역

To stay on top of Flutter announcements, including breaking changes, join the
[flutter-announce](https://groups.google.com/forum/#!forum/flutter-announce)
Google group.

**July 9, 2019**

Flutter 1.7 is live!

더 많은 정보를 원하시면, [Flutter Medium Publication]({{site.flutter-medium}})의
[Announcing Futter 1.7]({{site.flutter-medium}}/announcing-flutter-1-7-9cab4f34eacf)와
[Flutter wiki]({{site.github}}//flutter/flutter/wiki)의
[1.7.8 release
notes]({{site.github}}/flutter/flutter/wiki/Release-Notes-Flutter-1.7.8)를
참조하세요.

이 사이트의 새롭게 개편된 문서는 아래의 업데이트를 포함합니다:  

* The [Preparing an Android app for release](/docs/deployment/android)
  page is updated to discuss how to build an Android release
  using an app bundle, as well as how to create separate APK
  files for both 32-bit and 64-bit devices.
* The [DevTools](/docs/development/tools/devtools) docs are migrated
  to flutter.dev. If you haven't tried this browser-based suite
  of debugging, performance, memory, and inspection tools that
  work with both Flutter and Dart apps and can be launched from
  Android Studio/IntelliJ _and_ VS Code, please check it out!
* The [Simple app state
  management](/docs/development/data-and-backend/state-mgmt/simple)
  page is updated. The example code in the page now uses the 3.0
  release of the Provider package.
* A new animation recipe, [Animate a page route
  transition](/docs/cookbook/animation/page-route-animation)
  has been added to the [Cookbook](/docs/cookbook).
* The [Debugging](/docs/testing/debugging), 
  [Flutter's build modes](/docs/testing/build-modes),
  [Performance best practices](/docs/testing/best-practices), and
  [Performance profiling](/docs/testing/ui-performance)
  pages are updated to reflect DevTools. A
  [Debugging apps programmatically](/docs/testing/code-debugging)
  page has also been added.

Flutter 1.7 release는 새로운 컴포넌트
[RangeSlider]({{site.api}}/flutter/material/RangeSlider-class.html)
를 포함합니다. 이 컴포너트는 사용자가 하한과 상한 양쪽을 선택할 수 있게 해줍니다.
이 컴포넌트에 관해 더 많은 정보 원하시면, 혹은 이 컴포넌트를 커스터마이징 하고 싶다면,
[Material RangeSlider in Flutter]({{site.flutter-medium}}/material-range-slider-in-flutter-a285c6e3447d)를
참조하세요.

Happy Fluttering!

[What's new archive](/docs/whats-new-archive)

## Flutter가 처음인가요?

[시작하기](/docs/get-started/install)와
 [첫 번째 앱 작성하기](/docs/get-started/codelab)를 완료했다면, 
다음 단계가 준비되어 있습니다.

### 문서

다른 플랫폼 경험이 있으신가요? 아래 문서를 확인해보세요:
[Android](/docs/get-started/flutter-for/android-devs),
[iOS](/docs/get-started/flutter-for/ios-devs),
[웹](/docs/get-started/flutter-for/web-devs),
[React Native](/docs/get-started/flutter-for/react-native-devs),
[Xamarin.Forms](/docs/get-started/flutter-for/xamarin-forms-devs)

[Flutter에서 레이아웃](/docs/development/ui/layout)
: 레이아웃을 만드는 방법을 배워보세요. Flutter에서는 모든 게 위젯입니다.

[Flutter 앱에 상호작용 추가하기](/docs/development/ui/interactive)
: 상태를 저장할 수 있는 위젯을 만드는 방법을 배워보세요.

[Flutter 위젯 프레임워크 둘러보기](/docs/development/ui/widgets-intro)
: react-style 프레임워크 Flutter에 대해 더 알아봅시다.

[자주 묻는 질문](/docs/resources/faq)
: 자주 묻는 질문에 대한 답변을 살펴보세요.

### 영상

도움이 될만한 영상을 [Flutter 유튜브 채널]({{site.social.youtube}})을 통해 제공합니다!
특히 Flutter in Focus 시리즈를 보고, 
비디오 페이지에서 다른 시리즈를 통해 배워보세요.

<iframe style="max-width: 100%" width="560" height="315" src="https://www.youtube.com/embed/wgTBLj7rMPM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
Flutter in Focus: Learn Flutter features in 10 minutes or less.<br>
[Flutter in Focus 재생 목록](https://www.youtube.com/playlist?list=PLjxrf2q8roU2HdJQDjJzOeO6J3FoFLWr2)

Flutter에서는 "모든 것이 위젯"입니다!
Stateless과 Stateful라는 2가지 종류의 위젯을 보다 잘 이해하기 위해,
[Flutter in Focus](https://www.youtube.com/playlist?list=PLjxrf2q8roU2HdJQDjJzOeO6J3FoFLWr2) 시리즈
를 확인해보세요.

<iframe style="max-width: 100%" width="560" height="315" src="https://www.youtube.com/embed/wE7khGHVkYY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> <iframe width="560" height="315" src="https://www.youtube.com/embed/AqCMFXEmf3w" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 한 단계 더 나아가고 싶나요?

엔지니어가 코드를 작성하고, 실수하고, 고치는 모습을 보면서 배우길 원한다면
[Boring Flutter Show](https://www.youtube.com/watch?v=vqPG1tU6-c0&list=PLjxrf2q8roU28W3pXbISJbVA5REsA41Sx&index=3&t=9s)
영상 시리즈를 확인하세요:

<iframe style="max-width: 100%" width="560" height="315" src="https://www.youtube.com/embed/vqPG1tU6-c0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
[Boring Flutter Show 재생 목록](https://www.youtube.com/watch?v=vqPG1tU6-c0&list=PLjxrf2q8roU28W3pXbISJbVA5REsA41Sx&index=3&t=9s)

아래 문서들도 유용할 것입니다:

* [패키지 사용하기](/docs/development/packages-and-plugins/using-packages)
* [리소스 및 이미지 추가하기](/docs/development/ui/assets-and-images)
* [내비게이션 및 라우팅](/docs/development/ui/navigation)
* [상태 관리](/docs/development/data-and-backend/state-mgmt/intro)
* [애니메이션](/docs/development/ui/animations)

