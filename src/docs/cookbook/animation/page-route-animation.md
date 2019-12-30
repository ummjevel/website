---
title: 페이지 route 전환 애니메이션
next:
  title: 물리 시뮬레이션을 사용한 위젯 애니메이션
  path: /docs/cookbook/animation/physics-simulation
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

A design language, such as Material, defines standard behaviors when
transitioning between routes (or screens). Sometimes, though, a custom
transition between screens can make an app more unique. To help,
[`PageRouteBuilder`][] provides an [`Animation`] object.
This `Animation` can be used with [`Tween`][] and
[`Curve`][] objects to customize the transition animation.
This recipe shows how to transition between
routes by animating the new route into view from
the bottom of the screen.

To create a custom page route transition, this recipe uses the following steps:

1. Set up a PageRouteBuilder
2. Create a `Tween`
3. Add an `AnimatedWidget`
4. Use a `CurveTween`
5. Combine the two `Tween`s

## 1. Set up a PageRouteBuilder

To start, use a [`PageRouteBuilder`][] to create a [`Route`][].
`PageRouteBuilder` has two callbacks, one to build the content of the route
(`pageBuilder`), and one to build the route's transition (`transitionsBuilder`).

{{site.alert.note}}
  pageBuilder에서 만들어진 위젯이 transitionsBuilder의 `child` 매개 변수로 전달됩니다. 
  `pageBuilder` 함수는 route가 처음 빌드 될 때만 호출됩니다. 
  전환 과정에서 `child`가 동일하게 유지될 때는 
  프레임워크가 추가 작업을 하지 않을 수 있습니다.
{{site.alert.end}}

다음 예제는 두 개의 route를 만듭니다. 
"Go!" 버튼이 있는 홈 route와 제목이 "Page 2"인 두 번째 route입니다.

```dart
import 'package:flutter/material.dart';

main() {
  runApp(MaterialApp(
    home: Page1(),
  ));
}

class Page1 extends StatelessWidget {
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: RaisedButton(
          child: Text('Go!'),
          onPressed: () {
            Navigator.of(context).push(_createRoute());
          },
        ),
      ),
    );
  }
}

Route _createRoute() {
  return PageRouteBuilder(
    pageBuilder: (context, animation, secondaryAnimation) => Page2(),
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      return child;
    },
  );
}

class Page2 extends StatelessWidget {
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: Text('Page 2'),
      ),
    );
  }
}
```

## 2. Tween 생성

To make the new page animate in from the bottom, it should animate from
`Offset(0,1)` to `Offset(0, 0)` (usually defined using the `Offset.zero`
constructor). In this case, the Offset is a 2D vector for the
['FractionalTranslation'][] widget.
Setting the `dy` argument to 1 represents a vertical translation one
full height of the page.

`transitionsBuilder` 콜백에는 `animation` 매개 변수가 있습니다.
이 매개 변수는 0에서 1 사이의 값을 생성하는 `Animation<double>`입니다. 
Tween을 사용하여 Animation<double>을 Animation<Offset>으로 변환합니다: 

```dart
transitionsBuilder: (context, animation, secondaryAnimation, child) {
  var begin = Offset(0.0, 1.0);
  var end = Offset.zero;
  var tween = Tween(begin: begin, end: end);
  var offsetAnimation = animation.drive(tween);
  return child;
},
```

## 3. AnimatedWidget 사용

Flutter has a set of widgets extending [`AnimatedWidget`][]
that rebuild themselves when the value of the animation changes. For instance,
SlideTransition takes an `Animation<Offset>` and translates its child (using a
FractionalTranslation widget) whenever the value of the animation changes.

AnimatedWidget Return a [`SlideTransition`][]
with the `Animation<Offset>` and the child widget:

```dart
transitionsBuilder: (context, animation, secondaryAnimation, child) {
  var begin = Offset(0.0, 1.0);
  var end = Offset.zero;
  var tween = Tween(begin: begin, end: end);
  var offsetAnimation = animation.drive(tween);

  return SlideTransition(
    position: offsetAnimation,
    child: child,
  );
},
```

## 4. CurveTween 사용

Flutter provides a selection of easing curves that
adjust the rate of the animation over time.
The [`Curves`][] class
provides a predefined set of commonly used curves.
For example, `Curves.easeOut`
makes the animation start quickly and end slowly.

To use a Curve, create a new [`CurveTween`][]
and pass it a Curve:

```dart
var curve = Curves.ease;
var curveTween = CurveTween(curve: curve);
```

이 새로운 트윈은 여전히 0에서 1 사이의 값을 생성합니다. 
다음 단계에서는 2단계에서 만들었던 `Tween<Offset>`이 결합됩니다.

## 5. 두 개의 Tween 조합

To combine the tweens,
use [`chain()`][]:

```dart
var begin = Offset(0.0, 1.0);
var end = Offset.zero;
var curve = Curves.ease;

var tween = Tween(begin: begin, end: end).chain(CurveTween(curve: curve));
```

그런 다음 트윈을 `animation.drive()`에 전달하여 사용하세요.
이렇게 하면 `SlideTransition` 위젯에 전달할 수 있는 새로운 `Animation<Offset>`이 생성됩니다:

```dart
return SlideTransition(
  position: animation.drive(tween),
  child: child,
);
```

이 새로운 Tween(혹은 Animatable)은 
먼저 `CurveTween`을 계산하여 `Offset`을 값을 생성한 다음, `Tween<Offset>`을 계산합니다. 
애니메이션이 실행될 때, 값은 다음과 같은 순서로 계산됩니다.

1. (transitionsBuilder 콜백에서 제공되는) animation은 
   0에서 1 사이의 값을 생성합니다.
2. CurveTween은 해당 값을 curve를 기준으로 0과 1 사이의 
   새로운 값으로 매핑합니다.
3. `Tween<Offset>`은 `double` 값을 `Offset` 값에 매핑합니다.

easing curve로 `Animation<Offset>`을 만드는 또 다른 방법은  
`CurvedAnimation`을 사용하는 것입니다:

```dart
transitionsBuilder: (context, animation, secondaryAnimation, child) {
  var begin = Offset(0.0, 1.0);
  var end = Offset.zero;
  var curve = Curves.ease;

  var tween = Tween(begin: begin, end: end);
  var curvedAnimation = CurvedAnimation(
   parent: animation,
   curve: curve,
  );

  return SlideTransition(
   position: tween.animate(curvedAnimation),
   child: child,
  );
}
```

## Interactive example

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60
import 'package:flutter/material.dart';

main() {
  runApp(MaterialApp(
    home: Page1(),
  ));
}

class Page1 extends StatelessWidget {
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: RaisedButton(
          child: Text('Go!'),
          onPressed: () {
            Navigator.of(context).push(_createRoute());
          },
        ),
      ),
    );
  }
}

Route _createRoute() {
  return PageRouteBuilder(
    pageBuilder: (context, animation, secondaryAnimation) => Page2(),
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      var begin = Offset(0.0, 1.0);
      var end = Offset.zero;
      var curve = Curves.ease;

      var tween = Tween(begin: begin, end: end).chain(CurveTween(curve: curve));

      return SlideTransition(
        position: animation.drive(tween),
        child: child,
      );
    },
  );
}

class Page2 extends StatelessWidget {
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: Text('Page 2'),
      ),
    );
  }
}
```
<noscript>
  <img src="/images/cookbook/page-route-animation.gif" alt="Demo showing a custom page route transition animating up from the bottom of the screen" class="site-mobile-screenshot" />
</noscript>


[`AnimatedWidget`]: {{site.api}}/flutter/widgets/AnimatedWidget-class.html
[`Animation`]: {{site.api}}/flutter/animation/Animation-class.html
[`chain()`]: {{site.api}}/flutter/animation/Animatable/chain.html
[`Curve`]: {{site.api}}/flutter/animation/Curve-class.html
[`Curves`]: {{site.api}}/flutter/animation/Curves-class.html
[`CurveTween`]: {{site.api}}/flutter/animation/CurveTween-class.html
['FractionalTranslation']: {{site.api}}/flutter/widgets/FractionalTranslation-class.html
[`PageRouteBuilder`]: {{site.api}}/flutter/widgets/PageRouteBuilder-class.html
[`Route`]: {{site.api}}/flutter/widgets/Route-class.html
[`SlideTransition`]: {{site.api}}/flutter/widgets/SlideTransition-class.html
[`Tween`]: {{site.api}}/flutter/animation/Tween-class.html
