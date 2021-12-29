---
title: 반응형 및 적응형 앱 만들기
description: 이것은 앱을 만드는 데 있어서 모바일 혹은 웹에 상관없이 중요해서, 사이즈와 방향이 변했을 때 앱이 반응하도록 합니다.
short-title: 반응형과 적응형
---

플러터의 중요한 목표 중 하나는 어떤 플랫폼이든 보고 기분이 좋아지는 
하나의 코드베이스로부터 앱을 개발하는 것을 허용하는 프레임워크를 만드는 것입니다.

이것은 당신의 앱이 많은 다른 사이즈의 화면들, 와치에서든,
두 개의 스크린으로 이루어져 접을 수 있는 곳에서든, 고선명 모니터에서든
보여질 수 있다는 것을 의미합니다.

시나리오를 위한 컨셉을 설명하는 두 개의 용어는 
_적응형_ 과 _반응형_ 입니다. 이상적으로,
당신은 당신의 앱이 _두 가지_ 를 동시에 지원하기를 원하지만,
정확히, 이게 무슨 뜻이죠?
이 용어들은 비슷하지만, 같지 않습니다.  

## 적응형과 반응형 앱의 차이점

_적응형_ 과 _반응형_ 은 앱의 분리된 관점으로 보여질 수 있습니다:
당신은 반응형은 아니지만 적응형 앱을 만들 수도 있고, 반대로도 만들 수 있습니다.
그리고, 당연히, 앱은 두 가지 모두 가질 수 있고, 혹은 둘 다 없을 수 있습니다. 

**반응형**
: 전형적으로, _반응형_ 앱은 각 레이아웃이 스크린 사이즈에 맞춰지도록 조정되어있습니다.
  주로 이 뜻은 예를 들어, 만약 사용자가 창을 재조정했을 때 혹은 
  디바이스의 방향을 바꿨을 경우 UI를 다시 배치하는 것을 의미합니다.
  이것은 특히 같은 앱이 와치부터 폰, 태블릿, 노트북 혹은 데스크탑에 이르기까지
  다양한 디바이스를 지원해야할 때 필요합니다.

**적응형**
: _적응형_ 앱은 모바일과 데스크탑과 같은 다른 디바이스 유형에서 실행시키기 위해
  터치 입력을 제공하는 만큼 마우스와 키보드 입력을 다루는 것을
  제공합니다. 이것은 또한 앱의 시각적 밀도에 대해 다른 기대가 있다는 것을 의미합니다.
  어떻게 컴포넌트 선택이 작동하는지(예를 들어, 계층형 메뉴 vs 버텀 시트),
  특정 플랫폼의 특징들을 사용하는 것(탑-레벨 윈도우와 같은) 등입니다.

## 반응형 플러터 앱 만들기

플러터는 디바이스의 스크린 사이즈와 방향에 맞출 수 있는
자가적응하는 앱 만드는 것을 허용합니다.

반응형 디자인을 포함한 플러터 앱을 만들기 위해서 두 가지의 기본적인 접근방법이 있습니다:

**[`LayoutBuilder`][] 클래스를 사용하세요**
: [`builder`][] 속성으로 [`BoxConstraints`][] 오브젝트를 가질 수 있습니다.
  무엇을 보여주기 위한 것인지 결정하기 위해 제약사항의 속성을 분석합니다.
  예를 들어, 만약 당신의 [`maxWidth`][] 이 당신의 너비 중단점보다 더 클 경우,
  [`Scaffold`][] 오브젝트를 죄측의 리스트를 가진 로우와 함께 반환합니다.
  만약 그것이 더 좁으면, 해당 리스트를 포함하는 드로어와 함께 [`Scaffold`][] 오브젝트를 
  반환합니다. 당신은 디바이스의 높이, 가로 세로 비율, 혹은 일부 다른 속성을 기반으로 하여
  당신의 화면을 맞출 수 있습니다. 제약조건들이 변경될 때(예를 들면, 사용자가 전화기를 회전하거나, 
  안드로이드 누가에서 당신의 앱을 타일에 넣어놓는 것) 빌드 함수가 실행됩니다.

**당신의 빌드 함수들 안에 있는 [`MediaQuery.of()`][] 메소드를 사용하세요**
: 이것은 당신의 현재 앱의 사이즈, 방향 등을 알려줍니다.
  이것은 만약 당신이 단순히 당신의 특정한 위젯의 사이즈가 아닌
  완전한 컨텍스트를 기반으로 하여 결정하길 원할 때, 더 쓸모가 있습니다.
  또한, 만약 당신이 이것을 사용한다면, 그러면 당신의 빌드 함수는 자동적으로
  사용자가 어떤 이유로 앱의 사이즈를 변경할 때 실행됩니다.

반응형 UI를 만들기 위한 쓸모있는 다른 위젯들과 클래스들:

* [`AspectRatio`][]
* [`CustomSingleChildLayout`][]
* [`CustomMultiChildLayout`][]
* [`FittedBox`][]
* [`FractionallySizedBox`][]
* [`LayoutBuilder`][]
* [`MediaQuery`][]
* [`MediaQueryData`][]
* [`OrientationBuilder`][]

### 다른 리소스들

더 많은 정보를 원한다면, 여기에 플러터 커뮤니티로부터의 기여를 포함한
몇 가지 리소스들이 있습니다:

* [플러터에서 다양한 스크린 사이즈와 방향을 지원하는 개발][] by Deven Joshi
* [플러터에서 반응형 UI 만들기][] by Raouf Rahiche
* [반응형 랜딩 페이지를 지원하는 플러터 크로스 플랫폼 만들기][]
  by Priyanka Tyagi
* [다른 스크린 사이즈에 따른 반응형 플러터 앱 만드는 법?][], StackOverflow에 있는 질문


[`AspectRatio`]: {{site.api}}/flutter/widgets/AspectRatio-class.html
[`BoxConstraints`]: {{site.api}}/flutter/rendering/BoxConstraints-class.html
[플러터에서 반응형 UI 만들기]: {{site.medium}}/flutter-community/build-responsive-uis-in-flutter-fd450bd59158
[`builder`]: {{site.api}}/flutter/widgets/LayoutBuilder/builder.html
[`CustomMultiChildLayout`]: {{site.api}}/flutter/widgets/CustomMultiChildLayout-class.html
[`CustomSingleChildLayout`]: {{site.api}}/flutter/widgets/CustomSingleChildLayout-class.html
[플러터에서 다양한 스크린 사이즈와 방향을 지원하는 개발]: {{site.medium}}/flutter-community/developing-for-multiple-screen-sizes-and-orientations-in-flutter-fragments-in-flutter-a4c51b849434
[`FittedBox`]: {{site.api}}/flutter/widgets/FittedBox-class.html

[`FractionallySizedBox`]: {{site.api}}/flutter/widgets/FractionallySizedBox-class.html
[다른 스크린 사이즈에 따른 반응형 플러터 앱 만드는 법?]: {{site.so}}/questions/49704497/how-to-make-flutter-app-responsive-according-to-different-screen-size
[`LayoutBuilder`]: {{site.api}}/flutter/widgets/LayoutBuilder-class.html
[반응형 랜딩 페이지를 지원하는 플러터 크로스 플랫폼 만들기]: {{site.medium}}/flutter-community/making-cross-platform-flutter-landing-page-responsive-7fffe0655970
[`maxWidth`]: {{site.api}}/flutter/rendering/BoxConstraints/maxWidth.html
[`MediaQuery`]: {{site.api}}/flutter/widgets/MediaQuery-class.html
[`MediaQuery.of()`]: {{site.api}}/flutter/widgets/MediaQuery/of.html
[`MediaQueryData`]: {{site.api}}/flutter/widgets/MediaQueryData-class.html
[`OrientationBuilder`]: {{site.api}}/flutter/widgets/OrientationBuilder-class.html
[`Scaffold`]: {{site.api}}/flutter/material/Scaffold-class.html

## 적응형 플러터 앱 만들기

gskinner 팀이 쓴 [적응형 앱 만들기][] 과 함께
적응형 플러터 앱을 만들며 더 배워보세요.

You might also check out the following episodes
of The Boring Show:
당신은 지루한 쇼의 다음 에피소드도 확인할 수 있습니다:

<iframe style="max-width: 100%" width="560" height="315" src="{{site.youtube-site}}/embed/n6Awpg1MO6M" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
[적응형 레이아웃][]

<iframe style="max-width: 100%" width="560" height="315" src="{{site.youtube-site}}/embed/eikOZzfc0l4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
[적응형 레이아웃, 파트 2][]

적응형 앱의 훌륭한 예제를 위해,
gskinner와 플러터 팀이 함께 만든 스크랩부킹 앱인 플러터 폴리오를 확인해보세요:

<iframe width="560" height="315" src="{{site.youtube-site}}/embed/yytBENOnF0w" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

[폴리오 소스 코드][] 는 또한 깃허브에서도 볼 수 있습니다.
[gskinner 블로그][] 에 방문해 더 많은 것을 배워보세요.

### 다른 리소스들

당신은 아래 리소스들에서 플랫폼 적응형 앱을 만드는 것에 대해 
더 많은 것을 배울 수 있습니다:

* [플랫폼별 행동양식과 적응][], 이 사이트의 페이지.
* [진심으로 적응형 사용자 인터페이스를 설계하는 법][] 이번 년도의 플러터바이킹 컨퍼런스에서 
  발표된 Aloïs Deniel가 쓴 블로그 포스트와 비디오.
* [플러터 갤러리 앱][] ([repo][]) 는 적응형 앱으로써 쓰여졌습니다.

[적응형 레이아웃]: {{site.youtube-site}}/watch?v=n6Awpg1MO6M&t=694s
[적응형 레이아웃, 파트 2]: {{site.youtube-site}}/watch?v=eikOZzfc0l4&t=11s
[적응형 앱 만들기]: {{site.url}}/development/ui/layout/building-adaptive-apps

[진심으로 적응형 사용자 인터페이스를 설계하는 법]: https://aloisdeniel.com/#/posts/adaptative-ui
[플러터 갤러리 앱]]: {{site.gallery}}
[폴리오 소스 코드]: {{site.github}}/gskinnerTeam/flutter-folio
[gskinner 블로그]: https://blog.gskinner.com/
[플랫폼별 행동양식과 적응]: {{site.url}}/resources/platform-adaptations
[repo]: {{site.repo.gallery}}
