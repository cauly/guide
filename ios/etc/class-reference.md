# Class Reference

### Callback API

배너 광고

{% tabs %}
{% tab title="Swift" %}
```swift
// 배너광고 성공
func didReceiveAd(_ adView: CaulyAdView!, isChargeableAd: Bool)

// 배너광고 실패
func didFail(toReceiveAd adView: CaulyAdView!, errorCode: Int32, errorMsg: String!)

// 랜딩 화면 표시
func willShowLanding(_ adView: CaulyAdView!)

// 랜딩 화면이 닫혔을 때
func didCloseLanding(_ adView: CaulyAdView!)
```
{% endtab %}

{% tab title="Objective-C" %}
```objective-c
// 배너광고 성공
- (void)didReceiveAd:(CaulyAdView *)adView isChargeableAd:(BOOL)isChargeableAd;

// 배너광고 실패
- (void)didFailToReceiveAd:(CaulyAdView *)adView errorCode:(int)errorCode errorMsg:(NSString *)errorMsg;

// 랜딩 화면 표시
- (void)willShowLandingView:(CaulyAdView *)adView;

// 랜딩 화면이 닫혔을 때
- (void)didCloseLandingView:(CaulyAdView *)adView;
```
{% endtab %}
{% endtabs %}

전면 광고

{% tabs %}
{% tab title="Swift" %}
```swift
// 전면광고 성공
func didReceive(_ interstitialAd: CaulyInterstitialAd!, isChargeableAd: Bool)

// 전면광고 실패
func didFail(toReceive interstitialAd: CaulyInterstitialAd!, errorCode: Int32, errorMsg: String!)

// Interstitial 형태의 광고가 보여지기 직전
func willShow(_ interstitialAd: CaulyInterstitialAd!)

// Interstitial 형태의 광고가 닫혔을 때
func didClose(_ interstitialAd: CaulyInterstitialAd!)
```
{% endtab %}

{% tab title="Objective-C" %}
```objective-c
// 전면광고 성공
- (void)didReceiveInterstitialAd:(CaulyInterstitialAd *)interstitialAd isChargeableAd:(BOOL)isChargeableAd;

// 전면광고 실패
- (void)didFailToReceiveInterstitialAd:(CaulyInterstitialAd *)interstitialAd errorCode:(int)errorCode errorMsg:(NSString *)errorMsg;

// Interstitial 형태의 광고가 보여지기 직전
- (void)willShowInterstitialAd:(CaulyInterstitialAd *)interstitialAd;

// Interstitial 형태의 광고가 닫혔을 때
- (void)didCloseInterstitialAd:(CaulyInterstitialAd *)interstitialAd;
```
{% endtab %}
{% endtabs %}

네이티브 광고

{% tabs %}
{% tab title="Swift" %}
```swift
// 광고 정보 수신 성공
func didReceive(_ nativeAd: CaulyNativeAd!, isChargeableAd: Bool)

// 광고 정보 수신 실패
func didFail(toReceive nativeAd: CaulyNativeAd!, errorCode: Int32, errorMsg: String!)
```
{% endtab %}

{% tab title="Objective-C" %}
```objective-c
// 광고 정보 수신 성공
- (void)didReceiveNativeAd:(CaulyNativeAd *)nativeAd isChargeableAd:(BOOL)isChargeableAd;

// 광고 정보 수신 실패
- (void)didFailToReceiveNativeAd:(CaulyNativeAd *)nativeAd errorCode:(int)errorCode errorMsg:(NSString *)errorMsg;
```
{% endtab %}
{% endtabs %}

### 기타 API

배너 광고

```objective-c
// 배너광고 객체 생성 클래스 메소드
+ (id)caulyAdViewWithController:(UIViewController *)controller;

// 배너광고 객체 생성 인스턴스 메소드
- (id)initWithParentViewController:(UIViewController *)controller;

// 배너광고 요청
- (void)startBannerAdRequest;

// 배너광고 요청 중지
- (void)stopAdRequest;
```

전면 광고

```objective-c
// 전면광고 객체 생성 클래스 메소드
+ (id)caulyAdWithController:(UIViewController *)controller;

// 전면광고 객체 생성 인스턴스 메소드
- (id)initWithParentViewController:(UIViewController *)controller;

// 전면광고 요청
- (void)startInterstitialAdRequest;

// 전면광고 요청 중지
- (void)stopAdRequest;
```

네이티브 광고

```objective-c
// 네이티브광고 객체 생성 클래스 메소드
+ (id)caulyAdWithController:(UIViewController *)controller;

// 네이티브광고 객체 생성 인스턴스 메소드
- (id)initWithParentViewController:(UIViewController *)controller;

// 네이티브광고 요청
- (void)startNativeAdRequest:(int) adListSize nativeAdComponentType:(CaulyNativeAdComponentType) nativeAdComponentType imageSize:(NSString*) imageSize;

// 네이티브광고 요청 중지
- (void)stopAdRequest;
```



### Properties

배너 광고

```objective-c
// CaulyAdViewDelegate 프로토콜을 구현한 delegate 객체
delegate

// CaulyAdSetting 세팅된 객체 설정
localSetting

// parentViewController 객체
parentController

// 광고 에러 메시지
errorMsg
```

전면 광고

```objective-c
// CaulyInterstitialAdDelegate 프로토콜을 구현한 delegate 객체
delegate

// CaulyAdSetting 세팅된 객체 설정
localSetting

// parentViewController 객체
parentController

// 광고 에러 메시지
errorMsg
```

네이티브 광고

```objective-c
// CaulyNativeAdDelegate 프로토콜을 구현한 delegate 객체
delegate

// CaulyAdSetting 세팅된 객체 설정
localSetting

// parentViewController 객체
parentController

// 광고 에러 메시지
errorMsg
```
