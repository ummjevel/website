---
title: 선언형 UI 소개
short-title: 선언형 UI
description: 선언형과 명령형 프로그래밍 방식의 차이를 설명합니다.
---

_이번 소개에서는 Flutter에서 사용하는 선언형 방식과 다른 많은 UI 프레임워크에서 사용하는
명령형 방식의 개념적인 차이를 설명합니다._

## 왜 선언형 UI 인가?

Win32 프레임워크부터 웹을 거쳐 Android, iOS 까지 일반적으로 명령형 방식의 UI 프로그래밍을
사용했습니다. UIView 등과 같이 완전한 기능의 UI 개체를 일일이 구성하고, 나중에 UI가 바뀌면
메소드와 setter들을 이용해 변경하는, 매우 익숙한 방식입니다.

대조적으로 플러터는, 다양한 UI 상태들 사이의 전이를 어떻게 프로그래밍할 것인지에 대한 개발자들의
부담을 덜기위해, 개발자들이 현재의 UI 상태만 묘사하고 변화는 프레임워크에 맡기도록 했습니다.

그렇기에 UI를 어떻게 다루는지에 대한 약간의 사고 전환을 필요로 합니다.

## 선언형 프레임워크를 이용한 UI를 바꾸기

아래의 간단한 예제를 살펴보면:

<img src="/images/declarativeUIchanges.png" alt="View B (contained by view A) morphs from containing two views, c1 and c2, to containing only view c3">

명령형 방식에서는, ViewB의 소유자로 가서, selector 혹은 `findViewById`같은 것들로
인스턴스 `b`를 찾고, 변화를 적용합니다. (그리고 암묵적으로 뷰가 변경되었음을 표시합니다.)
예를 들면:

```java
// Imperative style
b.setColor(red)
b.clearChildren()
ViewC c3 = new ViewC(...)
b.add(c3)
```

또한, 인스턴스 `b` 자신 보다 오래 지속되는 `b`의 원본 데이터를 ViewB의 생성자내에 복제해야
하는 문제가 있습니다.

선언형 방식에서는, (Flutter의 위젯 같은)뷰 설정은 불변이며, 간단한 "청사진"일 뿐입니다.
UI를 바꾸기 위해, 위젯이 자신의 재빌드를 유발하고(대부분의 경우 Flutter StatefulWidgets
의 `setState()`를 호출) 새로운 위젯 하위 트리를 구성합니다.

<!-- skip -->
```dart
// Declarative style
return ViewB(
  color: red,
  child: ViewC(...),
)
```

여기서 UI가 변경될 때 기존 인스턴스 `b`를 변경하는 대신, Flutter가 새로운 위젯을 구성합니다.
프레임워크는 RenderObjects를 사용하여 (레이아웃의 상태를 유지하는)기존 UI 개체의 많은 책임
를 관리합니다. RenderObjects는 지속적으로 프레임을 유지하고, Flutter의 경량 위젯은
RenderObjects 상태를 변경 시키도록 프레임워크에 지시합니다.
나머지는 Flutter 프레임워크가 처리합니다.
