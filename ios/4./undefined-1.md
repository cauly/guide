# 전면

{% hint style="info" %}
* Cauly SDK를 프로젝트에 추가해야합니다.
* APP 등록 후 부여받은 APP CODE\[발급ID]를 사용합니다.
{% endhint %}

### Import 방법

{% tabs %}
{% tab title="Swift" %}
#### **XCFramework 사용 (SPM, Cocoapods 공통)**

```swift
import CaulySDK
```

#### **Static Library 사용**

```
Swift 코드에서는 Bridging Header 설정 후,
별도의 import 없이 바로 사용 가능합니다.
```
{% endtab %}

{% tab title="Objective-C" %}
#### **XCFramework 사용 (SPM, Cocoapods 공통)**

```objective-c
@import CaulySDK;
```

#### **Static Library 사용**

```objective-c
#import "Cauly.h"
#import "CaulyAdView.h"
#import "CaulyInterstitialAd.h"
#import "CaulyNativeAd.h"
```
{% endtab %}
{% endtabs %}

### Interstitial Ad 구현 샘플

{% tabs %}
{% tab title="Swift" %}
```swift
import UIKit
import CaulySDK
import AppTrackingTransparency

class ViewController: UIViewController, CaulyInterstitialAdDelegate {
    
    var interstitialAd: CaulyInterstitialAd?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
            if #available(iOS 14, *) {
                ATTrackingManager.requestTrackingAuthorization(completionHandler: { status in
                    switch status {
                    case .authorized:       // 승인
                        print("Authorized")
                        interstitialAdRequest()        // interstitial 광고 요청
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
        
        let adSetting = CaulyAdSetting.global()
        CaulyAdSetting.setLogLevel(CaulyLogLevelTrace)      //  Cauly Log 레벨
        adSetting?.appId = "1234567"                              //  App Store 에 등록된 App ID 정보 (필수)
        adSetting?.appCode = "Cauly"                        //  Cauly AppCode
        adSetting?.closeOnLanding = true;                   // 광고 랜딩 시 WebView 제거 여부
    }
    
    func interstitialAdRequest() {
        // Interstitial AD 호출 
        //  전면광고 객체 생성
        let _interstitialAd:CaulyInterstitialAd? = CaulyInterstitialAd.init(parentViewController: self)
        _interstitialAd?.delegate = self                //  전면 delegate 설정
        _interstitialAd?.startRequest()                 //  전면광고 요청
    }
    
    // Interstitial AD API
    #pragma mark - CaulyInterstitialAdDelegate
    
    // 광고 정보 수신 성공
    func didReceive(_ interstitialAd: CaulyInterstitialAd!, isChargeableAd: Bool) {
        print("didReceiveInterstitialAd");
        _interstitialAd?.show()
        // _interstitialAd?.show()를 호출하지 않으면 Interstitial AD가 보여지지 않음
    }
    
    // Interstitial 형태의 광고가 닫혔을 때
    func didClose(_ interstitialAd: CaulyInterstitialAd!) {
        print("didCloseInterstitialAd")
        _interstitialAd = nil
    }
    
    // Interstitial 형태의 광고가 보여지기 직전
    func willShow(_ interstitialAd: CaulyInterstitialAd!) {
        print("willShowInterstitialAd")
    }
    
    // 광고 정보 수신 실패
    func didFail(toReceive interstitialAd: CaulyInterstitialAd!, errorCode: Int32, errorMsg: String!) {
        print("Recevie fail intersitial errorCode:\(errorCode)(\(errorMsg!))");
        _interstitialAd = nil
    }
}
```
{% endtab %}

{% tab title="Objective-C" %}
```objective-c
#import <UIKit/UIKit.h>

#import "Cauly.h"
#import "CaulyInterstitialAd.h"
#import <AppTrackingTransparency/AppTrackingTransparency.h>

@interface ViewController () <CaulyInterstitialAdDelegate>

@property (strong) CaulyInterstitialAd * interstitialAd;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    if (@available(iOS 14, *)) {
        [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {
            switch (status) {
                // 승인
                case ATTrackingManagerAuthorizationStatusAuthorized:
                    [self interstitialAdRequest];     // interstitial 광고 요청
                    break;
                // 거부
                case ATTrackingManagerAuthorizationStatusDenied:
                    break;
                // 제한
                case ATTrackingManagerAuthorizationStatusRestricted:
                    break;
                // 미결정
                default:
                    break;
            }
        }];
    }
    
    CaulyAdSetting * adSetting = [CaulyAdSetting globalSetting];
    [CaulyAdSetting setLogLevel:CaulyLogLevelDebug];            //  Cauly Log 레벨
    adSetting.appId                 = @"1234567";               //  App Store 에 등록된 App ID 정보 (필수)
    adSetting.appCode               = @"Cauly";                 //  Cauly AppCode
    adSetting.closeOnLanding        = YES;                      //  Landing 이동시 webview control lose 여부
}

{
    // Interstitial AD 호출 
    //  전면광고 객체 생성
    CaulyInterstitialAd * _interstitialAd = [[CaulyInterstitialAd alloc] initWithParentViewController:self]; 
    _interstitialAd.delegate = self;    //  전면 delegate 설정
    [_interstitialAd startInterstitialAdRequest];   //  전면광고 요청
}

// Interstitial AD API
#pragma mark - CaulyInterstitialAdDelegate

// 광고 정보 수신 성공
- (void)didReceiveInterstitialAd:(CaulyInterstitialAd *)interstitialAd isChargeableAd:(BOOL)isChargeableAd {
    NSLog(@"didReceiveInterstitialAd");
    [_interstitialAd show]; 
    // [_interstitialAd show];를 호출하지 않으면 Interstitial AD가 보여지지 않음
}

// Interstitial 형태의 광고가 닫혔을 때
- (void)didCloseInterstitialAd:(CaulyInterstitialAd *)interstitialAd {
    NSLog(@"didCloseInterstitialAd");
    _interstitialAd = nil;
}

// Interstitial 형태의 광고가 보여지기 직전
- (void)willShowInterstitialAd:(CaulyInterstitialAd *)interstitialAd {
    NSLog(@"willShowInterstitialAd");
}

// 광고 정보 수신 실패
- (void)didFailToReceiveInterstitialAd:(CaulyInterstitialAd *)interstitialAd errorCode:(int)errorCode errorMsg:(NSString *)errorMsg {
    NSLog(@"didFailToReceiveInterstitialAd : %d(%@)", errorCode, errorMsg);
    _interstitialAd = nil;
}

```
{% endtab %}
{% endtabs %}
