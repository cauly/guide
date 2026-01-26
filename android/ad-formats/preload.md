# preload 배너

{% hint style="info" %}
Cauly 배너 광고는 Programmatic 방식과 XML 방식 2가지 형태의 구현과 사용이 가능합니다.
{% endhint %}

Programmatic 방식

{% tabs %}
{% tab title="Java" %}
```java
private static final String TAG = "CaulyExample";
private static final String APP_CODE = "YOUR_APP_CODE";

private RelativeLayout layout;          // activity_xml에서 id=layout
private CaulyAdBannerView bannerView;
private boolean isAdLoaded = false;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_java);

    Logger.setLogLevel(LogLevel.Debug);

    layout = findViewById(R.id.layout);

    loadBanner();
}
/**
 * 1) load()
 */
private void loadBanner() {
    isAdLoaded = false;
    cleanupBanner();

    CaulyAdInfo adInfo = new CaulyAdInfoBuilder(APP_CODE)
            .bannerHeight(CaulyAdInfoBuilder.FIXED)
            .effect("None")
            .build();

    bannerView = new CaulyAdBannerView(this);
    bannerView.setAdInfo(adInfo);
    bannerView.setAdViewListener(this);

    // load()의 parent로 루트 layout을 넘김
    bannerView.load(getApplication(), layout);

    Log.d(TAG, "banner load() called");
}
/**
 * 2) show()
 */
public void showBanner() {
    if (bannerView == null || !isAdLoaded) {
        Log.d(TAG, "show skipped: not loaded yet");
        return;
    }
    bannerView.show();
    Log.d(TAG, "banner show() called");
}

@Override
protected void onDestroy() {
    super.onDestroy();
    cleanupBanner();
}

private void cleanupBanner() {
    if (bannerView == null) return;
    try {
        if (layout != null) layout.removeView(bannerView);
    } catch (Throwable ignored) {}
    try {
        bannerView.destroy();
    } catch (Throwable ignored) {}
    bannerView = null;
    isAdLoaded = false;
}

// CaulyAdBannerViewListener
// 광고 동작에 대해 별도 처리가 필요 없는 경우,
// Activity의 "implements CaulyAdBannerViewListener" 부분 제거하고 생략 가능.
@Override
public void onReceiveAd(CaulyAdBannerView adView, boolean isChargeableAd) {
    Log.d(TAG, "banner AD received.");
    isAdLoaded = true;
}

@Override
public void onFailedToReceiveAd(CaulyAdBannerView adView, int errorCode, String errorMsg) {
    Log.d(TAG, "failed to receive banner AD. code=" + errorCode + ", msg=" + errorMsg);
    isAdLoaded = false;
}

@Override
public void onShowLandingScreen(CaulyAdBannerView adView) {
    Log.d(TAG, "banner AD landing screen opened.");
}

@Override
public void onCloseLandingScreen(CaulyAdBannerView adView) {
    Log.d(TAG, "banner AD landing screen closed.");
}

@Override
public void onClickAd(CaulyAdBannerView adView) {
    Log.d(TAG, "banner AD clicked.");
}

@Override
public void onTimeout(CaulyAdBannerView adView, String msg) {
    Log.d(TAG, "banner AD timeout. msg=" + msg);
    isAdLoaded = false;
}
```

'광고 삽입할 부분'.xml

```xml
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
</RelativeLayout>
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
* Lifecycle에 따라 CaulyAdBannerView의 destroy API를 호출하지 않을 경우, 광고 수신에 불이익을 받을 수 있습니다.

```java
// Lifecycle 처리 샘플코드
    @Override
    public void onDestroy() {
        if (adView != null) {
            adView.destroy();
            adView = null;
        }
    }
```



* 미디에이션 사용 시 카울리 배너 광고(CaulyAdBannerView)를 호출/교체할 때는, 광고를 삽입한 ViewGroup에서 기존 배너 View를 removeView() 한 뒤 destroy() 호출 후 null 처리해주시기 바랍니다. 또한 배너 광고 수신 실패(또는 timeout) 시에도, 이미 광고 View가 삽입되어 있다면 해당 View를 removeView() 해주세요.
{% endhint %}

## CaulyAdInfo 설정방법

<table data-header-hidden><thead><tr><th width="351">AdInfo</th><th>설명</th></tr></thead><tbody><tr><td>AdInfo</td><td>설명</td></tr><tr><td>AppCode</td><td>APP 등록 후 부여 받은 APP CODE[발급ID] 입력</td></tr><tr><td>Effect()</td><td><p>LeftSlide(기본값) : 왼쪽에서 오른쪽으로 슬라이드</p><p>RightSlide : 오른쪽에서 왼쪽으로 슬라이드</p><p>TopSlide : 위에서 아래로 슬라이드</p><p>BottomSlide : 아래서 위로 슬라이드</p><p>FadeIn : 전에 있던 광고가 서서히 사라지는 효과</p><p>Circle : 한 바퀴 롤링</p><p>None : 애니메이션 효과 없이 바로 광고 교체</p></td></tr><tr><td>reloadInterval()</td><td><p>min(기본값) : 20초) </p><p>max : 120 초</p></td></tr><tr><td>dynamicReloadInterval()</td><td><p>true(기본값) : 광고에 따라 노출 주기 조정할 수 있도록 하여 광고 수익 상승 효과 기대 </p><p>false : reloadInterval 설정 값으로 Rolling</p></td></tr><tr><td>bannerHeight()</td><td><p>Adaptive : 적응형 높이 형태 </p><p>Fixed : 높이 고정 형태</p></td></tr><tr><td>setBannerSize ()</td><td><p></p><p>Adaptive 지원하는 배너 사이즈 : (기본값)320x50, 320x100 </p><p>Fixed 지원하는 배너 사이즈 : (기본값)320x50, 320x100,300x250</p><ul><li>xml 방식의 경우 320x50만 지원합니다.</li></ul></td></tr><tr><td>threadPriority()</td><td>스레드 우선 순위 지정 : 1~10(기본값 : 5)</td></tr><tr><td>tagForChildDirectedTreatment(boolean)</td><td>14세 미만 일 때 true</td></tr><tr><td>gdprConsentAvailable(boolean)</td><td>gdpr 동의 일 때 true</td></tr><tr><td>allowLoadWhenScreenOff(boolean)</td><td>화면 off시 광고 요청 허용 일 때 true</td></tr><tr><td>enableLock(boolean)</td><td>잠금화면에서 광고 요청 허용 일 때 true</td></tr></tbody></table>
