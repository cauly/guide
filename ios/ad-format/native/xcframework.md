# XCFramework(SPM, Cocoapods 공용)

{% hint style="info" %}
* Cauly SDK를 프로젝트에 추가해야합니다.
* APP 등록 후 부여받은 APP CODE\[발급ID]를 사용합니다.
{% endhint %}

### Native Ad 구현 샘플

{% tabs %}
{% tab title="Swift" %}
**Native Ad**

```swift
import UIKit
import CaulySDK
import AppTrackingTransparency

class ViewController: UIViewController, CaulyNativeAdDelegate {
    
    var nativeAd: CaulyNativeAd?
    var nativeAdItem: Dictionary<String, Any>?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
            if #available(iOS 14, *) {
                ATTrackingManager.requestTrackingAuthorization(completionHandler: { status in
                    switch status {
                    case .authorized:       // 승인
                        print("Authorized")
                        nativeAdRequest()    // native 광고 요청
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
    
    func nativeAdRequest() {
        // Native Ad 호출
        var nativeAd = CaulyNativeAd(parentViewController: self)        // 네이티브 광고 객체 생성
        nativeAd!.delegate = self       // 네이티브 광고 delegate 설정
        
        // imageSize argument를 통해 다양한 사이즈를 사용하여 구성할 수 있습니다.
        // 첫번째 argument는 요청할 nativa 광고의 갯수 eg) 1- 한개, 2- 두개
        nativeAd!.startRequest(2, nativeAdComponentType: CaulyNativeAdComponentType_Image, imageSize: "720x480")
    }
    
    // Native AD API
    // MARK: - CaulyNativeAdDelegate
    
    // Native Ad 수신 성공
    func didReceive(_ nativeAd: CaulyNativeAd!, isChargeableAd: Bool) {
        NSLog(@"didReceiveNativeAd");
    }
    
    // 광고 정보 수신 실패
    func didFail(toReceive nativeAd: CaulyNativeAd!, errorCode: Int32, errorMsg: String!) {
        NSLog("didFailToReceiveNativeAd : %d(%@)", errorCode, errorMsg)
    }
}
```

***

**Native Ad View 생성예제**

* Sample code to generate View

```swift
// 네이티브 광고 정보 수신 성공
func didReceive(_ nativeAd: CaulyNativeAd!, isChargeableAd: Bool) {
    NSLog("didReceiveNativeAd")

    guard let caulyNativeAd = nativeAd.nativeAdItem(at: 0) else {
        NSLog("receive native ad no fill")
        return
    }

    // JSON parse
    do {
        nativeAdItem = try JSONSerialization.jsonObject(
            with: Data(caulyNativeAd.nativeAdJSONString.utf8),
            options: []
        ) as? [String: Any]
    } catch {
        NSLog("nativeAdItem parse error: %@", error.localizedDescription)
        nativeAdItem = nil
    }

    // NativeAdViewViewController
    let areaSelectView = NativeAdViewViewViewController(
        nibName: "NativeAdViewViewController",
        bundle: nil
    )
    areaSelectView.nativeAd = nativeAd

    self.navigationController?.modalPresentationStyle = .currentContext
    self.present(areaSelectView, animated: false, completion: nil)

    areaSelectView.view.alpha = 0

    // ---- (1) 텍스트/옵션 UI 바인딩----
    areaSelectView.mainTitle.text = nativeAdItem?["title"] as? String
    areaSelectView.subTitle.text = nativeAdItem?["subtitle"] as? String
    areaSelectView.descriptionLabel.text = nativeAdItem?["description"] as? String
    areaSelectView.link = nativeAdItem?["link"] as? NSString
    areaSelectView.jsonStringTextView.text = caulyNativeAd.nativeAdJSONString

    if let opt = nativeAdItem?["opt"] as? String {
        areaSelectView.optOutButton.isHidden = (opt == "N")
    } else {
        areaSelectView.optOutButton.isHidden = true
    }

    // ---- (2) 이미지 URL 추출 ----
    let iconURLString = nativeAdItem?["icon"] as? String
    let imageURLString = nativeAdItem?["image"] as? String

    // ---- (3) 아이콘/메인 이미지 비동기 로드 후 UI 업데이트 ----
    let group = DispatchGroup()

    var loadedIcon: UIImage?
    var loadedImage: UIImage?

    group.enter()
    ImageLoader.shared.loadImage(from: iconURLString) { image in
        loadedIcon = image
        group.leave()
    }

    if imageURLString != nil {
        group.enter()
        ImageLoader.shared.loadImage(from: imageURLString) { image in
            loadedImage = image
            group.leave()
        }
    }

    group.notify(queue: .main) {
        // 이미지 반영
        areaSelectView.icon.image = loadedIcon
        areaSelectView.image.image = loadedImage

        UIView.animate(withDuration: 0.5) {
            areaSelectView.view.alpha = 1
        }
    }
}

// 광고 정보 수신 실패
func didFail(toReceive nativeAd: CaulyNativeAd!, errorCode: Int32, errorMsg: String!) {
    NSLog("didFailToReceiveNativeAd : %d(%@)", errorCode, errorMsg)
}

final class ImageLoader {
    static let shared = ImageLoader()

    private let cache = NSCache<NSString, UIImage>()

    func loadImage(from urlString: String?, completion: @escaping (UIImage?) -> Void) {
        guard let urlString, let url = URL(string: urlString) else {
            completion(nil)
            return
        }

        if let cached = cache.object(forKey: urlString as NSString) {
            completion(cached)
            return
        }

        URLSession.shared.dataTask(with: url) { [weak self] data, _, _ in
            guard let data, let image = UIImage(data: data) else {
                completion(nil)
                return
            }
            self?.cache.setObject(image, forKey: urlString as NSString)
            completion(image)
        }.resume()
    }
}
```

***

* NativeAdViewViewController

```swift
import UIKit
import CaulySDK

class NativeAdViewViewViewController: UIViewController {
    
    @IBOutlet var mainTitle: UILabel!
    @IBOutlet var subTitle: UILabel!
    @IBOutlet var descriptionLabel: UILabel!
    @IBOutlet var icon: UIImageView!
    @IBOutlet var image: UIImageView!
    @IBOutlet var jsonStringTextView: UITextView!
    @IBOutlet var optOutButton: UIButton!
    @IBOutlet var closeButton: UIButton!
    
    var link: NSString?
    var nativeAd: CaulyNativeAd?
    var nativeAdItem: CaulyNativeAdItem?
    
    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.
    }

    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        NSLog("Inform")
        nativeAd?.sendInform(nativeAdItem)
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
    
    
    @IBAction func didViewTouchUpInside(_ sender: UIButton) {
        NSLog("Native ad Clicked")
     
        UIView.animate(withDuration: 0.5, animations: {
            self.view.alpha = 0
        }, completion: {b in
            self.nativeAd?.click(self.nativeAdItem)
            
            self.view.alpha = 1
            self.presentingViewController?.dismiss(animated: false, completion: nil)
        })
    }
    
    @IBAction func closeButtonTouchUpInside(_ sender: UIButton) {
        self.presentingViewController?.dismiss(animated: false, completion: nil)
    }
    
    @IBAction func optOutButtonTouchUpInside(_ sender: UIButton) {
        self.nativeAd?.send(toOptOutLinkUrl: nativeAdItem)
    }
}
```
{% endtab %}

{% tab title="Objective-C" %}
**Native Ad**

```objective-c
#import <UIKit/UIKit.h>

@import CaulySDK;
#import <AppTrackingTransparency/AppTrackingTransparency.h>

@interface ViewController () <CaulyNativeAdDelegate>

@property (strong, nonatomic) CaulyNativeAd * nativeAd;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    if (@available(iOS 14, *)) {
        [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {
            switch (status) {
                // 승인
                case ATTrackingManagerAuthorizationStatusAuthorized:
                    [self nativeAdRequest];     // native 광고 요청
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
    adSetting.closeOnLanding        = YES;                      //  Landing 이동시 webview control lose 여부
}

- (void)nativeAdRequest {
    // Native Ad 호출
    CaulyNativeAd _nativeAd = [[CaulyNativeAd alloc] initWithParentViewController:self];
    _nativeAd.delegate = self;
    
    [_nativeAd startNativeAdRequest:2
    nativeAdComponentType:CaulyNativeAdComponentType_IconImage 
    imageSize:@"720x480"];
}


// Native AD API
#pragma mark - CaulyNativeAdDelegate

// Native Ad 수신 성공
- (void)didReceiveNativeAd:(CaulyNativeAd *)nativeAd isChargeableAd:(BOOL)isChargeableAd{
    NSLog(@"didReceiveNativeAd");
}

// 광고 정보 수신 실패
- (void)didFailToReceiveNativeAd:(CaulyNativeAd *)nativeAd errorCode:(int)errorCode errorMsg:(NSString *)errorMsg{
    NSLog(@"didFailToReceiveNativeAd");
}
```

***

**Native Ad View 생성예제**

* Sample code to generate View

```objective-c
// 광고 정보 수신 성공
- (void)didReceiveNativeAd:(CaulyNativeAd *)nativeAd isChargeableAd:(BOOL)isChargeableAd{
    NSLog(@"didReceiveNativeAd");
    CaulyNativeAdItem* caulyNativeAd = [nativeAd nativeAdItemAt:0];

    NSArray* allList= [nativeAd nativeAdItemList];
    NSLog(@"%@",caulyNativeAd);
    NSLog(@"%@",caulyNativeAd.nativeAdJSONString);

    for(CaulyNativeAdItem* adItem in allList){
        NSLog(@"%@",adItem.nativeAdJSONString);
    }

    NSError *error;
    NSDictionary *nativeAdItem = [NSJSONSerialization JSONObjectWithData:[caulyNativeAd.nativeAdJSONString dataUsingEncoding:NSUTF8StringEncoding] options:kNilOptions error:&error];

    NativeAdViewViewController *areaSelectView = [[NativeAdViewViewController alloc] initWithNibName:@"NativeAdViewViewController" bundle:nil];

    areaSelectView.nativeAd = nativeAd;

    self.navigationController.modalPresentationStyle = UIModalPresentationCurrentContext;
    [self presentModalViewController:areaSelectView animated:NO];
    areaSelectView.view.alpha = 0;
    [UIView animateWithDuration:0.5 animations:^{
        areaSelectView.view.alpha = 1;

        NSURL *url = [NSURL URLWithString:nativeAdItem[@"icon"]];
        NSData *data = [NSData dataWithContentsOfURL:url];
        UIImage *icon = [UIImage imageWithData:data];

        if(nativeAdItem[@"image"]){
            url = [NSURL URLWithString:nativeAdItem[@"image"]];
            data = [NSData dataWithContentsOfURL:url];
            UIImage *image = [UIImage imageWithData:data];
            [areaSelectView.image setImage:image];
        }

        [areaSelectView.icon setImage:icon];

        [areaSelectView.mainTitle setText:nativeAdItem[@"title"]];
        [areaSelectView.subTitle setText:nativeAdItem[@"subtitle"]];
        [areaSelectView.descriptionLabel setText:nativeAdItem[@"description"]];
        areaSelectView.link = nativeAdItem[@"link"];
        [areaSelectView.optOutButton setHidden:[nativeAdItem[@"opt"] isEqualToString:@"N"]];

        [areaSelectView.jsonStringTextView setText:caulyNativeAd.nativeAdJSONString];
    }];

}

// 광고 정보 수신 실패
- (void)didFailToReceiveNativeAd:(CaulyNativeAd *)nativeAd errorCode:(int)errorCode errorMsg:(NSString *)errorMsg{
    NSLog(@"didFailToReceiveNativeAd");
}
```

***

* NativeAdViewViewController

```objective-c
#import <UIKit/UIKit.h>
@import CaulySDK;

@interface NativeAdViewViewController : UIViewController

@property (weak, nonatomic) IBOutlet UILabel *mainTitle;
@property (weak, nonatomic) IBOutlet UILabel *subTitle;
@property (weak, nonatomic) IBOutlet UILabel *descriptionLabel;
@property (weak, nonatomic) IBOutlet UIImageView *icon;
@property (weak, nonatomic) IBOutlet UIImageView *image;
@property (weak, nonatomic) IBOutlet UITextView *jsonStringTextView;
@property (weak, nonatomic) IBOutlet UIButton *optOutButton;
@property (weak, nonatomic) IBOutlet UIButton *closeButton;

@property (nonatomic) NSString* link;
@property (assign) CaulyNativeAd* nativeAd;
@property (assign) CaulyNativeAdItem* nativeAdItem;
@end
```

***

```objective-c
#import "NativeAdViewViewController.h"

@interface NativeAdViewViewController ()

@end

@implementation NativeAdViewViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view from its nib.
}

- (void)viewDidAppear:(BOOL)animated{
    [super viewDidAppear:animated];
    NSLog(@"Inform");
    [_nativeAd sendInform:_nativeAdItem];
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

- (IBAction)closeButtonTouchUpInside:(id)sender {
    [self.presentingViewController dismissModalViewControllerAnimated:NO];
}

// optOut 링크 처리
- (IBAction)optOutButtonTouchUpInside:(id)sender {
    [_nativeAd sendToOptOutLinkUrl:_nativeAdItem];
}


- (IBAction)didViewTouchUpInside:(id)sender {
    NSLog(@"Native ad Clicked");

    [UIView animateWithDuration:0.5 animations:^{
        self.view.alpha = 0;
    } completion:^(BOOL b){
        [_nativeAd click:_nativeAdItem];

        self.view.alpha = 1;
        [self.presentingViewController dismissModalViewControllerAnimated:NO];
    }];
}

@end
```
{% endtab %}
{% endtabs %}
