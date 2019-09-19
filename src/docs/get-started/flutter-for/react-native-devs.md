---
title: React Native 개발자를 위한 Flutter
description: React Native 개발자가 Flutter 앱을 만들 때 기존 지식을 활용하는 방법을 배워봅시다.
---

이 문서는기존 React Native 지식을 활용하여
Flutter로 모바일 앱을 만들려는 React Native(RN) 개발자를 위한 문서입니다.
RN 프레임워크의 핵심을 이해하고 있다면
이 문서를 Flutter 개발을 시작하는 방안으로 사용하실 수 있습니다.

필요한 부분에 가장 적합한 질문을 찾아내는 방식으로 이 문서를 
요리책(cookbook)처럼 활용하실 수도 있습니다.


## 자바스크립트 개발자를 위한 Dart 소개

React Native처럼, Flutter는 reactive-style 뷰를 사용합니다.
하지만, 네이티브 위젯으로 변환(transpile)하는 RN과 달리, 
Flutter는 모든 것을 네이티브 코드로 직접 컴파일합니다.
Flutter는 화면의 각 픽셀을 직접 제어하기 때문에, 자바스크립트 브리지로 인한 성능 문제가 발생하지 않습니다. 

Dart는 아래와 같은 기능을 제공하는 배우기 쉬운 언어입니다:

* 웹, 서버, 모바일 앱 개발이 가능한 확장성 있는 
  오픈 소스 프로그래밍 언어 제공
* AOT를 통해 네이티브로 컴파일되는 C 방식의 구문을 사용하는 
  객체 지향 단일 상속 언어를 제공 
* 자바스크립트로 변환(transcompile) 가능
* 인터페이스와 추상 클래스 지원

아래에 자바스크립트와 Dart의 몇 가지 차이점에 관한 
예시가 적혀있습니다.

### 진입점

자바스크립트에는 진입점 역할을 하는 함수가 미리 정의되어 있지 않습니다.
진입점을 직접 지정합니다.

```js
// JavaScript
function startHere() {
  // Can be used as entry point
}
```
Dart에서는 최상단에 앱의 진입점인 `main()` 함수가 
반드시 있어야 합니다.

<!-- skip -->
```dart
// Dart
main() {
}
```

[DartPad]({{site.dartpad}}/0df636e00f348bdec2bc1c8ebc7daeb1)에서 실행해보세요.

### 콘솔에 출력하기

`print()`를 사용하여 Dart 콘솔에 출력해보세요.

```js
// JavaScript
console.log('Hello world!');
```

<!-- skip -->
```dart
// Dart
print('Hello world!');
```

[DartPad]({{site.dartpad}}/cf9e652f77636224d3e37d96dcf238e5)에서 
실행해보세요.

### 변수

Dart는 타입 검사를 하는(type safe) 언어입니다. 
Dart는 정적 타입 검사와 런타임 타입 검사를 동시에 사용하여, 
변수의 값이 변수의 정적 타입과 항상 일치하는지 검사합니다.
타입이 필수이긴 하지만, Dart가 타입 추론을 하기 때문에 일부 타입 표기는 생략해도 됩니다.

#### 변수 생성 및 할당

자바스크립트에서는, 변수의 타입을 선언할 수 없습니다.

[Dart]({{site.dart-site}}/dart-2)에서는, 명시적으로 변수의 타입을 선언하거나   
타입 시스템이 자동으로 적절한 변수의 타입을 추론할 수 있어야 합니다.

```js
// JavaScript
var name = 'JavaScript';
```

<!-- skip -->
```dart
// Dart
String name = 'dart'; // 명시적으로 문자열 타입 선언.
var otherName = 'Dart'; // 문자열 타입으로 추론.
// Dart에서는 두 방법 다 가능.
```

[DartPad]({{site.dartpad}}/3f4625c16e05eec396d6046883739612)에서 시도해보세요.

더 많은 정보를 원하시면, [Dart의 타입 시스템]({{site.dart-site}}/guides/language/sound-dart)를
참조하세요.

#### 기본값

자바스크립트에서 초기화하지 않은 변수는 `undefined`입니다.

Dart에서 초기화하지 않은 변수는 `null` 값을 가집니다.
Dart에선 숫자도 객체이기 때문에, 
초기화하지 않은 숫자 타입 변수도 `null` 값을 가집니다. 

```js
// JavaScript
var name; // == undefined
```

<!-- skip -->
```dart
// Dart
var name; // == null
int x; // == null
```

[DartPad]({{site.dartpad}}/57ec21faa8b6fe2326ffd74e9781a2c7)에서 
시도해보세요.

더 많은 정보를 원하시면, 
[변수]({{site.dart-site}}/guides/language/language-tour#variables)를
참조하세요.

### null 또는 0 체크

자바스크립트는 1이나 null이 아닌 객체도 true로 취급합니다.

```js
// JavaScript
var myNull = null;
if (!myNull) {
  console.log('null is treated as false');
}
var zero = 0;
if (!zero) {
  console.log('0 is treated as false');
}
```
Dart는 boolean 값 `true`만 true로 취급합니다.

<!-- skip -->
```dart
// Dart
var myNull = null;
if (myNull == null) {
  print('use "== null" to check null');
}
var zero = 0;
if (zero == 0) {
  print('use "== 0" to check zero');
}
```

[DartPad]({{site.dartpad}}/c85038ad677963cb6dc943eb1a0b72e6)에서 
시도해보세요.

### 함수

Dart와 자바스크립트 함수는 대체로 유사합니다. 
주된 차이점은 선언입니다.

```js
// JavaScript
function fn() {
  return true;
}
```

<!-- skip -->
```dart
// Dart
fn() {
  return true;
}
// can also be written as
bool fn() {
  return true;
}
```

[DartPad]({{site.dartpad}}/5454e8bfadf3000179d19b9bc6be9918)에서 
시도해보세요.

더 많은 정보를 원하시면, 
[함수]({{site.dart-site}}/guides/language/language-tour#functions)를 참조하세요.

### 비동기 프로그래밍

#### Futures

자바스크립트와 마찬가지로 Dart도 단일 스레드 실행을 지원합니다. 
자바스크립트에서 Promise 객체는 
비동기 작업의 최종 완료 (또는 실패)와 결과 값을 나타냅니다.

Dart는 이러한 비동기 처리를 위해서 
[`Future`]({{site.dart-site}}/tutorials/language/futures) 객체를 사용합니다.

```js
// JavaScript
class Example {
  _getIPAddress() {
    const url = 'https://httpbin.org/ip';
    return fetch(url)
      .then(response => response.json())
      .then(responseJson => {
        const ip = responseJson.origin;
        return ip;
      });
  }
}

function main() {
  const example = new Example();
  example
    ._getIPAddress()
    .then(ip => console.log(ip))
    .catch(error => console.error(error));
}

main();
```

<!-- skip -->
```dart
// Dart
import 'dart:convert';
import 'package:http/http.dart' as http;

class Example {
  Future<String> _getIPAddress() {
    final url = 'https://httpbin.org/ip';
    return http.get(url).then((response) {
      String ip = jsonDecode(response.body)['origin'];
      return ip;
    });
  }
}

main() {
  final example = new Example();
  example
      ._getIPAddress()
      .then((ip) => print(ip))
      .catchError((error) => print(error));
}
```

더 많은 정보를 원하시면, 
[Futures]({{site.dart-site}}/tutorials/language/futures)를 참조하세요.

#### `async`와 `await`

`async` 함수 선언으로 비동기 함수를 정의합니다.

자바스크립트에서는 `async` 함수가 `Promise`를 반환합니다.
`await` 연산자는 `Promise`를 기다리기 위해서 사용합니다다.

```js
// JavaScript
class Example {
  async function _getIPAddress() {
    const url = 'https://httpbin.org/ip';
    const response = await fetch(url);
    const json = await response.json();
    const data = await json.origin;
    console.log(data);
  }
}

async function main() {
  const example = new Example();
  try {
    const ip = await example._getIPAddress();
    console.log(ip);
  } catch (error) {
    console.error(error);
  }
}

main();
```

Dart에서는 `async` 함수가 `Future`를 반환하고, 
함수의 구현된 내용은 나중에 실행되도록 예약됩니다.
`await` 연산자는 `Future`를 기다리기 위해서 사용됩니다.

<!-- skip -->
```dart
// Dart
import 'dart:convert';
import 'package:http/http.dart' as http;

class Example {
  Future<String> _getIPAddress() async {
    final url = 'https://httpbin.org/ip';
    final response = await http.get(url);
    String ip = jsonDecode(response.body)['origin'];
    return ip;
  }
}

main() async {
  final example = new Example();
  try {
    final ip = await example._getIPAddress();
    print(ip);
  } catch (error) {
    print(error);
  }
}
```

더 많은 정보를 원하시면, [`async`와 `await`][]을 참조하세요.

## 기본

### Flutter 앱을 만드는 방법은?

React Native로 앱을 만드려면 커멘드라인에서 
`create-react-native-app`을 실행하면 됩니다.

{% prettify %}
$ create-react-native-app <projectname>
{% endprettify%}

Flutter에서 앱을 만들기 위해서는, 아래 방법 중 하나를 수행하면 됩니다:

* Flutter와 Dart 플러그인이 설치된 IDE를 이용하세요.
* 커멘드라인에서 `flutter create` 명령을 실행하세요.
  Flutter SDK가 PATH에 들어있는지 확인이 필요합니다. 

{% prettify %}
$ flutter create <projectname>
{% endprettify%}

더 많은 정보를 원하시면, [시작하기](/docs/get-started)를 참조하세요. 
버튼 클릭으로 카운터를 조정하는 앱을 만드는 과정을 안내 받을 수 있습니다.
Flutter 프로젝트를 만들 때 
Android 및 iOS 기기에서 샘플 앱을 실행하는 데 필요한 모든 파일이 생성됩니다.

### 앱을 실행하는 방법은?

React Native를 사용할 때는, 프로젝트 디렉토리에서 
`npm run`이나 `yarn run`으로 앱을 실행했을 것입니다.

Flutter apps를 실행하는 몇가지 방법이 있습니다:

* Flutter와 Dart 플러그인이 설치된 IDE에서 "run"을 실행하세요. 
* 프로젝트 최상위 디렉토리에서 `flutter run`을 사용하세요.

앱이 연결된 기기나, iOS 시뮬레이터 
혹은 Android 에뮬레이터에서 실행될 것입니다.

더 많은 정보를 원하시면, Flutter [시작하기](/docs/get-started)를 
참조하세요.

### 위젯을 import 하는 방법은?

React Native에서는 필요한 모든 컴포넌트를 각각 import 해야 합니다.

```js
//React Native
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
```

Flutter에서는 머티리얼 디자인 라이브러리에서 위젯을 사용하기 위해 
`material.dart` 패키지를 import 합니다. 
iOS 스타일 위젯을 사용하기 위해 쿠퍼티노 라이브러리를 import 합니다. 
더 많은 기본 위젯을 사용하고 싶다면, 위젯 라이브러리를 import 합니다. 
아니면 직접 위젯 라이브러리를 작성하여 import 할 수도 있습니다.

<!-- skip -->
```dart
import 'package:flutter/material.dart';
import 'package:flutter/cupertino.dart';
import 'package:flutter/widgets.dart';
import 'package:flutter/my_widgets.dart';
```
어떤 위젯 패키지를 import 하건, 
Dart는 앱에서 사용되는 위젯만 가져옵니다.

더 많은 정보를 원하시면, [Flutter 위젯 카탈로그](/docs/development/ui/widgets)를 참조하세요.

### Flutter에서 React Native "Hello world!" 앱과 동일한 것은?

React Native에서는 `HelloWorldApp`이 `React.Component`를 상속 받고 
render 메서드가 view 컴포넌트를 반환하도록 구현합니다.

```js
// React Native
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';

export default class App extends React.Component {
  render() {
    return (
      <View style={styles.container}>
        <Text>Hello world!</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center'
  }
});
```

Flutter에서는, 기본 위젯 라이브러리의 `Center`와 `Text` 위젯을 활용하여 
"Hello world!" 앱과 동일한 것을 만들 수 있습니다. 
`Center` 위젯을 최상위 위젯으로 하고, 
자식으로 `Text` 위젯을 넣으면 됩니다. 

<!-- skip -->
```dart
// Flutter
import 'package:flutter/material.dart';

void main() {
  runApp(
    Center(
      child: Text(
        'Hello, world!',
        textDirection: TextDirection.ltr,
      ),
    ),
  );
}

```

아래 이미지는 기본 Flutter "Hello world!" 앱의 
Android와 iOS 화면을 보여줍니다. 

{% include android-ios-figure-pair.md image="react-native/hello-world-basic.png" alt="Hello world app" class="border" %}

이제 가장 기본적인 Flutter 앱을 봤고, 
다음 섹션에서는 Flutter의 풍부한 위젯 라이브러리를 활용하여 
현대적이고 세련된 앱을 만드는 방법을 확인할 수 있습니다.  

### 위젯을 사용하고 위젯 트리를 형성하는 방법은?

Flutter에서는 거의 모든 것이 위젯입니다.

위젯은 앱 UI의 기본 구성 요소 입니다.
위젯을 위젯 트리라고 불리는 계층 구조로 조합합니다. 
각각의 위젯은 부모 위젯 내부에 들어가게 되고, 부모로부터 속성을 상속 받습니다.
앱 객체 자체도 위젯입니다.
별도의 “application” 객체는 없습니다.
대신, 최상위 위젯이 그 역할을 담당합니다. 

위젯을 아래와 같이 정의할 수 있습니다:

* 버튼이나 메뉴와 같은 구조적 요소
* 글꼴이나 색 구성표와 같은 문체 요소
* 레이아웃과 같은 패딩 또는 정렬의 한 측면

아래 예제는 머티리얼 라이브러리의 위젯을 활용한 "Hello world!" 앱입니다.
이 예제에서 위젯 트리는 `MaterialApp` 위젯 내부에 위치하게 됩니다. 


<!-- skip -->
```dart
// Flutter
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          child: Text('Hello world'),
        ),
      ),
    );
  }
}

```


아래 이미지는 머티리얼 디자인 위젯으로 만든 "Hello world!"입니다. 
기본적인 "Hello world!"보다 좀 더 많은 기능을 자유롭게 이용할 수 있습니다.

{% include android-ios-figure-pair.md image="react-native/hello-world.png" alt="Hello world app" %}

앱을 작성할 때, 2가지 종류의 위젯을 사용하게 될겁니다:
[StatelessWidget]({{site.api}}/flutter/widgets/StatelessWidget-class.html) 또는
 [StatefulWidget]({{site.api}}/flutter/widgets/StatefulWidget-class.html).
StatelessWidget은 위젯인데 상태가 없는 위젯입니다. 
StatelessWidget은 한 번 만들어지고 절대 변하지 않습니다. 
StatefulWidget은 사용자 입력이나 
데이터 수신이 있으면 상태를 동적으로 변경합니다.

상태가 없는 위젯과 상태가 있는 위젯의 중요한 차이점은 
StatefulWidgets이 상태 데이터를 저장하고 그것을 트리 재구성을 통해 전달하는 State 객체가 있고,
그렇기 때문에 손실되지 않는다는 점입니다. 

간단하거나 기본적인 앱에서 위젯을 감싸는 것은 쉽지만,
코드 베이스가 점차 커지고 앱이 복잡해지면, 
너무 많이 감싸져 있는 위젯을 함수로 빼거나 작은 클래스로 분리해야 합니다.
분리된 함수를 만들면 위젯을 앱 내에서 재사용할 수 있게 됩니다. 

### 재사용 가능한 컴포넌트를 만드는 방법은?

React Native에서는 재사용 가능한 컴포넌트를 만들기 위해 클래스를 정의한 다음 
`props` 메서드를 사용하여 선택한 요소의 속성과 값을 설정하거나 반환합니다. 
아래 예제에서 `CustomCard` 클래스를 정의하고, 부모 클래스 안에서 사용합니다. 

```js
// React Native
class CustomCard extends React.Component {
  render() {
    return (
      <View>
        <Text> Card {this.props.index} </Text>
        <Button
          title="Press"
          onPress={() => this.props.onPress(this.props.index)}
        />
      </View>
    );
  }
}

// Usage
<CustomCard onPress={this.onPress} index={item.key} />
```

Flutter에서는 커스텀 위젯을 만들기 위해 클래스를 정의하면 그 위젯을 재사용할 수 있습니다.
또는 아래 예제와 같은 `build` 함수에서 
재사용 가능한 위젯을 정의하고 호출할 수 있습니다.

{% prettify dart %}

// Flutter
class CustomCard extends StatelessWidget {
  [[highlight]]CustomCard({@required this.index, @required [[/highlight]]
     [[highlight]]this.onPress});[[/highlight]]

  final index;
  final Function onPress;

  @override
  Widget build(BuildContext context) {
    return Card(
      child: Column(
        children: <Widget>[
          Text('Card $index'),
          FlatButton(
            child: const Text('Press'),
            onPressed: this.onPress,
          ),
        ],
      )
    );
  }
}
    ...
// Usage
CustomCard(
  [[highlight]]index: index,[[/highlight]]
  [[highlight]]onPress: () { [[/highlight]]
    print('Card $index');
  },
)
    ...

{% endprettify %}

이전 예제에서는 `CustomCard` 클래스의 생성자가
[optional parameters]({{site.dart-site}}/guides/language/language-tour#optional-parameters)를
표현하기 위해 Dart의 중괄호 구문 `{ }`을 사용했습니다.

이 필드를 필수로 만들고 싶다면, 
생성자에서 중괄호를 지우거나 `@required`를 추가하면 됩니다.  


다음 스크린샷은 재사용할 수 있는 CustomCard 클래스의 예를 보여줍니다.

{% include android-ios-figure-pair.md image="react-native/custom-cards.png" alt="Custom cards" class="border" %}


## 프로젝트 구조 및 리소스

### 어디서부터 코드 작성을 시작해야 하나요?

`main.dart` 파일에서 시작하세요.
Flutter 앱을 만들면 `main.dart`가 자동 생성됩니다.  

<!-- skip -->
```dart
// Dart
void main(){
 print('Hello, this is the main function.');
}
```

Flutter에서 진입점은 `’projectname’/lib/main.dart` 파일이고, 
`main` 함수부터 실행을 시작합니다.  

### Flutter 앱에서 파일은 어떻게 구성되어 있습니까?

Flutter 프로젝트를 새롭게 만들면, 아래와 같은 디렉토리 구조가 생성됩니다.
나중에 구조를 변경할 수는 있지만, 일단은 아래와 같은 상태에서 시작합니다. 

```
┬
└ projectname
  ┬
  ├ android      - Android 관련 파일 포함.
  ├ build        - iOS 및 Android 빌드 파일 저장.
  ├ ios          - iOS 관련 파일 포함.
  ├ lib          - 외부에서 접근할 수 있는 Dart 소스 파일 포함.
    ┬
    └ src        - 추가적인 소스 파일 포함.
    └ main.dart  - Flutter 진입점이며 새로운 앱의 시작.
                   Flutter 프로젝트를 만들 때 자동으로 생성.
                   Dart코드 작성을 시작하는 부분임.
  ├ test         - 자동화 테스트 파일 포함.
  └ pubspec.yaml - Flutter 앱의 메타데이터 포함.
                   React Native의 package.json 파일과 동일함.
```

### 리소스와 asset은 어디에 위치 시키고, 어떻게 사용하나요?

Flutter 리소스 또는 asset은 앱과 함께 묶여서 
배포되며 런타임에 접근할 수 있는 파일입니다. 
Flutter 앱은 아래와 같은 asset 유형을 포함할 수 있습니다:
* JSON 파일 같은 정적 데이터
* 설정 파일
* 아이콘과 이미지 (JPEG, PNG, GIF, Animated GIF, WebP, Animated WebP, BMP,
  and WBMP)

Flutter는 프로젝트 최상위에 위치한 
`pubspec.yaml` 파일을 사용하여 앱에 필요한 assets을 식별합니다.

```yaml
flutter:
  assets:
    - assets/my_icon.png
    - assets/background.png
```

`assets` 부분은 앱에 포함시켜야하는 파일을 지정합니다.
각 asset은 `pubspec.yaml`에 명시된 상대 경로에 의해서
 asset 파일이 어디에 있는지 구분됩니다.
asset이 정의된 순서는 중요하지 않습니다.
실제 디렉토리 이름(위 경우에는 `assets`)도 중요하지 않습니다. 
어떤 디렉토리에도 asset을 배치할 수는 있지만, `assets` 디렉토리에 배치하기를 권합니다.

빌드 중, Flutter는 런타임 때에 앱이 읽어올 *asset bundle*이라 불리는 
특수한 아카이브에 asset을 저장합니다.
`pubspec.yaml`에 asset의 경로가 지정되면, 빌드 프로세스는 인접한 디렉토리에서 같은 이름의 파일을 찾습니다. 
이 파일들은 지정된 asset과 함께 asset bundle에도 포함됩니다. 
Flutter는 앱에 적합한 해상도의 이미지를 선택할 때 
asset variants를 사용합니다.

React Native에서는 이미지 파일을 소스 코드 디렉토리에 놓은 뒤 
경로를 지정하여 정적 이미지를 추가할 수 있습니다.

```js
<Image source={require('./my-icon.png')} />
```

Flutter에서는 위젯의 build 메서드 안에서 `AssetImage` 클래스를 사용하여 
정적 이미지를 추가할 수 있습니다.

<!-- skip -->
```dart
image: AssetImage('assets/background.png'),
```

더 많은 정보를 원하시면 [Flutter에서 
Asset과 이미지 파일 추가하기](/docs/development/ui/assets-and-images)를 참조하세요.

### 네트워크를 통해 이미지를 가져오는 방법은?

React Native에서는 `Image` 컴포넌트의 `source` 속성에서 `uri`을 지정하고, 
필요하다면 크기도 지정합니다.

Flutter에서는 `Image.network` 생성자를 사용하여 해당 URL에서 이미지를 가져옵니다.

<!-- skip -->
```dart
// Flutter
body: Image.network(
          'https://flutter-io.kr/images/owl.jpg',
```

### 패키지와 패키지 플러그인을 설치하는 방법은?

Flutter에서 Flutter와 Dart 생태계의 
다른 개발자들이 만든 공유 패키지를 사용할 수 있습니다.
이를 통해 일일이 모든 것을 개발할 필요없이 빠르게 앱을 개발할 수 있습니다.
패키지 플러그인은 플랫폼별 코드가 포함된 패키지입니다.

React Native에서는 커멘드라인 `yarn add {package-name}`나 
`npm install --save {package-name}` 명령을 사용해서 패키지를 설치합니다. 

Flutter에서는 아래와 같은 방법으로 패키지를 설치합니다:

1. `pubspec.yaml`의 dependencies 부분에 패키지 이름과 버전을 추가합니다. 
아래 예제는 `pubspec.yaml` 파일에 `google_sign_in` Dart 패키지를 
추가하는 방법을 보여줍니다. 
**공백이 문제가 될 수 있기 때문에** YAML file의 공백을 잘 확인하세요! 

```yaml
dependencies:
  flutter:
    sdk: flutter
  google_sign_in: ^3.0.3
```

2. 커멘드라인에서 `flutter pub get`을 사용해서 패키지를 설치하세요.
   IDE를 이용하고 있으면, IDE가 종종 `flutter pub get`을 실행해주거나, 
   실행하도록 유도할 것입니다.
3. 아래 코드와 같은 패키지를 import 하세요:

<!-- skip -->
```dart
import 'package:flutter/cupertino.dart';
```

더 많은 정보를 원하시면, 
[패키지 사용하기](/docs/development/packages-and-plugins/using-packages)와
[패키지 & 플러그인 개발](/docs/development/packages-and-plugins/developing-packages)를
참조하세요.

[Pub site]({{site.pub}})의 
[Flutter Packages]({{site.pub}}/flutter/) 섹션에서
Flutter 개발자들이 공유한 많은 패키지를 만날 수 있습니다.

## Flutter 위젯

Flutter에서는 UI를 만들 때 현재 상태와 설정에 따라 어떻게 보일지에 대한 
정보를 담고 있는 위젯을 사용합니다.

위젯은 종종 강력한 효과를 내기 위해 
다수의 중첩된 작은 단일 목적의 위젯으로 구성됩니다.
예를 들어, Container 위젯은 레이아웃, 그리기, 위치, 크기를 담당하는 여러 위젯으로 구성됩니다.
구체적으로 말하자면, `Container` 위젯은 `LimitedBox`와 `ConstrainedBox`, 
`Align`, `Padding`, `DecoratedBox`, `Transform` 위젯을 포함하고 있습니다.
원하는 효과를 내기 위해 `Container` 를 서브클래스로 만드는 대신, 
여러 심플한 위젯들을 새롭고 독특한 방식으로 구성할 수 있습니다.

`Center` 위젯은 레이아웃을 어떻게 조절할 수 있는지를 보여주는 또 다른 예입니다.
위젯을 가운데 정렬 하기 위해, `Center` 위젯으로 감싼 다음
정렬, 행, 그리드를 배치하는 위젯을 사용하세요.
이러한 배치 위젯은 시각적으로 자기 자신을 나타내지는 않습니다.
대신, 그것들의 유일한 목적은 다른 위젯 레이아웃을 제어하는 것입니다.
왜 위젯이 이런 방식으로 그려지는지 이해하기 위해,
인접한 위젯을 조사하는 것이 종종 도움이 됩니다.

더 많은 정보는
[Flutter 기술 개요](/docs/resources/technical-overview)를
참조하세요.

위젯 패키지의 핵심 위젯에 대한 자세한 정보는, 
[Flutter 기본 위젯](/docs/development/ui/widgets/basics), 
[Flutter 위젯 카탈로그](/docs/development/ui/widgets), 
[Flutter 위젯 색인](/docs/reference/widgets)을 
참조하세요.

## Views

### `View` 컨테이너와 동일한 것은?

React Native에서는 `View`가 컨테이너이고, `Flexbox`를 이용한 레이아웃, 
스타일, 터치 핸들링, 접근성 제어를 지원합니다.

Flutter에서는 
[Container]({{site.api}}/flutter/widgets/Container-class.html)나
[Column]({{site.api}}/flutter/widgets/Column-class.html),
[Row]({{site.api}}/flutter/widgets/Row-class.html),
[Center]({{site.api}}/flutter/widgets/Center-class.html) 같은
위젯 라이브러리의 핵심 레이아웃 위젯을 사용할 수 있습니다.

더 많은 정보를 원하시면, [레이아웃 위젯](/docs/development/ui/widgets/layout) 카탈로그를 참조하세요.

### `FlatList`나 `SectionList`와 동일한 것은?

`List`는 수직으로 배열된 구성 요소의 스크롤 가능한 목록입니다.

React Native에서는 `FlatList` 혹은 `SectionList`를 사용하여 
단순 목록 혹은 섹션 목록을 그립니다.

```js
// React Native
<FlatList
  data={[ ... ]}
  renderItem={({ item }) => <Text>{item.key}</Text>}
/>
```

[`ListView`]({{site.api}}/flutter/widgets/ListView-class.html)는 
가장 많이 사용되는 스크롤 위젯입니다.
`ListView`는 기본 생성자를 통해 명시적으로 자식 목록을 받습니다. 
[`ListView`]({{site.api}}/flutter/widgets/ListView-class.html)는
목록의 수가 적은 경우에 가장 적합합니다.
무거운 목록이거나 무한 스크롤 목록일 때는, `ListView.builder`를 사용하세요. 
자식들을 필요할 때만 빌드하고, 자식들이 화면에 나타내야 할 자식들만 빌드합니다. 


<!-- skip -->
```dart
// Flutter
var data = [ ... ];
ListView.builder(
  itemCount: data.length,
  itemBuilder: (context, int index) {
    return Text(
      data[index],
    );
  },
)
```

{% include android-ios-figure-pair.md image="react-native/flatlist.gif" alt="Flat list" class="border" %}

무한 스크롤을 만드는 방법을 배우고 싶다면, 
[첫 Flutter 앱 만들기, part 1]({{site.codelabs}}/codelabs/first-flutter-app-pt1) 코드랩을
참조하세요.

### Canvas를 사용하여 그리거나 색을 입히는 방법은?

React Native에서는 캔버스 컴포넌트가 없기 때문에, `react-native-canvas` 같은 서드 파티 라이브러리를 사용합니다. 

```js
// React Native
handleCanvas = canvas => {
  const ctx = canvas.getContext('2d');
  ctx.fillStyle = 'skyblue';
  ctx.beginPath();
  ctx.arc(75, 75, 50, 0, 2 * Math.PI);
  ctx.fillRect(150, 100, 300, 300);
  ctx.stroke();
};

render() {
  return (
    <View>
      <Canvas ref={this.handleCanvas} />
    </View>
  );
}
```
Flutter에서는 
[`CustomPaint`]({{site.api}}/flutter/widgets/CustomPaint-class.html)와
[`CustomPainter`]({{site.api}}/flutter/rendering/CustomPainter-class.html) 클래스를
사용하여 캔버스에 그릴 수 있습니다.

아래 예시는 `CustomPaint` 위젯을 사용해서 페인트 단계에서 그리는 방법을 보여줍니다.
추상 클래스인 CustomPainter를 구현하고, 
이를 CustomPaint의 painter 속성에 전달합니다.
CustomPaint 서브 클래스는 `paint`와 `shouldRepaint`를 꼭 구현해야 합니다.

<!-- skip -->
```dart
// Flutter
class MyCanvasPainter extends CustomPainter {

  @override
  void paint(Canvas canvas, Size size) {
    Paint paint = Paint();
    paint.color = Colors.amber;
    canvas.drawCircle(Offset(100.0, 200.0), 40.0, paint);
    Paint paintRect = Paint();
    paintRect.color = Colors.lightBlue;
    Rect rect = Rect.fromPoints(Offset(150.0, 300.0), Offset(300.0, 400.0));
    canvas.drawRect(rect, paintRect);
  }

  bool shouldRepaint(MyCanvasPainter oldDelegate) => false;
  bool shouldRebuildSemantics(MyCanvasPainter oldDelegate) => false;
}
class _MyCanvasState extends State<MyCanvas> {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: CustomPaint(
        painter: MyCanvasPainter(),
      ),
    );
  }
}
```

{% include android-ios-figure-pair.md image="react-native/canvas.png" alt="Canvas" class="border" %}

## 레이아웃

### 위젯을 사용하여 레이아웃 속성을 정의하는 방법은?

React Native에서 대부분의 레이아웃은 특정 컴포넌트에 전달된 props로 속성을 정의할 수 있습니다. 
예를 들어 `View` 컴포넌트의 `style` prop을 사용해서 flexbox 속성을 지정할 수 있습니다.
컴포넌트들을 열로 배열하려면 
props를 아래와 같이 지정해주면 됩니다:
`flexDirection: “column”`.

```js
// React Native
<View
  style={%raw%}{{
    flex: 1,
    flexDirection: 'column',
    justifyContent: 'space-between',
    alignItems: 'center'
  }}{%endraw%}
>
```

Flutter에서는 컨트롤 위젯 및 스타일 속성을 결합하여 특별히 설계한 위젯을 통해서 
레이아웃을 정의합니다.

예를 들어, 
[Column]({{site.api}}/flutter/widgets/Column-class.html) 및
[Row]({{site.api}}/flutter/widgets/Row-class.html) 위젯은
자식 배열을 가지고, 그 배열을 각각 수직 혹은 수평으로 정렬합니다.
[Container]({{site.api}}/flutter/widgets/Container-class.html)
위젯은 레이아웃과 스타일링 속성을 조합하고
[`Center`]({{site.api}}/flutter/widgets/Center-class.html) 위젯은 자식 위젯을
중앙에 위치시킵니다.

<!-- skip -->
```dart
// Flutter
Center(
  child: Column(
    children: <Widget>[
      Container(
        color: Colors.red,
        width: 100.0,
        height: 100.0,
      ),
      Container(
        color: Colors.blue,
        width: 100.0,
        height: 100.0,
      ),
      Container(
        color: Colors.green,
        width: 100.0,
        height: 100.0,
      ),
    ],
  ),
)
```

Flutter는 기본 위젯 라이브러리에서 다양한 레이아웃 위젯을 제공합니다.
예를 들어, [`Padding`]({{site.api}}/flutter/widgets/Padding-class.html),
[`Align`]({{site.api}}/flutter/widgets/Align-class.html), 
[`Stack`]({{site.api}}/flutter/widgets/Stack-class.html)과 같은 핵심 위젯을 제공합니다.

전체 리스트를 원하시면 [레이아웃 위젯](/docs/development/ui/widgets/layout)를 참조하세요.

{% include android-ios-figure-pair.md image="react-native/basic-layout.gif" alt="Layout" class="border" %}

### 위젯을 겹쳐 쌓아올리는 방법은?

React Native에서는 `absolute` 포지션을 사용하여 컴포넌트를 겹치게 할 수 있습니다.

Flutter에서는
[`Stack`]({{site.api}}/flutter/widgets/Stack-class.html) 위젯을 사용하여
자식 위젯들을 겹치게 할 수 있습니다.
위젯이 전체적으로 또는 부분적으로 기본 위젯과 겹치게 할 수 있습니다.

`Stack` 위젯은 상자의 가장자리를 기준으로 자식들을 배치합니다.
이 클래스는 여러 자식 위젯을 간단하게 쌓아올리려고 할 때 유용합니다.

<!-- skip -->
```dart
// Flutter
Stack(
  alignment: const Alignment(0.6, 0.6),
  children: <Widget>[
    CircleAvatar(
      backgroundImage: NetworkImage(
        'https://avatars3.githubusercontent.com/u/14101776?v=4'),
    ),
    Container(
      decoration: BoxDecoration(
          color: Colors.black45,
      ),
      child: Text('Flutter'),
    ),
  ],
)
```

위 예제는 `Stack`을 사용하여 `CircleAvatar` 위에
Container(반투명 검정 배경을 가진 `Text`를 보여주고 있음)를 쌓아올립니다.
Stack은 정렬 속성과 정렬 좌표를 사용하여 
텍스트의 간격을 조정합니다. 

{% include android-ios-figure-pair.md image="react-native/stack.png" alt="Stack" class="border" %}

더 많은 정보를 원하시면, 
[Stack]({{site.api}}/flutter/widgets/Stack-class.html) 클래스 문서를 참조하세요.

## 스타일링

### 컴포넌트를 꾸미는 방법은?

React Native에서는 인라인 스타일링과 `stylesheets.create`를 사용하여 
컴포넌트를 꾸밉니다.

```js
// React Native
<View style={styles.container}>
  <Text style={%raw%}{{ fontSize: 32, color: 'cyan', fontWeight: '600' }}{%endraw%}>
    This is a sample text
  </Text>
</View>

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center'
  }
});
```

Flutter에서 `Text` 위젯은 style 속성에 `TextStyle` 클래스를 사용할 수 있습니다.
여러 곳에서 같은 텍스트 스타일을 사용하고 싶다면, 
[`TextStyle`]({{site.api}}/flutter/dart-ui/TextStyle-class.html) 클래스를
여러 `Text` 위젯에 사용할 수 있습니다.

<!-- skip -->
```dart
// Flutter
var textStyle = TextStyle(fontSize: 32.0, color: Colors.cyan, fontWeight:
   FontWeight.w600);
	...
Center(
  child: Column(
    children: <Widget>[
      Text(
        'Sample text',
        style: textStyle,
      ),
      Padding(
        padding: EdgeInsets.all(20.0),
        child: Icon(Icons.lightbulb_outline,
          size: 48.0, color: Colors.redAccent)
      ),
    ],
  ),
)
```

{% include android-ios-figure-pair.md image="react-native/flutterstyling.gif" alt="Styling" class="border" %}

### `Icons`와 `Colors`를 사용하는 방법은?

React Native는 아이콘을 지원하지 않기 때문에 서드 파티 라이브러리가 사용됩니다.

Flutter에서는 머티리얼 라이브러리를 import 하면 다양한 
[머티리얼 아이콘]({{site.api}}/flutter/material/Icons-class.html)과  
[컬러]({{site.api}}/flutter/material/Colors-class.html) 또한 함께 가져옵니다.

<!-- skip -->
```dart
Icon(Icons.lightbulb_outline, color: Colors.redAccent)
```

`Icons` 클래스를 사용할 때, 프로젝트의 `pubspec.yaml` 파일에 
`uses-material-design: true`로 설정 되어 있는지 확인하세요. 
이렇게 하면 아이콘을 표시하는 `MaterialIcons` 글꼴이 앱에 포함되도록 할 수 있습니다.
{% prettify dart %}
name: my_awesome_application
flutter: [[highlight]]uses-material-design: true[[/highlight]]
{% endprettify %}

Flutter의 [쿠퍼티노 (iOS-style)](/docs/development/ui/widgets/cupertino) 패키지는 
현재 iOS 디자인 언어를 잘 준수하는 위젯을 제공합니다. `CupertinoIcons`을 사용하기 위해서는 
프로젝트의 `pubspec.yaml` 파일에 `cupertino_icons` 의존성을 추가해야 합니다.

```yaml
name: my_awesome_application
dependencies:
  cupertino_icons: ^0.1.0
```

컴포넌트 전체적인 색과 스타일을 지정하고 싶다면, 
`ThemeData`을 사용해서 테마의 다양한 측에 기본 색상을 지정하세요. 
`MaterialApp`에서 theme 속성에 `ThemeData` 객체를 설정하세요.
[`Colors`]({{site.api}}/flutter/material/Colors-class.html) 클래스는 
머티리얼 디자인의 [color palette]({{site.material}}/guidelines/style/color.html)에 
해당하는 색상을 제공합니다.

아래 예제는 기본 색을 `blue`로, 
텍스트 선택을 `red`로 설정하는 예제입니다.

<!-- skip -->
{% prettify dart %}
class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        [[highlight]]primarySwatch: Colors.blue,[[/highlight]]
        [[highlight]]textSelectionColor: Colors.red[[/highlight]]
      ),
      home: SampleAppPage(),
    );
  }
}
{% endprettify %}

### 스타일 테마를 추가하는 방법은?

React Native에서, 컴포넌트 공통 테마는 stylesheets에 정의한 후 
컴포넌트에 사용합니다.

Flutter에서는 
[`ThemeData`]({{site.api}}/flutter/material/ThemeData-class.html) 클래스에 
스타일을 정의하고 
[`MaterialApp`]({{site.api}}/flutter/material/MaterialApp-class.html) 위젯의 
테마 속성에 전달함으로써 
거의 모든 곳에 균일한 스타일을 적용할 수 있습니다.

<!-- skip -->
```dart
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(
        primaryColor: Colors.cyan,
        brightness: Brightness.dark,
      ),
      home: StylingPage(),
    );
  }
```

`Theme`는 `MaterialApp` 위젯을 사용하지 않고도 적용될 수 있습니다. 
[`Theme`]({{site.api}}/flutter/material/Theme-class.html) 위젯은 
`data` 매개 변수에서 `ThemeData`를 가져와 
모든 자식 위젯에 `ThemeData`를 적용합니다.

<!-- skip -->
```dart
 @override
  Widget build(BuildContext context) {
    return Theme(
      data: ThemeData(
        primaryColor: Colors.cyan,
        brightness: brightness,
      ),
      child: Scaffold(
         backgroundColor: Theme.of(context).primaryColor,
              ...
              ...
      ),
    );
  }
```

## 상태 관리

상태(State)는 위젯이 빌드될 때 동기적으로 읽을 수 있는 정보이고,
위젯의 수명 동안 변경될 수 있는 정보입니다.
Flutter에서 앱의 상태를 관리하기 위해서는, State 객체와 함께 
[StatefulWidget]({{site.api}}/flutter/widgets/StatefulWidget-class.html)을 
사용하세요.

Flutter 상태 관리 방법에 대한 더 자세한 정보는
[State management][]를 참조하세요.

### The StatelessWidget

Flutter에서 `StatelessWidget`은 상태 변화가 필요 
없는(관리할 내부 상태가 없음) 위젯입니다.

Stateless 위젯은 
묘사하려는 UI의 일부분이  
객체의 자체적인 구성 정보나 위젯의 
[`BuildContext`]({{site.api}}/flutter/widgets/BuildContext-class.html) 
이외의 것에 의존하지 않을 때 유용합니다.

[AboutDialog]({{site.api}}/flutter/material/AboutDialog-class.html),
[CircleAvatar]({{site.api}}/flutter/material/CircleAvatar-class.html),
[Text]({{site.api}}/flutter/widgets/Text-class.html)가
[StatelessWidget]({{site.api}}/flutter/widgets/StatelessWidget-class.html)의
하위 클래스인 상태가 없는 위젯의 예시입니다.


<!-- skip -->
```dart
// Flutter
import 'package:flutter/material.dart';

void main() => runApp(MyStatelessWidget(text: 'StatelessWidget Example to show immutable data'));

class MyStatelessWidget extends StatelessWidget {
  final String text;
  MyStatelessWidget({Key key, this.text}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text(
        text,
        textDirection: TextDirection.ltr,
      ),
    );
  }
}
```

이전 예제에서, `final` 표시가 있는 `text`를 전달하는 
`MyStatelessWidget`을 생성자로 사용했습니다. 이 클래스는 `StatelessWidget`을 확장하고, 
불변 데이터를 포함하고 있습니다.

상태가 없는 위젯의 `build` 메서드는 보통 3가지 상황에서만 
호출됩니다.

* 위젯이 트리에 추가될 때
* 위젯의 부모가 설정이 변경됐을 때
* 사용하고 있는 
  [`InheritedWidget`]({{site.api}}/flutter/widgets/InheritedWidget-class.html)이
  변경될 때

### The StatefulWidget

[StatefulWidget]({{site.api}}/flutter/widgets/StatefulWidget-class.html)은 
상태가 변경되는 위젯입니다. `setState` 메서드를 사용하여 `StatefulWidget`의 
상태를 변경하세요. `setState`를 호출하면 Flutter 프레임워크에게 상태가 변경되었다고 알려주고,
그러면 앱이 `build` 메서드를 다시 실행하여 변경 사항을 반영할 수 있게 됩니다.

상태는 위젯이 빌드 될 때 동기적으로 읽을 수 있는 정보이고, 
위젯의 생명 주기 동안 변경 될 수 있습니다. 
상태가 변경 될 때 즉시 상태를 알리는 것은 위젯을 구현하는 사람의 책임입니다.
위젯이 동적으로 변화하는 경우 `StatefulWidget`을 사용하세요.
예를 들어, form에 타이핑을 하거나 slider를 움직여서 위젯의 상태가 변할 수 있습니다. 
또는, 시간이 지남에 따라 변경될 수도 있습니다(데이터 피드가 UI를 업데이트 할 수 있습니다).

[Checkbox]({{site.api}}/flutter/material/Checkbox-class.html),
[Radio]({{site.api}}/flutter/material/Radio-class.html),
[Slider]({{site.api}}/flutter/material/Slider-class.html),
[InkWell]({{site.api}}/flutter/material/InkWell-class.html),
[Form]({{site.api}}/flutter/widgets/Form-class.html),
[TextField]({{site.api}}/flutter/material/TextField-class.html)는
[StatefulWidget]({{site.api}}/flutter/widgets/StatefulWidget-class.html)의
하위클래스인 상태가 있는 위젯의 예시입니다.

아래 예시는 `createState()` 메서드를 필요로 하는 `StatefulWidget`을 선언하는 예시입니다.
이 메서드는 위젯의 상태를 관리하는 상태 객체 `_MyStatefulWidgetState`를 생성합니다.

<!-- skip -->
```dart
class MyStatefulWidget extends StatefulWidget {
  MyStatefulWidget({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _MyStatefulWidgetState createState() => _MyStatefulWidgetState();
}
```

아래 상태 클래스 `_MyStatefulWidgetState`는 위젯의 `build()` 메서드를 구현합니다. 
상태가 바뀌면(예를 들어, 사용자가 버튼을 누르면), 
`setState`가 새로운 toggle 값과 함께 호출됩니다.
이렇게 하면 프레임워크가 UI 위젯을 다시 빌드합니다.

<!-- skip -->
```dart
class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  bool showtext=true;
  bool toggleState=true;
  Timer t2;

  void toggleBlinkState(){
    setState((){
      toggleState=!toggleState;
    });
    var twenty = const Duration(milliseconds: 1000);
    if(toggleState==false) {
      t2 = Timer.periodic(twenty, (Timer t) {
        toggleShowText();
      });
    } else {
      t2.cancel();
    }
  }

  void toggleShowText(){
    setState((){
      showtext=!showtext;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          children: <Widget>[
            (showtext
              ?(Text('This execution will be done before you can blink.'))
              :(Container())
            ),
            Padding(
              padding: EdgeInsets.only(top: 70.0),
              child: RaisedButton(
                onPressed: toggleBlinkState,
                child: (toggleState
                  ?( Text('Blink'))
                  :(Text('Stop Blinking'))
                )
              )
            )
          ],
        ),
      ),
    );
  }
}
```

### StatefulWidget 및 StatelessWidget의 모범 사례는?

위젯을 설계할 때 고려해야 할 몇 가지 사항이 있습니다.

#### 1. 위젯이 StatefulWidget인지 StatelessWidget인지 결정하세요.


Flutter에서, 위젯은 상태 변화에 의존적인지에 따라 
Stateful이거나 Stateless입니다.

* 위젯이 변화한다면(사용자 인터렉션이 있거나 데이터로부터 UI 변경이 있다면), 
  Stateful을 사용해야 합니다.
* 위젯이 final이거나 immutable이면, Stateless를 사용해야 합니다.

#### 2. 어떤 객체가 위젯의 상태를 관리하는지 결정하세요. (StatefulWidget의 경우)

Flutter에서 상태를 관리하는 3가지 주된 방법이 있습니다.

* 위젯이 자신의 상태를 관리
* 부모 위젯이 상태를 관리
* 혼합하여 관리

어떤 접근 방식을 사용할 것인지 결정할 때, 아래 원칙을 고려하세요.

* 해당 상태가 사용자 데이터라면(예를 들어, 슬라이더 위치 
  혹은 체크박스의 선택과 취소) 
  상태를 상위 위젯에서 관리하는 것이 가장 좋습니다.
* 해당 상태가 보이는 것과 깊다면(예를 들어, 애니메이션)
  해당 위젯이 상태를 제일 잘 관리할 수 있습니다.
* 잘 모르겠을 때는 부모 위젯이 자식 위젯의 상태를 관리하게 하세요.

#### 3. StatefulWidget의 하위 클래스 및 State

`MyStatefulWidget` 클래스는 자신의 상태를 관리합니다. 이 클래스는
`StatefulWidget`를 상속 받고, `createState()` 메서드를 오버라이드하여 
State 객체를 만듭니다. 그리고 프레임워크는 `createState()`를 호출하여 위젯을 빌드합니다.
이 예제에서, `createState()`는 다음 모범 사례에서 구현되는
`_MyStatefulWidgetState`의 인스턴스를 만듭니다.

<!-- skip -->
```dart
class MyStatefulWidget extends StatefulWidget {
  MyStatefulWidget({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _MyStatefulWidgetState createState() => _MyStatefulWidgetState();
}

class _MyStatefulWidgetState extends State<MyStatefulWidget> {

  @override
  Widget build(BuildContext context) {
    ...
  }
}
```

#### 4. StatefulWidget을 위젯 트리에 추가하세요.

직접 만든 `StatefulWidget`을 앱의 빌드 메서드에 있는 위젯 트리에 추가하세요.

<!-- skip -->
```dart
class MyStatelessWidget extends StatelessWidget {
  // This widget is the root of your application.

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyStatefulWidget(title: 'State Change Demo'),
    );
  }
}
```

{% include android-ios-figure-pair.md image="react-native/state-change.gif" alt="State change" class="border" %}

## Props

React Native에서 대부분의 컴포넌트는 매개변수나 속성을 `props`로 전달하여 
커스터마이징할 수 있습니다. 
자식 컴포넌트에서 `this.props`를 사용해 매개변수에 접근할 수 있습니다.  

```js
// React Native
class CustomCard extends React.Component {
  render() {
    return (
      <View>
        <Text> Card {this.props.index} </Text>
        <Button
          title='Press'
          onPress={() => this.props.onPress(this.props.index)}
        />
      </View>
    );
  }
}
class App extends React.Component {

  onPress = index => {
    console.log('Card ', index);
  };

  render() {
    return (
      <View>
        <FlatList
          data={[ ... ]}
          renderItem={({ item }) => (
            <CustomCard onPress={this.onPress} index={item.key} />
          )}
        />
      </View>
    );
  }
}
```

Flutter에서는 매개변수가 있는 생성자에서 받은 속성을
`final`로 표시된 지역 변수나 함수에 할당합니다.

<!-- skip -->
```dart
// Flutter
class CustomCard extends StatelessWidget {

  CustomCard({@required this.index, @required this.onPress});
  final index;
  final Function onPress;

  @override
  Widget build(BuildContext context) {
  return Card(
    child: Column(
      children: <Widget>[
        Text('Card $index'),
        FlatButton(
          child: const Text('Press'),
          onPressed: this.onPress,
        ),
      ],
    ));
  }
}
    ...
//Usage
CustomCard(
  index: index,
  onPress: () {
    print('Card $index');
  },
)
```

{% include android-ios-figure-pair.md image="react-native/modular.png" alt="Cards" class="border" %}

## 로컬 저장소

아주 많은 데이터나 구조화가 필요하지 않다면, 
키-값 쌍 형태의 저장소인 `shared_preferences`로
기본 타입(booleans, floats, ints, longs, strings)
데이터를 읽고 쓸 수 있습니다.

### 앱에 전역적인 키-값 쌍을 지속성있게 저장하는 방법은?

React Native에서는 `AsyncStorage`의 
`setItem`과 `getItem` 함수를 사용하여 앱 전역에 걸쳐 
지속성있게 데이터를 저장하고 다시 찾아옵니다.  

```js
// React Native
await AsyncStorage.setItem( 'counterkey', json.stringify(++this.state.counter));
AsyncStorage.getItem('counterkey').then(value => {
  if (value != null) {
    this.setState({ counter: value });
  }
});
```

Flutter에서는 키-값 데이터를 읽고 쓰기 위해서 앱 내 전역에서 지속되는  
[`shared_preferences`]({{site.github}}/flutter/plugins/tree/master/packages/shared_preferences)
플러그인을 사용합니다. `shared_preferences` 플러그인은 간단한 데이터를 지속성있게 
저장할 수 있는 저장소를 제공하기 위해 iOS에선 `NSUserDefaults`, 
Android에선 `SharedPreferences`를 감싸고 있습니다.
플러그인을 사용하기 위해 `shared_preferences`를 
`pubspec.yaml` 파일의 의존성에 추가한 다음 Dart 파일에서 import 하세요 

```yaml
dependencies:
  flutter:
    sdk: flutter
  shared_preferences: ^0.4.3
```

<!-- skip -->
```dart
// Dart
import 'package:shared_preferences/shared_preferences.dart';
```

지속성있는 데이터 저장을 구현하려면 `SharedPreferences` 클래스의
setter 메서드를 사용하세요.
Setter 메서드는 `setInt`, `setBool`, `setString`과 같은 방식으로
다양한 기본형 타입에서 사용 가능합니다.
데이터를 읽으려면 `SharedPreferences` 클래스의 getter 메서드를 사용하세요.
각 setter에 상응하는 getter가 `getInt`, `getBool`, `getString`와 같은 형태로 존재합니다.


<!-- skip -->
```dart
SharedPreferences prefs = await SharedPreferences.getInstance();
_counter = prefs.getInt('counter');
prefs.setInt('counter', ++_counter);
setState(() {
  _counter = _counter;
});
```


## Routing

대부분의 앱은 다양한 종류의 정보를 보여주기 위해 여러 화면을 가지고 있습니다.
예를 들어, 사용자가 제품 사진을 눌렀을 때 더 많은 제품 정보를 보여주는 
제품 화면을 새로운 화면에서 띄울 수 있습니다. 

Android에서 새로운 화면은 새로운 Activity입니다. 
iOS에 새로운 화면은 새로운 ViewController입니다.
Flutter에서는 새로운 화면도 위젯입니다!
그리고 새로운 화면으로 이동하기 위해서는 Navigator 위젯을 사용합니다.

### 어떻게 화면을 이동하나요?

React Native에는 3개의 주요 네비게이터가 존재합니다: StackNavigator, TabNavigator, DrawerNavigator.
각각의 네비게이터는 화면에 대한 설정과 정의를 제공합니다.

```js
// React Native
const MyApp = TabNavigator(
  { Home: { screen: HomeScreen }, Notifications: { screen: tabNavScreen } },
  { tabBarOptions: { activeTintColor: '#e91e63' } }
);
const SimpleApp = StackNavigator({
  Home: { screen: MyApp },
  stackScreen: { screen: StackScreen }
});
export default (MyApp1 = DrawerNavigator({
  Home: {
    screen: SimpleApp
  },
  Screen2: {
    screen: drawerScreen
  }
}));
```

Flutter에서는 화면 사이를 이동하는 데 필요한 2개의 주요 위젯이 있습니다:
* [Route]({{site.api}}/flutter/widgets/Route-class.html)는 
  앱의 화면 또는 페이지를 추상화 한 것입니다.
* [Navigator]({{site.api}}/flutter/widgets/Navigator-class.html)는  
  route를 관리하는 위젯입니다.

`Navigator`는 스택을 사용하여 자식 위젯들을 관리하는 위젯입니다.
네비게이터는 `Route` 객체들의 스택을 관리하고, 
스택을 관리할 수 있는 
[`Navigator.push`]({{site.api}}/flutter/widgets/Navigator/push.html)나
[`Navigator.pop`]({{site.api}}/flutter/widgets/Navigator/pop.html) 같은
메서드들을 제공합니다.
Route의 목록은 
[`MaterialApp`]({{site.api}}/flutter/material/MaterialApp-class.html) 
위젯에서 지정할 수 있습니다.
또는 hero 애니메이션에서 처럼 동적으로 만들어질 수도 있습니다. 
아래 예제는 `MaterialApp` 위젯에서 named route를 지정하는 예제입니다. 

<!-- skip -->
```dart
// Flutter
class NavigationApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
            ...
      routes: <String, WidgetBuilder>{
        '/a': (BuildContext context) => usualNavscreen(),
        '/b': (BuildContext context) => drawerNavscreen(),
      }
            ...
  );
  }
}
```

Named route로 이동하기 위해,
[of]({{site.api}}/flutter/widgets/Navigator/of.html) 메서드를 사용하여
`Navigator` 위젯의 (위젯 트리에서 위젯의 위치를 조정하는) `BuildContext`를 지정합니다.
Route의 이름을 `pushNamed` 함수를 통해 전달하여 특정한 route로 이동합니다.

<!-- skip -->
```dart
Navigator.of(context).pushNamed('/a');
```

`Navigator`의 push 메서드로 주어진
[`route`]({{site.api}}/flutter/widgets/Route-class.html)를
가장 가까운
[`context`]({{site.api}}/flutter/widgets/BuildContext-class.html)를 가진
네비게이터에 추가하고 이동할 수도 있습니다.
아래 예제에서는 
[`MaterialPageRoute`]({{site.api}}/flutter/material/MaterialPageRoute-class.html)
위젯이 플랫폼에 적합한 전환효과와 함께 전체 스크린을 modal route로 전환시킵니다.
필수 매개변수로
[`WidgetBuilder`]({{site.api}}/flutter/widgets/WidgetBuilder.html)를
넣어줘야 합니다.

<!-- skip -->
```dart
Navigator.push(context, MaterialPageRoute(builder: (BuildContext context)
 => UsualNavscreen()));
```

### 탭 네비게이션과 Drawer(서랍) 네비게이션을 사용하는 방법은?

머티리얼 디자인 앱에서는 
Flutter 네비게이션으로 선택할 수 있는 탭과 drawer라는 2가지 주요 선택지가 있습니다.
탭을 위한 공간이 충분하지 않다면, drawer가 좋은 대안이 될 수 있습니다. 


#### 탭 네비게이션

React Native에서는 `createBottomTabNavigator`와 `TabNavigation`을 사용하여 
탭을 보여주고 탭을 이동할 수 있습니다.

```js
// React Native
import { createBottomTabNavigator } from 'react-navigation';

const MyApp = TabNavigator(
  { Home: { screen: HomeScreen }, Notifications: { screen: tabNavScreen } },
  { tabBarOptions: { activeTintColor: '#e91e63' } }
);
```

Flutter는 drawer 및 탭 네비게이션을 위한 여러 특수 위젯을 제공합니다:
* [TabController]({{site.api}}/flutter/material/TabController-class.html)&mdash;TabBar와
  TabBarView 사이에서 탭 선택을 조정.
* [TabBar]({{site.api}}/flutter/material/TabBar-class.html)&mdash;탭의 
  수평 열을 보여줌.
* [Tab]({{site.api}}/flutter/material/Tab-class.html)&mdash;머티리얼 디자인으로 
  TabBar 탭을 생성.
* [TabBarView]({{site.api}}/flutter/material/TabBarView-class.html)&mdash;현재 선택된
  탭에 맞는 위젯을 보여줌.


<!-- skip -->
```dart
// Flutter
TabController controller=TabController(length: 2, vsync: this);

TabBar(
  tabs: <Tab>[
    Tab(icon: Icon(Icons.person),),
    Tab(icon: Icon(Icons.email),),
  ],
  controller: controller,
),

```

`TabController`는 `TabBar`와 `TabBarView` 사이에서 탭 선택을 조정할 때 필요합니다.
`TabController` 생성자의 `length` 인자는 전체 탭 개수입니다. 
`TickerProvider`는 프레임이 상태 변화를 할 때마다 알려주기 위해 필요합니다.
`TickerProvider`는 `vsync`입니다.
새로운 `TabController`를 만들 때마다 
`TabController` 생성자에 `vsync: this` 인자를 넣으세요. 

[TickerProvider]({{site.api}}/flutter/scheduler/TickerProvider-class.html)는 
[`Ticker`]({{site.api}}/flutter/scheduler/Ticker-class.html) 객체를 제공할 수 있는 
클래스에 의해 구현된 인터페이스입니다.
Tickers는 프레임이 트리거 될 때마다 알림을 받아야하는 
모든 객체에서 사용할 수 있지만 보통은 
[`AnimationController`]({{site.api}}/flutter/animation/AnimationController-class.html)를 
통해 간접적으로 사용합니다.
`AnimationControllers`는 `Ticker`를 얻기 위해 `TickerProvider`를 필요로 합니다.
State에서 AnimationController를 만들고 있다면,
[`TickerProviderStateMixin`]({{site.api}}/flutter/widgets/TickerProviderStateMixin-mixin.html) 또는
[`SingleTickerProviderStateMixin`]({{site.api}}/flutter/widgets/SingleTickerProviderStateMixin-mixin.html)
클래스를 사용하여 적절한 `TickerProvider`를 얻을 수 있습니다.

아래 [`Scaffold`]({{site.api}}/flutter/material/Scaffold-class.html) 위젯은
새로운 `TabBar` 위젯을 감싸고 2개의 탭을 생성합니다.
`TabBarView` 위젯은 `Scaffold` 위젯의 `body` 인자로 전달됩니다.
`TabBar` 위젯의 탭에 해당하는 모든 화면은 
같은 `TabController`를 가지고 있는 
`TabBarView` 위젯의 자식들입니다.


<!-- skip -->
```dart
// Flutter

class _NavigationHomePageState extends State<NavigationHomePage> with SingleTickerProviderStateMixin {
  TabController controller=TabController(length: 2, vsync: this);
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      bottomNavigationBar: Material (
        child: TabBar(
          tabs: <Tab> [
            Tab(icon: Icon(Icons.person),)
            Tab(icon: Icon(Icons.email),),
          ],
          controller: controller,
        ),
        color: Colors.blue,
      ),
      body: TabBarView(
        children: <Widget> [
          home.homeScreen(),
          tabScreen.tabScreen()
        ],
        controller: controller,
      )
    );
  }
}
```

#### Drawer(서랍) 네비게이션

React Native에서는 react-navigation 패키지를 import 하고 
`createDrawerNavigator`과 `DrawerNavigation`를 사용합니다.

```js
// React Native
export default (MyApp1 = DrawerNavigator({
  Home: {
    screen: SimpleApp
  },
  Screen2: {
    screen: drawerScreen
  }
}));
```

Flutter에서는 `Drawer` 위젯을 `Scaffold`와 조합하여 
머티리얼 디자인의 drawer를 만들 수 있습니다.
앱에 `Drawer`를 추가하기 위해, 
`Drawer`를 `Scaffold` 위젯으로 감싸세요.
`Scaffold` 위젯은 [머티리얼 디자인]({{site.material}}/design) 가이드라인을 따르는 
일관성있는 시각적 구조를 앱에 제공합니다.
또한, `Drawers`, `AppBars`, `SnackBars`와 같은 
특수한 머티리얼 디자인 컴포넌트를 제공하기도 합니다. 

`Drawer` 위젯은 `Scaffold`의 왼쪽 혹은 오른쪽 모서리에서 슬라이드 형태로 등장하여
앱의 네비게이션 링크들을 보여주는 머티리얼 디자인 패널입니다.
[`Button`]({{site.api}}/flutter/material/RaisedButton-class.html)이나
[`Text`]({{site.api}}/flutter/widgets/Text-class.html) 위젯, 
또는 여러 아이템 목록을 `Drawer` 위젯의 자식으로 보여줄 수 있습니다.
아래 예제에서, 
[`ListTile`]({{site.api}}/flutter/material/ListTile-class.html)
위젯은 눌렀을 때 네비게이션 동작을 수행합니다.

<!-- skip -->
```dart
// Flutter
Drawer(
  child:ListTile(
    leading: Icon(Icons.change_history),
    title: Text('Screen2'),
    onTap: () {
      Navigator.of(context).pushNamed('/b');
    },
  ),
  elevation: 20.0,
),
```

`Scaffold` 위젯은 Drawer를 사용할 수 있을 때 `Drawer`로 연결되는 
IconButton을 띄워주는 `AppBar` 위젯 또한 포함하고 있습니다.
`Scaffold`를 사용하면 edge-swipe 동작을 했을 때 자동으로 
`Drawer`가 나타납니다.

<!-- skip -->
```dart
// Flutter
@override
Widget build(BuildContext context) {
  return Scaffold(
    drawer: Drawer(
      child: ListTile(
        leading: Icon(Icons.change_history),
        title: Text('Screen2'),
        onTap: () {
          Navigator.of(context).pushNamed('/b');
        },
      ),
      elevation: 20.0,
    ),
    appBar: AppBar(
      title: Text('Home'),
    ),
    body: Container(),
  );
}
```

{% include android-ios-figure-pair.md image="react-native/navigation.gif" alt="Navigation" class="border" %}

## 제스처 감지와 터치 이벤트 다루기

제스처를 감지하고 반응하기 위해, Flutter는 탭, 드래그, 크기 조절을 지원합니다. 
Flutter의 제스처 시스템은 2개의 층을 가지고 있습니다. 
첫번째 층은 포인터의 위치와 움직임을 표현하는 (터치, 마우스, 스타일러스 동작) 
원시 포인터 이벤트입니다.
두번째 층은 하나 이상의 포인터 움직임으로 만들어진 동작을 표현하는 
제스처입니다.  

### click 또는 press 리스너를 추가하는 방법은?

React Native에서는 `PanResponder` 또는 `Touchable` 컴포넌트를 사용하여
리스너를 추가합니다. 

```js
// React Native
<TouchableOpacity
  onPress={() => {
    console.log('Press');
  }}
  onLongPress={() => {
    console.log('Long Press');
  }}
>
  <Text>Tap or Long Press</Text>
</TouchableOpacity>
```

더 복잡한 제스처와 다양한 터치를 하나의 제스처로 조합할 때는,
[`PanResponder`](https://facebook.github.io/react-native/docs/panresponder.html)를
사용합니다.

```js
// React Native
class App extends Component {

  componentWillMount() {
    this._panResponder = PanResponder.create({
      onMoveShouldSetPanResponder: (event, gestureState) =>
        !!getDirection(gestureState),
      onPanResponderMove: (event, gestureState) => true,
      onPanResponderRelease: (event, gestureState) => {
        const drag = getDirection(gestureState);
      },
      onPanResponderTerminationRequest: (event, gestureState) => true
    });
  }

  render() {
    return (
      <View style={styles.container} {...this._panResponder.panHandlers}>
        <View style={styles.center}>
          <Text>Swipe Horizontally or Vertically</Text>
        </View>
      </View>
    );
  }
}
```

Flutter에서는 위젯에 click (또는 press) 리스너를 추가하기 위해
`onPress: field`를 가지고 있는 버튼이나 터치 가능한 위젯을 사용하세요.
또는 어떤 위젯이건 [`GestureDetector`]({{site.api}}/flutter/widgets/GestureDetector-class.html)로
감싸서 제스처 감지를 추가하세요.

<!-- skip -->
```dart
// Flutter
GestureDetector(
  child: Scaffold(
    appBar: AppBar(
      title: Text('Gestures'),
    ),
    body: Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          Text('Tap, Long Press, Swipe Horizontally or Vertically '),
        ],
      )
    ),
  ),
  onTap: () {
    print('Tapped');
  },
  onLongPress: () {
    print('Long Pressed');
  },
  onVerticalDragEnd: (DragEndDetails value) {
    print('Swiped Vertically');
  },
  onHorizontalDragEnd: (DragEndDetails value) {
    print('Swiped Horizontally');
  },
);
```
`GestureDetector` 콜백 등에 관한 더 많은 정보를 원하시면,
[GestureDetector class][]를 참조하세요.

[GestureDetector class]: {{site.api}}/flutter/widgets/GestureDetector-class.html#instance-properties

{% include android-ios-figure-pair.md image="react-native/flutter-gestures.gif" alt="Gestures" class="border" %}

## HTTP 네트워크 요청 만들기

대부분의 앱은 인터넷에서 데이터를 가져오는 게 일반적입니다.
그리고 Flutter에서는 `http` 패키지가 인터넷에서 데이터를 가져오는 가장 간단한 방법을 제공합니다. 

### API를 호출하여 데이터를 가져오는 방법은?

React Native는 fetch를 요청하고 데이터 응답을 받는 
네트워크 동작을 위해 Fetch API를 제공합니다. 

```js
// React Native
_getIPAddress = () => {
  fetch('https://httpbin.org/ip')
    .then(response => response.json())
    .then(responseJson => {
      this.setState({ _ipAddress: responseJson.origin });
    })
    .catch(error => {
      console.error(error);
    });
};
```

Flutter는 `http` 패키지를 사용합니다. `http` 패키지를 설치하기 위해,
pubspec.yaml의 의존성 부분에 추가하세요.

```yaml
dependencies:
  flutter:
    sdk: flutter
  http: <latest_version>
```

Flutter는 HTTP 클라이언트 지원에 
[`dart:io`]({{site.api}}/flutter/dart-io/dart-io-library.html)를 사용합니다.
HTTP 클라이언트를 만들기 위해 `dart:io`를 import 하세요.

<!-- skip -->
```dart
import 'dart:io';
```

이 클라이언트는 다음과 같은 HTTP 동작을 지원합니다: GET, POST, PUT, DELETE.

<!-- skip -->
```dart
// Flutter
final url = Uri.https('httpbin.org', 'ip');
final httpClient = HttpClient();
_getIPAddress() async {
  var request = await httpClient.getUrl(url);
  var response = await request.close();
  var responseBody = await response.transform(utf8.decoder).join();
  String ip = jsonDecode(responseBody)['origin'];
  setState(() {
    _ipAddress = ip;
  });
}
```

{% include android-ios-figure-pair.md image="react-native/api-calls.gif" alt="API calls" class="border" %}

## Form input

텍스트 필드로 사용자가 앱에 텍스트를 입력하게 하여, 
양식, 메시징 앱, 검색 환경 등을 구축하는 데 사용할 수 있습니다.
Flutter는 2가지 종류의 핵심 텍스트 필드 위젯을 제공합니다:
[TextField]({{site.api}}/flutter/material/TextField-class.html)와
[TextFormField]({{site.api}}/flutter/material/TextFormField-class.html).

### 텍스트 필드 위젯을 사용하는 방법은?

React Native에서는 텍스트를 입력하기 위해
`TextInput` 컴포넌트를 사용하여 텍스트 입력 상자를 보여주고
콜백을 사용하여 값을 변수에 저장합니다.

```js
// React Native
<TextInput
  placeholder="Enter your Password"
  onChangeText={password => this.setState({ password })}
 />
<Button title="Submit" onPress={this.validate} />
```

Flutter에서는 
[`TextEditingController`]({{site.api}}/flutter/widgets/TextEditingController-class.html)
클래스를 사용하여 `TextField` 위젯을 관리합니다.
이 컨트롤러는 텍스트 필드가 수정될 때마다 리스너에게 알립니다. 

리스너들은 사용자가 필드에 입력한 내용을 알기 위해 
text와 selection 속성을 읽습니다. 컨트롤러의 `text` 속성을 활용하여 
`TextField`에 있는 텍스트에 접근할 수 있습니다.

<!-- skip -->
```dart
// Flutter
final TextEditingController _controller = TextEditingController();
      ...
TextField(
  controller: _controller,
  decoration: InputDecoration(
    hintText: 'Type something', labelText: 'Text Field '
  ),
),
RaisedButton(
  child: Text('Submit'),
  onPressed: () {
    showDialog(
      context: context,
        child: AlertDialog(
          title: Text('Alert'),
          content: Text('You typed ${_controller.text}'),
        ),
     );
   },
 ),
)
```

이 예제에서는 사용자가 submit 버튼을 클릭했을 때 
현재 텍스트 필드에 입력된 텍스트를 알림창에 띄웁니다.
경고 메시지를 표시하는 
[`alertDialog`]({{site.api}}/flutter/material/AlertDialog-class.html) 위젯을
사용하면 되고,  
[TextEditingController]({{site.api}}/flutter/widgets/TextEditingController-class.html)의
`text` 속성을 사용하여 `TextField`에서 입력된 텍스트에 접근합니다.

### Form 위젯을 사용하는 방법은?

Flutter에서 submit 버튼과 
[`TextFormField`]({{site.api}}/flutter/material/TextFormField-class.html)
위젯들을 자식으로 가지는 
[`Form`]({{site.api}}/flutter/widgets/Form-class.html)을
사용하세요.
`TextFormField` 위젯에는 form이 저장될 때 실행되는 콜백을 받는 
[`onSaved`]({{site.api}}/flutter/widgets/FormField/onSaved.html)라는 매개 변수가 있습니다.
`FormState` 객체는 `Form` 하위에 있는 각 `FormField`를 저장하고, 
초기화하고, 검증하기 위해 사용됩니다.
Form의 부모 context와 함께 `Form.of`를 사용하거나, 
Form 생성자에 `GlobalKey`를 넘기고 `GlobalKey.currentState`를 호출하여 
`FormState`를 가져올 수 있습니다.

<!-- skip -->
```dart
final formKey = GlobalKey<FormState>();

...

Form(
  key:formKey,
  child: Column(
    children: <Widget>[
      TextFormField(
        validator: (value) => !value.contains('@') ? 'Not a valid email.' : null,
        onSaved: (val) => _email = val,
        decoration: const InputDecoration(
          hintText: 'Enter your email',
          labelText: 'Email',
        ),
      ),
      RaisedButton(
        onPressed: _submit,
        child: Text('Login'),
      ),
    ],
  ),
)
```

아래 예제는 submit하여 form을 저장할 때
`Form.save()`와 `GlobalKey`인 `formKey`가 어떻게 사용되는지를 보여줍니다.

<!-- skip -->
```dart
void _submit() {
  final form = formKey.currentState;
  if (form.validate()) {
    form.save();
    showDialog(
      context: context,
      child: AlertDialog(
        title: Text('Alert'),
        content: Text('Email: $_email, password: $_password'),
      )
    );
  }
}
```

{% include android-ios-figure-pair.md image="react-native/input-fields.gif" alt="Input" class="border" %}

## Platform-specific 코드

크로스 플랫폼 앱을 만들 때 여러 플랫폼에서 같은 코드를 최대한 많이 재사용하고 싶을 것입니다.
하지만 어떤 경우에는 OS마다 다른 코드를 작성하는 게 현명할 수도 있습니다.
그렇기 때문에 플랫폼 별로 분기 처리를 구현할 수 있어야 합니다.

React Native에서는 아래와 같은 구현이 가능합니다:

```js
// React Native
if (Platform.OS === 'ios') {
  return 'iOS';
} else if (Platform.OS === 'android') {
  return 'android';
} else {
  return 'not recognised';
}
```
Flutter에서는 아래와 같은 구현을 사용합니다:
<!-- skip -->
```dart
// Flutter
if (Theme.of(context).platform == TargetPlatform.iOS) {
  return 'iOS';
} else if (Theme.of(context).platform == TargetPlatform.android) {
  return 'android';
} else if (Theme.of(context).platform == TargetPlatform.fuchsia) {
  return 'fuchsia';
} else {
  return 'not recognised ';
}
```

## 디버깅

### Flutter에서 앱을 디버깅하는 데 어떤 도구를 사용할 수 있습니까?

Flutter 또는 Dart 앱 디버깅 [DevTools][]를 사용하세요.

DevTools은 프로파일링, 힙 검사, 위젯트리 조사, 로깅 진단,
디버깅, 실행된 코드 라인 관찰, 메모리 누수 및 메모리 조각화 디버깅을 지원합니다.
더 많은 정보를 원하시면,
[DevTools][] 문서를 참조하세요 

IDE를 사용 중이라면,
IDE의 디버거를 이용하여 앱을 디버깅할 수 있습니다.

### 어떻게 hot reload를 수행할 수 있나요?

Flutter의 Stateful Hot Reload 기능은 쉽고 빠른 실험, UI 구축, 기능 추가, 버그 수정을 지원합니다.
변경이 있을 때마다 앱을 다시 컴파일하지 않고, 
즉각적으로 hot reload 할 수 있습니다.
현재 state는 변경되지 않고, 
앱은 수정 사항을 반영하여 업데이트 됩니다.

React Native에서는 
iOS 시뮬레이터의 ⌘R 단축키를 사용하거나 Android 에뮬레이터에서 R를 두 번 눌러서 리로드합니다.

Flutter에서는 
IntelliJ IDE 또는 Android Studio를 사용하는 경우 모두 저장 (⌘s/ctrl-s)을 선택하거나 
도구 모음에서 Hot Reload 버튼을 클릭 할 수 있습니다.
`flutter run`을 이용하여 커멘드 라인에서 앱을 실행중이시면,
터미널 화면에서 `r`을 눌르세요.
터미널 창에 `R`을 입력하여 전체적으로 
재시작을 수행할 수도 있습니다.

### 인앱 개발자 메뉴에 접근하는 방법은?

React Native에서는 기기를 흔들어서 개발자 메뉴에 접근할 수 있습니다.
(iOS 시뮬레이터에서 ⌘D, Android 에뮬레이터에서 ⌘M을 눌러 접근 가능) 

Flutter에서는 IDE를 사용하고 있다면 IDE 도구를 사용할 수 있습니다. 
`flutter run`을 사용하여 앱을 실행했다면 터미널 창에서 `h`를 눌러서 접근할 수 있습니다.
혹은 아래와 같은 단축키를 사용할 수 있습니다:

<div class="table-wrapper" markdown="1">
| 동작| 터미널 단축키| 디버그 함수와 속성|
| :------- | :------: | :------ |
| 앱의 위젯 계층| `w`| debugDumpApp()|
| 앱의 렌더링 트리 | `t`| debugDumpRenderTree()|
| 레이어| `L`| debugDumpLayerTree()|
| 접근성 | `S` (traversal order) or<br>`U` (inverse hit test order)|debugDumpSemantics()|
| 위젯 검사기로 전환 | `i` | WidgetsApp. showWidgetInspectorOverride|
| 가이드 선(construction lines) 표시로 전환| `p` | debugPaintSizeEnabled|
| 다른 운영 체제를 시뮬레이션| `o` | defaultTargetPlatform|
| 성능 오버레이를 표시 | `P` | WidgetsApp. showPerformanceOverlay|
| flutter.png 스크린샷 저장| `s` ||
| 종료| `q` ||
{:.table.table-striped}
</div>

## 애니메이션

잘 설계된 애니메이션은 UI를 직관적으로 느끼게 하고, 
앱의 모양과 느낌에 기여하며 사용자 경험을 향상시킵니다. 
Flutter의 애니메이션은 간단하거나 복합적인 애니메이션을 쉽게 구현할 수 있도록 지원합니다.
Flutter SDK에는 기본적인 모션 효과가 있는 머티리얼 디자인 위젯을 포함되어 있고,
이러한 효과를 쉽게 커스터마이징하여 앱을 개인화 할 수 있습니다.

React Native에서는 애니메이션 API를 사용하여 애니메이션을 만듭니다.

Flutter에서는 
[`Animation`]({{site.api}}/flutter/animation/Animation-class.html)
클래스와 
[`AnimationController`]({{site.api}}/flutter/animation/AnimationController-class.html)
클래스를 사용합니다. `Animation`은 현재 값과 상태 (완료 또는 해제)를 알고 있는 추상 클래스입니다.
`AnimationController` 클래스를 사용하면
애니메이션을 앞뒤로 재생하거나, 애니메이션을 중지하고, 애니메이션을 특정 값으로 설정하는 등 
모션을 커스터마이징 할 수 있습니다.

### 간단한 fade-in 애니메이션을 추가하는 방법은?

아래 React Native 예제에서는 Animated API를 사용하여 
애니메이션 컴포넌트 `FadeInView`를 생성했습니다.
초기 불투명 상태, 최종 상태 및 전환이 발생하는 기간를 정의합니다. 
애니메이션 컴포넌트가 `Animated` 컴포넌트 내부에 추가되고, 
불투명 상태 `fadeAnim`이 움직이게 하고 싶은 Text 컴포넌트의 불투명도에 매핑 된 다음, `start()`를 
호출하여 애니메이션을 시작합니다.

```js
// React Native
class FadeInView extends React.Component {
  state = {
    fadeAnim: new Animated.Value(0) // Initial value for opacity: 0
  };
  componentDidMount() {
    Animated.timing(this.state.fadeAnim, {
      toValue: 1,
      duration: 10000
    }).start();
  }
  render() {
    return (
      <Animated.View style={%raw%}{{...this.props.style, opacity: this.state.fadeAnim }}{%endraw%} >
        {this.props.children}
      </Animated.View>
    );
  }
}
    ...
<FadeInView>
  <Text> Fading in </Text>
</FadeInView>
    ...
```

Flutter에서 같은 애니메이션을 생성하기 위해, `controller`라는 
[`AnimationController`]({{site.api}}/flutter/animation/AnimationController-class.html) 
객체를 만들고 기간을 지정하세요.
기본적으로, `AnimationController`는 특정 기간 동안 0.0 ~ 1.0 범위에서 값이 선형적으로 증가합니다.
이 애니메이션 컨트롤러는 앱이 실행되는 기기에서 새로운 프레임을 보여줄 준비가 됐을 때마다 새로운 값을 생성합니다.
일반적으로, 초당 약 60번을 생성하는 비율을 가집니다.  

`AnimationController`를 정의할 때, `vsync` 객체를 넘겨야만 합니다.
`vsync`가 있으면 화면이 꺼져 있을 때 애니메이션에 불필요한 리소스를 소비하지 않습니다.
상태가 있는 객체를 사용할 수도 있습니다. 
클래스 정의에 `TickerProviderStateMixin`을 추가하여 상태 저장 객체를 `vsync`로 사용할 수 있습니다.
`AnimationController`에는 생성자에 `vsync` 인수를 사용하여 만들어진 TickerProvider가 필요합니다.

[`Tween`]({{site.api}}/flutter/animation/Tween-class.html)은 
시작 값과 끝 값 사이의 보간(interpolation) 또는 입력 범위에서 출력 범위까지의 매핑을 표현합니다.
애니메이션과 함께 `Tween` 객체를 사용하기 위해, 
`Tween` 객체의 `animate` 매세드를 호출한 뒤 수정하고 싶은 `Animation` 객체로 넘기세요.

이 예제에서는 
[`FadeTransition`]({{site.api}}/flutter/widgets/FadeTransition-class.html)
위젯이 사용되고, `opacity` 속성은 `animation` 객체에 매핑됩니다.

애니메이션을 시작하기 위해, `controller.forward()`를 사용하세요. 
`fling()`이나 `repeat()`을 사용하여 다른 동작도 수행할 수 있습니다.  
이 예제에서는
[`FlutterLogo`]({{site.api}}/flutter/material/FlutterLogo-class.html)
위젯이 `FadeTransition` 위젯 안에 사용됩니다.

<!-- skip -->
```dart

// Flutter
import 'package:flutter/material.dart';

void main() {
  runApp(Center(child: LogoFade()));
}

class LogoFade extends StatefulWidget {
  _LogoFadeState createState() => _LogoFadeState();
}

class _LogoFadeState extends State<LogoFade> with TickerProviderStateMixin {
  Animation animation;
  AnimationController controller;

  initState() {
    super.initState();
    controller = AnimationController(
        duration: const Duration(milliseconds: 3000), vsync: this);
    final CurvedAnimation curve =
    CurvedAnimation(parent: controller, curve: Curves.easeIn);
    animation = Tween(begin: 0.0, end: 1.0).animate(curve);
    controller.forward();
  }

  Widget build(BuildContext context) {
    return FadeTransition(
      opacity: animation,
      child: Container(
        height: 300.0,
        width: 300.0,
        child: FlutterLogo(),
      ),
    );
  }

  dispose() {
    controller.dispose();
    super.dispose();
  }
}
```

{% include android-ios-figure-pair.md image="react-native/flutter-fade.gif" alt="Flutter fade" class="border" %}

### 카드에 스와이프 애니메이션을 추가하는 방법은?

React Native에서는 `PanResponder` 혹은 서드파티 라이브러리를 사용하여 
스와이프 애니메이션을 만듭니다.

Flutter에서는 스와이프 애니메이션을 추가하기 위해, 
[`Dismissible`]({{site.api}}/flutter/widgets/Dismissible-class.html)
위젯을 사용하여 자식 위젯을 감쌉니다.

<!-- skip -->
```dart
child: Dismissible(
  key: key,
  onDismissed: (DismissDirection dir) {
    cards.removeLast();
  },
  child: Container(
    ...
  ),
),
```

{% include android-ios-figure-pair.md image="react-native/card-swipe.gif" alt="Card swipe" class="border" %}

## React Native 및 Flutter 위젯 상응 컴포넌트

아래 표 목록은 일반적으로 사용되는 React Native 컴포넌트와 상응하는 Flutter 위젯, 
그리고 공통 위젯 속성을 보여줍니다. 

<div class="table-wrapper" markdown="1">
| React Native 컴포넌트                                                                       | Flutter 위젯                                                                                                | 설명                                                                                                                                    |
| ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| [Button](https://facebook.github.io/react-native/docs/button.html)                        | [Raised Button]({{site.api}}/flutter/material/RaisedButton-class.html)                           | 기본 양각 버튼(raised button).                                                                              |
|                                                                                           |  onPressed [required]                                                                                        | 버튼을 누르거나 활성화할 때 호출되는 콜백.                                                          |
|                                                                                           | Child                                                                              | 버튼의 라벨.                                                                                                      |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [Button](https://facebook.github.io/react-native/docs/button.html)                        | [Flat Button]({{site.api}}/flutter/material/FlatButton-class.html)                               | 기본 평평한 버튼(flat button).                                                                                                         |
|                                                                                           |  onPressed [required]                                                                                        | 버튼을 누르거나 활성화할 때 호출되는 콜백.                                                            |
|                                                                                           | Child                                                                              | 버튼의 라벨.                                                                                                      |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [ScrollView](https://facebook.github.io/react-native/docs/scrollview.html)                | [ListView]({{site.api}}/flutter/widgets/ListView-class.html)                                    | 선형으로 배열된 스크롤 가능한 위젯 목록.     |
||        children                                                                              | 	( <Widget\> [ ])  표시할 하위 위젯의 목록.
||controller |[ [Scroll Controller]({{site.api}}/flutter/widgets/ScrollController-class.html) ] 스크롤 가능한 위젯을 제어하는 데 사용할 수 있는 객체.
||itemExtent|[ double ] Null이 아닐 경우, 스크롤 방향으로 지정된 크기를 갖도록 한다.   
||scroll Direction|[ [Axis]({{site.api}}/flutter/painting/Axis-class.html) ] 스크롤 뷰가 스크롤되는 축.
||                                                                                                            |                                                                                                                                        |
| [FlatList](https://facebook.github.io/react-native/docs/flatlist.html)                    | [ListView. builder()]({{site.api}}/flutter/widgets/ListView/ListView.builder.html)               | 온디맨드 방식의 선형으로 배열된 위젯 목록 생성자.
||itemBuilder [required] |[[ Indexed Widget Builder]({{site.api}}/flutter/widgets/IndexedWidgetBuilder.html)] 온디맨드 방식으로 자식들을 생성. 이 콜백은 0보다 크거나 같고 itemCount보다 작은 인덱스로만 호출됩니다.      
||itemCount |[ int ] 최대 스크롤 범위를 추정하는 ListView의 기능을 향상시킴.   
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [Image]({{site.api}}/flutter/widgets/Image-class.html)                         | [Image](https://facebook.github.io/react-native/docs/image.html)                                           | 이미지를 보여주는 위젯.                                                                                                             |
|                                                                                           |  image [required]                                                                                          | 보여질 이미지.                                                                                                                  |
|                                                                                           | Image. asset                                                                                                | 이미지를 다양한 방법으로 지정할 수 있게 하기 위해 여러 생성자가 제공됨.                                                 |
|                                                                                           | width, height, color, alignment                                                                            | 이미지의 스타일 및 레이아웃.                                                                                                         |
|                                                                                           | fit                                                                                                        | 레이아웃에서 설정된 공간에 이미지를 위치시킴.                                                                               |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [Modal](https://facebook.github.io/react-native/docs/modal.html)                          | [ModalRoute]({{site.api}}/flutter/widgets/ModalRoute-class.html)                                | 이전 경로와의 상호 작용이 차단된 경로.                                                                                  |
|                                                                                           | animation                                                                                                  | 경로가 변화할 때 애니메이션 및 이전 경로에서 앞으로 갈 때 애니메이션.                                            |
|                                                                                           |                                                                                                            |                                                                                                                                        |
|  [Activity Indicator](https://facebook.github.io/react-native/docs/activityindicator.html) | [Circular Progress Indicator]({{site.api}}/flutter/material/CircularProgressIndicator-class.html) | 원을 따라 진행 상황을 보여주는 위젯.                                                                                           |
|                                                                                           | strokeWidth                                                                                                | 원을 그리는 데 사용되는 선의 너비.                                                                                         |
|                                                                                           | backgroundColor                                                                                            | 진행 표시자의 배경 색. 현재 테마의 `ThemeData.backgroundColor`가 기본.                                   |
|                                                                                           |                                                                                                            |                                                                                                                                        |
|  [Activity Indicator](https://facebook.github.io/react-native/docs/activityindicator.html) | [Linear Progress Indicator]({{site.api}}/flutter/material/LinearProgressIndicator-class.html)     | 원을 따라 진행 상황을 보여주는 위젯.                                                                                             |
|                                                                                           | value                                                                                                      | 진행 표시자의 값.                                                                                                   |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [Refresh Control](https://facebook.github.io/react-native/docs/refreshcontrol.html)        | [Refresh Indicator]({{site.api}}/flutter/material/RefreshIndicator-class.html)                   |  "스와이프하여 새로 고침"을 지원하는 위젯.                                                                          |
|                                                                                           | color                                                                                                      | 진행 표시기의 전경색.                                                                                             |
|                                                                                           | onRefresh                                                                                                  | 사용자가 앱을 새로고침 하기를 원하여 새로 고침 표시를 충분히 드래그 할 때 호출되는 함수입니다.      |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Container]({{site.api}}/flutter/widgets/Container-class.html)                                  | 자식 위젯을 둘러싼 위젯.                                                                                                                |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Column]({{site.api}}/flutter/widgets/Column-class.html)                                        | 자식들을 수직 배열로 배치하는 위젯.                                                                                              |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Row]({{site.api}}/flutter/widgets/Row-class.html)                                              | 자식들을 수평 배열로 배치하는 위젯.                                                                                            |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Center]({{site.api}}/flutter/widgets/Center-class.html)                                        | 자식을 중심에 배치하는 위젯.                                                                                                       |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Padding]({{site.api}}/flutter/widgets/Padding-class.html)                                      | 주어진 여백과 함께 자식을 삽입하는 위젯.                                                                                                 |
|                                                                                           | padding [required]                                                                                         | [ EdgeInsets ] 일정 공간과 함께 자식을 삽입하는 위젯.
|||
| [Touchable Opacity](https://facebook.github.io/react-native/docs/touchableopacity.html)    | [Gesture Detector]({{site.api}}/flutter/widgets/GestureDetector-class.html)                      | 제스처를 감지하는 위젯.                                                                                                                       |
|                                                                                           | onTap                                                                                                      | 탭이 발생할 때의 콜백.                                                                                                               |
|                                                                                           | onDoubleTap                                                                                                | 탭이 동일한 위치에서 두 번 연속해서 빠르게 발생할 때의 콜백입니다.
|||
| [Text Input]({{site.api}}/flutter/services/TextInput-class.html)                | [Text Input](https://facebook.github.io/react-native/docs/textinput.html)                                   | 시스템의 텍스트 입력 제어에 대한 인터페이스.                                                                                           |
|                                                                                           | controller                                                                                                 | [ [Text Editing Controller]({{site.api}}/flutter/widgets/TextEditingController-class.html) ] 텍스트를 액세스하고 수정하는 데 사용.
|||
| [Text](https://facebook.github.io/react-native/docs/text.html)                          | [Text]({{site.api}}/flutter/widgets/Text-class.html)                                            | 단일 스타일로 텍스트 문자열을 표시하는 텍스트 위젯.                                                                                                                                                                              |
|                                                                                         | data                                                                                                      | [ String ] 보여줄 텍스트.                                                                                                                                                                                 |
|                                                                                         | textDirection                                                                                             | [ [Text Align]({{site.api}}/flutter/dart-ui/TextAlign-class.html) ] 텍스트가 흘러가는 방향.                                                                                     |
|                                                                                         |                                                                                                           |                                                                                                                                                                                                              |
| [Switch](https://facebook.github.io/react-native/docs/switch.html)                      | [Switch]({{site.api}}/flutter/material/Switch-class.html)                                      | 머티리얼 디자인 스위치.                                                                                                                                                                                    |
|                                                                                         | value [required]                                                                                          | [ boolean ] 스위치가 켜져 있는지 꺼져 있는지 여부.                                                                                                                                                                 |
|                                                                                         | onChanged [required]                                                                                      | [ callback ] 사용자가 스위치를 켜거나 끌 때 호출됨.                                                                                                                                               |
|                                                                                         |                                                                                                           |                                                                                                                                                                                                              |
| [Slider](https://facebook.github.io/react-native/docs/slider.html)                      | [Slider]({{site.api}}/flutter/material/Slider-class.html)                                      | 값 범위에서 선택해야 할 때 사용됨.                                                                                                                                                                       |
|                                                                                         | value [required]                                                                                          | [ double ] 슬라이더의 현재 값.                                                                                                                                                                           |
|                                                                                         | onChanged [required]                                                                                      | 사용자가 슬라이더의 새 값을 선택할 때 호출됨.                                                                                                                                                      |
{:.table.table-striped}
</div>


[DevTools]: /docs/development/tools/devtools
[State management]: /docs/development/data-and-backend/state-mgmt
[async와 await]: {{site.dart-site}}/guides/language/language-tour#asynchrony-support
