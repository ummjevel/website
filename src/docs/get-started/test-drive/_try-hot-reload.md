앱 빌드가 완료되면, 기기에서 초기 앱을 확인할 수 있습니다. 

{% include app-figure.md img-class="site-mobile-screenshot border"
    path-prefix="get-started" platform="iOS" image="starter-app.png"
    caption="Starter app" %}

## Hot reload

Flutter offers a fast development cycle with _Stateful Hot Reload_,
the ability to reload the code of a live running app without
restarting or losing app state.
Make a change to app source,
tell your IDE or command-line tool that you want to hot reload,
and see the change in your simulator, emulator, or device.

 1. `lib/main.dart` 여세요.
 1. 문자열을 변경하세요

    {% prettify dart %}
      'You have [[strike]]pushed[[/strike]] the button this many times'
    {% endprettify %}

    에서

    {% prettify dart %}
      'You have [!clicked!] the button this many times'
    {% endprettify %}

    로 

    {{site.alert.important}}
      앱을 중단하면 _안 됩니다._ 앱이 실행 상태를 유지해야 합니다.
    {{site.alert.end}}

 1. 변경 내용을 저장하세요. {{include.save_changes}}

실행 중인 앱에 변경된 문자열이 거의 즉시 나타납니다.
