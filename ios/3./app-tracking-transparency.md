# App Tracking Transparency

iOS 14 이상 지원하는 경우 [ATT (App Tracking Transparency) Framework](https://developer.apple.com/documentation/apptrackingtransparency?language=objc)를 적용해야 IDFA (Identifier for Advertisers) 식별자를 얻을 수 있습니다.

* iOS 14 이전에는 광고주가 IDFA (Identifier for Advertisers)를 사용하여 광고 성과 측정 및 맞춤형 광고를 할 수 있었습니다. 하지만 iOS 14 이상에서 ATT (App Tracking Transparency) 도입으로 인하여 개인 정보 보호가 강화됨에 따라, 사용자가 동의를 허용한 경우에만 IDFA 값을 가져올 수 있습니다.

### 1. SKAdNetwork 구성

사용자의 ATT 동의 여부와 무관하게, 애플에서 공식으로 제공하는 광고 캠페인의 성공을 측정하기 위한 목적으로 [SKAdNetwork](https://developer.apple.com/documentation/storekit/skadnetwork/) 를 도입했습니다. SKAdNetwork 를 사용하기 위해 `Info.plist` 파일에 **광고 식별자 목록 정보를 추가**합니다.

{% hint style="info" %}
광고 네트워크에 지원되는 식별자 목록은 이 문서의 [SKAdNetwork ID List](skadnetwork-id-list.md) 에서 확인할 수 있습니다.
{% endhint %}

### 2. 권한 사용에 대한 설명 문구 추가

`Info.plist` 파일에 `NSUserTrackingUsageDescription` 키와 권한 사용에 대한 동의를 구하는 메시지를 추가합니다.

```xml
 <key> NSUserTrackingUsageDescription </key>
 <string> 맞춤형 광고 제공을 위해 사용자의 데이터가 사용됩니다. </string>
```

### 3. 권한 요청

* ATT는 앱이 완전히 실행되어 Active 상태일 때 호출해주셔야 정상적으로 팝업이 노출됩니다.
  * `application:didFinishLaunchingWithOptions:`에서 ATT를 호출하고 있었다면, iOS 15 부터는 동작하지 않습니다.
* 사용자가 앱 추적 투명성 권한을 부여하면 광고 SDK에서 광고 요청에 IDFA를 사용할 수 있도록 완료 callback이 호출된 후, 광고를 요청합니다.

{% tabs %}
{% tab title="Swift" %}
```swift
 import AppTrackingTransparency
 ... 
 DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
     if #available(iOS 14, *) {
         ATTrackingManager.requestTrackingAuthorization(completionHandler: { status in
             switch status {
             case .authorized:       // 승인
                 print("Authorized")
                 // 권한 요청이 완료된 다음, 광고를 요청해 주세요.
             case .denied:           // 거부
                 print("Denied")
             case .notDetermined:        // 미결정
                 print("Not Determined")
             case .restricted:           // 제한
                 print("Restricted")
             @unknown default:           // 알려지지 않음
                 print("Unknow")
             }
         })
     }
 }
```
{% endtab %}

{% tab title="Objective-C" %}
```objective-c
 #import <AppTrackingTransparency/AppTrackingTransparency.h>
 ...
 if (@available(iOS 14, *)) {
 [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {
     switch (status) {
         // 승인
         case ATTrackingManagerAuthorizationStatusAuthorized:
         NSLog(@"ATTrackingManagerAuthorizationStatusAuthorized");
         // 권한 요청이 완료된 다음, 광고를 요청해 주세요.
             break;
         // 거부
         case ATTrackingManagerAuthorizationStatusDenied:
         NSLog(@"ATTrackingManagerAuthorizationStatusDenied");
             break;
         // 제한
         case ATTrackingManagerAuthorizationStatusRestricted:
         NSLog(@"ATTrackingManagerAuthorizationStatusRestricted");
             break;
         // 미결정
         default:
         NSLog(@"Unknow");
             break;
     	}
     }];
 }
```
{% endtab %}
{% endtabs %}
