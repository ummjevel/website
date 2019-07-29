---
title: 화면을 넘나드는 위젯 애니메이션
prev:
  title: 서비스에 에러 보고 하기
  path: /docs/cookbook/maintenance/error-reporting
next:
  title: 새로운 화면으로 이동하고, 되돌아오기
  path: /docs/cookbook/navigation/navigation-basics
---

한 화면에서 다른 화면으로 넘어갈 때, 사용자에게 어떠한 가이드를 주는 것은 종종 도움이 됩니다.
앱에서 사용할 수 있는 일반적인 기술은 다음 화면으로 전환할 때 위젯에 애니메이션 효과를 주는 것입니다.
이러한 방법은 두 화면을 이어주는 시각적 연결 고리를 만들어 줍니다.

한 화면에서 다음 화면으로 전환할 떄 애니메이션 효과를 주기 위해 
[`Hero`]({{site.api}}/flutter/widgets/Hero-class.html) 위젯을 사용하세요.
여기서는 아래와 같은 단계로 진행합니다:

  1. 같은 이미지를 보여주는 2개의 화면을 만듭니다.
  2. 첫 번째 화면에 `Hero` 위젯을 추가합니다.
  3. 두 번째 화면에 `Hero` 위젯을 추가합니다.

## 1. 같은 이미지를 보여주는 2개의 화면을 만듭니다.

이 예제에서는 같은 이미지를 두 개의 화면 모두에 보여줄 것입니다. 
첫 번째 화면에서 사용자가 이미지를 탭하면 두 번째 화면으로 전환되면서 애니메이션이 발생합니다.
일단 시각적 구조를 만들고, 본격적인 
애니메이션은 다음 단계에서 다루겠습니다.

{{site.alert.note}}
  이 예제는 
  [새로운 화면으로 이동하고, 되돌아오기](/docs/cookbook/navigation/navigation-basics)
  와 [탭 다루기](/docs/cookbook/gestures/handling-taps) 예제를 기반으로 합니다.
{{site.alert.end}}


```dart
class MainScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Main Screen'),
      ),
      body: GestureDetector(
        onTap: () {
          Navigator.push(context, MaterialPageRoute(builder: (_) {
            return DetailScreen();
          }));
        },
        child: Image.network(
          'https://picsum.photos/250?image=9',
        ),
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(
        onTap: () {
          Navigator.pop(context);
        },
        child: Center(
          child: Image.network(
            'https://picsum.photos/250?image=9',
          ),
        ),
      ),
    );
  }
}
```

## 2. 첫 번째 화면에 `Hero` 위젯을 추가합니다.

두 화면을 하나의 애니메이션으로 연결하기 위해,
각 화면에 존재하는 `Image`위젯을 `Hero`위젯으로 감쌉니다. 
`Hero` 위젯에 2개의 인자를 넘겨주어야 합니다:

<dl>
  <dt>`tag`</dt>
  <dd>`Hero`위젯을 식별하기 위한 객체로 
      양쪽 모두 동일한 값을 가져야 합니다.</dd>
  <dt>`child`</dt>
  <dd>화면 전환 시 애니메이션 효과를 적용할 위젯입니다.</dd>
</dl>

<!-- skip -->
```dart
Hero(
  tag: 'imageHero',
  child: Image.network(
    'https://picsum.photos/250?image=9',
  ),
);
```

## 3. 두 번째 화면에 `Hero` 위젯을 추가합니다.

첫 번째 화면과의 연결하기 위해, 
두 번째 화면의 `Image`도 첫 번째 화면에 사용한 것과 동일한 `tag`를 사용한 
`Hero` 위젯으로 감싸주어야 합니다. 

두 번째 화면에 `Hero` 위젯을 적용하기만 하면, 
화면 사이의 애니메이션이 동작합니다.

<!-- skip -->
```dart
Hero(
  tag: 'imageHero',
  child: Image.network(
    'https://picsum.photos/250?image=9',
  ),
);
```

{{site.alert.note}}
  이 코드는 첫 번째 화면에 사용했던 것과 동일한 코드입니다.
  코드를 반복하기보다는 재사용 가능한 위젯을 만들어 사용하는 게 모범 사례입니다.
  이 예제에서는 단순화하기 위해 동일 코드를 양쪽 위젯에서 사용합니다.
{{site.alert.end}}
  
## 완성된 예제

```dart
import 'package:flutter/material.dart';

void main() => runApp(HeroApp());

class HeroApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Transition Demo',
      home: MainScreen(),
    );
  }
}

class MainScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Main Screen'),
      ),
      body: GestureDetector(
        child: Hero(
          tag: 'imageHero',
          child: Image.network(
            'https://picsum.photos/250?image=9',
          ),
        ),
        onTap: () {
          Navigator.push(context, MaterialPageRoute(builder: (_) {
            return DetailScreen();
          }));
        },
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(
        child: Center(
          child: Hero(
            tag: 'imageHero',
            child: Image.network(
              'https://picsum.photos/250?image=9',
            ),
          ),
        ),
        onTap: () {
          Navigator.pop(context);
        },
      ),
    );
  }
}
```

![Hero demo](/images/cookbook/hero.gif){:.site-mobile-screenshot}
