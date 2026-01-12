# BGM이 있는 앱에서의 광고 재생 처리

광고 소재에 따라 **사운드(BGM)가 포함된 크리에이티브**가 노출될 수 있습니다.\
앱에서 자체적으로 BGM을 재생 중인 경우, 광고 랜딩(Landing View) 표시 시점에 앱의 BGM을 일시 중지하고 광고 종료 후 재개하는 것을 권장합니다.

#### 적용 방법

* **광고 랜딩이 표시되기 직전**: `willShowLandingView`에서 앱 BGM **일시 정지**
* **광고 랜딩이 종료된 직후**: `didCloseLandingView`에서 앱 BGM **재시작**

> 적용 목적\
> 광고 사운드와 앱 BGM이 동시에 재생되는 상황을 방지하여 사용자 경험을 개선합니다.

**예시 코드**

{% tabs %}
{% tab title="Swift" %}
```swift
// 광고 랜딩 화면이 표시되기 직전
func willShowLandingView(_ adView: CaulyAdView!) {
    MyBgmManager.shared.pause()
}

// 광고 랜딩 화면이 닫힌 직후
func didCloseLandingView(_ adView: CaulyAdView!) {
    MyBgmManager.shared.resume()
}
```
{% endtab %}

{% tab title="Objective-C" %}
```objective-c
// 광고 랜딩 화면이 표시되기 직전
- (void)willShowLandingView:(CaulyAdView *)adView {
    // 앱 BGM 일시 중지
    [[MyBgmManager shared] pause];
}

// 광고 랜딩 화면이 닫힌 직후
- (void)didCloseLandingView:(CaulyAdView *)adView {
    // 앱 BGM 재시작
    [[MyBgmManager shared] resume];
}
```
{% endtab %}
{% endtabs %}
