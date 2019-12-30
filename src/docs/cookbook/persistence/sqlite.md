---
title: SQLite에 데이터 저장하기
prev:
  title: WebSocket 사용하기
  path: /docs/cookbook/networking/web-sockets
next:
  title: File 읽고 쓰기
  path: /docs/cookbook/persistence/reading-writing-files
---

로컬 디바이스에 많은 데이터를 저장하고 쿼리를 요청해야 한다면, 로컬 파일이나 키-값 저장소 대신 데이터베이스를
사용해보세요. 일반적으로 데이터베이스는 다른 로컬 솔루션보다 더 빠른 쓰기, 수정, 읽기 성능을 제공합니다.

Flutter apps can make use of the SQLite databases via the
[`sqflite`][] plugin available on pub.
This recipe demonstrates the basics of using `sqflite`
to insert, read, update, and remove data about various Dogs.

If you are new to SQLite and SQL statements, review the
[SQLite Tutorial][] to learn the basics before
completing this recipe.

여기서는 아래와 같은 순서로 진행합니다:

  1. 의존성 추가.
  2. `Dog` 데이터 모델 정의.
  3. 데이터베이스 열기.
  4. `dogs` 테이블 생성.
  5. 데이터베이스에 `Dog` 추가.
  6. Dog 리스트 조회.
  7. 데이터베이스에 있는 `Dog` 수정.
  8. 데이터베이스에서 `Dog` 삭제.

## 1. 의존성 추가

To work with SQLite databases, import the `sqflite` and `path` packages.

  * The `sqflite` package provides classes and functions to
    interact with a SQLite database.
  * The `path` package provides functions to
    define the location for storing the database on disk.

```yaml
dependencies:
  flutter:
    sdk: flutter
  sqflite:
  path:
```

Make sure to import the packages in the file you'll be working in.

<!-- skip -->
```dart
import 'dart:async';

import 'package:path/path.dart';
import 'package:sqflite/sqflite.dart';
```

## 2. Define the Dog data model

Dog 정보를 저장할 테이블을 생성하기 전에, 저장할 데이터 구조를 정의하겠습니다. 
본 예제에서는 세 개의 데이터를 갖는 Dog 클래스를 정의할 것입니다: 
각 강아지는 고유 id와 name, age를 가집니다.

<!-- skip -->
```dart
class Dog {
  final int id;
  final String name;
  final int age;

  Dog({this.id, this.name, this.age});
}
```

## 3. 데이터베이스 열기

Before reading and writing data to the database, open a connection
to the database. This involves two steps:

  1. `path` 패키지의 `path` 함수와 `sqflite` 패키지의 `getDatabasesPath()`를 조합하여 
  데이터베이스 파일을 위한 경로를 정의하세요. 
  2. `sqflite`의 `openDatabase()` 함수를 사용하여 데이터베이스를 여세요.

<!-- skip -->
```dart
// 데이터베이스를 열고 참조 값을 얻습니다.
final Future<Database> database = openDatabase(
  // 데이터베이스 경로를 지정합니다. 참고: `path` 패키지의 `join` 함수를 사용하는 것이
  // 각 플랫폼 별로 경로가 제대로 생성됐는지 보장할 수 있는 가장 좋은 방법입니다.
  join(await getDatabasesPath(), 'doggie_database.db'),
);
```

## 4. `dogs` 테이블 생성

다음으로, 다양한 강아지 정보를 저장할 테이블을 만들겠습니다. `dogs` 테이블을 생성하세요. 이 경우,
각 `Dog`는 `id`, `name`, `age`를 갖고 있습니다. 그리고 이것들은 `dogs` 테이블에서 세 개의 
컬럼으로 표현될 것입니다.

  1. `id`는 Dart의 `int` 타입이며, SQLite에는 `INTEGER` 타입으로 저장됩니다. `id`를 
  테이블의 기본키로 사용하는 것은 쿼리, 수정 성능을 높이기 위한 좋은 접근법입니다.
  2. `name`은 Dart의 `String` 타입이며, SQLite에는 `TEXT` 타입으로 저장됩니다.
  3. `age` 역시 Dart의 `int` 타입이므로, SQLite에는 `INTEGER` 타입으로 저장됩니다.

For more information about the available Datatypes that can be stored in a
SQLite database, see the [official SQLite Datatypes documentation][].

<!-- skip -->
```dart
final Future<Database> database = openDatabase(
  // Set the path to the database.
  join(await getDatabasesPath(), 'doggie_database.db'),
  // 데이터베이스가 처음 생성될 때, dog를 저장하기 위한 테이블을 생성합니다.
  onCreate: (db, version) {
    // 데이터베이스에 CREATE TABLE 수행
    return db.execute(
      "CREATE TABLE dogs(id INTEGER PRIMARY KEY, name TEXT, age INTEGER)",
    );
  },
  // 버전을 설정하세요. onCreate 함수에서 수행되며 데이터베이스 업그레이드와 다운그레이드를 
  // 수행하기 위한 경로를 제공합니다.
  version: 1,
);
```

## 5. 데이터베이스에 Dog 추가

Now that you have a database with a table suitable for storing information
about various dogs, it's time to read and write data.

먼저, `dogs` 테이블에 `Dog`를 추가해보겠습니다. 다음 두 단계를 수행하세요:

1. Convert the `Dog` into a `Map`
2. Use the [`insert()`][] method to store the
   `Map` in the `dogs` table.

<!-- skip -->
```dart
// Dog 클래스에 `toMap` 메서드를 추가하세요
class Dog {
  final int id;
  final String name;
  final int age;

  Dog({this.id, this.name, this.age});

  // Convert a Dog into a Map. The keys must correspond to the names of the
  // columns in the database.
  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'name': name,
      'age': age,
    };
  }
}

// 데이터베이스에 dog를 추가하는 함수를 정의합니다.
Future<void> insertDog(Dog dog) async {
  // 데이터베이스 reference를 얻습니다.
  final Database db = await database;

  // Insert the Dog into the correct table. You might also specify the
  // `conflictAlgorithm` to use in case the same dog is inserted twice.
  //
  // In this case, replace any previous data.
  await db.insert(
    'dogs',
    dog.toMap(),
    conflictAlgorithm: ConflictAlgorithm.replace,
  );
}

// 이제 Dog를 생성하고 dogs 테이블에 추가할 수 있습니다.
final fido = Dog(
  id: 0,
  name: 'Fido',
  age: 35,
);

await insertDog(fido);
```

## 6. Dog 리스트 조회

이제 데이터베이스에 하나의 `Dog` 정보가 저장되어 있으므로, 특정 dog를 조회하거나 모든 dog 리스트를
조회할 수 있습니다. 다음 두 단계로 수행할 수 있습니다:

  1. Run a `query` against the `dogs` table. This returns a `List<Map>`.
  2. Convert the `List<Map>` into a `List<Dog>`.

<!-- skip -->
```dart
// dogs 테이블의 모든 dog를 얻는 메서드
Future<List<Dog>> dogs() async {
  // 데이터베이스 reference를 얻습니다.
  final Database db = await database;

  // 모든 Dog를 얻기 위해 테이블에 질의합니다.
  final List<Map<String, dynamic>> maps = await db.query('dogs');

  // List<Map<String, dynamic>를 List<Dog>으로 변환합니다.
  return List.generate(maps.length, (i) {
    return Dog(
      id: maps[i]['id'],
      name: maps[i]['name'],
      age: maps[i]['age'],
    );
  });
}

// 이제, 모든 dog를 얻을 수 있는 위 메서드를 사용할 수 있습니다.
print(await dogs()); // Fido를 포함한 리스트를 출력합니다.
```

## 7. 데이터베이스에 있는 `Dog` 수정

After inserting information into the database,
you might want to update that information at a later time.
You can do this by using the [`update()`][]
method from the `sqflite` library.

다음 두 단계로 수행할 수 있습니다:

  1. Dog를 Map으로 변환하세요.
  2. 올바른 Dog를 수정하기 위해 `where` 절을 사용하세요.

<!-- skip -->
```dart
Future<void> updateDog(Dog dog) async {
  // 데이터베이스 reference를 얻습니다.
  final db = await database;

  // 주어진 Dog를 수정합니다.
  await db.update(
    'dogs',
    dog.toMap(),
    // Dog의 id가 일치하는 지 확인합니다.
    where: "id = ?",
    // Dog의 id를 whereArg로 넘겨 SQL injection을 방지합니다.
    whereArgs: [dog.id],
  );
}

// Fido의 나이를 수정합니다.
await updateDog(Dog(
  id: 0,
  name: 'Fido',
  age: 42,
));

// 수정된 결과를 출력합니다.
print(await dogs()); // Fido의 정보를 나이 42와 함께 출력합니다.
```

{{site.alert.warning}}
  Always use `whereArgs` to pass arguments to a `where` statement.
  This helps safeguard against SQL injection attacks.

  `where: "id = ${dog.id}"`와 같은 String interpolation 문법을 사용하지 마세요!
{{site.alert.end}}


## 8. 데이터베이스에서 `Dog` 삭제

In addition to inserting and updating information about Dogs,
you can also remove dogs from the database. To delete data,
use the [`delete()`][] method from the `sqflite` library.

여기에서는 하나의 id 값을 인자로 받아 데이터베이스에서 id 값이 일치하는 
dog를 삭제하는 함수를 만들 것입니다.
삭제될 항목을 제한하기 위해 `where` 절을 제공해야 합니다.

<!-- skip -->
```dart
Future<void> deleteDog(int id) async {
  // 데이터베이스 reference를 얻습니다.
  final db = await database;

  // 데이터베이스에서 Dog를 삭제합니다.
  await db.delete(
    'dogs',
    // 특정 dog를 제거하기 위해 `where` 절을 사용하세요
    where: "id = ?",
    // Dog의 id를 where의 인자로 넘겨 SQL injection을 방지합니다.
    whereArgs: [id],
  );
}
```

## 예제

예제를 실행하기 위해:

  1. 새로운 Flutter 프로젝트를 생성하세요.
  2. `pubspec.yaml`에 `sqflite`와 `path` 패키지를 추가하세요.
  3. 새로운 파일 `lib/db_test.dart`에 아래 코드를 붙여 넣으세요.
  4. `flutter run lib/db_test.dart` 코드를 실행하세요.

```dart
import 'dart:async';

import 'package:path/path.dart';
import 'package:sqflite/sqflite.dart';

void main() async {
  final database = openDatabase(
    // 데이터베이스 경로를 지정합니다. 참고: `path` 패키지의 `join` 함수를 사용하는 것이
    // 각 플랫폼 별로 경로가 제대로 생성됐는지 보장할 수 있는 가장 좋은 방법입니다.
    join(await getDatabasesPath(), 'doggie_database.db'),
    // 데이터베이스가 처음 생성될 때, dog를 저장하기 위한 테이블을 생성합니다.
    onCreate: (db, version) {
      return db.execute(
        "CREATE TABLE dogs(id INTEGER PRIMARY KEY, name TEXT, age INTEGER)",
      );
    },
    // 버전을 설정하세요. onCreate 함수에서 수행되며 데이터베이스 업그레이드와 다운그레이드를 
    // 수행하기 위한 경로를 제공합니다.
    version: 1,
  );

  Future<void> insertDog(Dog dog) async {
    // 데이터베이스 reference를 얻습니다.
    final Database db = await database;

    // Dog를 올바른 테이블에 추가하세요. 또한  
    // `conflictAlgorithm`을 명시할 것입니다. 본 예제에서는
    // 만약 동일한 dog가 여러번 추가되면, 이전 데이터를 덮어쓸 것입니다.
    await db.insert(
      'dogs',
      dog.toMap(),
      conflictAlgorithm: ConflictAlgorithm.replace,
    );
  }

  Future<List<Dog>> dogs() async {
    // 데이터베이스 reference를 얻습니다.
    final Database db = await database;

    // 모든 Dog를 얻기 위해 테이블에 질의합니다.
    final List<Map<String, dynamic>> maps = await db.query('dogs');

    // List<Map<String, dynamic>를 List<Dog>으로 변환합니다.
    return List.generate(maps.length, (i) {
      return Dog(
        id: maps[i]['id'],
        name: maps[i]['name'],
        age: maps[i]['age'],
      );
    });
  }

  Future<void> updateDog(Dog dog) async {
    // 데이터베이스 reference를 얻습니다.
    final db = await database;

    // 주어진 Dog를 수정합니다.
    await db.update(
      'dogs',
      dog.toMap(),
      // Dog의 id가 일치하는 지 확인합니다.
      where: "id = ?",
      // Dog의 id를 whereArg로 넘겨 SQL injection을 방지합니다.
      whereArgs: [dog.id],
    );
  }

  Future<void> deleteDog(int id) async {
    // 데이터베이스 reference를 얻습니다.
    final db = await database;

    // 데이터베이스에서 Dog를 삭제합니다.
    await db.delete(
      'dogs',
      // 특정 dog를 제거하기 위해 `where` 절을 사용하세요
      where: "id = ?",
      // Dog의 id를 where의 인자로 넘겨 SQL injection을 방지합니다.
      whereArgs: [id],
    );
  }

  var fido = Dog(
    id: 0,
    name: 'Fido',
    age: 35,
  );

  // 데이터베이스에 dog를 추가합니다.
  await insertDog(fido);

  // dog 목록을 출력합니다. (지금은 Fido만 존재합니다.)
  print(await dogs());

  // Fido의 나이를 수정한 뒤 데이터베이스에 저장합니다.
  fido = Dog(
    id: fido.id,
    name: fido.name,
    age: fido.age + 7,
  );
  await updateDog(fido);

  // Fido의 수정된 정보를 출력합니다.
  print(await dogs());

  // Fido를 데이터베이스에서 제거합니다.
  await deleteDog(fido.id);

  // dog 목록을 출력합니다. (비어있습니다.)
  print(await dogs());
}

class Dog {
  final int id;
  final String name;
  final int age;

  Dog({this.id, this.name, this.age});

  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'name': name,
      'age': age,
    };
  }

  // 각 dog 정보를 보기 쉽도록 print 문을 사용하여 toString을 구현하세요
  @override
  String toString() {
    return 'Dog{id: $id, name: $name, age: $age}';
  }
}
```


[`delete()`]: {{site.pub-api}}/sqflite/latest/sqlite_api/DatabaseExecutor/delete.html'
[`insert()`]: {{site.pub-api}}/sqflite/latest/sqlite_api/DatabaseExecutor/insert.html
[`sqflite`]: {{site.pub-pkg}}/sqflite
[SQLite Tutorial]: http://www.sqlitetutorial.net/
[official SQLite Datatypes documentation]: https://www.sqlite.org/datatype3.html
[`update()`]: {{site.pub-api}}/sqflite/latest/sqlite_api/DatabaseExecutor/update.html
