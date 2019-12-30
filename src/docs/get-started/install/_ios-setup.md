## iOS 설정

### Xcode 설치

iOS용 Flutter 앱을 개발하기 위해서는 Xcode가 설치된 맥이 필요합니다.

 1. 최신 안정 버전의 Xcode를 설치하세요
    ([웹에서 다운로드][] 혹은 [맥 앱스토어][]).
 1. 아래 명령을 실행하여 새롭게 설치된 버전을 사용하도록 
    Xcode 커멘드라인 도구를 설정하세요

    ```terminal
    $ sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
    $ sudo xcodebuild -runFirstLaunch
    ```

    This is the correct path for most cases,
    when you want to use the latest version of Xcode.
    If you need to use a different version,
    specify that path instead.

 1. Make sure the Xcode license agreement is signed by
    either opening Xcode once and confirming or running
    `sudo xcodebuild -license` from the command line.

Versions older than the latest stable version may still work,
but are not recommended for Flutter development.
Using old versions of Xcode to target bitcode is not
supported, and is likely not to work.

Xcode를 사용하여 iOS 기기 또는 시뮬레이터에서 Flutter 앱을 실행할 수 있습니다.

### iOS 시뮬레이터 설정

iOS 시뮬레이터에서 Flutter 앱을 실행하고 테스트하기 위해서, 
아래 단계를 수행하세요: 

 1. 맥에서 Spotlight를 통해 혹은 아래 명령을 입력하여 시뮬레이터를 찾으세요.

    ```terminal
    $ open -a Simulator
    ```

 2. 시뮬레이터의 **Hardware > Device**메뉴의 설정에서 시뮬레이터가 64비트 기기(iPhone 5s 이상)를 활용하고 있는지 확인하세요.
 3. 기기 화면 크기에 따라서, 시뮬레이터에서 높은 해상도의 iOS 기기가 화면을 초과할 수 있습니다.
    시뮬레이터의 **Window > Scale** 메뉴에서 기기의 스케일을 설정하세요. 

### 간단한 Flutter 앱 만들고 실행하기

첫번째 Flutter 앱을 만들고 설정이 잘 되는지 확인하기 위해, 아래 단계를 수행하세요:

 1. Create a new Flutter app by running the following from the
    command line:

    ```terminal
    $ flutter create my_app
    ```

 2. A `my_app` directory is created, containing Flutter's starter app.
    Enter this directory:

    ```terminal
    $ cd my_app
    ```

 3. To launch the app in the Simulator,
    ensure that the Simulator is running and enter:

    ```terminal
    $ flutter run
    ```

### iOS 기기에 배포

Flutter 앱을 실제 iOS 기기에 배포하려면 
서드 파티 CocoaPods 의존성 관리자와 Apple 개발자 계정이 필요합니다.
또한, Xcode에서 실제 기기 배포 설정을 해야합니다.

 1. 아래 명령어로 CocoaPods을 설치하고 설정하세요:

    ```terminal
    $ sudo gem install cocoapods
    $ pod setup
    ```

 1. Xcode 서명 흐름에 따라 프로젝트를 프로비저닝하세요:

     {: type="a"}
     1. Open the default Xcode workspace in your project by
        running `open ios/Runner.xcworkspace` in a terminal
        window from your Flutter project directory.
     1. Select the device you intend to deploy to in the device
        drop-down menu next to the run button.
     1. Select the `Runner` project in the left navigation panel.
     1. In the `Runner` target settings page,
        make sure your Development Team is selected.
        The UI varies depending on your version of Xcode.
        * For Xcode 10, look under **General > Signing > Team**.
        * For Xcode 11 and newer, look under
        **Signing & Capabilities > Team**.

        When you select a team,
        Xcode creates and downloads a Development Certificate,
        registers your device with your account,
        and creates and downloads a provisioning profile (if needed).

        * To start your first iOS development project,
          you might need to sign into
          Xcode with your Apple ID. ![Xcode account add][]{:.mw-100}
          Development and testing is supported for any Apple ID.
          Enrolling in the Apple Developer Program is required to
          distribute your app to the App Store.
          For details about membership types,
          see [Choosing a Membership][].

        * The first time you use an attached physical device for iOS
          development, you need to trust both your Mac and the
          Development Certificate on that device.
          Select `Trust` in the dialog prompt when
          first connecting the iOS device to your Mac.

          ![Trust Mac][]{:.mw-100}

          다음으로, iOS 기기의 설정으로 가서, 
          **General > Device Management**로 이동하여 
          인증서 신뢰하기를 선택하세요.  

        * Xcode에서 자동 서명이 실패한다면, 
          프로젝트의 **General > Identity > Bundle Identifier**가 고유한지 확인해보세요. 
          ![Check the app's Bundle ID][]{:.mw-100}

 1. `flutter run` 명령으로 앱을 시작하세요.

[Check the app's Bundle ID]: /images/setup/xcode-unique-bundle-id.png
[맴버십 선택]: https://developer.apple.com/support/compare-memberships
[맥 앱스토어]: https://itunes.apple.com/kr/app/xcode/id497799835
[Trust Mac]: /images/setup/trust-computer.png
[웹에서 다운로드]: https://developer.apple.com/xcode/
[Xcode account add]: /images/setup/xcode-account.png
