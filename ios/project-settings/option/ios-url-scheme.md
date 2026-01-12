# iOS URL Scheme 설정 안내

Cauly iOS SDK는 **광고 랜딩 과정에서의 사용자 경험 개선을 위해**\
일부 환경에서 외부 앱 설치 여부를 확인하는 로직을 내부적으로 사용할 수 있습니다.

iOS 정책상, 앱에서 특정 URL Scheme에 대한 사용 가능 여부를 확인하려면\
`Info.plist`에 해당 Scheme을 사전에 등록해야 합니다.

#### Info.plist 설정

아래 항목을 앱의 `Info.plist`에 추가해 주세요.

```xml
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>naversearchapp</string>
</array>
```

#### 적용 시 유의사항

* 본 설정은 **Cauly iOS SDK 내부 로직 동작을 위한 설정**입니다.
* SDK 사용 중 특정 광고/랜딩 환경에서만 참조되며,\
  앱에서 직접 URL Scheme을 호출하거나 별도의 구현을 추가할 필요는 없습니다.
* 해당 항목이 누락될 경우, 일부 광고 랜딩 과정에서 **의도한 사용자 경험이 제공되지 않을 수 있습니다.**
