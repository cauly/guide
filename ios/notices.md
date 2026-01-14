# 주요공지 및 안내사항

최근 Apple의 **iOS 14 개인정보 보호 정책 강화**에 따라\
**App Tracking Transparency(ATT)** 정책이 업데이트되었습니다.

해당 정책에 따라 **2020년 12월 8일 이후 신규 등록되거나 업데이트되는 앱**은\
**App Store Connect > Privacy Policy** 메뉴를 통해\
앱에서 수집하는 데이터 항목을 반드시 제출해야 합니다.

Cauly는 Apple의 개인정보 보호 정책을 준수하고,\
보다 안정적인 광고 서비스를 제공하기 위해\
**Cauly iOS SDK에서 수집되는 항목**에 대해 아래와 같이 안내드립니다.

***

### Cauly SDK 수집 항목 안내

아래 문서를 통해 Cauly iOS SDK에서 수집하는 데이터 항목과\
App Store Connect 개인정보 설정 시 참고해야 할 내용을 확인하실 수 있습니다.

[카울리 SDK 수집 항목 알아보기](https://docs.google.com/spreadsheets/d/1-qHu6Vxn_qV86KciIjKNALF2n23wXA4v8cvvcpBjEVU/edit#gid=0)

***

### iOS 14 개인정보 보호 정책 관련 참고 자료

Apple의 공식 정책 문서는 아래 링크를 통해 확인하실 수 있습니다.

* [App Tracking Transparency(ATT)](https://developer.apple.com/documentation/apptrackingtransparency)
* [App Store Connect 개인정보 보호](https://developer.apple.com/kr/news/?id=vlj9jty9)
* [사용자 개인정보 보호 및 데이터 사용](https://developer.apple.com/kr/app-store/user-privacy-and-data-use)

***

### ATT 정책 적용 가이드

ATT(App Tracking Transparency) 정책은 App Store 심사 시 필수로 확인되는 항목으로,\
앱에 적용하는 방법과 사용자 동의 요청 처리 방식은\
아래 가이드를 참고해 주세요.

[**\[ATT 정책 사용 방법 안내\]**](project-settings/app-tracking-transparency.md)

***

#### App Transport Security(ATS) 설정 안내

iOS 앱에서 App Transport Security(ATS) 설정이 올바르게 적용되지 않은 경우,

* 네트워크 보안 정책 위반으로 인해 광고가 정상적으로 노출되지 않거나
* App Store 심사 과정에서 반려될 수 있습니다.

Cauly iOS SDK 사용 시 필요한 ATS 설정 방법과\
Info.plist 적용 예시는 아래 가이드를 참고해 주세요.

[**\[App Transport Security 설정 안내\]**](project-settings/app-transparent-security.md)

***

### 앱 심사 반려 관련 안내

App Store 심사 과정에서\
본 안내 사항 또는 개인정보 설정과 관련하여 반려가 발생한 경우,\
[카울리 고객센터](https://www.cauly.net/index.html#/common/contactUs/public) 로 문의주시기 바랍니다.



***

### CocoaPods 지원 중단 안내

CocoaPods 측에서 **Specs Repository(trunk)를 read-only 상태로 전환할 예정**임을 공식적으로 안내하였습니다.\
이에 따라 **2026년 12월 2일 이후에는 새로운 Podspec 등록 및 수정이 더 이상 허용되지 않습니다.**

> CocoaPods 공식 공지에서는\
> &#xNAN;_“CocoaPods trunk is moving to be read-only”_ 라고 명시하고 있으며,\
> 자세한 내용은 아래 블로그를 통해 확인하실 수 있습니다.\
> [https://blog.cocoapods.org/CocoaPods-Specs-Repo/](https://blog.cocoapods.org/CocoaPods-Specs-Repo/)

이와 같은 CocoaPods 정책 변경으로 인해,\
Cauly iOS SDK 또한 **CocoaPods 기반 배포 및 신규 업데이트를 더 이상 제공할 수 없다고 판단하였습니다.**

이에 따라 **Cauly iOS SDK의 CocoaPods 지원은 2026년 내 종료될 예정**이며,\
CocoaPods 환경에서는 **신규 기능 추가 및 장기적인 업데이트가 제공되지 않습니다.**

**기존 CocoaPods 사용자 안내**

* 기존에 CocoaPods로 연동된 프로젝트는 **즉시 사용이 중단되지 않습니다.**
* 다만, **CocoaPods 환경에서는 신규 SDK 버전이 제공되지 않으므로**,\
  장기적인 유지보수 및 안정적인 SDK 사용을 위해\
  **Swift Package Manager(SPM) 또는 수동 다운로드 방식으로의 전환을 권장**드립니다.

**권장 설치 방식**

* **Swift Package Manager (SPM)** – 권장
* **수동 다운로드 방식**
* CocoaPods – 지원 종료 예정 (2026년 내)
