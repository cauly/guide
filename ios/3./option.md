# Option

* BGM이 포함된 광고가 있을 수 있으니, APP에 BGM이 있는 경우\
  willShowLandingView API를 이용하여 일시 중지 해주세요.\
  광고 종료 후 didCloseLandingView API를 이용하여 BGM을 재 시작 하시면 됩니다.
* 광고뷰가 화면에 보여지지 않는 경우에도 광고 요청을 할 수 있습니다. \
  광고 요청을 중단하고자 할 때 `[CaulyAdView 객체 stopAdRequest];` 명령을 실행하여 광고 요청을 반드시 중지하기 바랍니다.
*   사용자 앱 내 광고 경험 개선을 위한 URL Scheme 적용

    * info.plist 작성

    ```xml
    <key>LSApplicationQueriesSchemes</key>
    <array>
        <string>naversearchapp</string>
    </array>
    ```

