---
title: Android 앱 출시 준비하기
short-title: Android
---

Flutter 앱을 개발하는 동안, 커멘드 라인에서의 `flutter run`을 실행하거나
IDE에 있는 툴바 **Run** 과 **Debug**를 선택하여 
앱을 테스트할 수 있습니다. 
Flutter는 기본적으로 앱의 _debug_ 버전을 빌드합니다.

When you're ready to prepare a *release* version for Android, for example to
[publish to the Google Play Store][play], follow the steps on this page.

앱의 _release_ 버전을 출시할 준비가 됐을 때,
[Google Play 출시][play]에 있는 예시를 참조하면
도움이 될 것입니다. 출시하기 직전에
마지막 작업이 앱에 필요할 것입니다.
이 페이지는 아래와 같은 주제를 다룹니다.

* [런처 아이콘 추가하기](#런처-아이콘-추가하기)
* [앱 서명하기](#앱-서명하기)
* [Proguard 사용](#Proguard-사용)
* [앱 매니페스트 검토하기](#앱-매니페스트-검토하기)
* [빌드 구성 검토하기](#빌드-구성-검토하기)
* [release 앱 번들 빌드하기](#release-앱-번들-빌드하기)
* [Google Play 스토어에 APK 출시하기](#Google-Play-스토어에-APK-출시하기)
* [Android 출시 관련 자주 묻는 질문](#Android-출시-관련-자주-묻는-질문)

## 런처 아이콘 추가하기

새로 생성된 Flutter 앱의 아이콘은 기본 런처 아이콘으로 설정되어있습니다. 
런처 아이콘을 원하는 아이콘으로 변경하고 싶다면, 
[flutter_launcher_icons]({{site.pub}}/packages/flutter_launcher_icons) 
package를 확인해보세요.

package를 사용하지 않고, 런처 아이콘을 바꾸려면 아래 지시사항을 따르세요.

1. 아이콘 디자인을 위해 [Material Design product
   icons][launchericons] 가이드라인을 확인합니다.

1. `<app dir>/android/app/src/main/res/`에서 아이콘 파일을 
   [configuration qualifiers][]을 사용하여 이름지어 폴더에 넣습니다.
   기본 `mipmap-` 폴더들은 적당한 명명 규칙을 보여줍니다. 

1. `AndroidManifest.xml` 에서 [`application`][applicationtag] 
   태그의 `android:icon` 항목을 업데이트하여 이전 단계에서 넣은 아이콘을 가져옵니다. 
   (예를 들어, 
   `<application android:icon="@mipmap/ic_launcher> ...`)

1. 아이콘이 변경된 것을 확인하기 위해 
   앱을 실행하고 런처에서 앱 아이콘을 확인합니다.

## 앱 서명하기

Google Play 스토어에 출시하기 위해서는 반드시 앱에 디지털 서명을 해야 합니다.
앱에 디지털 서명을 하기 위해 다음 지시사항을 따르세요.

### keystore 만들기
이미 keystore를 가지고 있다면 다음 단계로 넘어가세요. 
그렇지 않다면, 아래 명령줄 코드를 통해 keystore를 생성하세요:

맥/리눅스에서는 아래 명령어를 사용하세요:
```
keytool -genkey -v -keystore ~/key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key
```
윈도우에서는 아래 명령어를 사용하세요:
```
keytool -genkey -v -keystore c:/Users/USER_NAME/key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key
```

{{site.alert.note}}
  이 파일은 항상 개인적으로 보관하세요; 
  절대 공개된 저장소에 올리지 마세요.
{{site.alert.end}}

{{site.alert.note}}
  `keytool` 은 프로젝트 경로에 존재하지 않을 수 있습니다. 
  해당 파일은 Android 스튜디오와 함께 설치되는 Java JDK에 포함되는 파일입니다. 
  해당 파일에 대한 구체적인 경로는 명령줄에 
  `flutter doctor -v` 을 입력한 후 표시되는 
  'Java binary at:' 다음에 나타나는 경로에서 
  `java`를 포함하고 있는 디렉토리의 `keytool` 파일을 통해 확인할 수 있습니다.
  `Program Files`처럼 공백이 있는 경로라면,
  따옴표를 사용하여 감싸주세요.
  예: `/"Program Files"/`
{{site.alert.end}}

### 앱으로부터 keystore 참조하기

keystore 참조에 관한 구성을 담을 
`<app dir>/android/key.properties` 파일을 생성합니다:

```
storePassword=<password from previous step>
keyPassword=<password from previous step>
keyAlias=key
storeFile=<key store 파일 위치, 예) /Users/<user name>/key.jks>
```

{{site.alert.note}}
  이 파일은 항상 개인적으로 보관하세요; 
  절대 공개된 저장소에 업로드 하지 마세요.
{{site.alert.end}}

### Gradle에서 서명 구성하기

앱의 서명을 구성하기 위하여 
`<app dir>/android/app/build.gradle` 파일을 수정합니다.


<ol markdown="1">
<li markdown="1">Replace the following:

```
   android {
```

   속성 파일로부터 keystore 정보를 가져올 수 있도록 변경하세요.

```
   def keystoreProperties = new Properties()
   def keystorePropertiesFile = rootProject.file('key.properties')
   if (keystorePropertiesFile.exists()) {
       keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
   }

   android {
```
</li>

<li markdown="1"> 다음 코드를:

```
   buildTypes {
       release {
           // TODO: release 빌드 버전을 위한 서명 구성을 추가하세요.
           // Signing with the debug keys for now,
           // so `flutter run --release` works.
           signingConfig signingConfigs.debug
       }
   }
```

   다음과 같이 변경하세요:

```
   signingConfigs {
       release {
           keyAlias keystoreProperties['keyAlias']
           keyPassword keystoreProperties['keyPassword']
           storeFile file(keystoreProperties['storeFile'])
           storePassword keystoreProperties['storePassword']
       }
   }
   buildTypes {
       release {
           signingConfig signingConfigs.release
       }
   }
```
</li>
</ol>

이제 앱의 release 빌드에서 자동으로 서명이 될 것 입니다.

## Proguard 사용

기본적으로, Flutter는 Android 관련 코드를 난독화하거나 축소화하지 않습니다.
서드파티 자바 라이브러리나 Android 라이브러리를 사용한다면 Proguard 적용을 통해 APK의 크기를 줄이고 리버스 엔지니어링으로부터 코드를 보호할 수 있습니다.

Dart 코드 난독화에 대한 정보를 얻기 위해서는 
[Flutter wiki]({{site.github}}/flutter/flutter/wiki)의 [Obfuscating Dart
Code]({{site.github}}/flutter/flutter/wiki/Obfuscating-Dart-Code)를 참조하세요.

### Step 1 - Proguard 구성하기

 Proguard Rule을 구성하기 위하여 `/android/app/proguard-rules.pro` 파일을 생성하고 
 다음과 같은 규칙을 추가합니다.

```
## Flutter wrapper
-keep class io.flutter.app.** { *; }
-keep class io.flutter.plugin.**  { *; }
-keep class io.flutter.util.**  { *; }
-keep class io.flutter.view.**  { *; }
-keep class io.flutter.**  { *; }
-keep class io.flutter.plugins.**  { *; }
```

이 설정은 Flutter 엔진 라이브러리를 보호합니다. </br>
다른 라이브러리(예. Firebase)를 보호하기 위해서는 
추가적으로 규칙을 작성해야합니다.

### Step 2 - 난독화와 축소 사용하기

`/android/app/build.gradle` 파일을 열고 `buildTypes`가 정의된 부분을 찾으세요.
`release` 설정 부분 안에 
`minifiyEnabled`와 `useProguard` 플래그가 true로 설정되어 있습니다.
여기에 step 1 에서 만든 Proguard 규칙 파일을 추가해야 합니다.

```
android {

    ...

    buildTypes {

        release {

            signingConfig signingConfigs.release

            minifyEnabled true
            useProguard true

            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'

        }
    }
}
```

{{site.alert.note}}
  난독화와 축소 과정으로 인해 Android 앱의 컴파일 시간이 
  크게 늘어날 수 있습니다.
{{site.alert.end}}

## 앱 매니페스트 검토하기

올바른 앱 설정을 위하여 `<app dir>/android/app/src/main`에 있는 기본 
[앱 매니페스트][manifest] 파일인 `AndroidManifest.xml`을 검토하고 
올바른 값들을 포함하는지 확인하세요. 특히:

* `application`: 앱의 최종 이름을 반영하기 위해 [`application`][applicationtag]에 있는 
  `android:label`을 수정하세요.

* `uses-permission`: 앱 내에서 인터넷 접근이 필요하지 않다면 
  `android.permission.INTERNET` [permission][permissiontag]을 제거하세요. 
   기본 템플릿에서는 Flutter 도구와 실행 중인 앱의 커뮤니케이션을 위해 해당 권한을 포함합니다.

## 빌드 구성 검토하기

올바른 빌드 구성을 위하여 `<app dir>/android/app`에 있는 
기본 [Gradle build file][gradlebuild] 파일인 `build.gradle`을 검토하고 
올바른 값들을 포함하는지 확인하세요. 특히:

* `defaultConfig`:

  * `applicationId`: 고유한 최종 (Application Id)[appid]를 지정하세요.

  * `versionCode` & `versionName`: 내부 앱 버전 번호를 지정하고, 
     문자열 형태로 명시하세요. pubspec.yaml 파일에 `version` 속성을 설정하여 
     내부 앱 버전 번호를 문자열 형태로 지정할 수 있습니다. 
     버전 정보 지침에 대해서는 [버전 문서][versions]를 참조하세요.

  * `minSdkVersion` & `targetSdkVersion`: 최소 API 레벨과 
     개발 대상 버전으로 지정한 지정 API 레벨을 명시하세요. 
     자세한 내용은 [버전 문서][versions]의 API 레벨 영역을 참조하세요.

## release 앱 번들 빌드하기

플레이 스토어에 게시할 때 
2가지 release 방식을 사용할 수 있습니다. 

* 앱 번들 (추천)
* APK

{{site.alert.note}}
  구글 플레이 스토어는 앱 번들 방식을 권합니다.
  더 많은 정보는 [Android App Bundle][bundle]과 
  [About Android App Bundles][bundle2]를 참조하세요.
{{site.alert.end}}

### 앱 번들 빌드

여기서는 어떻게 release 앱 번들을 빌드하는지 소개합니다. 
위 단락의 서명하기를 완료했다면, 
release 번들이 서명될 것입니다.

커멘드 라인에서:

1. `cd <app dir>`를 입력하세요<br>
   (당신의 앱 디렉토리로 `<app dir>` 를 변경하세요.)
1. `flutter build appbundle`을 실행하세요.<br> 
   (`flutter build` 실행은 기본적으로 release 빌드 입니다.)

앱의 릴리즈 번들은   
`<app dir>/build/app/outputs/bundle/release/app.aab`에 생성됩니다.

기본적으로, 앱 번들은 [armeabi-v7a][] (32-bit) and [arm64-v8a][] (64-bit)을
위한 Dart 코드와 Flutter 런타임을 포함합니다.

## 앱 번들 테스트하기

다양한 방식으로 앱 번들을 테스트 할 수 있습니다. 
이번에는 두가지 방식의 앱 번들 테스트 하는 방법을 소개합니다.


#### 번들 도구를 이용한 오프라인 테스트

1. 앱 번들을 이미 완료했다면, [GitHub 저장소][]에서 
   `bundletool`을 다운받기.
1. 앱 번들로부터 [APK 세트 생성하기][].
1. 연결된 기기로 [APK 배포하기][].

#### 구글 플레이를 이용한 온라인 테스트

1. 테스트 할 번들을 구글 플레이에 업로드합니다. 
   내부 테스트 트랙을 사용하거나, 
   알파 혹은 베타 채널을 이용해 
   프로덕션 배포 이전에 테스트할 수도 있습니다.
2. 플레이 스토어에 올리기 위해 [these steps to upload your bundle][upload-bundle]을 
   참고하세요.

### Build an APK

APK보다 앱 번들을 추천하기는 하지만, 
아직은 앱 번들을 지원하지 않는 스토어가 있습니다.
이 경우, 각 타겟 ABI(Application Binary Interface)에 맞게
APK를 빌드하세요.

위 단락의 서명하기를 완료했다면, 
APK가 서명될 것입니다.

커멘드 라인에서:

1. `cd <app dir>`를 입력하세요<br>
   (현재 앱 디렉토리로 `<app, dir>` 를 변경하세요.)
1. `flutter build apk --split-per-abi` 실행하세요<br>
   (`flutter build` 실행은 기본적으로 release 빌드 입니다.)

그 결과, release 번들이 아래 두 가지 디렉토리에 생성됩니다.

* `<app dir>/build/app/outputs/apk/release/app-armeabi-v7a-release.apk`
* `<app dir>/build/app/outputs/apk/release/app-arm64-v8a-release.apk`

`--split-per-abi` 플래그를 제거하면 _모든_ 타겟 ABI를 위한 방식으로
코드가 컴파일 되어 APK가 무거워집니다. 
두개로 분할하지 않아 이렇게 APK가 커지면 사용자가 
기기의 아키텍처에 적용되지 않는 원시 바이너리까지 다운로드 하게 됩니다.  

### 기기에 APK 설치

다음 지시사항에 따라 이전 단계에서 생성한 APK를 연결된 Android 기기에 설치하세요.

명령줄을 사용하세요:

1. Android 기기를 컴퓨터에 연결합니다.
1. `cd <app dir>` (설치하려는 앱의 디렉토리입니다).
1. `flutter install` 을 통해 설치합니다.

## Google Play 스토어에 APK 출시하기

Google Play 스토어 앱 출시에 대한 자세한 내용은 
[Google Play 출시 문서][play]에서 확인하세요.

## Android 출시 관련 자주 묻는 질문

안드로이드 앱 배포에 관해 공통적으로 자주 묻는 질문이 있습니다.

### 언제 앱 번들을 사용하고 언제 APK를 사용해야 하나요?

구글 플레이 스토어는 앱을 사용자들에게 더 효율적인 방식으로 전달할 수 있기 때문에
APK 배포보다는 앱 번들 배포를 권합니다. 
그러나, 플레이 스토어가 아닌 다른 곳에 앱을 배포해야 한다면,
APK를 선택할 수밖에 없을 것입니다.

### Fat APK란?

[Fat APK][]는 하나의 APK에 여러 ABI 바이너리가 포함되어 있는 APK를 말합니다.
하나의 APK가 여러 아키텍처에서 동작하여 호환성이 좋다는 장점이 있지만,
파일 사이즈가 훨씬 커져서 사용자가 앱을 다운로드할 때 
더 많은 용량이 필요하다는 단점이 있습니다.
앱 번들 대신 APK로 빌드할 때
[build an APK](#build-an-apk)에 나오는 것 처럼
`--split-per-abi` 플래그를 사용하여 
APK를 나눠서 빌드하기를 강력히 권고합니다. 

### 지원되는 타겟 아키텍처(supported target architectures)란?

릴리스 모드로 앱을 빌드할 때,
Flutter 앱은 [armeabi-v7a][] (32-bit) 방식과 [arm64-v8a][] (64-bit) 방식으로 
컴파일 될 수 있습니다. Flutter는 현재 x86 안드로이드 방식 빌드를 지원하지 않습니다.
([Issue 9253][] 참고)

### `flutter build appbundle` 명령으로 생성되는 앱 번들에 서명하는 방법은??

[Signing the app](#signing-the-app)를 참고하세요.

### Android Studio에서 릴리스 빌드를 하는 방법은?

Android Studio에서 앱 폴더 아래에 있는 `android/` 폴더를 여세요.
그런 다음 프로젝트 패널에 있는 **build.gradle (Module: app)**를 선택하세요.

{% asset 'deployment/android/gradle-script-menu.png' alt='screenshot of gradle build script menu' %}

다음으로, 빌드 variant를 선택하세요. 메인 메뉴에서 **Build > Select Build Variant**를 클릭하세요. 
**Build Variants** 패널에서 variant를 선택하세요(debug가 기본입니다): 

{% asset 'deployment/android/build-variant-menu.png' alt='screenshot of build variant menu' %}

앱 폴더 안 `build/app/outputs`에
앱 번들이나 APK 파일이 생성됩니다. 

{% comment %}
### Are there any special considerations with add-to-app?
{% endcomment %}

[manifest]: {{site.android-dev}}/guide/topics/manifest/manifest-intro
[manifesttag]: {{site.android-dev}}/guide/topics/manifest/manifest-element
[appid]: {{site.android-dev}}/studio/build/application-id
[permissiontag]: {{site.android-dev}}/guide/topics/manifest/uses-permission-element
[applicationtag]: {{site.android-dev}}/guide/topics/manifest/application-element
[gradlebuild]: {{site.android-dev}}/studio/build/#module-level
[versions]: {{site.android-dev}}/studio/publish/versioning
[launchericons]: {{site.material}}/design/iconography/
[configuration qualifiers]: {{site.android-dev}}/guide/topics/resources/providing-resources#AlternativeResources
[play]: {{site.android-dev}}/distribute/googleplay/start
[bundle]: {{site.android-dev}}/platform/technology/app-bundle
[bundle2]: {{site.android-dev}}/guide/app-bundle
[upload-bundle]: {{site.android-dev}}/studio/publish/upload-bundle
[GitHub 저장소]: {{site.github}}/google/bundletool/releases/latest
[APK 세트 생성하기]: {{site.android-dev}}/studio/command-line/bundletool#generate_apks
[APK 배포하기]: {{site.android-dev}}/studio/command-line/bundletool#deploy_with_bundletool
[armeabi-v7a]: {{site.android-dev}}/ndk/guides/abis#v7a
[arm64-v8a]: {{site.android-dev}}/ndk/guides/abis#arm64-v8a
[Fat APK]: https://en.wikipedia.org/wiki/Fat_binary
[Issue 9253]: {{site.github}}/flutter/flutter/issues/9253
[Issue 18494]: {{site.github}}/flutter/flutter/issues/18494

