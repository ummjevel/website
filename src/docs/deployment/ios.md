---
title: iOS 앱 출시 준비하기
short-title: iOS
---

이 가이드는 [앱스토어][appstore]와 [TestFlight][testflight]에 Flutter 앱을 출시하는 단계를 제공합니다.

다트 코드 난독화에 대한 정보를 얻고 싶다면, [Obfuscating Dart Code]({{site.github}}/flutter/flutter/wiki/Obfuscating-Dart-Code)를 참조하세요.

## 들어가며

앱을 배포하기 전에 애플의 [App Store 심사 지침][appreview]을 충족하는지 확인하세요. 

앱스토어에 앱을 등록하기 위해서는 [Apple Developer Program][devprogram]에 가입해야 합니다. 더 다양한 맴버십 옵션을 확인하려면 [멤버십 선택하기][devprogram_membership]에서 볼 수 있습니다.  

## App Store Connect에서 앱 등록하기

[App Store Connect][appstoreconnect] (또는 iTunes Connect)는 앱의 라이프 사이클을 관리할 수 있는 곳입니다. 앱의 이름과 설명을 정하고 스크린샷을 추가하고 가격을 설정하여 앱스토어와 TestFlight에 출시를 관리하세요.

앱을 등록할때는 고유 번들 ID 등록과 App Store Connect에 애플리케이션 레코드 생성을 하는 두 단계가 포함됩니다.

App Store Connect에 관한 자세한 내용은 [App Store Connect][appstoreconnect_guide]를 확인하세요.

### 번들 ID 등록하기

모든 iOS 애플리케이션은 Apple에 등록된 고유값인 번들 ID와 연결 됩니다. 앱의 번들 ID를 등록하려면 다음 단계를 따라 진행하세요.

1. 개발자 계정의 [App IDs][devportal_appids]를 엽니다. 
1. 새로운 번들 ID를 만들려면 **+** 를 클릭하세요.  
1. 앱 이름을 입력하고 **Explicit App ID**를 선택한 후에 번들 ID를 입력하세요.
1. 앱 서비스에서 사용할 서비스를 선택한 다음 **Continue**를 클릭하세요.
1. 다음 페이지에서 입력한 정보를 확인하고 **Register**를 클릭하여 번들 ID를 등록하세요.   

### App Store Connect에서 애플리케이션 레코드 생성하기

다음으로, App Store Connect에 앱을 등록합니다.

1. 웹 브라우저에서 [App Store Connect][appstoreconnect_login]에 접속합니다.
1. App Store Connect 첫 페이지에서 **나의 앱**을 클릭하세요.
1. 왼쪽 상단에 있는 **+** 를 클릭한 다음 **신규 앱**을 선택합니다.   
1. 나타난 폼 양식에 앱의 세부 정보를 입력하세요. 플랫폼에서는 iOS가 체크되어 있어야 합니다. 반면 Flutter는 현재 tvOS를 지원하지 않으므로 그 부분은 체크하지 않습니다. 그리고 **생성**을 클릭하세요.   
1. 애플리케이션 세부 정보로 이동하여 사이드바에 있는 **앱 정보**를 선택합니다.   
1. 일반 정보의 번들 ID에 이전 단계에서 등록한 번들 ID를 선택하세요.   

보다 자세한 내용은 [계정에 앱 추가하기][appstoreconnect_guide_register]를 참조하세요.

## Xcode 프로젝트 설정하기

여기서는 Xcode에서 가장 중요한 설정하는 법을 소개합니다. 자세한 절차 및 설명은 [앱 배포 준비하기]
                                                [distributionguide_config]를 참조하세요.

Xcode에서 설정으로 이동합니다.

1. `ios` 폴더에서 `Runner.xcworkspace`를 실행시켜서 Xcode를 엽니다.
1. 앱 설정을 보려면 Xcode 좌측에서 **Runner**를 선택하세요. 그런 뒤 메인 뷰 좌측의 **Runner** target을 선택합니다. 
1. **General** 탭을 선택하세요.

다음으로, 가장 중요한 설정을 확인합니다.

Identity 설정에서

  * `Display Name:` 홈스크린등에 표시될 앱의 이름입니다.
  * `Bundle Identifier:` App Store Connect에서 등록한 번들 ID입니다.

Signing 설정에서

  * `Automatically manage signing:` Xcode가 앱 서명 및 프로비저닝을 자동으로 관리하는지에 대한 여부입니다. 대부분의 앱은 기본설정인 `true`면 충분합니다. 보다 복잡한 시나리오는 [코드 서명 가이드][codesigning_guide]를 참조하세요.
  * `Team:` Apple Developer 계정에 등록된 팀을 선택하세요. 필요한 경우 **Add Account...** 를 선택한 다음 이 설정을 업데이트하세요.    

Deployment Info 설정에서

  * `Deployment Target:` 앱이 지원할 최소 iOS 버전을 선택합니다. 참고로 Flutter는 iOS 8.0 이상을 지원합니다. 만일 iOS 8에서 사용할 수 없는 API를 사용한다면 이 설정을 업데이트하세요.    

General 탭에서의 설정은 다음과 유사해야 합니다.

![Xcode Project Settings](/images/releaseguide/xcode_settings.png)

앱 서명에 대한 자세한 내용은 [인증서 생성, 내보내기, 삭제][appsigning]를 참조하세요.

## 앱 아이콘 추가하기

Flutter 앱을 만들면 기본 아이콘이 생성됩니다. 이 단계에서는 기본 아이콘을 앱 아이콘으로 바꿉니다.

1. [iOS 앱 아이콘][appicon] 가이드라인을 검토하세요.
1. Xcode 프로젝트의 좌측 탐색기에서 `Runner` 폴더의 `Assets.xcassets`를 선택합니다. 그곳에서 기본 아이콘을 자신의 앱 아이콘으로 업데이트하세요.   
1. `flutter run`을 통해서 아이콘이 변경되었는지 확인합니다. 

## 빌드 아카이브 생성하기

여기서는 빌드 아카이브를 생성하여 App Store Connect에 빌드를 업로드 합니다.

개발 중에 *디버그* 빌드로 빌딩, 디버깅, 테스트를 수행하셨을 겁니다. 앱 스토어나 TestFlight에서 사용자에게 앱을 사용하게 할 준비가 되면 *릴리즈* 빌드로 준비해야 합니다.

명령 줄 인터페이스에서 애플리케이션 디렉토리로 이동한 뒤 다음을 실행하세요.

1. `flutter build ios`를 실행하여 릴리즈 빌드를 생성합니다. (`flutter build` 는 `--release`를 기본값으로 가집니다.).
1. Xcode에서 릴리즈 모드 설정을 새로 고치도록 하려면 Xcode를 껐다 다시 켜세요. Xcode 8.3 이상에서는 필요하지 않습니다.   

Xcode에서 앱 버전을 설정하고 빌드합니다.

1. `ios` 폴더에서 `Runner.xcworkspace`를 실행시켜서 Xcode를 엽니다.
1. **Product > Scheme > Runner** 를 선택합니다..
1. 다음은 **Product > Destination > Generic iOS Device** 를 선택합니다.
1. Xcode 좌측에서 **Runner** 를 선택합니다. 그리고 사이드바에서 targets의 **Runner** 를 선택합니다.
1. Identity 섹션에서 당신이 원하는 **Version** 으로 업데이트 합니다.
1. Identity 섹션의 **Build** 에 App Store Connect에서 추적 가능한 고유 빌드 번호로 업데이트하세요. 각 업로드에는 고유한 빌드 번호가 필요합니다.   

마지막으로 빌드 아카이브를 생성하고 App Store Connect에 업로드 합니다.

1. 빌드 아카이브를 생성하기 위해 **Product > Archive** 를 선택하세요. 
1. 새롭게 뜨는 Xcode 관리자 창의 사이드바에서 iOS 앱을 선택한 다음 방금 생성한 아카이브를 선택합니다.   
1. 우측의 **Validate app** 버튼을 클릭합니다. 만약 이슈가 보고되면 문제를 처리하고 다른 빌드를 생성하세요. 아카이브를 업로드하기 전까지는 동일한 빌드 ID를 사용할 수 있습니다.   
1. 유효성을 검사가 끝나면 **Distribute App** 버튼을 클릭하세요. [App Store Connect][appstoreconnect_login]의 앱 세부정보 페이지로 들어가서 활동내역 탭으로 제출된 빌드 상태를 볼 수 있습니다.   

TestFlight의 테스터에게 배포할 수 있음을 알리는 이메일을 30분 이내에 받을 수 있습니다. 그런 뒤에 당신은 TestFlight로 출시할지, 아니면 앱을 앱 스토어에 출시할지 선택할 수 있습니다.

자세한 내용은 [App Store Connect에 앱 업로드 하기][distributionguide_upload]를 참조하세요.

## TestFlight에 앱 배포하기

TestFlight를 통해 내부나 외부 테스터에게 자신의 앱을 푸시할 수 있습니다. 이번 단계에서는 TestFlight에 앱을 릴리즈 하는 방법을 알아보겠습니다.

1. [App Store Connect][appstoreconnect_login]의 앱 상세 페이지에서 TestFlight 탭으로 이동합니다.   
1. 사이드 바의 **Internal Testing**를 선택합니다.
1. 테스터에게 보낼 빌드를 선택하고 **Save**를 클릭합니다.
1. 내부 테스터의 이메일 주소를 추가하세요. 페이지 상단의 드롭다운 메뉴에 있는 사용자 및 액세스 페이지에서 내부 사용자를 추가할 수 있습니다.

더 자세한 정보는 [TestFlight를 이용하여 앱 배포하기][distributionguide_testflight]를 참조하세요.

## 앱스토어에 앱 배포하기

앱을 배포할 준비가 되었다면 릴리즈 하기 위한 단계롤 진행하세요. 

1. [App Store Connect][appstoreconnect_login]의 앱 상세 페이지의 좌측 사이드바에서 **가격 및 사용 가능 여부**를 선택하여 들어갑니다. 그런 뒤에 필요한 정보를 입력하고 저장합니다.    
1. 좌측 사이드바에서 상태를 선택합니다. 만일 앱을 처음 출시할 경우 **1.0 제출 준비 중** 상태가 됩니다. 그런 뒤 필요한 정보를 입력합니다.    
1. **심사를 위해 제출**을 클릭합니다.

애플은 앱 리뷰가 끝나면 알림을 줄 것입니다. 그러면 앱은 **버전 출시** 섹션에서 선택한 항목에 맞게 출시 될 것입니다. 

자세한 내용은 [앱스토어를 통해 앱 배포하기][distributionguide_submit]를 참조하세요.

## 문제해결

[앱 배포하기][distributionguide]에서는 앱 스토어에 앱을 배포하는 단계에 대한 자세한 내용을 제공합니다.


[appicon]: https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/app-icon/
[appreview]: https://developer.apple.com/app-store/review/
[appsigning]: https://help.apple.com/xcode/mac/current/#/dev154b28f09
[appstore]: https://developer.apple.com/app-store/submissions/
[codesigning_guide]: https://developer.apple.com/library/content/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html
[devportal_appids]: https://developer.apple.com/account/ios/identifier/bundle
[devprogram]: https://developer.apple.com/kr/programs/
[devprogram_membership]: https://developer.apple.com/kr/support/compare-memberships/
[distributionguide]: https://help.apple.com/xcode/mac/current/#/dev8b4250b57
[distributionguide_config]: https://help.apple.com/xcode/mac/current/#/dev91fe7130a
[distributionguide_submit]: https://help.apple.com/xcode/mac/current/#/dev067853c94
[distributionguide_testflight]: https://help.apple.com/xcode/mac/current/#/dev2539d985f
[distributionguide_upload]: https://help.apple.com/xcode/mac/current/#/dev442d7f2ca
[appstoreconnect]: https://developer.apple.com/kr/support/app-store-connect/
[appstoreconnect_guide]: https://developer.apple.com/kr/support/app-store-connect/
[appstoreconnect_guide_register]: https://help.apple.com/app-store-connect/#/dev2cd126805
[appstoreconnect_login]: https://appstoreconnect.apple.com/
[testflight]: https://developer.apple.com/testflight/
