---
title: JSON과 직렬화
---

어느 시점부터 웹 서버와 통신하지 않거나 구조화된 데이터를 적절하게 보관하지 않는 모바일 앱을
생각하기 어려워졌습니다. 네트워크와 연결된 앱을 제작할 때, 결국에는 제법 괜찮은 JSON을
사용하게 되어있습니다.

이 가이드에서는 Flutter로 JSON을 사용하는 방법에 대해 알아보려 합니다. 각기 다른 
시나리오에서 어떤 방법을 써야 하는지 그리고 그 이유를 다뤄보겠습니다.

{{site.alert.info}}
  **용어:** _인코딩_ 과 _직렬화_ 는 자료 구조를 문자열로 변환하는 서로 같은 일을 합니다. 
  _디코딩_ 과 _역직렬화_ 는 문자열을 자료 구조로 변환하는 정반대 과정을 거칩니다.
  하지만, _직렬화_ 는 일반적으로 자료 구조를 보다 읽기 쉬운 형태로 변환하는 과정을 가리키기도 합니다.

  혼란을 피하기 위해, 이 문서에서는 전반적인 과정을 언급할 때 "직렬화"라 하고,
구체적으로 각각의 과정들을 언급할 때 "인코딩"과 "디코딩"이라 합니다.
{{site.alert.end}}

## 어떤 JSON 직렬화 방법이 나에게 적절할까요?

본 글에서는 JSON을 다루는 두 가지 일반적인 방법을 소개합니다:

* 일반 직렬화
* 코드 생성을 이용한 자동화된 직렬화

각각의 프로젝트들은 서로 다른 복잡도와 사용 사례에 직면합니다. 소규모의 개념 증명 프로젝트와
빠른 변화를 요구하는 프로토타입에 코드 생성기를 이용하는 것은 지나칠 수 있습니다. 더 복잡한 JSON 모델을 다루는 앱은
수작업으로 인코딩을 하기에는 지루하고, 반복적이고, 자그마한 에러들이 많이 일어나게 될 수 있습니다.

### 소규모 프로젝트에는 일반 직렬화를 사용하세요

일반 JSON 디코딩은 `dart:convert`에 탑재되어 있는 JSON 디코더를 사용하는 것을 가리킵니다.
JSON 문자열을 `jsonDecode()` 함수에 전달한 후 결괏값 `Map<String, dynamic>`에서
필요한 값을 참조하면 됩니다.
추가적인 의존성이나 특별한 설치 과정이 없어도 됩니다.
그래서 빠른 개념 증명에 적합합니다.

이러한 방법은 프로젝트가 커지게되면 적합하지 않습니다.
디코딩 로직을 손수 작성하면, 관리하기 어려워질 수 있고 오류 발생 가능성이 커질 수 있습니다.
만약 존재하지 않는 JSON 필드에 접근하는 실수를 한다면, 코드가 런타임 도중에 에러가 발생 할 수도 있습니다.

프로젝트에 JSON 모델이 많이 없고 개념을 빠르게 검증해 보고 싶다면,
일반 직렬화가 시작하기 적합한 방법입니다.
일반 직렬화 예제는 다음 링크를 참고해주세요.
[dart:convert를 이용해 수동으로 JSON을 직렬화하기](#manual-encoding).

### 중대형 프로젝트에는 코드 생성을 사용하세요

코드 생성을 이용한 JSON 직렬화는 외부 라이브러리를 통해 인코딩 보일러 플레이트 코드를 생성하는 것을 의미합니다.
초기 작업 후, 모델 클래스로부터 코드를 생성해주는 파일 감시자를 실행합니다.
예를 들어,
[json_serializable]({{site.pub}}/packages/json_serializable)와
[built_value]({{site.pub}}/packages/built_value)가 대표적인 라이브러리입니다.

이러한 방법은 큰 프로젝트에 대해 잘 대응합니다. 손수 작성한 보일러 플레이트 코드가 필요하지 않고,
JSON 필드에 접근할 때 발생하는 실수를 컴파일 타임에 잡아냅니다. 코드 생성의 단점은 초기 작업이
필요하다는 것입니다. 또한, 생성된 소스 파일들은 프로젝트 탐색기를 어수선하게 만들 수도 있습니다.

중규모나 대규모 프로젝트를 진행할 때 JSON 직렬화를 위해 코드 생성을 사용하고 싶으시면,
다음의 코드 생성 기반 JSON 인코딩 예제를 참고하시길 바랍니다.
[코드 생성 라이브러리를 통한 JSON 직렬화](#code-generation).

## Flutter에서 GSON/<wbr>Jackson/<wbr>Moshi 와 같은 라이브러리가 있나요?

요약하자면, 없습니다.

이러한 라이브러리들은 런타임 [리플렉션][]을 요구하는데, Flutter에서는 비활성화가 되어있습니다.
런타임 리플렉션은 [Tree shaking][]을 방해하는데, Dart가 꽤 오랫동안 지원했던 기능입니다.
Tree shaking으로 릴리즈 빌드에서 사용하지 않는 코드를 "제거할 수 있습니다". Tree shaking을 통해
상당히 앱의 크기를 최적화시켜줄 수 있습니다.

리플렉션은 기본적으로 코드를 암시적으로 사용되게 만들기에 Tree shaking을 어렵게 만듭니다. 그래서 런타임 상에서
어떤 부분이 쓰이는지 알 수 없어지고, 중복되는 코드를 제거하기 어렵게 만듭니다.
리플렉션을 사용하면 앱 크기를 최적화하기가 어려워집니다.

{{site.alert.info}}
  **그럼 dartson 은 어떤가요?**
  [dartson]({{site.pub}}/packages/dartson) 라이브러리는 런타임
  [리플렉션][]을 사용하기에 Flutter에서 호환되지 않습니다.
{{site.alert.end}}

비록 런타임 리플렉션을 Flutter와 사용할 수 없더라도, 몇몇 라이브러리들은
코드 생성을 기반으로 하긴 하지만, 간편하게 사용할 수 있는 유사한 방식의 API를 제공합니다. 이 방법은
[코드 생성 라이브러리](#code-generation) 섹션에서 더 자세히 소개합니다.

<a name="manual-encoding"></a>
## dart:convert를 이용해 수동으로 JSON을 직렬화하기

Flutter에서 기본적인 JSON 인코딩은 매우 간단합니다. Flutter는 간단한 JSON 인코더와 디코더가
내장된 `dart:convert` 라이브러리를 가지고 있습니다.

아래에 간단한 user 모델을 이용한 JSON 예제가 있습니다.

```json
{
  "name": "홍길동",
  "email": "gildong@example.com"
}
```

`dart:convert`를 통해 이 JSON 모델을 두 가지 방법으로 인코딩할 수 있습니다.

### 인라인에서 JSON 직렬화

[dart:convert][]의 문서를 보게 되면,
JSON 문자열을 인자에 넣고
`jsonDecode()` 함수를 호출하여 JSON을 디코드할 수 있다는 걸 알 수 있습니다.

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

하지만, 현실의 시나리오는 그렇게 간단하지 않습니다.
이런 간단한 JSON 응답은 거의 사용하지 않습니다. 일반적으로는 중첩된 JSON 객체들이 사용되죠.

만약 JSON 인코딩과 디코딩을 처리해주는 무언가가 있으면 정말 좋겠지요. 운 좋게도 있습니다!

<a name="code-generation"></a>
## 코드 생성 라이브러리를 통한 JSON 직렬화

여러 다른 라이브러리들이 있으나, 이 가이드에서는 JSON 직렬화 보일러 플레이트 코드를 생성해주는 소스 코드 자동 생성기인,
[json_serializable 패키지]({{site.pub}}/packages/json_serializable)를 사용하겠습니다.

직렬화 코드가 더는 손수 작성되지 않고, 수동적으로 관리되지 않기에, 런타임 때 JSON 직렬화 오류의
위험을 최소화할 수 있습니다.

### 프로젝트에서 json_serializable 설정하기

프로젝트에 `json_serializable`를 포함하기 위해, 일반 의존성 한 개와 _개발 의존성_ 두 개가 필요합니다.
간단히 말하자면,  _개발 의존성_ 은 앱 소스 코드에 포함되지 않고 오직 개발 환경에서만 사용되는 의존성입니다.

필요한 의존성들의 최신 버전은 JSON 직렬화 예제의
[pubspec 파일]()에서 찾아볼 수 있습니다.

**pubspec.yaml**

```yaml
dependencies:
  # 다른 의존성들
  json_annotation: ^2.0.0

dev_dependencies:
  # 다른 개발 의존성들
  build_runner: ^1.0.0
  json_serializable: ^2.0.0
```

프로젝트의 루트 폴더에서 `flutter pub get`을 실행하여 (혹은 에디터에서 **Packages Get** 을
클릭하여) 프로젝트에서 새 의존성들을 사용할 수 있도록 하세요.

### json_serializable 의 방법으로 모델 클래스를 생성하기

다음은 `json_serializable`에 맞게 `User` 클래스를 변경하는 법을 알려줍니다.
간단하게 하기 위해, 아래의 코드는 전 예제에 사용했던 간이 JSON 모델을 사용합니다.

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

필요하다면, 네이밍 전략을 바꾸기도 쉽습니다. 예를 들어, API가 _snake\_case_ 로 반환하는데,
모델에는 _lowerCamelCase_ 를 사용하고 싶으면, name 매개 변수와 함께 `@JsonKey` 어노테이션을 사용하실 수 있습니다.

<!-- skip -->
```dart
/// json_serializable 에게 "registration_date_millis" 는
/// 이 속성과 매핑되어야 한다고 알려줍니다.
@JsonKey(name: 'registration_date_millis')
final int registrationDateMillis;
```

### 코드 생성 유틸리티 실행하기

`json_serializable` 클래스를 처음 생성할 때, 아래의 이미지와 비슷한 에러를 겪게 될 것입니다.

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

`json_serializable` 를 사용하면 `User` 클래스에서의 일반 JSON 직렬화는 잊어도 됩니다.
소스 코드 생성기는 모든 필수 직렬화 로직이 담긴 `user.g.dart`라는 파일을 생성합니다.
더는 직렬화가 동작하는지 검증하기 위해 자동화된 테스트를 작성하지 않아도 됩니다.
&mdash; 이제 직렬화가 제대로 동작하는지 확인하는 것은 _라이브러리의 책임_ 입니다.

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

The Address class is nested inside the `User` class:

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

For more information, see [explicitToJson][] in the 
[JsonSerializable][] class for the [json_annotation][] package.

[explicitToJson]: {{site.pub}}/documentation/json_annotation/latest/json_annotation/JsonSerializable/explicitToJson.html
[JsonSerializable]: {{site.pub}}/documentation/json_annotation/latest/json_annotation/JsonSerializable-class.html
[json_annotation]: {{site.pub}}/packages/json_annotation

## 더 많은 참고 자료

더 많은 자료를 위해서는 다음의 자료들을 확인해보세요:

* [dart:convert][] 와 [JsonCodec][] 의 문서
* [Pub 에 있는 json_serializable 패키지]({{site.pub}}/packages/json_serializable)
* [Github 에 있는 json_serializable 예제](https://github.com/dart-lang/json_serializable/blob/master/example/lib/example.dart)
* [Flutter 에서 dart:mirrors 에 대한 토론](https://github.com/flutter/flutter/issues/1150)

[dart:convert]: {{site.dart.api}}/{{site.dart.sdk.channel}}/dart-convert
[JsonCodec]: {{site.dart.api}}/{{site.dart.sdk.channel}}/dart-convert/JsonCodec-class.html
[리플렉션]: https://ko.wikipedia.org/wiki/%EB%B0%98%EC%98%81_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99)
[Tree shaking]: https://en.wikipedia.org/wiki/Tree_shaking
[the pubspec file]: https://raw.githubusercontent.com/dart-lang/json_serializable/master/example/pubspec.yaml
