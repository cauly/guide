# Ad Settings

* SKAdNetwork 를 지원하게 되면서 아래 초기화 부분에서 반드시 adSetting.appId 로 App Store 의 App ID 정보를 입력해주셔야 합니다.
* 만약, 아직 출시 전 앱인 경우는 0 으로 지정할 수는 있으나 App Store 에 등록된 앱인 경우에는 반드시 입력해야 합니다.
* 추가적으로, 기존에는 로그 레벨이 CaulyLogLevelRelease 였으나 CaulyLogLevelInfo 로 변경되었습니다.

{% tabs %}
{% tab title="Swift" %}
```swift
{
   // 상세 설정 항목들은 하단 표 참조, 설정되지 않은 항목들은 기본값으로 설정됩니다.
   let caulySetting = CaulyAdSetting.global();
   CaulyAdSetting.setLogLevel(CaulyLogLevelInfo)//  Cauly Log 레벨
   caulySetting?.appId = "1234567"                 //  App Store 에 등록된 App ID 정보 (필수)
   caulySetting?.appCode = "CAULY"                 //  Cauly 로부터 발급 받은 ID 입력
   caulySetting?.animType = CaulyAnimNone          //  화면 전환 효과
   
   //  App 으로 이동할 때 webview popup 창을 자동으로 닫아줍니다. 기본값은 false입니다.
   caulySetting?.closeOnLanding = true
}

 // 랜딩 화면 표시
func willShowLanding(_ adView: CaulyAdView!) {
     print("willShowLandingView")
}

// 랜딩 화면이 닫혔을 때
func didCloseLanding(_ adView: CaulyAdView!) {
     print("didCloseLandingView")
}
```
{% endtab %}

{% tab title="Objective-C" %}
```objective-c
{
    // 상세 설정 항목들은 하단 표 참조, 설정되지 않은 항목들은 기본값으로 설정됩니다.
    CaulyAdSetting * adSetting = [CaulyAdSetting globalSetting];
    [CaulyAdSetting setLogLevel:CaulyLogLevelInfo];               //  Cauly Log 레벨
    adSetting.appId = @"1234567";           //  App Store 에 등록된 App ID 정보 (필수)
    adSetting.appCode = @"CAULY";             //  Cauly 로부터 발급 받은 ID 입력
    adSetting.animType = CaulyAnimNone;        //  화면 전환 효과	     
    
    // app으로 이동할 때 webview popup창을 자동으로 닫아줍니다. 기본값은 NO입니다.
    adSetting.closeOnLanding = YES
}

// 랜딩 화면 표시
- (void)willShowLandingView:(CaulyAdView *)adView {
    NSLog(@"willShowLandingView");
}

// 랜딩 화면이 닫혔을 때
- (void)didCloseLandingView:(CaulyAdView *)adView {
    NSLog(@"didCloseLandingView");
}
```
{% endtab %}
{% endtabs %}

### CaulyAdInfo 설정방법

| 속성                   | 설명                                                                                                                                                                                                                                                                               |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| appCode              | APP 등록 후 부여 받은 APP CODE\[발급ID] 입력                                                                                                                                                                                                                                                |
| animType             | <p><strong>광고 교체 애니메이션 효과</strong><br>CaulyAnimNone (기본값) : 효과 없음<br>CaulyAnumCurlDown : 아래쪽으로 말려 내려가는 효과<br>CaulyAnumCurlUp : 위쪽으로 말려 올라가는 효과<br>CaulyAnimFadeOut : 서서히 사라지는 효과<br>CaulyAnimFlipFromLeft : 왼쪽에서 회전하며 나타나는 효과<br>CaulyAnimFlipFromRight : 오른쪽에서 회전하며 나타나는 효과</p> |
| adSize               | <p>CaulyAdSize_IPhone : 320 x 50<br>CaulyAdSize_IPhoneLarge : 320 x 100<br>CaulyAdSize_IPhoneMediumRect : 300 x 250<br>CaulyAdSize_IPadLarge : 728 x 90<br>CaulyAdSize_IPadSmall : 468 x 60</p>                                                                                  |
| reloadTime           | <p>CaulyReloadTime_30 (기본값) : 30초<br>CaulyReloadTime_60 : 60초<br>CaulyReloadTime_120 : 120초</p>                                                                                                                                                                                  |
| useDynamicReloadTime | <p>YES (기본값) : 광고에 따라 노출 주기 조정할 수 있도록 하여 광고 수익 상승 효과 기대<br>NO : 설정 시 reloadTime 설정 값으로 Rolling</p>                                                                                                                                                                               |
