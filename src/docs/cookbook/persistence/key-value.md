---
title: 디스크에 키-값 데이터 저장하기
prev:
  title: File 읽고 쓰기
  path: /docs/cookbook/persistence/reading-writing-files
next:
  title: Video 다루기
  path: /docs/cookbook/plugins/play-video
---

If you have a relatively small collection of key-values
to save, you can use the [shared_preferences][] plugin.

Normally,
you would have to write native platform integrations for storing
data on both iOS and Android. Fortunately,
the [shared_preferences][] plugin can be used to persist
key-value data on disk. The shared preferences plugin
wraps `NSUserDefaults` on iOS and `SharedPreferences` on Android,
providing a persistent store for simple data.

여기서는 아래와 같은 단계로 진행합니다:

  1. 의존성 추가하기.
  2. 데이터 저장하기.
  3. 데이터 읽기.
  4. 데이터 삭제하기.

## 1. 의존성 추가하기

Before starting, add the [shared_preferences][]
plugin to the `pubspec.yaml` file:

```yaml
dependencies:
  flutter:
    sdk: flutter
  shared_preferences: "<newest version>"
```

## 2. 데이터 저장하기

`SharedPreferences` 클래스가 제공하는 setter 메서드를 사용하여 데이터를 저장합니다.
Setter 메서드는 `setInt`, `setBool`, `setString`와 같이 다양한 원시 타입을 지원합니다.

Setter 메서드는 두 가지 작업을 수행합니다: 먼저, 메모리 상에서 키-값 쌍을 동기적으로
업데이트합니다. 다음으로, 데이터를 디스크에 저장합니다.

<!-- skip -->
```dart
// shared preferences 얻기
final prefs = await SharedPreferences.getInstance();

// 값 저장하기
prefs.setInt('counter', counter);
```

## 3. 데이터 읽기

`SharedPreferences` 클래스가 제공하는 getter 메서드를 사용하여 데이터를 읽을 수
있습니다. 각 setter에 상응하는 getter가 존재합니다. 예를 들어 `getInt`, `getBool`, 
`getString` 와 같은 메서드를 사용할 수 있습니다.

<!-- skip -->
```dart
final prefs = await SharedPreferences.getInstance();

// counter 키에 해당하는 데이터 읽기를 시도합니다. 만약 존재하지 않는 다면 0을 반환합니다.
final counter = prefs.getInt('counter') ?? 0;
```

## 4. 데이터 삭제하기

`remove()` 메서드를 사용하여 데이터를 삭제하세요.

<!-- skip -->
```dart
final prefs = await SharedPreferences.getInstance();

prefs.remove('counter');
```

## 지원되는 자료형

키-값 저장소를 사용하는 것은 쉽고 간편하지만, 
몇가지 제약 사항이 있습니다:

* 오직 원시 타입만 사용 가능합니다: `int`, `double`, `bool`, `string`, `stringList`
* 대용량 데이터 저장을 위해 설계되지 않았습니다.

For more information about shared preferences on Android,
see the [shared preferences documentation][]
on the Android developers website.

## 테스팅 지원

`shared_preferences`를 사용하여 데이터를 저장하는 코드를 테스트하는 것은
좋은 생각입니다. 이를 위해 `shared_preferences` 라이브러리를 mocking 하는
`MethodChannel` 객체를 생성해야 합니다.

테스트 파일 `setupAll()` 메서드 안에서 아래 코드를 수행하면 초깃값과 함께 
`SharedPreferences`를 생성하세요:

<!-- skip -->
```dart
const MethodChannel('plugins.flutter.io/shared_preferences')
  .setMockMethodCallHandler((MethodCall methodCall) async {
    if (methodCall.method == 'getAll') {
      return <String, dynamic>{}; // 필요하다면 여기서 초깃값을 설정합니다.
    }
    return null;
  });
```

## 예제

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // 이 위젯은 어플리케이션의 최상위 위젯입니다.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Shared preferences demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(title: 'Shared preferences demo'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  @override
  void initState() {
    super.initState();
    _loadCounter();
  }

  //시작할 때 counter 값을 불러옵니다.
  _loadCounter() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    setState(() {
      _counter = (prefs.getInt('counter') ?? 0);
    });
  }

  //클릭하면 counter를 증가시킵니다.
  _incrementCounter() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    setState(() {
      _counter = (prefs.getInt('counter') ?? 0) + 1;
      prefs.setInt('counter', _counter);
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.display1,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ), // 이 마지막 콤마는 build 메서드에 자동 서식이 잘 적용될 수 있도록 도와줍니다.
    );
  }
}
```


[shared_preferences]: {{site.pub}}/packages/shared_preferences
[shared preferences documentation]: {{site.android-dev}}/guide/topics/data/data-storage#pref
