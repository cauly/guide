# XCFramework(SPM, Cocoapods 공용)

{% hint style="info" %}
* Cauly SDK를 프로젝트에 추가해야합니다.
* APP 등록 후 부여받은 APP CODE\[발급ID]를 사용합니다.
{% endhint %}

### 배너 광고 사이즈

| CaulyAdSize                   | Size (width x height) |
| ----------------------------- | --------------------- |
| CaulyAdSize\_IPhone           | 320 x 50              |
| CaulyAdSize\_IPhoneLarge      | 320 x 100             |
| CaulyAdSize\_IPhoneMediumRect | 300 x 250             |
| CaulyAdSize\_IPadLarge        | 728 x 90              |
| CaulyAdSize\_IPadSmall        | 468 x 60              |

### Banner Ad 구현 샘플

{% tabs %}
{% tab title="Swift" %}
```swift
import UIKit
import CaulySDK
import AppTrackingTransparency

class ViewController: UIViewController, CaulyAdViewDelegate {
    
    @IBOutlet var bannerView: UIView!
    var adView: CaulyAdView?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
            if #available(iOS 14, *) {
                ATTrackingManager.requestTrackingAuthorization(completionHandler: { status in
                    switch status {
                    case .authorized:       // 승인
                        print("Authorized")
                        bannerAdRequest()        // banner 광고 요청
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
        adSetting?.appId = "1234567"                        //  App Store 에 등록된 App ID 정보 (필수)
        adSetting?.appCode = "Cauly"                        //  Cauly AppCode
        adSetting?.animType = CaulyAnimNone                 // 화면 전환 효과
        adSetting?.adSize = CaulyAdSize_IPhone              // 광고 크기
        
        adSetting?.reloadTime = CaulyReloadTime_30          // 광고 자동 갱신 시간 (기본값)
        adSetting?.useDynamicReloadTime = true;             // 광고 자동 갱신 사용 여부 (기본값)
        adSetting?.closeOnLanding = true;                   // 광고 랜딩 시 WebView 제거 여부
    }
    
    func bannerAdRequest() {
        // Banner AD 호출 
        //  CaulyAdView 객체 생성
        let caulyView:CaulyAdView = CaulyAdView.init(parentViewController: self)     
        view.addSubview(caulyView)
        caulyView.delegate = self            //  delegate 설정
        caulyView.startBannerAdRequest()    //  배너광고 요청
    }
    
    // Banner AD API
    // MARK: - CaulyAdViewDelegate
    
    // 광고 정보 수신 성공
    func didReceiveAd(_ adView: CaulyAdView!, isChargeableAd: Bool) {
        print("Loaded didReceiveAd callback")
    }
    
    // 광고 정보 수신 실패
    func didFail(toReceiveAd adView: CaulyAdView!, errorCode: Int32, errorMsg: String!) {
        print("didFailToReceiveAd:\(errorCode)(\(errorMsg!))");
    }
}
```
{% endtab %}

{% tab title="Objective-C" %}
```objective-c
#import <UIKit/UIKit.h>

@import CaulySDK;
#import <AppTrackingTransparency/AppTrackingTransparency.h>

@interface ViewController () <CaulyAdViewDelegate>

@property (weak, nonatomic) IBOutlet UIView *bannerView;
@property (strong, nonatomic) CaulyAdView * adView;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    if (@available(iOS 14, *)) {
        [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {
            switch (status) {
                // 승인
                case ATTrackingManagerAuthorizationStatusAuthorized:
                    [self bannerAdRequest];        // banner 광고 요청
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
    [CaulyAdSetting setLogLevel:CaulyLogLevelTrace];            //  Cauly Log 레벨
    adSetting.appId                 = @"1234567";               //  App Store 에 등록된 App ID 정보 (필수)
    adSetting.appCode               = @"Cauly";                 //  Cauly AppCode
    adSetting.animType              = CaulyAnimNone             // 화면 전환 효과
    adSetting.adSize                = CaulyAdSize_IPhone;       //  광고 크기
    
    adSetting.reloadTime            = CaulyReloadTime_30;       //  광고 갱신 시간
    adSetting.useDynamicReloadTime  = YES;                      //  동적 광고 갱신 허용 여부
    adSetting.closeOnLanding        = YES;                      //  Landing 이동시 webview control lose 여부
    
}

- (void)bannerAdRequest {
    // Banner AD 호출 
    //  CaulyAdView 객체 생성
    CaulyAdView *_adView = [[CaulyAdView alloc] initWithParentViewController:self];      
    [self.view addSubview:_adView];
    _adView.delegate = self;                                                //  delegate 설정
    [_adView startBannerAdRequest];                                         //  배너광고 요청
}

// Banner AD API
#pragma mark - CaulyAdViewDelegate

// 광고 정보 수신 성공
- (void)didReceiveAd:(CaulyAdView *)adView isChargeableAd:(BOOL)isChargeableAd{
    NSLog(@"didReceiveAd");
}

// 광고 정보 수신 실패
- (void)didFailToReceiveAd:(CaulyAdView *)adView errorCode:(int)errorCode errorMsg:(NSString *)errorMsg {
    NSLog(@"didFailToReceiveAd : %d(%@)", errorCode, errorMsg);
}
```
{% endtab %}
{% endtabs %}
