---
title: 카메라 사진 찍기
prev:
  title: 영상 재생 및 일시 중지
  path: /docs/cookbook/plugins/play-video
next:
  title: 통합 테스팅 소개
  path: /docs/cookbook/testing/integration/introduction
---

Many apps require working with the device's cameras to
take photos and videos.  Flutter provides the [`camera`][] plugin
for this purpose. The `camera` plugin provides tools to get a list of the
available cameras, display a preview coming from a specific camera,
and take photos or videos.

This recipe demonstrates how to use the `camera` plugin to display a preview,
take a photo, and display it using the following steps:

  1. 필요한 의존성 추가하기.
  2. 이용가능한 카메라 목록 가져오기.
  3. `CameraController` 생성하고 초기화하기.
  4. `CameraPreview`를 사용하여 카메라 피드 보여주기.
  5. `CameraController`를 사용하여 사진 찍기.
  6. 촬영한 사진을 `Image` 위젯으로 보여주기.

## 1. 필요한 의존성 추가하기

본 예제를 수행하기 위해, 아래 세 개의 의존성을 추가해야 합니다:

[`camera`][]
: Provides tools to work with the cameras on the device.

[`path_provider`][]
: Finds the correct paths to store images.

[`path`][]
: Creates paths that work on any platform.

```yaml
dependencies:
  flutter:
    sdk: flutter
  camera:
  path_provider:
  path:
```
{{site.alert.tip}}
  - For android, You must have to update `minSdkVersion` to 21 (or higher).
{{site.alert.end}}

## 2. 이용가능한 카메라 목록 가져오기

이제 `camera` 플러그인을 사용하여 이용가능한 카메라 목록을 받아오세요.

<!-- skip -->
```dart
// 디바이스에서 이용가능한 카메라 목록을 받아옵니다.
final cameras = await availableCameras();

// Get a specific camera from the list of available cameras.
final firstCamera = cameras.first;
```

## 3. `CameraController` 생성하고 초기화하기

카메라가 준비되었으면, 아래 단계를 따라 
`CameraController`를 생성하고 초기화할 차례입니다. 
이 과정을 통해 디바이스의 카메라와 연결하여 
카메라를 제어하고 카메라 피드의 프리뷰를 보여줄 수 있게 됩니다. 

  1. Create a `StatefulWidget` with a companion `State` class.
  2. Add a variable to the `State` class to store the `CameraController`.
  3. Add a variable to the `State` class to store the `Future`
     returned from `CameraController.initialize()`.
  4. Create and initialize the controller in the `initState()` method.
  5. Dispose of the controller in the `dispose()` method.

<!-- skip -->
```dart
// A screen that takes in a list of cameras and the Directory to store images.
class TakePictureScreen extends StatefulWidget {
  final CameraDescription camera;

  const TakePictureScreen({
    Key key,
    @required this.camera,
  }) : super(key: key);

  @override
  TakePictureScreenState createState() => TakePictureScreenState();
}

class TakePictureScreenState extends State<TakePictureScreen> {
  // CameraController와 Future를 저장하기 위해 두 개의 변수를 state 클래스에 
  // 정의합니다.
  CameraController _controller;
  Future<void> _initializeControllerFuture;

  @override
  void initState() {
    super.initState();
    // 카메라의 현재 출력물을 보여주기 위해 
    // CameraController를 생성합니다.
    _controller = CameraController(
      // Get a specific camera from the list of available cameras.
      // 이용 가능한 카메라 목록에서 특정 카메라를 가져옵니다.
      widget.camera,
      // 적용할 해상도를 지정합니다.
      ResolutionPreset.medium,
    );

    // 다음으로 controller를 초기화합니다. 초기화 메서드는 Future를 반환합니다.
    _initializeControllerFuture = _controller.initialize();
  }

  @override
  void dispose() {
    // 위젯의 생명주기 종료시 컨트롤러 역시 해제시켜줍니다.
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // 다음 단계에서 이 부분을 작성하겠습니다.
  }
}
```

{{site.alert.warning}}
  만약 `CameraController`를 초기화하지 않으면, 
  프리뷰를 보여주거나 사진 촬영을 *할 수 없습니다.*
{{site.alert.end}}

## 4. `CameraPreview`를 사용하여 카메라 피드 보여주기

다음으로 `camera` 패키지의 `CameraPreview` 위젯을 사용하여 카메라 피드의 프리뷰를
보여줄 것입니다.

{{site.alert.secondary}}
  **기억하세요** 카메라를 사용하기 전에 컨트롤러의 초기화가 완료될 때까지 
  기다려야 합니다. 그러므로 `CameraPreview`를 
  보여주기 전에 이전 단계에서 만든 `_initializeControllerFuture()`가
  완료될 때까지 기다려야 합니다.
{{site.alert.end}}

Use a [`FutureBuilder`][] for exactly this purpose.

<!-- skip -->
```dart
// 카메라 프리뷰를 보여주기 전에 컨트롤러 초기화를 기다려야 합니다. 컨트롤러 초기화가 
// 완료될 때까지 FutureBuilder를 사용하여 로딩 스피너를 보여주세요.
FutureBuilder<void>(
  future: _initializeControllerFuture,
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.done) {
      // Future가 완료되면, 프리뷰를 보여줍니다.
      return CameraPreview(_controller);
    } else {
      // Otherwise, display a loading indicator.
      // 그렇지 않다면, 진행 표시기를 보여줍니다.
      return Center(child: CircularProgressIndicator());
    }
  },
)
```

## 5. `CameraController`를 사용하여 사진 찍기

You can use the `CameraController` to take pictures using the
[`takePicture()`][] method. In this example,
create a `FloatingActionButton` that takes a picture
using the `CameraController` when a user taps on the button.

촬영한 사진을 저장하기 위해 3 단계가 필요합니다:

  1. Ensure the camera is initialized
  2. Construct a path that defines where the picture should be saved
  3. Use the controller to take a picture and save the result to the path

It is good practice to wrap these operations in a `try / catch` block in order
to handle any errors that might occur.

<!-- skip -->
```dart
FloatingActionButton(
  child: Icon(Icons.camera_alt),
  // onPressed 콜백을 제공합니다.
  onPressed: () async {
    // try / catch 블럭에서 사진을 촬영합니다. 만약 뭔가 잘못된다면 에러에
    // 대응할 수 있습니다.
    try {
      // 카메라 초기화가 완료됐는지 확인합니다.
      await _initializeControllerFuture;

      // path 패키지를 사용하여 이미지가 저장될 경로를 지정합니다.
      final path = join(
        // 본 예제에서는 임시 디렉토리에 이미지를 저장합니다. `path_provider`
        // 플러그인을 사용하여 임시 디렉토리를 찾으세요.
        (await getTemporaryDirectory()).path,
        '${DateTime.now()}.png',
      );

      // 사진 촬영을 시도하고 저장되는 경로를 로그로 남깁니다.
      await _controller.takePicture(path);
    } catch (e) {
      // 만약 에러가 발생하면, 콘솔에 에러 로그를 남깁니다.
      print(e);
    }
  },
)
```
## 6. 촬영한 사진을 `Image` 위젯으로 보여주기

성공적으로 사진을 촬영했다면, `Image` 위젯을 사용하여 저장된 사진을 보여줄 수 있습니다.
이 경우 사진은 디바이스에 파일로 저장되어 있습니다.

그러므로 `Image.file` 생성자에 `File`을 제공해야 합니다. 이전 단계에서 생성한 경로를 
인자로 넘겨 주어 `File` 클래스의 인스턴스를 생성할 수 있습니다.

<!-- skip -->
```dart
Image.file(File('path/to/my/picture.png'))
```

## 완성된 예제

```dart
import 'dart:async';
import 'dart:io';

import 'package:camera/camera.dart';
import 'package:flutter/material.dart';
import 'package:path/path.dart' show join;
import 'package:path_provider/path_provider.dart';

Future<void> main() async {
  // 디바이스에서 이용가능한 카메라 목록을 받아옵니다.
  final cameras = await availableCameras();

  // 이용가능한 카메라 목록에서 특정 카메라를 얻습니다.
  final firstCamera = cameras.first;

  runApp(
    MaterialApp(
      theme: ThemeData.dark(),
      home: TakePictureScreen(
        // 적절한 카메라를 TakePictureScreen 위젯에게 전달합니다.
        camera: firstCamera,
      ),
    ),
  );
}

// 사용자가 주어진 카메라를 사용하여 사진을 찍을 수 있는 화면
class TakePictureScreen extends StatefulWidget {
  final CameraDescription camera;

  const TakePictureScreen({
    Key key,
    @required this.camera,
  }) : super(key: key);

  @override
  TakePictureScreenState createState() => TakePictureScreenState();
}

class TakePictureScreenState extends State<TakePictureScreen> {
  CameraController _controller;
  Future<void> _initializeControllerFuture;

  @override
  void initState() {
    super.initState();
    // 카메라의 현재 출력물을 보여주기 위해 CameraController를 생성합니다.
    _controller = CameraController(
      // 이용 가능한 카메라 목록에서 특정 카메라를 가져옵니다.
      widget.camera,
      // 적용할 해상도를 지정합니다.
      ResolutionPreset.medium,
    );

    // 다음으로 controller를 초기화합니다. 초기화 메서드는 Future를 반환합니다.
    _initializeControllerFuture = _controller.initialize();
  }

  @override
  void dispose() {
    // 위젯의 생명주기 종료시 컨트롤러 역시 해제시켜줍니다.
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Take a picture')),
      // 카메라 프리뷰를 보여주기 전에 컨트롤러 초기화를 기다려야 합니다. 컨트롤러 초기화가 
      // 완료될 때까지 FutureBuilder를 사용하여 로딩 스피너를 보여주세요.
      body: FutureBuilder<void>(
        future: _initializeControllerFuture,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.done) {
            // Future가 완료되면, 프리뷰를 보여줍니다.
            return CameraPreview(_controller);
          } else {
            // 그렇지 않다면, 진행 표시기를 보여줍니다.
            return Center(child: CircularProgressIndicator());
          }
        },
      ),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.camera_alt),
        // onPressed 콜백을 제공합니다.
        onPressed: () async {
          // try / catch 블럭에서 사진을 촬영합니다. 만약 뭔가 잘못된다면 에러에
          // 대응할 수 있습니다.
          try {
            // 카메라 초기화가 완료됐는지 확인합니다.
            await _initializeControllerFuture;

            // Construct the path where the image should be saved using the
            // pattern package.
            final path = join(
              // 본 예제에서는 임시 디렉토리에 이미지를 저장합니다. `path_provider`
              // 플러그인을 사용하여 임시 디렉토리를 찾으세요.
              (await getTemporaryDirectory()).path,
              '${DateTime.now()}.png',
            );

            // 사진 촬영을 시도하고 저장되는 경로를 로그로 남깁니다.
            await _controller.takePicture(path);

            // 사진을 촬영하면, 새로운 화면으로 넘어갑니다.
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) => DisplayPictureScreen(imagePath: path),
              ),
            );
          } catch (e) {
            // 만약 에러가 발생하면, 콘솔에 에러 로그를 남깁니다.
            print(e);
          }
        },
      ),
    );
  }
}

// 사용자가 촬영한 사진을 보여주는 위젯
class DisplayPictureScreen extends StatelessWidget {
  final String imagePath;

  const DisplayPictureScreen({Key key, this.imagePath}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Display the Picture')),
      // 이미지는 디바이스에 파일로 저장됩니다. 이미지를 보여주기 위해 주어진 
      // 경로로 `Image.file`을 생성하세요.
      body: Image.file(File(imagePath)),
    );
  }
}
```


[`camera`]: {{site.pub-pkg}}/camera
[`FutureBuilder`]: {{site.api}}/flutter/widgets/FutureBuilder-class.html
[`path`]: {{site.pub-pkg}}/path
[`path_provider`]: {{site.pub-pkg}}/path_provider
[`takePicture()`]: {{site.pub-api}}/camera/latest/camera/CameraController/takePicture.html
