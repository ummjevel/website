---
title: JSON and serialization
description: How to use JSON with Flutter.
---

어느 시점부터 웹 서버와 통신하지 않거나 구조화된 데이터를 적절하게 보관하지 않는 모바일 앱을
생각하기 어려워졌습니다. 네트워크와 연결된 앱을 제작할 때, 결국에는 제법 괜찮은 JSON을
사용하게 되어있습니다.

This guide looks into ways of using JSON with Flutter.
It covers which JSON solution to use in different scenarios, and why.

{{site.alert.info}}
  **Terminology:** _Encoding_ and _serialization_ are the same
  thing&mdash;turning a data structure into a string.
  _Decoding_ and _deserialization_ are the
  opposite process&mdash;turning a string into a data structure.
  However, _serialization_ also commonly refers to the entire process of
  translating data structures to and from a more easily readable format.

  To avoid confusion, this doc uses "serialization" when referring to the
  overall process, and "encoding" and "decoding" when specifically
  referring to those processes.
{{site.alert.end}}

## 어떤 JSON 직렬화 방법이 나에게 적절할까요?

본 글에서는 JSON을 다루는 두 가지 일반적인 방법을 소개합니다:

* 일반 직렬화
* 코드 생성을 이용한 자동화된 직렬화

Different projects come with different complexities and use cases.
For smaller proof-of-concept projects or quick prototypes,
using code generators might be overkill.
For apps with several JSON models with more complexity,
encoding by hand can quickly become tedious, repetitive,
and lend itself to many small errors.

### 소규모 프로젝트에는 일반 직렬화를 사용하세요

Manual JSON decoding refers to using the built-in JSON decoder in
`dart:convert`. It involves passing the raw JSON string to the `jsonDecode()`
function, and then looking up the values you need in the resulting
`Map<String, dynamic>`.
It has no external dependencies or particular setup process,
and it's good for a quick proof of concept.

이러한 방법은 프로젝트가 커지게되면 적합하지 않습니다.
디코딩 로직을 손수 작성하면, 관리하기 어려워질 수 있고 오류 발생 가능성이 커질 수 있습니다.
만약 존재하지 않는 JSON 필드에 접근하는 실수를 한다면, 코드가 런타임 도중에 에러가 발생 할 수도 있습니다.

If you do not have many JSON models in your project and are looking to test a
concept quickly, manual serialization might be the way you want to start.
For an example of manual encoding, see
[Serializing JSON manually using dart:convert][].

### 중대형 프로젝트에는 코드 생성을 사용하세요

JSON serialization with code generation means having an external library
generate the encoding boilerplate for you. After some initial setup,
you run a file watcher that generates the code from your model classes.
For example, [`json_serializable`][] and [`built_value`][] are these
kinds of libraries.

이러한 방법은 큰 프로젝트에 대해 잘 대응합니다. 손수 작성한 보일러 플레이트 코드가 필요하지 않고,
JSON 필드에 접근할 때 발생하는 실수를 컴파일 타임에 잡아냅니다. 코드 생성의 단점은 초기 작업이
필요하다는 것입니다. 또한, 생성된 소스 파일들은 프로젝트 탐색기를 어수선하게 만들 수도 있습니다.

You might want to use generated code for JSON serialization when you have a
medium or a larger project. To see an example of code generation based JSON
encoding, see [Serializing JSON using code generation libraries][].

## Flutter에서 GSON/<wbr>Jackson/<wbr>Moshi 와 같은 라이브러리가 있나요?

요약하자면, 없습니다.

이러한 라이브러리들은 런타임 [리플렉션][]을 요구하는데, Flutter에서는 비활성화가 되어있습니다.
런타임 리플렉션은 [Tree shaking][]을 방해하는데, Dart가 꽤 오랫동안 지원했던 기능입니다.
Tree shaking으로 릴리즈 빌드에서 사용하지 않는 코드를 "제거할 수 있습니다". Tree shaking을 통해
상당히 앱의 크기를 최적화시켜줄 수 있습니다.

리플렉션은 기본적으로 코드를 암시적으로 사용되게 만들기에 Tree shaking을 어렵게 만듭니다. 그래서 런타임 상에서
어떤 부분이 쓰이는지 알 수 없어지고, 중복되는 코드를 제거하기 어렵게 만듭니다.
리플렉션을 사용하면 앱 크기를 최적화하기가 어려워집니다.

Although you cannot use runtime reflection with Flutter, some libraries give
you similarly easy-to-use APIs but are based on code generation instead. This
approach is covered in more detail in the [code generation
libraries](#code-generation) section.

<a name="manual-encoding"></a>
## dart:convert를 이용해 수동으로 JSON을 직렬화하기

Basic JSON serialization in Flutter is very simple. Flutter has a built-in
`dart:convert` library that includes a straightforward JSON encoder and
decoder.

The following sample JSON implements a simple user model.

```json
{
  "name": "홍길동",
  "email": "gildong@example.com"
}
```

With `dart:convert`,
you can serialize this JSON model in two ways.

### 인라인에서 JSON 직렬화

By looking at the [`dart:convert`][] documentation,
you'll see that you can decode the JSON by calling the
`jsonDecode()` function, with the JSON string as the method argument.

<!-- skip -->
```dart
Map<String, dynamic> user = jsonDecode(jsonString);

print('안녕하세요, ${user['name']}님!');
print('${user['email']}으로 인증 링크를 보냈습니다.');
```

안타깝게도, `jsonDecode()`는 `Map<String, dynamic>`을 돌려주어,
런타임 이전까지는 값의 자료형을 알 수 없게 됩니다. 이런 접근 방식을 사용하면,
정적 타입 언어의 기능인 타입 안전성, 자동완성, 그리고 가장 중요한 컴파일 타임 오류를 사용할 수 없게 됩니다.
이로 인해 코드는 순식간에 오류가 발생하기 쉬운 환경에 처하게 됩니다.

예를 들어 `name`이나 `email` 필드에 접근하려 할 때, 오타가 생길 수 있습니다. 
JSON이 map 자료구조로 변환되기 때문에 컴파일러가 오타를 탐지할 수 없습니다.

### 모델 클래스에서 JSON 직렬화

평범한 모델 클래스를 사용하여 앞서 말한 문제를 해결할 수 있습니다. 
이번 예제에서는 `User` 모델 클래스를 사용해보겠습니다.
`User` 클래스 내에는:

* map 구조에서 새로운 `User` 객체를 생성하기 위한 생성자인 `User.fromJson()` 생성자
* `User` 객체를 map 구조로 변환하기 위한 메서드인 `toJson()` 메서드

가 있습니다.

이 방법으로, _호출 코드_ 에서 `name`과 `email` 필드에 대해 타입 안전성, 자동완성, 
컴파일 타임 예외처리가 가능하게 됩니다. 만약 필드를 `String` 형 대신에 `int` 형으로
취급한다던가, 그런 오타를 내면 런타임 오류 대신 컴파일조차 되지 않을 것입니다.

**user.dart**

<!-- skip -->
```dart
class User {
  final String name;
  final String email;

  User(this.name, this.email);

  User.fromJson(Map<String, dynamic> json)
      : name = json['name'],
        email = json['email'];

  Map<String, dynamic> toJson() =>
    {
      'name': name,
      'email': email,
    };
}
```

디코딩 로직의 책임이 이제 모델 내부로 옮겨졌기에, 아래의 새로운 방법으로 유저를 쉽게 디코드할 수 있습니다.

<!-- skip -->
```dart
Map userMap = jsonDecode(jsonString);
var user = User.fromJson(userMap);

print('안녕하세요, ${user.name}님!');
print('${user.email}으로 인증 링크를 보냈습니다.');
```

유저를 인코딩하기 위해서는 `jsonEncode()` 함수에 `User` 객체를 넘겨줍니다.
인코딩을 `toJson()` 메서드 대신 `jsonEncode()`가 해주므로, 메서드를 호출할 필요는 없습니다.

<!-- skip -->
```dart
String json = jsonEncode(user);
```

이 방법으로는 호출 코드에서 JSON 직렬화에 대해 걱정하지 않아도 됩니다. 하지만,
모델 클래스는 아직 직렬화에 대해 견고해야 합니다. 프로덕션 앱에서도 직렬화가 제대로 동작하는지
보장되어야 하겠죠. 실제로는, `User.fromJson()`과 `User.toJson()` 메서드 모두
올바른 동작인지 검증하기 위해 유닛 테스트를 거쳐야 합니다.

{{site.alert.info}}
  The cookbook contains [a more comprehensive worked example of using
  JSON model classes][json background parsing], using an isolate to parse
  the JSON file on a background thread. This approach is ideal if you
  need your app to remain responsive while the JSON file is being
  decoded.
{{site.alert.end}}

However, real-world scenarios are not always that simple.
Sometimes JSON API responses are more complex, for example since they 
contain nested JSON objects that must be parsed through their own model
class.

만약 JSON 인코딩과 디코딩을 처리해주는 무언가가 있으면 정말 좋겠지요. 운 좋게도 있습니다!

<a name="code-generation"></a>
## 코드 생성 라이브러리를 통한 JSON 직렬화

Although there are other libraries available, this guide uses
[`json_serializable`][], an automated source code generator that
generates the JSON serialization boilerplate for you.

{{site.alert.info}}
  **Choosing a library:**
  You might have noticed two [Flutter Favorite][] packages
  on pub.dev that generate JSON serialization code,
  [`json_serializable`][] and [`built_value`][].
  How do you choose between these packages?
  The `json_serializable` package allows you to make regular
  classes serializable by using annotations, 
  whereas the `built_value` package provides a higher-level way
  of defining immutable value classes that can also be
  serialized to JSON.
{{site.alert.end}}

직렬화 코드가 더는 손수 작성되지 않고, 수동적으로 관리되지 않기에, 런타임 때 JSON 직렬화 오류의
위험을 최소화할 수 있습니다.

### 프로젝트에서 json_serializable 설정하기

프로젝트에 `json_serializable`를 포함하기 위해, 일반 의존성 한 개와 _개발 의존성_ 두 개가 필요합니다.
간단히 말하자면,  _개발 의존성_ 은 앱 소스 코드에 포함되지 않고 오직 개발 환경에서만 사용되는 의존성입니다.

The latest versions of these required dependencies can be seen by
following the [pubspec file][] in the JSON serializable example.

**pubspec.yaml**

```yaml
dependencies:
  # Your other regular dependencies here
  json_annotation: <latest_version>

dev_dependencies:
  # Your other dev_dependencies here
  build_runner: <latest_version>
  json_serializable: <latest_version>
```

Run `flutter pub get` inside your project root folder
(or click **Packages get** in your editor)
to make these new dependencies available in your project.

### json_serializable 의 방법으로 모델 클래스를 생성하기

The following shows how to convert the `User` class to a
`json_serializable` class. For the sake of simplicity,
this code uses the simplified JSON model
from the previous samples.

**user.dart**

<!-- skip -->
{% prettify dart %}
import 'package:json_annotation/json_annotation.dart';

/// 이 구문은 `User` 클래스가 생성된 파일의 private 멤버들을
/// 접근할 수 있도록 해줍니다. 여기에는 *.g.dart 형식이 들어갑니다.
/// * 에는 소스 파일의 이름이 들어갑니다.
part '[[highlight]]user[[/highlight]].g.dart';

/// 코드 생성기에 이 클래스가 JSON 직렬화 로직이 만들어져야 한다고 알려주는 어노테이션입니다.
[[highlight]]@JsonSerializable()[[/highlight]]

class User {
  User(this.name, this.email);

  String name;
  String email;

  /// map에서 새로운 User 인스턴스를 생성하기 위해 필요한 팩토리 생성자입니다.
  /// 생성된 `_$UserFromJson()` 생성자에게 map을 전달해줍니다.
  /// 생성자의 이름은 클래스의 이름을 따릅니다. 본 예제의 경우 User를 따릅니다.
  factory User.fromJson(Map<String, dynamic> json) => _$[[highlight]]User[[/highlight]]FromJson(json);

  /// `toJson`은 클래스가 JSON 인코딩의 지원을 선언하는 규칙입니다.
  /// 이의 구현은 생성된 private 헬퍼 메서드 `_$UserToJson`을 단순히 호출합니다.
  Map<String, dynamic> toJson() => _$[[highlight]]User[[/highlight]]ToJson(this);
}
{% endprettify %}

이 작업으로 소스 코드 생성기는 인코딩을 위한 코드와, JSON으로 부터 `name`과 `email` 필드를
디코딩하는 코드를 생성해줍니다.

If needed, it is also easy to customize the naming strategy.
For example, if the API returns objects with _snake\_case_,
and you want to use _lowerCamelCase_ in your models,
you can use the `@JsonKey` annotation with a name parameter:

<!-- skip -->
```dart
/// json_serializable 에게 "registration_date_millis" 는
/// 이 속성과 매핑되어야 한다고 알려줍니다.
@JsonKey(name: 'registration_date_millis')
final int registrationDateMillis;
```

### 코드 생성 유틸리티 실행하기

When creating `json_serializable` classes the first time,
you'll get errors similar to what is shown in the image below.

![아직 모델 클래스를 위해 생성된 코드가 존재하지 않을 때 뜨는 IDE 경고.](/images/json/ide_warning.png){:.mw-100}

이 에러들은 완전 정상이고 그저 모델 클래스를 위해 생성된 코드들이 아직 존재하지 않기에 뜨는 것입니다.
해결하기 위해서는 직렬화 보일러 플레이트를 생성하는 코드 생성기를 실행시키면 됩니다.

코드 생성기를 실행하는 데에는 두 가지 방법이 있습니다.

#### 일회성 코드 생성

`flutter pub run build_runner build`를 프로젝트 루트에서 실행하면, 
모델의 JSON 직렬화 코드를 생성할 수 있습니다. 이 명령은 소스 코드를 살펴보며 관련된 부분을 찾고,
필요한 직렬화 코드를 생성해내는 일회성 빌드입니다.

이 방법도 편리하지만, 모델 클래스에 변화가 있을 때마다 빌드를 실행하지 않으면 더 좋을 것 같네요.

#### 지속적인 코드 생성

_watcher_ 가 소스 코드 생성과정을 좀 더 편리하게 만들어 줍니다. 이는 프로젝트 파일들의 변화를 지켜 보고
자동으로 필요한 파일을 빌드해 줍니다. 프로젝트 루트에서 
`flutter pub run build_runner watch`를 실행하여 watcher를 시작하세요.

watcher를 시작한 후 백그라운드에서 실행되도록 두어도 안전합니다.

### json_serializable 모델 다루기

`json_serializable`으로 JSON 문자열을 디코드하기 위해서
이전 예제 코드에서 변경을 할 필요가 없습니다.

<!-- skip -->
```dart
Map userMap = jsonDecode(jsonString);
var user = User.fromJson(userMap);
```
인코딩도 마찬가지입니다. 호출하는 API는 이전과 같습니다.

<!-- skip -->
```dart
String json = jsonEncode(user);
```

With `json_serializable`,
you can forget any manual JSON serialization in the `User` class.
The source code generator creates a file called `user.g.dart`,
that has all the necessary serialization logic.
You no longer have to write automated tests to ensure
that the serialization works&mdash;it's now
_the library's responsibility_ to make sure the serialization works
appropriately.

## Generating code for nested classes

You might have code that has nested classes within a class.
If that is the case, and you have tried to pass the class in JSON format
as an argument to a service (such as Firebase, for example),
you might have experienced an`Invalid argument` error.

Consider the following `Address` class:

```dart
import 'package:json_annotation/json_annotation.dart';
part 'address.g.dart';

@JsonSerializable()
class Address {
  String street;
  String city;

  Address(this.street, this.city);

  factory Address.fromJson(Map<String, dynamic> json) => _$AddressFromJson(json);
  Map<String, dynamic> toJson() => _$AddressToJson(this);
}
```

The `Address` class is nested inside the `User` class:

```dart
import 'address.dart';
import 'package:json_annotation/json_annotation.dart';
part 'user.g.dart';

@JsonSerializable()
class User {
  String firstName;
  Address address;

  User(this.firstName, this.address);

  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
  Map<String, dynamic> toJson() => _$UserToJson(this);
}
```

Running `flutter pub run build_runner build` in the terminal creates
the `*.g.dart` file, but the private `_$UserToJson()` function
looks something like the following:

```dart
(
Map<String, dynamic> _$UserToJson(User instance) => <String, dynamic>{
  'firstName': instance.firstName,
  'address': instance.address,
};
```

All looks fine now, but if you do a print() on the user object:

```dart
Address address = Address("My st.", "New York");
User user = User("John", address);
print(user.toJson());
```

The result is:

```json
{name: John, address: Instance of 'address'}
```

When what you probably want is output like the following:

```json
{name: John, address: {street: My st., city: New York}}
```

To make this work, pass `explicitToJson: true` in the `@JsonSerializable()`
annotation over the class declaration. The `User` class now looks as follows:

``` dart
import 'address.dart';
import 'package:json_annotation/json_annotation.dart';
part 'user.g.dart';

@JsonSerializable(explicitToJson: true)
class User {
  String firstName;
  Address address;

  User(this.firstName, this.address);

  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
  Map<String, dynamic> toJson() => _$UserToJson(this);
}
```

For more information, see [`explicitToJson`][] in the
[`JsonSerializable`][] class for the [`json_annotation`][] package.

## 더 많은 참고 자료

더 많은 자료를 위해서는 다음의 자료들을 확인해보세요:

* The [`dart:convert`][] and [`JsonCodec`][] documentation
* The [`json_serializable`][] package on pub.dev
* The [`json_serializable` examples][] on GitHub


[`built_value`]: {{site.pub}}/packages/built_value
[`dart:convert`]: {{site.dart.api}}/{{site.dart.sdk.channel}}/dart-convert
[`explicitToJson`]: {{site.pub}}/documentation/json_annotation/latest/json_annotation/JsonSerializable/explicitToJson.html
[Flutter Favorite]: /docs/development/packages-and-plugins/favorites
[json background parsing]: https://flutter.dev/docs/cookbook/networking/background-parsing
[`JsonCodec`]: {{site.dart.api}}/{{site.dart.sdk.channel}}/dart-convert/JsonCodec-class.html
[`JsonSerializable`]: {{site.pub}}/documentation/json_annotation/latest/json_annotation/JsonSerializable-class.html
[`json_annotation`]: {{site.pub}}/packages/json_annotation
[`json_serializable`]: {{site.pub}}/packages/json_serializable
[`json_serializable` examples]: {{site.github}}/dart-lang/json_serializable/blob/master/example/lib/example.dart
[pubspec file]: https://raw.githubusercontent.com/dart-lang/json_serializable/master/example/pubspec.yaml
[reflection]: https://en.wikipedia.org/wiki/Reflection_(computer_programming)
[Serializing JSON manually using dart:convert]: #manual-encoding
[Serializing JSON using code generation libraries]: #code-generation
[tree shaking]: https://en.wikipedia.org/wiki/Tree_shaking
