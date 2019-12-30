<div class="tab-pane" id="vscode" role="tabpanel" aria-labelledby="vscode-tab" markdown="1">

## 앱 생성 {#create-app}

{% include_relative _web-app.md  %}

  1. **View > Command Palette**를 호출하세요.
  1. "flutter"를 입력한 후, **Flutter: New Project**를 선택하세요.
  1. 프로젝트 이름을 입력하고 **Enter**를 누르세요. (예, `myapp`)
  1. 새 프로젝트를 위한 디렉토리를 생성하거나 선택하세요.
  1. 프로젝트가 생성되어 `main.dart` 파일이 나타날 때까지 기다리세요. 

위 명령은 [머티리얼 컴포넌트][]를 사용한 간단한 예시 앱이 들어있는 
Flutter 프로젝트 디렉토리 `myapp`을 생성합니다.

{% include_relative _package-name.md  %}

{% include_relative _main-code-note.md  %}

## Run the app

 1. Locate the VS Code status bar (the blue bar at the bottom of the
    window):<br> ![status bar][]{:.mw-100}
 1. Select a device from the **Device Selector** area.
    For details, see [Quickly switching between Flutter devices][].
    - If no device is available and you want to use a device simulator,
      click **No Devices** and launch a simulator.
    - To setup a real device, follow the device-specific instructions on the
      [Install][] page for your OS.
 1. Press the **Settings button**&mdash;a cog icon gear
    on the top right (now marked with a red or orange indicator)
    next to the DEBUG text box that reads **No Configuration**.
    Select flutter. And choose the debug configuration:
    To create your emulator if it is closed or to run the
    emulator or device that is now connected.
 1. Invoke **Debug > Start Debugging** or press <kbd>F5</kbd>.
 1. Wait for the app to launch &mdash; progress is printed
    in the **Debug Console** view.

{% capture save_changes -%}
  : invoke **Save All**, or click **Hot Reload**
  <i class="material-icons align-bottom">offline_bolt</i>.
  {% comment %} Or, as an alternative:
    {% asset 'get-started/hot-reload-button.png' alt='looks like a lightning bolt' %}.
  {% endcomment -%}
{% endcapture %}

{% include_relative _try-hot-reload.md save_changes=save_changes %}
{% include run-profile.md %}

[설치]: /docs/get-started/install
[머티리얼 컴포넌트]: {{site.material}}/guidelines
[Flutter 기기간 빠른 전환]: https://dartcode.org/docs/quickly-switching-between-flutter-devices
[status bar]: {% asset tools/vs-code/device_status_bar.png @path %}

</div>
