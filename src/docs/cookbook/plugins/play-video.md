---
title: 영상 재생 및 일시 중지
prev:
  title: 디스크에 키-값 데이터 저장하기
  path: /docs/cookbook/persistence/key-value
next:
  title: 카메라 사진 찍기
  path: /docs/cookbook/plugins/picture-using-camera
---

Playing videos is a common task in app development,
and Flutter apps are no exception. To play videos,
the Flutter team provides the [`video_player`][] plugin.
You can use the `video_player` plugin to play videos
stored on the file system, as an asset, or from the internet.

On iOS, the `video_player` plugin makes use of
[`AVPlayer`][] to handle playback. On Android,
it uses [`ExoPlayer`][].

본 예제에서는 `video_player` 패키지를 사용하여 인터넷 영상을 스트리밍하고
재생 및 일시 정지와 같은 기본적인 컨트롤을 아래와 같은 순서로 해볼 것입니다.

  1. `video_player` 의존성 추가하기.
  2. 앱에 권한 부여하기.
  3. `VideoPlayerController` 생성 및 초기화하기.
  4. video player 화면에 보여주기.
  5. 영상을 재생 및 일시 중지하기.

## 1. `video_player` 의존성 추가하기

본 예제는 Flutter 플러그인에 의존합니다: `video_player`. 우선 이 라이브러리를
`pubspec.yaml`에 추가합니다.

```yaml
dependencies:
  flutter:
    sdk: flutter
  video_player:
```

## 2. 앱에 권한 부여하기

다음으로, `Android`와 `ios` configuration을 수정하여 
앱이 인터넷 영상을 스트리밍하기 위해 올바른 권한을 가졌는지 확인하세요.

### Android

다음의 권한을 `AndroidManifest.xml` 파일의 `<application>` 바로 다음에 추가하세요.
`AndroidManifest.xml`은 `<projectroot>/android/app/src/main/AndroidManifest.xml`에서
찾을 수 있습니다.

<!-- skip -->
```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    <application ...>

    </application>

    <uses-permission android:name="android.permission.INTERNET"/>
</manifest>
```

### iOS

For iOS, add the following to the `Info.plist` file found at
`<project root>/ios/Runner/Info.plist`.

<!-- skip -->
```xml
<key>NSAppTransportSecurity</key>
<dict>
  <key>NSAllowsArbitraryLoads</key>
  <true/>
</dict>
```

{{site.alert.warning}}
`video_player` 플러그인은 iOS 시뮬레이터에서는 동작하지 않으므로, 영상 재생 테스트는
반드시 실제 iOS 기기에서 해야 합니다.
{{site.alert.end}}

## 3. `VideoPlayerController` 생성 및 초기화하기

이제 `video_player`과 올바른 권한 모두 준비되었고, `VideoPlayerController`를 만들
차례입니다. `VideoPlayerController` 클래스를 통해 다른 여러 유형의 영상에 연결하고
제어할 수 있습니다.

영상을 재생하기 전에, controller를 `초기화`해야 합니다. 이 작업을 통해 영상과 연결하고
영상 제어를 위한 준비 작업을 하게 됩니다.

`VideoPlayerController`를 생성하고 초기화하기 위해 아래와 같이 진행하세요:

  1. Create a `StatefulWidget` with a companion `State` class
  2. Add a variable to the `State` class to store the `VideoPlayerController`
  3. Add a variable to the `State` class to store the `Future` returned from
  `VideoPlayerController.initialize`
  4. Create and initialize the controller in the `initState` method
  5. Dispose of the controller in the `dispose` method

<!-- skip -->
```dart
class VideoPlayerScreen extends StatefulWidget {
  VideoPlayerScreen({Key key}) : super(key: key);

  @override
  _VideoPlayerScreenState createState() => _VideoPlayerScreenState();
}

class _VideoPlayerScreenState extends State<VideoPlayerScreen> {
  VideoPlayerController _controller;
  Future<void> _initializeVideoPlayerFuture;

  @override
  void initState() {
    // VideoPlayerController를 저장하기 위한 변수를 만듭니다. VideoPlayerController는
    // asset, 파일, 인터넷 등의 영상들을 제어하기 위해 다양한 생성자를 제공합니다.
    _controller = VideoPlayerController.network(
      'https://flutter.github.io/assets-for-api-docs/assets/videos/butterfly.mp4',
    );

    _initializeVideoPlayerFuture = _controller.initialize();

    super.initState();
  }

  @override
  void dispose() {
    // 자원을 반환하기 위해 VideoPlayerController를 dispose 시키세요.
    _controller.dispose();

    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // 다음 단계에서 영상을 보여줄 것입니다.
  }
}
```

## 4. video player 화면에 보여주기

Now, display the video. The `video_player` plugin provides the
[`VideoPlayer`][] widget to display the video initialized by
the `VideoPlayerController`.
By default, the `VideoPlayer` widget takes up as much space as possible.
This often isn't ideal for videos because they are meant
to be displayed in a specific aspect ratio, such as 16x9 or 4x3.

Therefore, wrap the `VideoPlayer` widget in an [`AspectRatio`][]
widget to ensure that the video has the correct proportions.

추가로 `_initializeVideoPlayerFuture()`가 완료된 후에 `VideoPlayer`를 보여줘야 합니다.
`FutureBuilder`를 사용하여 초기화가 진행되는 동안 로딩 스피너를 보여줄 수 있습니다. 참고:
컨트롤러 초기화가 영상을 재생시키지는 않습니다.

<!-- skip -->
```dart
// VideoPlayerController가 초기화를 진행하는 동안 로딩 스피너를 보여주기 위해
// FutureBuilder를 사용합니다.
FutureBuilder(
  future: _initializeVideoPlayerFuture,
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.done) {
      // 만약 VideoPlayerController 초기화가 끝나면, 제공된 데이터를 사용하여
      // VideoPlayer의 종횡비를 제한하세요.
      return AspectRatio(
        aspectRatio: _controller.value.aspectRatio,
        // 영상을 보여주기 위해 VideoPlayer 위젯을 사용합니다.
        child: VideoPlayer(_controller),
      );
    } else {
      // 만약 VideoPlayerController가 여전히 초기화 중이라면, 
      // 로딩 스피너를 보여줍니다.
      return Center(child: CircularProgressIndicator());
    }
  },
)
```

## 5. 영상을 재생 및 일시 중지하기

By default, the video starts in a paused state. To begin playback,
call the [`play()`][] method provided by the `VideoPlayerController`.
To pause playback, call the [`pause()`][] method.

For this example,
add a `FloatingActionButton` to your app that displays a play
or pause icon depending on the situation.
When the user taps the button,
play the video if it's currently paused,
or pause the video if it's playing.

<!-- skip -->
```dart
FloatingActionButton(
  onPressed: () {
    // 재생/일시 중지 기능을 `setState` 호출로 감쌉니다. 이렇게 함으로써 올바른 아이콘이
    // 보여집니다.
    setState(() {
      // 영상이 재생 중이라면, 일시 중지 시킵니다.
      if (_controller.value.isPlaying) {
        _controller.pause();
      } else {
        // 만약 영상이 일시 중지 상태였다면, 재생합니다.
        _controller.play();
      }
    });
  },
  // 플레이어의 상태에 따라 올바른 아이콘을 보여줍니다.
  child: Icon(
    _controller.value.isPlaying ? Icons.pause : Icons.play_arrow,
  ),
)
```

## Complete example

```dart
import 'dart:async';

import 'package:flutter/material.dart';
import 'package:video_player/video_player.dart';

void main() => runApp(VideoPlayerApp());

class VideoPlayerApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Video Player Demo',
      home: VideoPlayerScreen(),
    );
  }
}

class VideoPlayerScreen extends StatefulWidget {
  VideoPlayerScreen({Key key}) : super(key: key);

  @override
  _VideoPlayerScreenState createState() => _VideoPlayerScreenState();
}

class _VideoPlayerScreenState extends State<VideoPlayerScreen> {
  VideoPlayerController _controller;
  Future<void> _initializeVideoPlayerFuture;

  @override
  void initState() {
    // VideoPlayerController를 저장하기 위한 변수를 만듭니다. VideoPlayerController는
    // asset, 파일, 인터넷 등의 영상들을 제어하기 위해 다양한 생성자를 제공합니다.
    _controller = VideoPlayerController.network(
      'https://flutter.github.io/assets-for-api-docs/assets/videos/butterfly.mp4',
    );

    // 컨트롤러를 초기화하고 추후 사용하기 위해 Future를 변수에 할당합니다.
    _initializeVideoPlayerFuture = _controller.initialize();

    // 비디오를 반복 재생하기 위해 컨트롤러를 사용합니다.
    _controller.setLooping(true);

    super.initState();
  }

  @override
  void dispose() {
    // 자원을 반환하기 위해 VideoPlayerController를 dispose 시키세요.
    _controller.dispose();

    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Butterfly Video'),
      ),
      // VideoPlayerController가 초기화를 진행하는 동안 로딩 스피너를 보여주기 위해
      // FutureBuilder를 사용합니다.
      body: FutureBuilder(
        future: _initializeVideoPlayerFuture,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.done) {
            // 만약 VideoPlayerController 초기화가 끝나면, 제공된 데이터를 사용하여
            // VideoPlayer의 종횡비를 제한하세요.
            return AspectRatio(
              aspectRatio: _controller.value.aspectRatio,
              // 영상을 보여주기 위해 VideoPlayer 위젯을 사용합니다.
              child: VideoPlayer(_controller),
            );
          } else {
            // 만약 VideoPlayerController가 여전히 초기화 중이라면, 
            // 로딩 스피너를 보여줍니다.
            return Center(child: CircularProgressIndicator());
          }
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          // 재생/일시 중지 기능을 `setState` 호출로 감쌉니다. 이렇게 함으로써 올바른 아이콘이
          // 보여집니다.
          setState(() {
            // 영상이 재생 중이라면, 일시 중지 시킵니다.
            if (_controller.value.isPlaying) {
              _controller.pause();
            } else {
              // 만약 영상이 일시 중지 상태였다면, 재생합니다.
              _controller.play();
            }
          });
        },
        // 플레이어의 상태에 따라 올바른 아이콘을 보여줍니다.
        child: Icon(
          _controller.value.isPlaying ? Icons.pause : Icons.play_arrow,
        ),
      ), // 이 마지막 콤마는 build 메서드에 자동 서식이 잘 적용될 수 있도록 도와줍니다.
    );
  }
}
```


[`AspectRatio`]: {{site.api}}/flutter/widgets/AspectRatio-class.html
[`AVPlayer`]: https://developer.apple.com/documentation/avfoundation/avplayer
[`ExoPlayer`]: https://google.github.io/ExoPlayer/
[`pause()`]: {{site.pub-api}}/video_player/latest/video_player/VideoPlayerController/pause.html
[`play()`]: {{site.pub-api}}/video_player/latest/video_player/VideoPlayerController/play.html
[`video_player`]: {{site.pub-pkg}}/video_player
[`VideoPlayer`]: {{site.pub-api}}/video_player/latest/video_player/VideoPlayer-class.html
