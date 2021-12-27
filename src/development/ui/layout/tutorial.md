---
title: Building layouts
short-title: Tutorial
description: 레이아웃 쌓는 법을 배웁니다.
diff2html: true
---

{% assign api = '{{site.api}}/flutter' -%}
{% capture examples -%} {{site.repo.this}}/tree/{{site.branch}}/examples {%- endcapture -%}
{% assign rawExFile = 'https://raw.githubusercontent.com/flutter/website/master/examples' -%}
{% capture demo -%} {{site.repo.flutter}}/tree/{{site.branch}}/examples/flutter_gallery/lib/demo {%- endcapture -%}

<style>dl, dd { margin-bottom: 0; }</style>

{{site.alert.secondary}}
  <h4 class="no_toc">배우게 될 것</h4>

  * 플러터의 레이아웃 메카니즘이 작동하는 법을 알아봅니다.
  * 위젯들을 수평과 수직으로 배치시키는 법을 알아봅니다. 
  * 플러터 레이아웃을 배치시키는 법을 알아봅니다. 
{{site.alert.end}}

이것은 플러터로 레이아웃을 배치하기 위한 가이드입니다.
당신은 아래 보여지는 앱처럼 레이아웃을 배치시킬 것입니다.

{% include docs/app-figure.md img-class="site-mobile-screenshot border"
    image="ui/layout/lakes.jpg" caption="The finished app" %}

이 가이드는 한발짝 물러나 플러터의 레이아웃 접근법에 대해 설명하고, 
스크린에 단일 위젯을 위치시키는 법을 보여줍니다.
위젯을 수평으로 또는 수직으로 배치시키는지에 대한 논의 후, 가장 일반적인 레이아웃 위젯 일부가 포함됩니다.

If you want a "big picture" understanding of the layout mechanism,
start with 
만약 큰 그림, 즉 레이아웃 메커니즘에 대한 이해가 필요하다면, [플러터의 레이아웃 접근법][] 에서 시작할 수 있습니다.

## 단계 0: 앱의 기반 코드를 작성합니다.

[설치][]을 확실히 완료한 다음, 따라하세요:

 1. [기본적인 "Hello World" 플러터 앱 만들기][hello-world].
 2. 앱 바 타이틀과 앱 타이틀을 아래처럼 변경하세요:

    <?code-excerpt "layout/base/lib/{main_starter,main}.dart"?>
    ```diff
    --- layout/base/lib/main_starter.dart
    +++ layout/base/lib/main.dart
    @@ -12,10 +12,10 @@
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
    -      title: 'Welcome to Flutter',
    +      title: 'Flutter layout demo',
           home: Scaffold(
             appBar: AppBar(
    -          title: const Text('Welcome to Flutter'),
    +          title: const Text('Flutter layout demo'),
             ),
             body: const Center(
               child: Text('Hello World'),
    ```

## Step 1: 레이아웃 다이어그램

레이아웃을 정복하기 위한 첫번째 걸음은 레이아웃의 기본적인 요소들입니다:

* 행과 열을 파악합니다.
* 레이아웃이 그리드를 포함하나요?
* 요소들이 겹쳐져 있나요?
* UI에서 탭들이 필요한가요?
* 영역들에 정렬, 패딩 혹은 경계선들이 필요한지 확인하세요.

처음으로, 제일 큰 요소를 파악합니다. 예를 들면,
네 개의 요소들은 하나의 행 안에서 배열됩니다: 그림, 두 개의 행, 그리고 한 문장.

{% include docs/app-figure.md img-class="site-mobile-screenshot border"
    image="ui/layout/lakes-column-elts.png" caption="Column elements (circled in red)" %}

그 다음, 각 행을 그리세요. 제목 부분이라고 불리는 첫번째 행은 세 개의 자식을 가지고 있습니다:
텍스트의 한 컬럼, 별 모양 아이콘, 그리고 숫자.
첫번째 자식인 컬럼은 텍스트 두 줄을 포함합니다.
첫번째 컬럼은 많은 공간을 차지하기 때문에, 확장된 위젯으로 감싸져야 합니다.

{% include docs/app-figure.md image="ui/layout/title-section-parts.png" alt="Title section" %}

버튼 영역이라고 불리는 두 번째 행은, 세 개의 자식을 가지고 있습니다:
각 자식은 아이콘과 텍스트를 포함한 컬럼입니다.
{% include docs/app-figure.md image="ui/layout/button-section-diagram.png" alt="Button section" %}

한번 레이아웃이 그려지면, 레이아웃을 구현하기 위한 상향식 접근은 가장 쉬운 방법입니다.
깊게 중첩된 레이아웃 코드의 시각적 혼란을 최소화하기 위해서,
변수들과 함수들의 구현 일부에 위치시킵니다.


## Step 2: 제목 행을 구현합니다.

<?code-excerpt path-base="layout/lakes/step2"?>

먼저, 제목 영역 안의 왼쪽 컬럼부터 배치시킬 것입니다.
아래의 코드를 `MyApp` 클래스의 `build()` 함수의 위쪽에 추가하세요:

<?code-excerpt "lib/main.dart (titleSection)" title?>
```dart
Widget titleSection = Container(
  padding: const EdgeInsets.all(32),
  child: Row(
    children: [
      Expanded(
        /*1*/
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            /*2*/
            Container(
              padding: const EdgeInsets.only(bottom: 8),
              child: const Text(
                'Oeschinen Lake Campground',
                style: TextStyle(
                  fontWeight: FontWeight.bold,
                ),
              ),
            ),
            Text(
              'Kandersteg, Switzerland',
              style: TextStyle(
                color: Colors.grey[500],
              ),
            ),
          ],
        ),
      ),
      /*3*/
      Icon(
        Icons.star,
        color: Colors.red[500],
      ),
      const Text('41'),
    ],
  ),
);
```

{:.numbered-code-notes}
 1. 해당 행에 여유 공간을 남겨두는 것을 모두 쓰기 위해 늘어나는 `확장되는` 위젯의 내부에
    `컬럼`을 위치시킵니다.
    `crossAxisAlignment` 속성을 `CrossAxisAlignment.start` 로 
    해당 행의 시작 컬럼에 위치시킵니다.
 2. `Container` 내부의 텍스트의 첫번째 행을 위치시켜서 패딩을 추가할 수 있도록 합니다.
    `Column`의 두번째 자식 또한, 텍스트이며 회색으로 보여집니다.
 3. 제목 행의 마지막 두 아이템은 붉게 칠해진 별모양 아이콘과 "41"이라는 텍스트입니다.
    전체 행은 `Container` 내부와 각 가장자리를 따라 32픽셀씩 패딩처리됩니다.
    제목 영역을 앱 바디에 다음과 같이 추가하세요:

<?code-excerpt path-base="layout/lakes"?>
<?code-excerpt "{../base,step2}/lib/main.dart" from="return MaterialApp"?>
```diff
--- ../base/lib/main.dart
+++ step2/lib/main.dart
@@ -14,11 +48,13 @@
     return MaterialApp(
       title: 'Flutter layout demo',
       home: Scaffold(
         appBar: AppBar(
           title: const Text('Flutter layout demo'),
         ),
-        body: const Center(
-          child: Text('Hello World'),
+        body: Column(
+          children: [
+            titleSection,
+          ],
         ),
       ),
     );
```

{{site.alert.tip}}
  * 앱에 코드를 붙여넣을 때, 들여쓰기가 변형될 수 있습니다. 
    [자동 리포맷팅 지원][] 을 사용하여 당신의 플러터 에디터로 
    이것을 고칠 수 있습니다.
  * 빠른 개발 경험을 위해, 플러터의 [핫 리로드][] 기능을 사용해보세요.
  * 만약 문제가 있다면, 당신의 코드를 [`lib/main.dart`][] 과 비교해보세요.
{{site.alert.end}}

## Step 3: 버튼 행을 구현합니다.

<?code-excerpt path-base="layout/lakes/step3"?>

버튼 영역은 동일한 레이아웃(텍스트 행 위의 아이콘)에서 
사용되는 세 개의 컬럼을 포함합니다.
이 행 내부에 있는 컬럼들은 균일하게 띄워져 있으며, 
텍스트와 아이콘들은 기본 색으로 칠해져 있습니다.

각 칼럼을 구현한 코드가 거의 동일하기 때문에,
`buildButtonColumn()` 라는 이름의 프라이빗 헬퍼 메소드를 만듭니다.
`Icon` 과 `Text` 의 색을 지정하고, 
주어진 색으로 칠해진 해당 위젯으로 이루어진 컬럼을 반환합니다.

<?code-excerpt "lib/main.dart (_buildButtonColumn)" title?>
```dart
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // ···
  }

  Column _buildButtonColumn(Color color, IconData icon, String label) {
    return Column(
      mainAxisSize: MainAxisSize.min,
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        Icon(icon, color: color),
        Container(
          margin: const EdgeInsets.only(top: 8),
          child: Text(
            label,
            style: TextStyle(
              fontSize: 12,
              fontWeight: FontWeight.w400,
              color: color,
            ),
          ),
        ),
      ],
    );
  }
}
```

이 함수는 아이콘을 직접적으로 컬럼에 추가합니다.
`Container` 내부의 위쪽만 여백이 있는 텍스트가 있으며,
해당 텍스트는 아이콘으로부터 분리되어 있습니다.

함수를 호출하고 해당 열에 해당하는 색상, `Icon`과 텍스트를 전달하는 
이러한 컬럼이 포함된 행을 작성합니다. 컬럼들을 `MainAxisAlignment.spaceEvenly`을 사용하여 남는 공간을 균일하게 각 컬럼의 앞, 사이, 중간에 배열하고 주축에 따라 정렬합니다.
`titleSection` 정의 내부의 `build()` 메소드 
바로 아래의 주어진 코드를 추가합니다:

<?code-excerpt "lib/main.dart (buttonSection)" title?>
```dart
Color color = Theme.of(context).primaryColor;

Widget buttonSection = Row(
  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
  children: [
    _buildButtonColumn(color, Icons.call, 'CALL'),
    _buildButtonColumn(color, Icons.near_me, 'ROUTE'),
    _buildButtonColumn(color, Icons.share, 'SHARE'),
  ],
);
```

버튼 영역을 바디에 추가합니다:

<?code-excerpt path-base="layout/lakes"?>
<?code-excerpt "{step2,step3}/lib/main.dart" from="return MaterialApp" to="}"?>
```diff
--- step2/lib/main.dart
+++ step3/lib/main.dart
@@ -48,3 +59,3 @@
     return MaterialApp(
       title: 'Flutter layout demo',
       home: Scaffold(
@@ -54,8 +65,9 @@
         body: Column(
           children: [
             titleSection,
+            buttonSection,
           ],
         ),
       ),
     );
   }
```

## Step 4: 텍스트 영역을 구현합니다.

<?code-excerpt path-base="layout/lakes/step4"?>

텍스트 영역을 변수로써 정의합니다. 
텍스트를 `Container` 안에 넣고, 각 가장자리에 따라 패딩을 추가합니다.
`buttonSection` 정의 바로 아래 주어진 코드를 추가합니다:

<?code-excerpt "lib/main.dart (textSection)" title?>
```dart
Widget textSection = const Padding(
  padding: EdgeInsets.all(32),
  child: Text(
    'Lake Oeschinen lies at the foot of the Blüemlisalp in the Bernese '
    'Alps. Situated 1,578 meters above sea level, it is one of the '
    'larger Alpine Lakes. A gondola ride from Kandersteg, followed by a '
    'half-hour walk through pastures and pine forest, leads you to the '
    'lake, which warms to 20 degrees Celsius in the summer. Activities '
    'enjoyed here include rowing, and riding the summer toboggan run.',
    softWrap: true,
  ),
);
```

`softwrap` 을 true 로 세팅함으로써, 텍스트 줄들은 단어 영역을 감싸기 전에 컬럼 너비로 채워질 것입니다.

텍스트 영역을 바디에 추가합니다:

<?code-excerpt path-base="layout/lakes"?>
<?code-excerpt "{step3,step4}/lib/main.dart" from="return MaterialApp"?>
```diff
--- step3/lib/main.dart
+++ step4/lib/main.dart
@@ -59,3 +72,3 @@
     return MaterialApp(
       title: 'Flutter layout demo',
       home: Scaffold(
@@ -66,6 +79,7 @@
           children: [
             titleSection,
             buttonSection,
+            textSection,
           ],
         ),
       ),
```

## Step 5: 이미지 영역을 구현합니다.

4개의 컬럼 요소들 중 3개는 현재 완성되었고, 이미지만을 남겨두고 있습니다.
예제에 이미지 파일을 추가합니다: 

* `images` 폴더를 프로젝트의 최상단에 생성합니다.
* [`lake.jpg`][] 를 추가합니다.

{{site.alert.info}}
  `wget` 은 저장되는 해당 바이너리 파일에서는 동작하지 않는 것을 주의하세요.
  원본 이미지는 [사용가능한 온라인][] 로 크리에이티브 커먼스 라이선스에 속하지만,
  용량이 크고 가지고 오기 느립니다.
{{site.alert.end}}

* `assets` 태그를 포함하기 위해 `pubspec.yaml` 파일을 업데이트 합니다.
  이것은 당신의 코드에 이미지가 보여질 수 있도록 만듭니다.

  <?code-excerpt "{step4,step5}/pubspec.yaml"?>
  ```diff
  --- step4/pubspec.yaml
  +++ step5/pubspec.yaml
  @@ -18,3 +18,5 @@

   flutter:
     uses-material-design: true
  +  assets:
  +    - images/lake.jpg
  ```
{{site.alert.tip}}
  * `pubspec.yaml` 은 대소문자를 구분한다는 것을 주의하면서,
    `assets:` 을 쓰고 이미지 URL은 위와 같이 보여집니다.
  * 이 pubspec 파일은 또한 여백에 민감하여, 적절한 들여쓰기를 사용하세요.
  * pubspec 파일의 변경사항을 적용하려면 당신은 아마도 
    실행되고 있는 프로그램을 재시작해야할 것입니다.
    (시뮬레이터에서 혹은 연결된 디바이스에서) 
{{site.alert.end}}

이제 당신은 당신의 코드로부터 이미지를 참조할 수 있습니다:

<?code-excerpt "{step4,step5}/lib/main.dart"?>
```diff
--- step4/lib/main.dart
+++ step5/lib/main.dart
@@ -77,6 +77,12 @@
         ),
         body: Column(
           children: [
+            Image.asset(
+              'images/lake.jpg',
+              width: 600,
+              height: 240,
+              fit: BoxFit.cover,
+            ),
             titleSection,
             buttonSection,
             textSection,
```

`BoxFit.cover` 은 이미지가 되도록 작아야 하지만, 해당 렌더 박스 전체를 차지하도록
해야한다는 것을 프레임워크에게 말해줍니다.

## Step 6: 마무리

In this final step, arrange all of the elements in a
`ListView`, rather than a `Column`, because a
`ListView` supports app body scrolling when the app is run
on a small device.
이 마지막 단계에서, `Column` 보다는 `ListView` 안에 있는 모든 요소들을 정렬합니다. 
왜냐하면 `ListView` 는 앱이 작은 디바이스에서 실행될 때 앱바디의 스크롤링을 지원하기 때문입니다.

<?code-excerpt "{step5,step6}/lib/main.dart" diff-u="6" from="return MaterialApp"?>
```diff
--- step5/lib/main.dart
+++ step6/lib/main.dart
@@ -72,13 +77,13 @@
     return MaterialApp(
       title: 'Flutter layout demo',
       home: Scaffold(
         appBar: AppBar(
           title: const Text('Flutter layout demo'),
         ),
-        body: Column(
+        body: ListView(
           children: [
             Image.asset(
               'images/lake.jpg',
               width: 600,
               height: 240,
               fit: BoxFit.cover,
```

**Dart code:** [`main.dart`][]<br>
**Image:** [images][]<br>
**Pubspec:** [`pubspec.yaml`][]<br>

이겁니다! 만약 앱에 핫 리로드를 할 때, 
당신은 해당 페이지의 위쪽에 스크린샷으로써 
같은 앱의 레이아웃을 볼 것입니다.

당신은 이 레이아웃에 [당신의 앱에 상호작용성을 추가하기][] 을 따라함으로써 상호작용성을 추가할 수 있습니다.


[당신의 앱에 상호작용성을 추가하기]: {{site.url}}/development/ui/interactive
[자동 리포맷팅 지원]: {{site.url}}/development/tools/formatting
[사용가능한 온라인]: https://images.unsplash.com/photo-1471115853179-bb1d604434e0?dpr=1&amp;auto=format&amp;fit=crop&amp;w=767&amp;h=583&amp;q=80&amp;cs=tinysrgb&amp;crop=
[플러터의 레이아웃 접근법]: {{site.url}}/development/ui/layout
[hello-world]: {{site.url}}/get-started/codelab#step-1-create-the-starter-flutter-app
[images]: {{examples}}/layout/lakes/step6/images
[`lake.jpg`]: {{rawExFile}}/layout/lakes/step5/images/lake.jpg
[`lib/main.dart`]: {{examples}}/layout/lakes/step2/lib/main.dart
[핫 리로드]: {{site.url}}/development/tools/hot-reload
[`main.dart`]: {{examples}}/layout/lakes/step6/lib/main.dart
[`pubspec.yaml`]: {{examples}}/layout/lakes/step6/pubspec.yaml
[설치]: {{site.url}}/get-started/install
