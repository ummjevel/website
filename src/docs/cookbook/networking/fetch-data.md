---
title: 인터넷에서 데이터 가져오기
prev:
  title: 새로운 화면으로 데이터 보내기
  path: /docs/cookbook/navigation/passing-data
next:
  title: 인증된 요청 생성하기
  path: /docs/cookbook/networking/authenticated-requests
---

인터넷을 통해 데이터를 가져오는 것은 대부분의 앱에서 필수적입니다. 다행스럽게도, Dart와 Flutter는
이러한 유형의 작업을 위해 도구를 제공합니다.
여기서는 아래와 같은 단계로 진행합니다:

  1. `http` 패키지 추가하기.
  2. `http` 패키지를 사용하여 네트워크 요청 생성하기.
  3. 응답 정보를 커스텀 Dart 객체로 변환하기.
  4. 가져온 데이터를 Flutter에 보여주기.

## 1. `http` 패키지 추가하기

[`http`]({{site.pub-pkg}}/http) 패키지를 사용하면 인터넷으로부터
데이터를 손쉽게 가져올 수 있습니다.

The [`http`][] package provides the
simplest way to fetch data from the internet.

To install the `http` package, add it to the dependencies section
of the `pubspec.yaml`. You can find the latest version of the
[http package][] the pub.dev.

```yaml
dependencies:
  http: <latest_version>
```

Import the http package.

<!-- skip -->
```dart
import 'package:http/http.dart' as http;
```

## 2. 네트워크 요청하기 

In this example, fetch a sample post from the
[JSONPlaceholder][] using the [http.get()][] method.

<!-- skip -->
```dart
Future<http.Response> fetchPost() {
  return http.get('https://jsonplaceholder.typicode.com/posts/1');
}
```

`http.get()` 메서드는 `Response`를 포함하고 있는 `Future`를 반환합니다.

* [`Future`][] is a core Dart class for working with
  async operations. A Future object represents a potential
  value or error that will be available at some time in the future.
* The `http.Response` class contains the data received from a successful
  http call.

## 3. 응답 정보를 커스텀 Dart 객체로 변환하기

네트워크 요청 생성은 쉽지만 `Future<http.Response>`을 사용하는 것은 편리하지 
않습니다. 이를 좀 더 쉽게 하기 위해 `http.Response`를 Dart 객체로 변환하겠습니다.

### `Post` 클래스를 생성하세요

First, create a `Post` class that contains the data from the
network request. It includes a factory constructor that
creates a `Post` from JSON.

Converting JSON by hand is only one option.
For more information, see the full article on
[JSON and serialization][].

<!-- skip -->
```dart
class Post {
  final int userId;
  final int id;
  final String title;
  final String body;

  Post({this.userId, this.id, this.title, this.body});

  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(
      userId: json['userId'],
      id: json['id'],
      title: json['title'],
      body: json['body'],
    );
  }
}
```

### `http.Response`를 `Post`로 변환하세요

이제 `fetchPost()` 함수가 `Future<Post>`를 반환하도록 수정하겠습니다. 이 작업을 위해
다음의 것들이 필요합니다:

  1. `dart:convert` 패키지를 통해 응답 내용을 JSON `Map`으로 변환하세요.
  2. 만약 서버가 상태 코드 200과 함께 "OK" 응답을 반환하면, factory 메서드 
     `fromJson()`를 사용하여 JSON `Map`을 `Post`로 변환하세요.
  3. 만약 서버가 예상치 못한 응답을 반환하면, 에러를 던지세요.

<!-- skip -->
```dart
Future<Post> fetchPost() async {
  final response =
      await http.get('https://jsonplaceholder.typicode.com/posts/1');

  if (response.statusCode == 200) {
    // 만약 서버가 OK 응답을 반환하면, JSON을 파싱합니다.
    return Post.fromJson(json.decode(response.body));
  } else {
    // 만약 응답이 OK가 아니면, 에러를 던집니다.
    throw Exception('Failed to load post');
  }
}
```

와우! 이제 인터넷에서 Post를 가져올 수 있는 함수가 생겼습니다.

## 4. 데이터 가져오기

## 4. Fetch the data

Call the fetch method in either the
[`initState()`][] or [`didChangeDependencies()`][]
methods.

The `initState()` method is called exactly once and then never again.
If you want to have the option of reloading the API in response to an
[`InheritedWidget`][] changing, put the call into the
`didChangeDependencies()` method. See [`State`][] for more details.  
<!-- skip -->
```dart
class _MyAppState extends State<MyApp> {
  Future<Post> post;

  @override
  void initState() {
    super.initState();
    post = fetchPost();
  }
```

이 Future를 다음 단계에서 활용합니다.

## 5. 데이터 보여주기 

## 5. Display the data
To to display the data on screen, use the
[`FutureBuilder`][] widget.
The `FutureBuilder` widget comes with Flutter and
makes it easy to work with async data sources.

두 가지 파라미터를 넣어야 합니다:

  1. 원하는 `Future`를 넣습니다. 여기서는 `fetchPost()` 함수에서 반환된 future를 넣습니다.
  2. `Future`의 상태(로딩, 성공 혹은 에러)에 따라 Flutter에게 무엇을 랜더링해야 할지 
     알려줄 `builder` 함수를 넣습니다.

<!-- skip -->
```dart
FutureBuilder<Post>(
  future: post,
  builder: (context, snapshot) {
    if (snapshot.hasData) {
      return Text(snapshot.data.title);
    } else if (snapshot.hasError) {
      return Text("${snapshot.error}");
    }

    // 기본적으로 로딩 Spinner를 보여줍니다.
    return CircularProgressIndicator();
  },
);
```

## 5. fetchPost()를 initState()에서 호출하는 이유는?

편리하겠지만, API 요청 코드를 `build()` 메서드 안에 위치시키는 것은 바람직하지
않습니다.

Flutter는 무언가 변경될 때마다 `build()` 메서드를 호출하는데, 이 호출은 놀랄 
만큼 자주 일어납니다. 만약 네트워크 요청 코드를 `build()` 메서드에 그대로 남겨둔다면,
불필요한 API 요청이 아주 많이 발생하고 앱이 느려질 수 있습니다.

## Testing

이 기능을 테스트하는 방법에 대해 더 자세히 알고 싶다면,
아래 문서를 확인하세요:

  * [Introduction to unit testing][]
  * [Mock dependencies using Mockito][]

## 전체 예제

```dart
import 'dart:async';
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

Future<Post> fetchPost() async {
  final response =
      await http.get('https://jsonplaceholder.typicode.com/posts/1');

  if (response.statusCode == 200) {
    // 만약 서버로의 요청이 성공하면, JSON을 파싱합니다.
    return Post.fromJson(json.decode(response.body));
  } else {
    // 만약 요청이 실패하면, 에러를 던집니다.
    throw Exception('Failed to load post');
  }
}

class Post {
  final int userId;
  final int id;
  final String title;
  final String body;

  Post({this.userId, this.id, this.title, this.body});

  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(
      userId: json['userId'],
      id: json['id'],
      title: json['title'],
      body: json['body'],
    );
  }
}

void main() => runApp(MyApp());

class MyApp extends StatefulWidget {
  MyApp({Key key}) : super(key: key);

  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
Future<Post> post;

  @override
  void initState() {
    super.initState();
    post = fetchPost();
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Fetch Data Example',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: Scaffold(
        appBar: AppBar(
          title: Text('Fetch Data Example'),
        ),
        body: Center(
          child: FutureBuilder<Post>(
            future: post,
            builder: (context, snapshot) {
              if (snapshot.hasData) {
                return Text(snapshot.data.title);
              } else if (snapshot.hasError) {
                return Text("${snapshot.error}");
              }

              // 기본적으로 로딩 Spinner를 보여줍니다.
              return CircularProgressIndicator();
            },
          ),
        ),
      ),
    );
  }
}
```


[`didChangeDependencies()`]: {{site.api}}/flutter/widgets/State/didChangeDependencies.html
[`Future`]: {{site.api}}/flutter/dart-async/Future-class.html
[`FutureBuilder`]: {{site.api}}/flutter/widgets/FutureBuilder-class.html
[JSONPlaceholder]: https://jsonplaceholder.typicode.com/
[`http`]: {{site.pub-pkg}}/http
[http.get()]: {{site.pub-api}}/http/latest/http/get.html
[http package]: {{site.pub}}/packages/http#-installing-tab-
[`InheritedWidget`]: {{site.api}}/flutter/widgets/InheritedWidget-class.html
[Introduction to unit testing]: /docs/cookbook/testing/unit/introduction
[`initState()`]: {{site.api}}/flutter/widgets/State/initState.html
[Mock dependencies using Mockito]: /docs/cookbook/testing/unit/mocking
[JSON and serialization]: /docs/development/data-and-backend/json
[`State`]: {{site.api}}/flutter/widgets/State-class.html

