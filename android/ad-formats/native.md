# 네이티브



{% tabs %}
{% tab title="Java" %}
## BASE 방식

```java
public class JavaActivity extends Activity implements CaulyCloseAdListener {

  private static final String APP_CODE = "CAULY"; // 광고 요청을 위한 App Code
    private ViewGroup native_container;
    private CaulyNativeAdView nativeAd;
    private WebView webView;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        native_container = (ViewGroup) findViewById(R.id.native_container);
        request_Native_Base(APP_CODE);
    }

    private void request_Native_Base(String APP_CODE) {
        if (nativeAd != null) {
            nativeAd.destroy();
            native_container.removeView(nativeAd);
        }
        if (webView != null) {
            webView.removeAllViews();
            webView.clearCache(true);
            webView.loadUrl("about:blank");
        }
        // Request Native AD
        // 네이티브 애드에 보여질 디자인을 정의하고 세팅하는 작업을 수행한다. (icon, image, title, subtitle, description ...)
        // CaulyNativeAdViewListener 를 등록하여 onReceiveNativeAd or onFailedToReceiveNativeAd 로 네이티브광고의 상태를 전달받는다.
        CaulyAdInfo  caulyNativeAdInfoBuilder = new CaulyNativeAdInfoBuilder(APP_CODE) // 광고 요청을 위한 App Code
                .layoutID(R.layout.activity_native_iconlist) // 네이티브애드에 보여질 디자인을 작성하여 등록한다.
                .mainImageID(R.id.main_image) //메인이미지
                .titleID(R.id.title)         // 제목 등록
                .subtitleID(R.id.subtitle)   // 부제목 등록
                .textID(R.id.description)
                .iconImageID(R.id.icon)      // 아이콘 등록
                .adRatio("720x720") //메인이미지 비율설정  안할경우, default: 720x480  or 480x720
                .build();
        nativeAd = new CaulyNativeAdView(this);
        nativeAd.setAdInfo(caulyNativeAdInfoBuilder);
        nativeAd.setAdViewListener(new CaulyNativeAdViewListener() {
            // 네이티브애드가 정상적으로 수신되었을 떄, 호출된다.
            @Override
            public void onReceiveNativeAd(CaulyNativeAdView adView, boolean isChargeableAd) {
                adView.attachToView(native_container);
            }
            // 네이티브애드가 없거나, 네트웍상의 이유로 정상적인 수신이 불가능 할 경우 호출이 된다.
            @Override
            public void onFailedToReceiveNativeAd(CaulyNativeAdView adView, int errorCode, String errorMsg) {

            }
            
            // 네이티브 애드가 클릭되었을 때, 호출된다.
            @Override
            public void onClickNativeAd(CaulyNativeAdView adView) {
                Log.d("CaulyExample", "naive AD clicked.");
            }
        });
        nativeAd.request();
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        nativeAd.destroy();
    }
}
```

### activity\_native\_iconlist.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content">

    <RelativeLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        tools:ignore="WebViewLayout">

        <WebView
            android:id="@+id/main_image"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_alignParentTop="true" />

        <RelativeLayout
            android:layout_width="wrap_content"
            android:layout_height="wrap_content">

            <ImageView
                android:id="@+id/icon"
                android:layout_width="wrap_content"
                android:layout_height="match_parent"
                android:scaleType="fitXY" />

            <ImageView
                android:id="@+id/sponsor"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_centerHorizontal="true"
                android:visibility="gone" />
        </RelativeLayout>

        <TextView
            android:id="@+id/title"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginLeft="114dp"
            android:layout_marginTop="30dp"
            android:layout_marginRight="14dp"
            android:lines="1"
            android:textColor="#000000"
            android:textSize="13dp" />

        <TextView
            android:id="@+id/subtitle"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginLeft="114dp"
            android:layout_marginTop="50dp"
            android:layout_marginRight="14dp"
            android:lines="2"
            android:textColor="#8a837e"
            android:textSize="10dp" />

        <TextView
            android:id="@+id/description"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_alignParentRight="true"
            android:layout_marginRight="14dp"
            android:layout_marginBottom="9dp"
            android:lines="1"
            android:textColor="#e15052"
            android:textSize="15dp" />
    </RelativeLayout>

</RelativeLayout>
```

## Custom 방식

1. CaulyAdInfo 생성
2. CaulyCustomAd 객체 생성후, CaulyAdInfo 적용
3. CaulyCustomAd의 CaulyCustomAdListener등록
4. CaulyCustomAd의 JSON 타입의 광고정보 요청
5. 수신한 JSON Data Format.
6. 소재가 화면에 보여지게 되면 노출로그 전송
   * sendImpressInform(광고ID)
7. 광고 클릭시, linkUrl 로 이동
   * CaulyBrowserUtil.openBrowser(Context, linkUrl)
8. 구현 형태
   * WebView 형태로 구현해주세요
9. 파싱방법
   * List\<HashMap\<KEY,VALUE>> 방식으로 가져오는 경우, `mCaulyAdView.getAdsList();`
   * Raw JSON String을 직접가져 경우 `mCaulyAdView.getJsonString();`

```java
{
    "ads":[
    {
        "id":"광고ID",
        "ad_charge_type":"0 : 유료광고, 100: 하우스 광고",
        "icon":"아이콘 이미지",
        "image":"메인 이미지",
        "title":"제목",
        "subtitle":"부제목",
        "description":"설명"
        "linkUrl":"랜딩 페이지 URL",
        "optout_img_url":"optout 버튼 이미지(png)",
        "optout_url":"optout page url",
        "optout":"관심기반 광고 여부 , true or false"
    }
    ]
}
```

### WebView 형태

```java
WebView web = (WebView)findViewById(R.id.webbanner);
web.getSettings().setJavaScriptEnabled(true);
web.setVerticalScrollBarEnabled(false);
web.setHorizontalScrollBarEnabled(false);
web.getSettings().setUseWideViewPort(true);
web.getSettings().setLoadsImagesAutomatically(true);
web.getSettings().setLoadWithOverviewMode(true);
web.getSettings().setBuiltInZoomControls(true);
web.getSettings().setDomStorageEnabled(true);
web.loadUrl(item.image);
```

### 파싱방법

```java
{
CaulyAdInfo adInfo = new CaulyNativeAdInfoBuilder(APP_CODE)
                .iconImageID(R.id.main_image) // 아이콘 이미지를 사용할 경우  
                .mainImageID(R.id.icon)       //메인 이미지를 사용할 경우
                .adRatio("720x720")			  //메인이미지 비율설정  안할경우, default: 720x480  or 480x720
        .build();
            mCaulyAdView = new CaulyCustomAd(this);
               mCaulyAdView.setAdInfo(adInfo);
            mCaulyAdView.setCustomAdListener(new CaulyCustomAdListener() {
            @Override
            public void onShowedAd() {
            }

            //광고 호출이 성공할 경우, 호출된다.
            @Override
            public void onLoadedAd(boolean isChargeableAd) {
                List<HashMap<String, Object>> adlist = caulyCustomAd.getAdsList();
                if (adlist != null && adlist.size() > 0) {
                    for (HashMap<String, Object> map : adlist) {
                        AdItem data = new AdItem();
                        data.id = (String) map.get("id");
                        data.image = (String) map.get("image");
                        data.linkUrl = (String) map.get("linkUrl");
                        data.image_content_type = (String) map.get("image_content_type");
                        addWebBanner(data, caulyCustomAd);
                    }
                }
            }

            // 광고 호출이 실패할 경우, 호출된다.
            @Override
            public void onFailedAd(int errCode, String errMsg) {
            }

            // 광고가 클릭된 경우, 호출된다.
            @Override
            public void onClikedAd() {
            }

        });
     // CaulyCustomAd.INTERSTITIAL_PORTRAIT,CaulyCustomAd.NATIVE_PORTRAIT,CaulyCustomAd.NATIVE_LANDSCAPE
     CaulyCustomAd.requestAdData (type, ad_count);
}

private void addWebBanner(final AdItem item, CaulyCustomAd mCaulyAdView, boolean check) {

    WebView web = findViewById(R.id.webbanner);
    web.getSettings().setJavaScriptEnabled(true);
    web.setVerticalScrollBarEnabled(false);
    web.setHorizontalScrollBarEnabled(false);
    web.getSettings().setUseWideViewPort(true);
    web.getSettings().setLoadsImagesAutomatically(true);
    web.getSettings().setLoadWithOverviewMode(true);
    web.getSettings().setBuiltInZoomControls(false);
    web.getSettings().setDomStorageEnabled(true);
    web.getSettings().setLayoutAlgorithm(WebSettings.LayoutAlgorithm.NORMAL);
    web.setScrollBarStyle(WebView.SCROLLBARS_OUTSIDE_OVERLAY);
    web.setScrollbarFadingEnabled(false);
    web.loadUrl(item.image);
    web.setOnTouchListener(new View.OnTouchListener() {
        @Override
        public boolean onTouch(View v, MotionEvent event) {
            if (event.getAction() == MotionEvent.ACTION_UP) {
                CaulyBrowserUtil.openBrowser(MainActivity.this, item.linkUrl);
            }
            return true;
        }
    });

    //광고 유효노출 호출
    mCaulyAdView.sendImpressInform(item.id);
}
```


{% endtab %}

{% tab title="Kotlin" %}
## BASE 방식

```kotlin
class KotlinNativeViewActivity : AppCompatActivity(), CaulyNativeAdViewListener {

    companion object {
        // 광고 요청을 위한 App Code
        private const val APP_CODE = "CAULY"
    }

    private var native_container: ViewGroup? = null


    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_kotlin)
        native_container = findViewById<View>(R.id.native_container) as ViewGroup
        request_Native_Base(APP_CODE)
    }

    private fun request_Native_Base(APP_CODE: String?) {
        // Request Native AD
        // 네이티브 애드에 보여질 디자인을 정의하고 세팅하는 작업을 수행한다. (icon, image, title, subtitle, description ...)
        // CaulyNativeAdViewListener 를 등록하여 onReceiveNativeAd or onFailedToReceiveNativeAd 로 네이티브광고의 상태를 전달받는다.
        val caulyNativeAdInfoBuilder = CaulyNativeAdInfoBuilder(APP_CODE) // 광고 요청을 위한 App Code
            .layoutID(R.layout.activity_native_iconlist) // 네이티브애드에 보여질 디자인을 작성하여 등록한다.
            .mainImageID(R.id.main_image) // 메인이미지
            .iconImageID(R.id.icon)       // 아이콘 등록
            .titleID(R.id.title)          // 제목 등록
            .subtitleID(R.id.subtitle)    // 부제목 등록
            .adRatio("720x720") //메인이미지 비율설정  안할경우, default: 720x480  or 480x720
            .sponsorPosition(R.id.sponsor, CaulyAdInfo.Direction.CENTER)
            .build()

        val nativeAd = CaulyNativeAdView(this)
        nativeAd.adInfo = caulyNativeAdInfoBuilder
        nativeAd.setAdViewListener(this)
        nativeAd.request()

    }

    // 네이티브애드가 없거나, 네트웍상의 이유로 정상적인 수신이 불가능 할 경우 호출이 된다.
    override fun onFailedToReceiveNativeAd(adView: CaulyNativeAdView, errorCode: Int, errorMsg: String) {
    }

    // 네이티브애드가 정상적으로 수신되었을 떄, 호출된다.
    override fun onReceiveNativeAd(adView: CaulyNativeAdView, isChargeableAd: Boolean) {
        adView.attachToView(native_container) //지정된 위치에 adView를 붙인다.
    }
    
    // 네이티브 애드가 클릭되었을 때, 호출된다.
    override fun onClickNativeAd(adView: CaulyNativeAdView) {
        Log.d("CaulyExample", "naive AD clicked.")
    }

    override fun onDestroy() {
        super.onDestroy()
        nativeAd!!.destroy()
    }
}
```

### activity\_native\_iconlist.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:background="@drawable/listcolor" >

    <RelativeLayout
        android:layout_width="fill_parent"
        android:layout_height="100dp" >
         <RelativeLayout
        android:layout_width="100dp"
        android:layout_height="100dp" >
         <ImageView
            android:id="@+id/icon"
            android:layout_width="100dp"
            android:layout_height="100dp"
            android:scaleType="fitXY"
           />
         <ImageView
            android:id="@+id/sponsor"
            android:layout_width="27dp"
            android:layout_height="9dp"
            android:layout_centerHorizontal="true"
           />
        </RelativeLayout>
         
           <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
             android:layout_marginLeft="114dp"
               android:layout_marginTop="13dp"
            android:textColor="#8a837e"
             android:lines="1"
             android:text="sponsor"
            android:textSize="10dp" />
        <TextView
            android:id="@+id/title"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
             android:layout_marginLeft="114dp"
              android:layout_marginRight="14dp"
               android:layout_marginTop="30dp"
            android:textColor="#000000"
             android:lines="1"
            android:textSize="13dp" />
         <TextView
            android:id="@+id/subtitle"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:layout_marginLeft="114dp"
              android:layout_marginRight="14dp"
               android:layout_marginTop="50dp"
               android:lines="2"
            android:textColor="#8a837e"
            android:textSize="10dp" />
         
          <TextView
            android:id="@+id/description"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
              android:layout_marginRight="14dp"
              android:layout_alignParentRight="true"
                 android:layout_alignParentBottom="true"
               android:layout_marginBottom="9dp"
               android:lines="1"
            android:textColor="#e15052"
            android:textSize="15dp" />
    </RelativeLayout>

</RelativeLayout>
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
광고영역에 WebView 권장 및 Lifecycle에 따라 pause/resume/destroy API를 호출하지 않을 경우, 광고 수신에 불이익을 받을 수 있습니다.
{% endhint %}
