<div class="tab-pane active" id="androidstudio" role="tabpanel" aria-labelledby="androidstudio-tab" markdown="1">

## 앱 생성 {#create-app}

{% include_relative _web-app.md  %}

 1. IDE를 켜고 **Start a new Flutter project**를 선택하세요.
 1. 프로젝트 타입으로 **Flutter Application**을 선택하세요. **Next**을 누르세요.
 1. K의 위치에 맞게 경로가 설정되어 있는지 확인하세요.
    (텍스트 필드가 비어 있으면 **Install SDK...**를 선택하세요).
 1. 프로젝트 이름을 입력하세요 (예, `myapp`). 그런 다음 **Next**를 누르세요.
 1. **Finish**를 클릭하세요.
 1. Android 스튜디오가 SDK를 설치하고 프로젝트를 생성할 때까지 기다리세요.

{% include_relative _package-name.md  %}

위 명령은 [머티리얼 컴포넌트][]를 사용한 간단한 예시 앱이 들어있는 Flutter 프로젝트 디렉토리 `myapp`을 생성합니다.

{% include_relative _main-code-note.md  %}

## 앱 실행

 1. Android 스튜디오 기본 툴바를 찾으세요: <br>
    ![Main IntelliJ toolbar][]{:.mw-100}
 1. In the **target selector**, select an Android device for running the app.
    If none are listed as available,
    select **Tools> Android > AVD Manager** and create one there.
    For details, see [Managing AVDs][].
 1. Click the run icon in the toolbar, or invoke the menu item **Run > Run**.

{% capture save_changes -%}
  : invoke **Save All**, or click **Hot Reload**
  <i class="material-icons align-bottom">offline_bolt</i>.
  {% comment %} Or, as an alternative:
    {% asset 'get-started/hot-reload-button.png' alt='looks like a lightning bolt' %}.
  {% endcomment -%}
{% endcapture %}

{% capture ide_profile -%}
  to invoke the menu item **Run > Profile** in the IDE, or
{% endcapture %}

{% include_relative _try-hot-reload.md save_changes=save_changes %}
{% include run-profile.md ide_profile=ide_profile %}

[Main IntelliJ toolbar]: {% asset tools/android-studio/main-toolbar.png @path %}
[AVD 관리]: {{site.android-dev}}/studio/run/managing-avds
[머티리얼 컴포넌트]: {{site.material}}/guidelines
</div>
