---
title: 박스 제약 조건 처리
description: 어떻게 박스 제약조건이 잘못될 수 있는지.
short-title: 박스 제약조건
---

{{site.alert.note}}
  만약 프레임워크가 박스 제약조건을 포함한 문제를 감지했을 때 
  당신은 이 페이지로 연결되었을 것입니다.
{{site.alert.end}}

{{site.alert.note}}
  만약 당신이 플러터에서 제약조건들, 사이징, 그리고 포지셔닝에 대해
  혼란스러울 때 [제약조건 이해하기][] 을 찾아보세요.
{{site.alert.end}}

플러터에서, 위젯들은 그들의 [`RenderBox`][] 오브젝트 기반으로 나타납니다.
렌더 박스들은 그들의 부모로부터 제약조건을 상속받고, 그들 스스로 
그 제약조건에 따라 사이즈를 조정합니다. 제약조건들은 최소 그리고 최대의 
폭과 높이로 구성되고, 사이즈는 특정한 폭과 높이로 구성됩니다.

일반적으로, 세 종류의 박스가 있으며, 
어떻게 그들이 그들의 제약조건을 다루는지에 관한 것입니다.

* 가능한 큰 크기를 가지도록 합니다.
  예를 들어, 박스들은 [`Center`][] 와 [`ListView`][] 에 의해 사용됩니다.
* 그들의 자식들과 같은 사이즈를 가지도록 합니다.
  예를 들어, 박스들은 [`Transform`][] 과 [`Opacity`][] 에서 사용됩니다. 
* 특정한 사이즈를 가지도록 합니다.
  예를 들어, 박스들은 [`Image`][] 와 [`Text`][] 에서 사용됩니다.

몇몇의 위젯은, 예를 들어 [`Container`][] 는 생성자 전달인자에 따라 종류마다 다릅니다.
[`Container`][]의 경우, 기본적으로 가능한 크지만 
예를 들어 `폭` 을 제공하면, 그 특정 사이즈에 따라 조정됩니다.

그 외에, 예를 들어 [`Row`][] 와 [`Column`][] (유연한 박스들) 은
"Flex" 영역 내에 설명되어 있는 대로 그들에게 주어진 제약조건을 기반으로 달라집니다.

제약조건들은 가끔 "엄격" 합니다. 
이것은 렌더 박스가 사이즈를 결정하는데에 있어서 여유가 없다는 뜻입니다.
(예를 들어, 만약 최소와 최대 폭이 같다면, 그것은 엄격한 폭을 가지고 있다고 할 수 있습니다.)
[`RenderView`][] 에 의해 포함되어 있는 `App` 위젯의 예로, 
박스는 애플리케이션의 콘텐트 영역(일반적으로, 전체 스크린)을  정확히 채우도록 강제하는 제약조건이 주어진
어플리케이션의 [`build`][] 함수에 의해 반환되어진 자식에 의해 쓰여집니다.
플러터의 많은 박스들은, 특히 하나의 자식을 가지고 있는 박스는, 그들의 제약조건을 그들의 자식에게 전달합니다.
이것은 만약 당신이 서로 내부의 많은 상자를 중첩한다면, 
애플리케이션의 렌더 트리 루트에서 엄격한 제약조건으로 인해 모두 정확히 맞을 것입니다. 

일부 박스들은 제약조건을 _완화_ 합니다.
이것은 최대가 유지되지만 최소가 제거됨을 의미합니다.
예를 들어, [`Center`][] 가 있습니다.

## 무제한 제약

]특정한 상황에서, 박스에 주어진 제약조건은 _무제한_, 혹은 무한합니다.
이것은 최대 폭 혹은 최대 높이가 [`double.infinity`][] 로 세팅됨을 의미합니다.

가능한 크게 되려고 하는 박스는 무제한 제약사항이 주어진 상황에서나,
이 파일을 가리키는 예외를 던지는 통합과 같은 디버그 모드에서 유용한 함수가 될 수 없습니다.

렌더 박스가 무제한 조건에서 자기자신을 찾을 수 있는 가장 일반적인 경우는 
유연한 박스([`Row`][] 와 [`Column`][]) 내, 
그리고 **스크롤 할 수 있는 영역** (`ListView`][] 와 다른 [`ScrollView`][] 의 서브클래스들) 입니다.

특히, [`ListView`][] 는 크로스-디렉션(예를 들어, 만약 세로 스크롤 블록이 있고, 
그것은 그것의 부모만큼 넓어지려고 할 때) 공간을 가능한 알맞게 맞추려 확장하려고 노력합니다. 
만약 당신이 가로 스크롤이 가능한 `ListView` 내부의 세로 스크롤이 가능한 [`ListView`][] 
를 중첩시키려할 때, 밖의 것이 그 방향으로 스크롤이 가능하기 때문에
안에 있는 것은 가능한 무한한 폭으로 넓어지려 할 것입니다.

## 유연한

유연한 박스는 그들 스스로 ([`Row`][] 와 [`Column`][])
그들의 주어진 명령에 내에서 한정된 제약사항 혹은 무제한 제약사항에 
속해 있는지를 기반으로 하여 다르게 행동합니다.

한정된 제약사항에서,
그들은 그 명령 내에서 가능한 크려고 합니다.

무제한 제약사항에서는, 그들은 그 명령 내에서 그들의 자식과 맞추려 합니다.
이런 경우에 당신은 자식들에 0 외에 다른 `flex` 를 세팅할 수 없습니다.
위젯 라이브러리에서, 이것은 다른 유연한 박스 내부 혹은 스크롤 가능한 내부에 있는 
유연한 박스에서 [`Expanded`][] 를 사용할 수 없음을 의미합니다.

이런 _크로스_ 디렉션에서, 예를 들면, 
[`Column`][] (세로방향에서 유연한) 을 위한 폭 안 혹은
[`Row`][] (가로방향에서 유연한) 을 위한 높이 안에서,
그들은 반드시 무제한 될 수 없으며, 그렇지 않으면 그들의 자식들을 적절히 정렬시킬 수 없습니다.


[`build`]: {{site.api}}/flutter/widgets/State/build.html
[`Center`]: {{site.api}}/flutter/widgets/Center-class.html
[`Column`]: {{site.api}}/flutter/widgets/Column-class.html
[`Container`]: {{site.api}}/flutter/widgets/Container-class.html
[`Expanded`]: {{site.api}}/flutter/widgets/Expanded-class.html
[`Image`]: {{site.api}}/flutter/dart-ui/Image-class.html
[`ListView`]: {{site.api}}/flutter/widgets/ListView-class.html
[`Opacity`]: {{site.api}}/flutter/widgets/Opacity-class.html
[`RenderBox`]: {{site.api}}/flutter/rendering/RenderBox-class.html
[`RenderView`]: {{site.api}}/flutter/rendering/RenderView-class.html
[`Row`]: {{site.api}}/flutter/widgets/Row-class.html
[`ScrollView`]: {{site.api}}/flutter/widgets/ScrollView-class.html
[`Text`]: {{site.api}}/flutter/widgets/Text-class.html
[`Transform`]: {{site.api}}/flutter/widgets/Transform-class.html
[제약조건 이해하기]: {{site.url}}/development/ui/layout/constraints
[`double.infinity`]: {{site.api}}/flutter/dart-core/double/infinity-constant.html
