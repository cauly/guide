# 배너

{% hint style="info" %}
Cauly 배너 광고는 Programmatic 방식과 XML 방식 2가지 형태의 구현과 사용이 가능합니다.
{% endhint %}

Programmatic 방식

{% tabs %}
{% tab title="Java" %}
```java
private CaulyAdView javaAdView;

@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_java);
    // Cauly 로그 수준 지정 : 로그의 상세함 순서는 다음과 같다.
    //LogLevel.Verbose > LogLevel.Debug > LogLevel.Info > LogLevel.Warn > LogLevel.Error > LogLevel.None
    Logger.setLogLevel(LogLevel.Debug);

    // CaulyAdInfo 상세 설정 방법은 하단 표 참조
    // 설정하지 않은 항목들은 기본값으로 설정됨
    CaulyAdInfo adInfo = new CaulyAdInfoBuilder(APP_CODE).
            bannerHeight(CaulyAdInfoBuilder.ADAPTIVE).
    setBannerSize(320,50). // 배너 지원 사이즈 (320x50 , 320x100, 300x250)
            build();

    // CaulyAdInfo를 이용, CaulyAdView 생성.
    javaAdView = new CaulyAdView(this);
    javaAdView.setAdInfo(adInfo);
    javaAdView.setAdViewListener(this);

    RelativeLayout rootView = (RelativeLayout) findViewById(R.id.java_root_view);
    // 예시 : 화면 하단에 배너 부착
    RelativeLayout.LayoutParams params = new RelativeLayout.LayoutParams(
            LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT);
    params.addRule(RelativeLayout.ALIGN_PARENT_BOTTOM);
    rootView.addView(javaAdView, params);
}

// CaulyAdViewListener
//	광고 동작에 대해 별도 처리가 필요 없는 경우,
//	Activity의 "implements CaulyAdViewListener" 부분 제거하고 생략 가능.
@Override
public void onReceiveAd(CaulyAdView adView, boolean isChargeableAd) {
    // 광고 수신 성공 & 노출된 경우 호출됨.
        Log.d("CaulyExample", "banner AD received.");
}

@Override
public void onFailedToReceiveAd(CaulyAdView adView, int errorCode, String errorMsg) {
    // 배너 광고 수신 실패할 경우 호출됨.
    Log.d("CaulyExample", "failed to receive banner AD.");
}

@Override
public void onShowLandingScreen(CaulyAdView adView) {
    // 광고 배너를 클릭하여 WebView를 통해 랜딩 페이지가 열린 경우 호출됨.
    Log.d("CaulyExample", "banner AD landing screen opened.");
}

@Override
public void onCloseLandingScreen(CaulyAdView adView) {
    // 광고 배너를 클릭하여 WebView를 통해 열린 랜딩 페이지가 닫힌 경우 호출됨.
    Log.d("CaulyExample", "banner AD landing screen closed.");
}

@Override
public void onClickAd(CaulyAdView adView) {
    // 광고 배너를 클릭할 경우 호출됨.
    Log.d("CaulyExample", "banner AD clicked.");
}

// Activity 버튼 처리
// - Java 배너 광고 갱신 버튼
public void onReloadJavaAdView(View button) {
    javaAdView.reload();
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

{% tab title="Kotlin" %}
```kotlin
private var javaAdView: CaulyAdView? = null

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_kotlin)
    // Cauly 로그 수준 지정 : 로그의 상세함 순서는 다음과 같다.
    //LogLevel.Verbose > LogLevel.Debug > LogLevel.Info > LogLevel.Warn > LogLevel.Error > LogLevel.None
    Logger.setLogLevel(Logger.LogLevel.Debug)

    // CaulyAdInfo 상세 설정 방법은 하단 표 참조
    // 설정하지 않은 항목들은 기본값으로 설정됨
    val adInfo = CaulyAdInfoBuilder(APP_CODE)
        .bannerHeight(CaulyAdInfoBuilder.FIXED)
        .setBannerSize(320,50)
        .build()


    // CaulyAdInfo를 이용, CaulyAdView 생성.
    javaAdView = CaulyAdView(this)
    javaAdView!!.setAdInfo(adInfo)
    javaAdView!!.setAdViewListener(this)

    val rootView = findViewById<View>(R.id.java_root_view) as RelativeLayout

    // 화면 하단에 배너 부착
    val params = RelativeLayout.LayoutParams(
        LinearLayout.LayoutParams.WRAP_CONTENT, LinearLayout.LayoutParams.WRAP_CONTENT
    )
    params.addRule(RelativeLayout.ALIGN_PARENT_BOTTOM)
    rootView.addView(javaAdView, params)
}

// CaulyAdViewListener
//	광고 동작에 대해 별도 처리가 필요 없는 경우,
//	Activity의 "implements CaulyAdViewListener" 부분 제거하고 생략 가능.
override fun onReceiveAd(adView: CaulyAdView, isChargeableAd: Boolean) {
    // 광고 수신 성공 & 노출된 경우 호출됨.
        Log.d("CaulyExample", "banner AD received.")
}

override fun onFailedToReceiveAd(adView: CaulyAdView, errorCode: Int, errorMsg: String) {
    // 배너 광고 수신 실패할 경우 호출됨.
    Log.d("CaulyExample", "failed to receive banner AD.")
}

override fun onShowLandingScreen(adView: CaulyAdView) {
    // 광고 배너를 클릭하여 WebView를 통해 랜딩 페이지가 열린 경우 호출됨.
    Log.d("CaulyExample", "banner AD landing screen opened.")
}

override fun onCloseLandingScreen(adView: CaulyAdView) {
    // 광고 배너를 클릭하여 WebView를 통해 열린 랜딩 페이지가 닫힌 경우 호출됨.
    Log.d("CaulyExample", "banner AD landing screen closed.")
}

override fun onClickAd(adView: CaulyAdView) {
    // 광고 배너를 클릭할 경우 호출됨.
    Log.d("CaulyExample", "banner AD clicked.")
}

// Activity 버튼 처리
// - Java 배너 광고 갱신 버튼
fun onReloadJavaAdView(button: View?) {
    javaAdView.reload()
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



XML 방식

{% hint style="info" %}
설정하지 않은 항목들은 기본값으로 설정됩니다.

banner size는 320x50만 지원합니다.
{% endhint %}

```xml
<com.fsn.cauly.CaulyAdView
    xmlns:app="http://schemas.android.com/apk/res/[개발자 프로젝트 PACKAGENAME]"
    android:id="@+id/xmladview"
    android:layout_width="wrap_content"
    android:layout_height="match_parent"
    android:layout_alignParentBottom="true"
    app:appcode="CAULY"
    app:bannerHeight="Adaptive" />
```

'project > res > values' 에 'arrts.xml' 파일 생성 후 아래 코드 추가

```xml
<declare-styleable name="com.cauly.android.ad.AdView">
    <attr name="appcode" format="string" />
    <attr name="effect" format="string" />
    <attr name="dynamicReloadInterval" format="boolean" />
    <attr name="reloadInterval" format="integer" />
    <attr name="threadPriority" format="integer" />
    <attr name="bannerHeight" format="string" />
</declare-styleable>
```



{% hint style="warning" %}
* Lifecycle에 따라 CaulyAdView의 pause/resume/destroy API를 호출하지 않을 경우, 광고 수신에 불이익을 받을 수 있습니다.
* 미디에이션 사용시 카울리배너광고 호출하면 광고 삽입한 부분의 View를 removeView 및 CaulyAdView의 destroy, null 처리 해주시길 바랍니다. 또한 배너 광고 수신 실패할 경우 광고 삽입한 부분의 View를 removeView 해주세요.
{% endhint %}

## CaulyAdInfo 설정방법

<table data-header-hidden><thead><tr><th width="351">AdInfo</th><th>설명</th></tr></thead><tbody><tr><td>AdInfo</td><td>설명</td></tr><tr><td>AppCode</td><td>APP 등록 후 부여 받은 APP CODE[발급ID] 입력</td></tr><tr><td>Effect()</td><td>LeftSlide(기본값) : 왼쪽에서 오른쪽으로 슬라이드 RightSlide : 오른쪽에서 왼쪽으로 슬라이드 TopSlide : 위에서 아래로 슬라이드 BottomSlide : 아래서 위로 슬라이드 FadeIn : 전에 있던 광고가 서서히 사라지는 효과 Circle : 한 바퀴 롤링 None : 애니메이션 효과 없이 바로 광고 교체</td></tr><tr><td>reloadInterval()</td><td>min(기본값) : 20초) max : 120 초</td></tr><tr><td>dynamicReloadInterval()</td><td>true(기본값) : 광고에 따라 노출 주기 조정할 수 있도록 하여 광고 수익 상승 효과 기대 false : reloadInterval 설정 값으로 Rolling</td></tr><tr><td>bannerHeight()</td><td>Adaptive : 적응형 높이 형태 Fixed : 높이 고정 형태</td></tr><tr><td>setBannerSize ()</td><td><p></p><p>Adaptive 지원하는 배너 사이즈 : (기본값)320x50, 320x100 Fixed 지원하는 배너 사이즈 : (기본값)320x50, 320x100,300x250</p><ul><li>xml 방식의 경우 320x50만 지원합니다.</li></ul></td></tr><tr><td>threadPriority()</td><td>스레드 우선 순위 지정 : 1~10(기본값 : 5)</td></tr><tr><td>tagForChildDirectedTreatment(boolean)</td><td>14세 미만 일 때 true</td></tr><tr><td>gdprConsentAvailable(boolean)</td><td>gdpr 동의 일 때 true</td></tr></tbody></table>
