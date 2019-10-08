{{site.alert.warning}}
  `Flutter` 도구는 사용 통계와 기본 크래시 보고서를 
  구글 애널리틱스에 익명으로 보고합니다.
  이 데이터는 Flutter 도구를 지속적으로 개선하는 데 사용됩니다.
   
  Flutter 도구는 첫 번째로 실행할 때는 애널리틱스 데이터를 전송하지 않고,  
  flutter config를 포함하여 실행했을 때도 분석 데이터를 전송하지 않습니다.
  따라서, 애널리틱스에 전혀 보고하지 않은 상태로 애널리틱스를 제외할(opt-out) 수 있습니다. 
  애널리틱스 보고를 종료하기 위해 `flutter config --no-analytics`를 입력하고, 
  `flutter config`를 입력하여 현재 설정을 보여주세요.

  Flutter SDK를 다운로드하면 Google 서비스 약관에 동의하게 됩니다. 
  참고: 구글의 [개인 정보 정책][]은 이 서비스에서
  데이터가 어떻게 사용되는지 설명합니다.

  또한, Flutter는 Dart SDK를 포함하고,
  Dart SDK는 사용 지표와 오류 정보를 구글에 보고합니다. 

  [개인 정보 정책]: https://policies.google.com/privacy
{{site.alert.end}}
