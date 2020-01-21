---
title: Flutter 레이아웃
short-title: 레이아웃
description: Flutter 레이아웃의 메커니즘과 레이아웃을 만드는 방법에 대해 배워봅시다.
diff2html: true
---

{% assign api = site.api | append: '/flutter' -%}
{% capture code -%} {{site.repo.this}}/tree/{{site.branch}}/src/_includes/code {%- endcapture -%}
{% capture examples -%} {{site.repo.this}}/tree/{{site.branch}}/examples {%- endcapture -%}
{% assign rawExFile = 'https://raw.githubusercontent.com/flutter/website/master/examples' -%}
{% capture demo -%} {{site.repo.flutter}}/tree/{{site.branch}}/examples/flutter_gallery/lib/demo {%- endcapture -%}

<style>dl, dd { margin-bottom: 0; }</style>

{{site.alert.secondary}}
  <h4 class="no_toc">이 글의 핵심은?</h4>

  * 위젯은 UI를 만들기 위해 사용하는 class입니다.
  * 위젯은 레이아웃과 UI 요소 양쪽에 모두 사용됩니다.
  * 단순한 위젯들을 구성하여 복잡한 위젯들을 만드세요.
{{site.alert.end}}

Flutter 레이아웃 메커니즘의 핵심은 위젯입니다.
Flutter에서는 거의 모든 것이 위젯이고 심지어 레이아웃 모델도 위젯이죠.
Flutter 앱 내에서 볼 수 있는 이미지, 아이콘, 글자 모두 위젯입니다.
하지만 row, column, grid 같이 볼 수 없는 위젯들도 있는데요.
이들은 보이는 위젯들을 제어하고, 제한하며, 정렬시켜줍니다.

더 복잡한 위젯들을 만들기 위해 위젯들을 구성하여 레이아웃을 생성하세요.
예를 들어, 아래 첫 번째 스크린샷은 각각 아래에 레이블이 있는 아이콘 3개를 보여줍니다.

<div class="row mb-4">
  <div class="col-12 text-center">
    {% asset ui/layout/lakes-icons.png class="border mt-1 mb-1 mw-100" alt="Sample layout" %}
    {% asset ui/layout/lakes-icons-visual.png class="border mt-1 mb-1 mw-100" alt="Sample layout with visual debugging" %}
  </div>
</div>

두 번째 스크린샷은 시각적 레이아웃을 표시하는데,
각각 아이콘과 레이블이 포함된 열 3개가 있는 행을 보여줍니다.

{{site.alert.note}}
  이 튜토리얼 내 대부분의 스크린샷은 `debugPaintSizeEnabled`가
  true로 설정된 상태로 표시되므로 시각적 레이아웃을 볼 수 있습니다.
  더 자세한 정보는, [Using the Flutter inspector][] 안에 있는
  [Debugging layout issues visually][] 섹션을 봐주세요.
{{site.alert.end}}

다음은 해당 UI에 대한 위젯 트리 다이어그램입니다.

{% asset ui/layout/sample-flutter-layout.png class="mw-100" alt="Node tree" %}
{:.text-center}

대부분이 예상대로 보이지만, (분홍색으로 표시된)container가 뭔지 궁금하실겁니다.
[Container][]는 자식 위젯들을 커스터마이징할 수 있는 위젯 클래스인데요.
여백, 간격, 테두리 또는 배경색을 추가하고 싶을 때 `Container`를 사용할 수 있습니다.

이 예제에서, 각 [Text][] 위젯은 간격 추가를 위해 `Container` 안에 배치되었고,
마찬가지로 [Row][] 전체를 감싸는 여백을 추가하기 위해 `Container` 안에 배치되었습니다.

이 예제의 나머지 UI는 속성에 의해 제어됩니다.
[Icon][]의 색상은 `color` 속성을 이용해 설정했습니다.
폰트, 색상, 굵기 등을 설정하기 위해서는 `Text.style` 속성을 사용하세요.
Column과 Row에는 자식들을 세로 혹은 가로로 정렬하는 방법과
자식들이 차지해야 하는 공간을 지정할 수 있는 속성이 있습니다.

## 위젯 배치하기

Flutter에서 단일 위젯을 어떻게 배치하시나요?
이 섹션에서는 간단한 위젯을 작성하고 표시하는 방법을 보여줍니다.
또 간단한 Hello World 앱에 대한 전체 코드도 보여줍니다.

Flutter에서 화면에 텍스트, 아이콘, 이미지를 표시하기 위해서는 몇 단계만 거치면 됩니다.

### 1. 레이아웃 위젯 선택하기

보이는 위젯을 정렬하거나 제한하려는 방법에 따라 다양한 [레이아웃 위젯들][] 중에 선택하면,
일반적으로 포함되어 있는 위젯에 이런 특성이 전달됩니다. 

이 예제는 컨텐츠를 중앙에 맞추는 [Center][]를 사용합니다.

### 2. 보이는 위젯 만들기

예를 들어, [Text][] 위젯을 생성해보면:

<?code-excerpt "layout/base/lib/main.dart (text)" replace="/child: //g"?>
```dart
Text('Hello World'),
```

[Image][] 위젯을 생성해보면:

<?code-excerpt "layout/lakes/step5/lib/main.dart (Image-asset)" remove="/width|height/"?>
```dart
Image.asset(
  'images/lake.jpg',
  fit: BoxFit.cover,
),
```

[Icon][] 위젯을 생성해보면:

<?code-excerpt "layout/lakes/step5/lib/main.dart (Icon)"?>
```dart
Icon(
  Icons.star,
  color: Colors.red[500],
),
```

### 3. 레이아웃 위젯에 보이는 위젯 추가하기

<?code-excerpt path-base="layout/base"?>

모든 레이아웃 위젯들은 다음 중 하나를 가지는데:

- 단일 자식을 가지는 경우 `child` 속성 -- 예시로 `Center` 또는 `Container`
- 위젯 목록을 가지는 경우 `children` 속성 -- 예시로 `Row`, `Column`, `ListView`, `Stack`

`Center` 위젯에 `Text` 위젯을 추가하면:

<?code-excerpt "lib/main.dart (centered-text)" replace="/body: //g"?>
```dart
Center(
  child: Text('Hello World'),
),
```

### 4. 페이지에 레이아웃 위젯 추가하기

Flutter 앱은 그 자체로 위젯이고, 대부분의 위젯들은 [build()][] 메서드를 가지는데요.
앱의 `build()` 메서드에서 위젯을 인스턴스화하고 반환하면 해당 위젯이 표시됩니다.

#### Material 앱

`Material`의 경우, [Scaffold][] 위젯을 사용할 수 있는데요.
이 위젯은 기본 배너, 배경색을 제공하고 drawer, snack bar, bottom sheet를 추가하기 위한 API를 가집니다.
그러면 홈페이지의 `body` 속성에 직접 `Center` 위젯을 추가할 수 있습니다.

<?code-excerpt path-base="layout/base"?>
<?code-excerpt "lib/main.dart (MyApp)" title?>
```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter layout demo',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Flutter layout demo'),
        ),
        body: Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}
```

{{site.alert.note}}
  [Material 라이브러리][]는 [Material 디자인][]의 원칙을 따르는 위젯을 구현합니다.
  UI를 디자인할 때, 표준 [위젯 라이브러리][]에 있는 위젯을 단독으로 사용하거나,
  Material 라이브러리에 있는 위젯들을 사용할 수 있습니다.
  두 라이브러리에 있는 위젯을 혼합할 수 있고, 기존 위젯을 커스터마이징할 수 있으며,
  당신만의 커스텀 위젯 모음을 만들 수 있습니다.
{{site.alert.end}}

#### Non-Material 앱

Non-Material 앱의 경우 앱의 `build()` 메서드에 `Center` 위젯을 추가할 수 있는데:

<?code-excerpt path-base="layout/non_material"?>
<?code-excerpt "lib/main.dart (MyApp)" title?>
```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: BoxDecoration(color: Colors.white),
      child: Center(
        child: Text(
          'Hello World',
          textDirection: TextDirection.ltr,
          style: TextStyle(
            fontSize: 32,
            color: Colors.black87,
          ),
        ),
      ),
    );
  }
}
```

기본적으로 non-Material 앱은 `AppBar`, 제목, 배경색을 포함하지 않습니다.
이런 기능들을 non-Material 앱에서 원한다면 직접 구축해야 하는데요.
이 앱은 Material app을 흉내내기 위해 배경색은 흰색 그리고 글자는 짙은 회색으로 변경합니다.

<div class="row">
<div class="col-md-6" markdown="1">
  그게 전부입니다! 앱을 실행하면, _Hello World_를 보실 겁니다.

  앱 소스 코드:
  - [Material 앱]({{examples}}/layout/base)
  - [Non-Material 앱]({{examples}}/layout/non_material)
</div>
<div class="col-md-6">
  {% include app-figure.md img-class="site-mobile-screenshot border w-75"
      image="ui/layout/hello-world.png" alt="Hello World" %}
</div>
</div>

<hr>

## 가로와 세로로 여러 위젯 배치하기

<?code-excerpt path-base=""?>

가장 일반적인 레이아웃 패턴 중 하나는 위젯을 가로와 세로로 나열하는 것 입니다.
위젯을 가로로 나열하기 위해 Row, 그리고 세로로 나열하기 위해 Column을 사용할 수 있습니다.

{{site.alert.secondary}}
  <h4 class="no_toc">요점이 뭔가요?</h4>

  * Row와 Column은 가장 일반적으로 사용되는 두 가지 레이아웃 패턴입니다.
  * Row와 Column은 각각 자식 위젯 리스트를 가집니다.
  * 자식 위젯은 Row, Column, 혹은 그 외 다른 복잡한 위젯일 수 있습니다.
  * Row나 Column이 자식들을 가로나 세로로 정렬하는 방법을 지정할 수 있습니다.
  * 특정 자식 위젯을 늘리거나 제한할 수 있습니다.
  * 자식 위젯들이 Row나 Column의 사용 가능한 공간을 사용하는 방법에 대해 지정할 수 있습니다.
{{site.alert.end}}

Flutter에서 행이나 열을 만들기 위해, [Row][] 또는 [Column][] 위젯에 자식 위젯 리스트를 추가합니다.
결국, 각 자식은 행이나 열, 기타 등등이 될 수 있습니다.
다음 예제는 행이나 열 내부에 행이나 열을 배치할 수 있는 방법을 보여줍니다.

이 레이아웃은 행으로 구성되어 있습니다.
행은 두 자식들, 왼쪽의 열과 오른쪽의 이미지를 가지고 있습니다.

{% asset ui/layout/pavlova-diagram.png class="mw-100"
    alt="Screenshot with callouts showing the row containing two children" %}

왼쪽 열의 위젯 트리는 행과 열들을 중첩하고 있습니다.

{% asset ui/layout/pavlova-left-column-diagram.png class="mw-100"
    alt="Diagram showing a left column broken down to its sub-rows and sub-columns" %}

[Nesting rows and columns](#nesting-rows-and-columns)에 있는 Pavlova의 레이아웃 코드 중 일부를 구현할 겁니다.

{{site.alert.note}}
  행과 열은 가로와 세로 레이아웃을 위한 기본적인 원시 위젯입니다&mdash;
  이런 저레벨 위젯들은 최대한 사용자 정의를 수용합니다.
  Flutter는 또한 여러분의 필요에 충분한 전문화되고 더 높은 수준의 위젯들을 제공합니다.
  예를 들어, Row 대신에 leading과 trailing icon 속성을 제공하고,
  텍스트가 최대 3줄이여서, 사용하기 쉬운 위젯인
  [ListTile]({{api}}/material/ListTile-class.html)을 선호할 수 있습니다.  
  Column 대신에, 사용 가능한 공간에 맞추기 위해 내용이
  너무 길어지면 자동으로 스크롤하는 column-like 레이아웃인
  [ListView]({{api}}/widgets/ListView-class.html)를 선호할 수 있습니다.  
  더 많은 정보는 [Common layout widgets](#common-layout-widgets)을 봐주세요.
{{site.alert.end}}

### Aligning widgets

You control how a row or column aligns its children using the
`mainAxisAlignment` and `crossAxisAlignment` properties.
For a row, the main axis runs horizontally and the cross axis runs
vertically. For a column, the main axis runs vertically and the cross
axis runs horizontally.

<div class="mb-2 text-center">
  {% asset ui/layout/row-diagram.png class="mb-2 mw-100"
      alt="Diagram showing the main axis and cross axis for a row" %}
  {% asset ui/layout/column-diagram.png class="mb-2 mr-2 ml-2 mw-100"
      alt="Diagram showing the main axis and cross axis for a column" %}
</div>

The [MainAxisAlignment]({{api}}/rendering/MainAxisAlignment-class.html)
and [CrossAxisAlignment]({{api}}/rendering/CrossAxisAlignment-class.html)
classes offer a variety of constants for controlling alignment.

{{site.alert.note}}
  When you add images to your project,
  you need to update the `pubspec.yaml` file to access them&mdash;this
  example uses `Image.asset` to display the images.  For more information,
  see this example's [pubspec.yaml
  file]({{examples}}/layout/row_column/pubspec.yaml),
  or [Adding Assets and Images in Flutter](/docs/development/ui/assets-and-images).
  You don't need to do this if you're referencing online images using
  `Image.network`.
{{site.alert.end}}

In the following example, each of the 3 images is 100 pixels wide.
The render box (in this case, the entire screen) is more than 300 pixels wide,
so setting the main axis alignment to `spaceEvenly` divides the free
horizontal space evenly between, before, and after each image.

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/row_column/lib/main.dart (Row)" replace="/Row/[!$&!]/g"?>
  {% prettify dart context="html" %}
  [!Row!](
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      Image.asset('images/pic1.jpg'),
      Image.asset('images/pic2.jpg'),
      Image.asset('images/pic3.jpg'),
    ],
  );
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  {% asset ui/layout/row-spaceevenly-visual.png class="mw-100" alt="Row with 3 evenly spaced images" %}

  **App source:** [row_column]({{examples}}/layout/row_column)
</div>
</div>

Columns work the same way as rows. The following example shows a column
of 3 images, each is 100 pixels high. The height of the render box
(in this case, the entire screen) is more than 300 pixels, so
setting the main axis alignment to `spaceEvenly` divides the free vertical
space evenly between, above, and below each image.

<div class="row">
<div class="col-lg-8" markdown="1">
  <?code-excerpt "layout/row_column/lib/main.dart (Column)" replace="/Column/[!$&!]/g"?>
  {% prettify dart context="html" %}
  [!Column!](
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      Image.asset('images/pic1.jpg'),
      Image.asset('images/pic2.jpg'),
      Image.asset('images/pic3.jpg'),
    ],
  );
  {% endprettify %}

  **App source:** [row_column]({{examples}}/layout/row_column)
</div>
<div class="col-lg-4 text-center">
  {% asset ui/layout/column-visual.png class="mb-4" height="250px"
      alt="Column showing 3 images spaced evenly" %}
</div>
</div>

### Sizing widgets

When a layout is too large to fit a device, a yellow and black striped pattern
appears along the affected edge. Here is an [example][sizing] of a row that is
too wide:

{% asset ui/layout/layout-too-large.png class="mw-100" alt="Overly-wide row" %}
{:.text-center}

Widgets can be sized to fit within a row or column by using the [Expanded][]
widget. To fix the previous example where the row of images is too wide for its
render box, wrap each image with an `Expanded` widget.

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/sizing/lib/main.dart (expanded-images)" replace="/Expanded/[!$&!]/g"?>
  {% prettify dart context="html" %}
  Row(
    crossAxisAlignment: CrossAxisAlignment.center,
    children: [
      [!Expanded!](
        child: Image.asset('images/pic1.jpg'),
      ),
      [!Expanded!](
        child: Image.asset('images/pic2.jpg'),
      ),
      [!Expanded!](
        child: Image.asset('images/pic3.jpg'),
      ),
    ],
  );
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  {% asset ui/layout/row-expanded-2-visual.png class="mw-100"
      alt="Row of 3 images that are too wide, but each is constrained to take only 1/3 of the space" %}

  **App source:** [sizing]({{examples}}/layout/sizing)
</div>
</div>

Perhaps you want a widget to occupy twice as much space as its siblings. For
this, use the `Expanded` widget `flex` property, an integer that determines the
flex factor for a widget. The default flex factor is 1. The following code sets
the flex factor of the middle image to 2:

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/sizing/lib/main.dart (expanded-images-with-flex)" replace="/flex.*/[!$&!]/g"?>
  {% prettify dart context="html" %}
  Row(
    crossAxisAlignment: CrossAxisAlignment.center,
    children: [
      Expanded(
        child: Image.asset('images/pic1.jpg'),
      ),
      Expanded(
        [!flex: 2,!]
        child: Image.asset('images/pic2.jpg'),
      ),
      Expanded(
        child: Image.asset('images/pic3.jpg'),
      ),
    ],
  );
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  {% asset ui/layout/row-expanded-visual.png class="mw-100"
      alt="Row of 3 images with the middle image twice as wide as the others" %}

  **App source:** [sizing]({{examples}}/layout/sizing)
</div>
</div>

[sizing]: {{examples}}/layout/sizing

### Packing widgets

By default, a row or column occupies as much space along its main axis
as possible, but if you want to pack the children closely together,
set its `mainAxisSize` to `MainAxisSize.min`. The following example
uses this property to pack the star icons together.

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/pavlova/lib/main.dart (stars)" replace="/mainAxisSize.*/[!$&!]/g; /\w+ \w+ = //g; /;//g"?>
  {% prettify dart context="html" %}
  Row(
    [!mainAxisSize: MainAxisSize.min,!]
    children: [
      Icon(Icons.star, color: Colors.green[500]),
      Icon(Icons.star, color: Colors.green[500]),
      Icon(Icons.star, color: Colors.green[500]),
      Icon(Icons.star, color: Colors.black),
      Icon(Icons.star, color: Colors.black),
    ],
  )
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  {% asset ui/layout/packed.png class="border mw-100"
      alt="Row of 5 stars, packed together in the middle of the row" %}

  **App source:** [pavlova]({{examples}}/layout/pavlova)
</div>
</div>

### Nesting rows and columns

The layout framework allows you to nest rows and columns inside of rows
and columns as deeply as you need. Let's look at the code for the outlined
section of the following layout:

{% asset ui/layout/pavlova-large-annotated.png class="border mw-100"
    alt="Screenshot of the pavlova app, with the ratings and icon rows outlined in red" %}
{:.text-center}

The outlined section is implemented as two rows. The ratings row contains
five stars and the number of reviews. The icons row contains three
columns of icons and text.

The widget tree for the ratings row:

{% asset ui/layout/widget-tree-pavlova-rating-row.png class="mw-100" alt="Ratings row widget tree" %}
{:.text-center}

The `ratings` variable creates a row containing a smaller row of 5 star icons,
and text:

<?code-excerpt "layout/pavlova/lib/main.dart (ratings)" replace="/ratings/[!$&!]/g"?>
```dart
var stars = Row(
  mainAxisSize: MainAxisSize.min,
  children: [
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.black),
    Icon(Icons.star, color: Colors.black),
  ],
);

final [!ratings!] = Container(
  padding: EdgeInsets.all(20),
  child: Row(
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      stars,
      Text(
        '170 Reviews',
        style: TextStyle(
          color: Colors.black,
          fontWeight: FontWeight.w800,
          fontFamily: 'Roboto',
          letterSpacing: 0.5,
          fontSize: 20,
        ),
      ),
    ],
  ),
);
```

{{site.alert.tip}}
  To minimize the visual confusion that can result from heavily nested layout
  code, implement pieces of the UI in variables and functions.
{{site.alert.end}}

The icons row, below the ratings row, contains 3 columns; each column contains
an icon and two lines of text, as you can see in its widget tree:

{% asset ui/layout/widget-tree-pavlova-icon-row.png class="mw-100" alt="Icon widget tree" %}
{:.text-center}

The `iconList` variable defines the icons row:

<?code-excerpt "layout/pavlova/lib/main.dart (iconList)" replace="/iconList/[!$&!]/g"?>
```dart
final descTextStyle = TextStyle(
  color: Colors.black,
  fontWeight: FontWeight.w800,
  fontFamily: 'Roboto',
  letterSpacing: 0.5,
  fontSize: 18,
  height: 2,
);

// DefaultTextStyle.merge() allows you to create a default text
// style that is inherited by its child and all subsequent children.
final [!iconList!] = DefaultTextStyle.merge(
  style: descTextStyle,
  child: Container(
    padding: EdgeInsets.all(20),
    child: Row(
      mainAxisAlignment: MainAxisAlignment.spaceEvenly,
      children: [
        Column(
          children: [
            Icon(Icons.kitchen, color: Colors.green[500]),
            Text('PREP:'),
            Text('25 min'),
          ],
        ),
        Column(
          children: [
            Icon(Icons.timer, color: Colors.green[500]),
            Text('COOK:'),
            Text('1 hr'),
          ],
        ),
        Column(
          children: [
            Icon(Icons.restaurant, color: Colors.green[500]),
            Text('FEEDS:'),
            Text('4-6'),
          ],
        ),
      ],
    ),
  ),
);
```

The `leftColumn` variable contains the ratings and icons rows, as well as the
title and text that describes the Pavlova:

<?code-excerpt "layout/pavlova/lib/main.dart (leftColumn)" replace="/leftColumn/[!$&!]/g"?>
```dart
final [!leftColumn!] = Container(
  padding: EdgeInsets.fromLTRB(20, 30, 20, 20),
  child: Column(
    children: [
      titleText,
      subTitle,
      ratings,
      iconList,
    ],
  ),
);
```

The left column is placed in a `Container` to constrain its width.
Finally, the UI is constructed with the entire row (containing the
left column and the image) inside a `Card`.

The [Pavlova image][] is from [Pixabay][].
You can embed an image from the net using `Image.network()` but,
for this example, the image is saved to an images directory in the project,
added to the [pubspec file,]({{examples}}/layout/pavlova/pubspec.yaml)
and accessed using `Images.asset()`. For more information, see
[Adding assets and images](/docs/development/ui/assets-and-images).

<?code-excerpt "layout/pavlova/lib/main.dart (body)"?>
```dart
body: Center(
  child: Container(
    margin: EdgeInsets.fromLTRB(0, 40, 0, 30),
    height: 600,
    child: Card(
      child: Row(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Container(
            width: 440,
            child: leftColumn,
          ),
          mainImage,
        ],
      ),
    ),
  ),
),
```

{{site.alert.tip}}
  The Pavlova example runs best horizontally on a wide device, such as a tablet.
  If you are running this example in the iOS simulator, you can select a
  different device using the **Hardware > Device** menu. For this example, we
  recommend the iPad Pro. You can change its orientation to landscape mode using
  **Hardware > Rotate**. You can also change the size of the simulator window
  (without changing the number of logical pixels) using **Window > Scale**.
{{site.alert.end}}

**App source:** [pavlova]({{examples}}/layout/pavlova)

[Pavlova image]: https://pixabay.com/en/photos/pavlova
[Pixabay]: https://pixabay.com/en/photos/pavlova

<hr>

## Common layout widgets

Flutter has a rich library of layout widgets. Here are a few of those most
commonly used. The intent is to get you up and running as quickly as possible,
rather than overwhelm you with a complete list.  For information on other
available widgets, refer to the [Widget catalog][],
or use the Search box in the [API reference docs]({{api}}).
Also, the widget pages in the API docs often make suggestions
about similar widgets that might better suit your needs.

The following widgets fall into two categories: standard widgets from the
[widgets library][], and specialized widgets from the [Material library][]. Any
app can use the widgets library but only Material apps can use the Material
Components library.

### Standard widgets

* [Container](#container): Adds padding, margins, borders, background color, or
  other decorations to a widget.
* [GridView](#gridview): Lays widgets out as a scrollable grid.
* [ListView](#listview): Lays widgets out as a scrollable list.
* [Stack](#stack): Overlaps a widget on top of another.

### Material widgets

* [Card](#card): Organizes related info into a box with rounded corners and a
  drop shadow.
* [ListTile](#listtile): Organizes up to 3 lines of text, and optional leading
  and trailing icons, into a row.

### Container

Many layouts make liberal use of [Container][]s to separate widgets using
padding, or to add borders or margins. You can change the device's background
by placing the entire layout into a `Container` and changing its background
color or image.

<div class="row">
<div class="col-lg-6" markdown="1">
  <h4 class="no_toc">Summary (Container)</h4>

  * Add padding, margins, borders
  * Change background color or image
  * Contains a single child widget, but that child can be a Row, Column,
    or even the root of a widget tree
</div>
<div class="col-lg-6 text-center">
  {% asset ui/layout/margin-padding-border.png class="mb-4 mw-100"
      width="230px"
      alt="Diagram showing: margin, border, padding, and content" %}
</div>
</div>

#### Examples (Container)
{:.no_toc}

This layout consists of a column with two rows, each containing 2 images. A
[Container][] is used to change the background color of the column to a lighter
grey.

<div class="row">
<div class="col-lg-7">
  <?code-excerpt "layout/container/lib/main.dart (column)" replace="/\bContainer/[!$&!]/g;"?>
  {% prettify dart context="html" %}
  Widget _buildImageColumn() => [!Container!](
        decoration: BoxDecoration(
          color: Colors.black26,
        ),
        child: Column(
          children: [
            _buildImageRow(1),
            _buildImageRow(3),
          ],
        ),
      );
  {% endprettify %}
</div>
<div class="col-lg-5 text-center">
  {% asset ui/layout/container.png class="mb-4 mw-100" width="230px"
      alt="Screenshot showing 2 rows, each containing 2 images" %}
</div>
</div>

A `Container` is also used to add a rounded border and margins to each image:

<?code-excerpt "layout/container/lib/main.dart (row)" replace="/\bContainer/[!$&!]/g;"?>
```dart
Widget _buildDecoratedImage(int imageIndex) => Expanded(
      child: [!Container!](
        decoration: BoxDecoration(
          border: Border.all(width: 10, color: Colors.black38),
          borderRadius: const BorderRadius.all(const Radius.circular(8)),
        ),
        margin: const EdgeInsets.all(4),
        child: Image.asset('images/pic$imageIndex.jpg'),
      ),
    );

Widget _buildImageRow(int imageIndex) => Row(
      children: [
        _buildDecoratedImage(imageIndex),
        _buildDecoratedImage(imageIndex + 1),
      ],
    );
```

You can find more `Container` examples in the [tutorial][] and the [Flutter
Gallery][].

**App source:** [container]({{examples}}/layout/container)

<hr>

### GridView

Use [GridView][] to lay widgets out as a two-dimensional list. `GridView`
provides two pre-fabricated lists, or you can build your own custom grid. When a
`GridView` detects that its contents are too long to fit the render box, it
automatically scrolls.

#### Summary (GridView)
{:.no_toc}

* Lays widgets out in a grid
* Detects when the column content exceeds the render box and automatically
  provides scrolling
* Build your own custom grid, or use one of the provided grids:
  * `GridView.count` allows you to specify the number of columns
  * `GridView.extent` allows you to specify the maximum pixel width of a tile
{% comment %}
* Use `MediaQuery.of(context).orientation` to create a grid that changes
  its layout depending on whether the device is in landscape or portrait mode.
{% endcomment %}

{{site.alert.note}}
  When displaying a two-dimensional list where it's important which
  row and column a cell occupies (for example,
  it's the entry in the "calorie" column for the "avocado" row), use
  [Table]({{api}}/widgets/Table-class.html) or
  [DataTable]({{api}}/material/DataTable-class.html).
{{site.alert.end}}

#### Examples (GridView)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/gridview-extent.png class="mw-100" alt="A 3-column grid of photos" %}
  {:.text-center}

  Uses `GridView.extent` to create a grid with tiles a maximum 150 pixels wide.

  **App source:** [grid_and_list]({{examples}}/layout/grid_and_list)
</div>
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/gridview-count-flutter-gallery.png class="mw-100"
      alt="A 2 column grid with footers" %}
  {:.text-center}

  Uses `GridView.count` to create a grid that's 2 tiles wide in portrait mode,
  and 3 tiles wide in landscape mode. The titles are created by setting the
  `footer` property for each [GridTile][].

  **Dart code:** [grid_list_demo.dart]({{demo}}/material/grid_list_demo.dart)
  from the [Flutter Gallery][]
</div>
</div>

<?code-excerpt "layout/grid_and_list/lib/main.dart (grid)" replace="/\GridView/[!$&!]/g;"?>
```dart
Widget _buildGrid() => [!GridView!].extent(
    maxCrossAxisExtent: 150,
    padding: const EdgeInsets.all(4),
    mainAxisSpacing: 4,
    crossAxisSpacing: 4,
    children: _buildGridTileList(30));

// The images are saved with names pic0.jpg, pic1.jpg...pic29.jpg.
// The List.generate() constructor allows an easy way to create
// a list when objects have a predictable naming pattern.
List<Container> _buildGridTileList(int count) => List.generate(
    count, (i) => Container(child: Image.asset('images/pic$i.jpg')));
```

<hr>

### ListView

[ListView]({{api}}/widgets/ListView-class.html),
a column-like widget, automatically provides scrolling when
its content is too long for its render box.

#### Summary (ListView)
{:.no_toc}

* A specialized [Column][] for organizing a list of boxes
* Can be laid out horizontally or vertically
* Detects when its content won't fit and provides scrolling
* Less configurable than `Column`, but easier to use and supports scrolling

#### Examples (ListView)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/listview.png class="border mw-100"
      alt="ListView containing movie theaters and restaurants" %}
  {:.text-center}

  Uses `ListView` to display a list of businesses using `ListTile`s. A `Divider`
  separates the theaters from the restaurants.

  **App source:** [grid_and_list]({{examples}}/layout/grid_and_list)
</div>
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/listview-flutter-gallery.png class="border mw-100"
      alt="ListView containing shades of blue" %}
  {:.text-center}

  Uses `ListView` to display the [Colors]({{api}}/material/Colors-class.html) from
  the [Material Design palette]({{site.material}}/guidelines/style/color.html)
  for a particular color family.

  **Dart code:** [colors_demo.dart]({{demo}}/colors_demo.dart) from the
  [Flutter Gallery][]
</div>
</div>

<?code-excerpt "layout/grid_and_list/lib/main.dart (list)" replace="/\ListView/[!$&!]/g;"?>
```dart
Widget _buildList() => [!ListView!](
      children: [
        _tile('CineArts at the Empire', '85 W Portal Ave', Icons.theaters),
        _tile('The Castro Theater', '429 Castro St', Icons.theaters),
        _tile('Alamo Drafthouse Cinema', '2550 Mission St', Icons.theaters),
        _tile('Roxie Theater', '3117 16th St', Icons.theaters),
        _tile('United Artists Stonestown Twin', '501 Buckingham Way',
            Icons.theaters),
        _tile('AMC Metreon 16', '135 4th St #3000', Icons.theaters),
        Divider(),
        _tile('K\'s Kitchen', '757 Monterey Blvd', Icons.restaurant),
        _tile('Emmy\'s Restaurant', '1923 Ocean Ave', Icons.restaurant),
        _tile(
            'Chaiya Thai Restaurant', '272 Claremont Blvd', Icons.restaurant),
        _tile('La Ciccia', '291 30th St', Icons.restaurant),
      ],
    );

ListTile _tile(String title, String subtitle, IconData icon) => ListTile(
      title: Text(title,
          style: TextStyle(
            fontWeight: FontWeight.w500,
            fontSize: 20,
          )),
      subtitle: Text(subtitle),
      leading: Icon(
        icon,
        color: Colors.blue[500],
      ),
    );
```

<hr>

### Stack

Use [Stack][] to arrange widgets on top of a base widget&mdash;often an image.
The widgets can completely or partially overlap the base widget.

#### Summary (Stack)
{:.no_toc}

* Use for widgets that overlap another widget
* The first widget in the list of children is the base widget;
  subsequent children are overlaid on top of that base widget
* A `Stack`'s content can't scroll
* You can choose to clip children that exceed the render box

#### Examples (Stack)
{:.no_toc}

<div class="row">
<div class="col-lg-7" markdown="1">
  {% asset ui/layout/stack.png class="mw-100" width="200px" alt="Circular avatar image with a label" %}
  {:.text-center}

  Uses `Stack` to overlay a `Container` (that displays its `Text` on a translucent
  black background) on top of a `CircleAvatar`.
  The `Stack` offsets the text using the `alignment` property and
  `Alignment`s.

  **App source:** [card_and_stack]({{examples}}/layout/card_and_stack)
</div>
<div class="col-lg-5" markdown="1">
  {% asset ui/layout/stack-flutter-gallery.png class="mw-100" alt="An image with a grey gradient across the top" %}
  {:.text-center}

  Uses `Stack` to overlay a gradient to the top of the image. The gradient
  ensures that the toolbar's icons are distinct against the image.

  **Dart code:** [contacts_demo.dart]({{demo}}/contacts_demo.dart)
  from the [Flutter Gallery][]
</div>
</div>

<?code-excerpt "layout/card_and_stack/lib/main.dart (Stack)" replace="/\bStack/[!$&!]/g;"?>
```dart
Widget _buildStack() => [!Stack!](
    alignment: const Alignment(0.6, 0.6),
    children: [
      CircleAvatar(
        backgroundImage: AssetImage('images/pic.jpg'),
        radius: 100,
      ),
      Container(
        decoration: BoxDecoration(
          color: Colors.black45,
        ),
        child: Text(
          'Mia B',
          style: TextStyle(
            fontSize: 20,
            fontWeight: FontWeight.bold,
            color: Colors.white,
          ),
        ),
      ),
    ],
  );
```

<hr>

### Card

A [Card][], from the [Material library][], contains related nuggets of
information and can be composed from almost any widget, but is often used with
[ListTile][]. `Card` has a single child, but its child can be a column, row,
list, grid, or other widget that supports multiple children. By default, a
`Card` shrinks its size to 0 by 0 pixels. You can use [SizedBox][] to constrain
the size of a card.

In Flutter, a `Card` features slightly rounded corners and a drop shadow, giving
it a 3D effect. Changing a `Card`'s `elevation` property allows you to control
the drop shadow effect. Setting the elevation to 24, for example, visually lifts
the `Card` further from the surface and causes the shadow to become more
dispersed. For a list of supported elevation values, see [Elevation][] in the
[Material guidelines][Material Design]. Specifying an unsupported value disables
the drop shadow entirely.

#### Summary (Card)
{:.no_toc}

* Implements a [Material card][]
* Used for presenting related nuggets of information
* Accepts a single child, but that child can be a `Row`, `Column`, or other
  widget that holds a list of children
* Displayed with rounded corners and a drop shadow
* A `Card`'s content can't scroll
* From the [Material library][]

#### Examples (Card)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/card.png class="mw-100" alt="Card containing 3 ListTiles" %}
  {:.text-center}

  A `Card` containing 3 ListTiles and sized by wrapping it with a `SizedBox`. A
  `Divider` separates the first and second `ListTiles`.

  **App source:** [card_and_stack]({{examples}}/layout/card_and_stack)
</div>
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/card-flutter-gallery.png class="mw-100"
      alt="Card containing an image, text and buttons" %}
  {:.text-center}

  A `Card` containing an image and text.

  **Dart code:** [cards_demo.dart]({{demo}}/material/cards_demo.dart)
  from the [Flutter Gallery][]
</div>
</div>

<?code-excerpt "layout/card_and_stack/lib/main.dart (Card)" replace="/\bCard/[!$&!]/g;"?>
```dart
Widget _buildCard() => SizedBox(
    height: 210,
    child: [!Card!](
      child: Column(
        children: [
          ListTile(
            title: Text('1625 Main Street',
                style: TextStyle(fontWeight: FontWeight.w500)),
            subtitle: Text('My City, CA 99984'),
            leading: Icon(
              Icons.restaurant_menu,
              color: Colors.blue[500],
            ),
          ),
          Divider(),
          ListTile(
            title: Text('(408) 555-1212',
                style: TextStyle(fontWeight: FontWeight.w500)),
            leading: Icon(
              Icons.contact_phone,
              color: Colors.blue[500],
            ),
          ),
          ListTile(
            title: Text('costa@example.com'),
            leading: Icon(
              Icons.contact_mail,
              color: Colors.blue[500],
            ),
          ),
        ],
      ),
    ),
  );
```
<hr>

### ListTile

Use [ListTile][], a specialized row widget from the [Material library][], for an
easy way to create a row containing up to 3 lines of text and optional leading
and trailing icons. `ListTile` is most commonly used in [Card][] or
[ListView][], but can be used elsewhere.

#### Summary (ListTile)
{:.no_toc}

* A specialized row that contains up to 3 lines of text and optional icons
* Less configurable than `Row`, but easier to use
* From the [Material library][]

#### Examples (ListTile)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/card.png class="mw-100" alt="Card containing 3 ListTiles" %}
  {:.text-center}

  A `Card` containing 3 `ListTiles`.

  **App source:** [card_and_stack]({{examples}}/layout/card_and_stack)
</div>
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/listtile-flutter-gallery.png class="border mw-100" height="200px"
      alt="3 ListTiles, each containing a pull-down button" %}
  {:.text-center}

  Uses `ListTile` to list 3 drop down button types.<br>
  **Dart code:** [buttons_demo.dart]({{demo}}/material/buttons_demo.dart)
  from the [Flutter Gallery][]
</div>
</div>

<hr>

## Videos

다음 비디오들은 [Flutter in Focus](https://www.youtube.com/watch?v=wgTBLj7rMPM&list=PLjxrf2q8roU2HdJQDjJzOeO6J3FoFLWr2)
시리즈의 일부로, Stateless 및 Stateful 위젯에 대해서 설명합니다.

<iframe width="560" height="315" src="https://www.youtube.com/embed/wE7khGHVkYY?rel=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> <iframe width="560" height="315" src="https://www.youtube.com/embed/AqCMFXEmf3w?rel=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
[Flutter in Focus 재생 목록](https://www.youtube.com/watch?v=wgTBLj7rMPM&list=PLjxrf2q8roU2HdJQDjJzOeO6J3FoFLWr2)

---

[Widget of the Week 시리즈](https://www.youtube.com/playlist?list=PLjxrf2q8roU23XGwz3Km7sQZFTdB996iG)의
각 에피소드는 위젯에 중점을 둡니다.
그 중에서 몇 가지는 레이아웃 위젯을 포함합니다.

<iframe width="560" height="315" src="https://www.youtube.com/embed/b_sQ9bMltGU?rel=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
[Flutter Widget of the Week playlist](https://www.youtube.com/watch?v=yI-8QHpGIP4&index=5&list=PLjxrf2q8roU23XGwz3Km7sQZFTdB996iG)

## Other resources

The following resources may help when writing layout code.

* [Layout tutorial](/docs/development/ui/layout/tutorial)
: 레이아웃 만드는 방법을 배워보세요.
* [Widget Overview](/docs/development/ui/widgets)
: Flutter에서 사용 가능한 여러 위젯들을 설명합니다.
* [HTML/CSS Analogs in Flutter](/docs/get-started/flutter-for/web-devs)
: 웹 프로그래밍에 익숙한 사용자들을 위해, 이 페이지에서는 HTML/CSS 기능을 Flutter 기능에 매핑합니다.
* [Flutter Gallery][]
: 수많은 Material Design 위젯들과 그 외의 Flutter 기능들을 선보이는 데모입니다.
* [Flutter API documentation]({{api}})
: 모든 Flutter 라이브러리에 대한 참고 문서입니다.
* [Dealing with Box Constraints in Flutter](/docs/development/ui/layout/box-constraints)
: 위젯이 어떻게 렌더링 상자에 의해 제약받는지 설명합니다.
* [Adding Assets and Images in Flutter](/docs/development/ui/assets-and-images)
: 앱 패키지에 이미지와 에셋들을 추가하는 방법에 대해 설명합니다.
* [Zero to One with Flutter]({{site.medium}}/@mravn/zero-to-one-with-flutter-43b13fd7b354)
: 처음 Flutter 앱을 작성한 사람의 경험담입니다.

[build()]: {{api}}/widgets/StatelessWidget/build.html
[Card]: {{api}}/material/Card-class.html
[Center]: {{api}}/widgets/Center-class.html
[Column]: {{api}}/widgets/Column-class.html
[Container]: {{api}}/widgets/Container-class.html
[Elevation]: {{site.material}}/design/environment/elevation.html
[Expanded]: {{api}}/widgets/Expanded-class.html
[Flutter Gallery]: {{site.repo.flutter}}/tree/master/examples/flutter_gallery
[GridView]: {{api}}/widgets/GridView-class.html
[GridTile]: {{api}}/material/GridTile-class.html
[Icon]: {{api}}/material/Icons-class.html
[Image]: {{api}}/widgets/Image-class.html
[layout widgets]: /docs/development/ui/widgets/layout
[ListTile]: {{api}}/material/ListTile-class.html
[ListView]: {{api}}/widgets/ListView-class.html
[Material card]: {{site.material}}/design/components/cards.html
[Material Design]: {{site.material}}/design
[Material library]: {{api}}/material/material-library.html
[Row]: {{api}}/widgets/Row-class.html
[Scaffold]: {{api}}/material/Scaffold-class.html
[SizedBox]: {{api}}/widgets/SizedBox-class.html
[Stack]: {{api}}/widgets/Stack-class.html
[Text]: {{api}}/widgets/Text-class.html
[tutorial]: /docs/development/ui/layout/tutorial
[widgets library]: {{api}}/widgets/widgets-library.html
[Widget catalog]: /docs/development/ui/widgets
[Debugging layout issues visually]: /docs/development/tools/devtools/inspector#debugging-layout-issues-visually
[Using the Flutter inspector]: /docs/development/tools/devtools/inspector
