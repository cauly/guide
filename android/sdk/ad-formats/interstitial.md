# 전면



{% tabs %}
{% tab title="Java" %}
## FullScreen Type 전면 광고

```java
// 아래와 같이 전면 광고 표시 여부 플래그를 사용하면, 전면 광고 수신 후, 노출 여부를 선택할 수 있다.
private boolean showInterstitial = false;

// Activity 버튼 처리
// - 전면 광고 요청 버튼
public void onRequestInterstitial(View button) {

    // CaulyAdInfo 생성
    CaulyAdInfo adInfo = new CaulyAdInfoBuilder(APP_CODE)
                      // statusbarHide(boolean) : 상태바 가리기 옵션(true : 상태바 가리기)
                        .build();

    // 전면 광고 생성
    CaulyInterstitialAd interstial = new CaulyInterstitialAd();
    interstial.setAdInfo(adInfo);
    interstial.setInterstialAdListener(this);

    // 전면광고 노출 후 back 버튼 사용을 막기 원할 경우 disableBackKey();을 추가한다
    // 단, requestInterstitialAd 위에서 추가되어야 합니다.
    // interstitialAd.disableBackKey();

    // 광고 요청. 광고 노출은 CaulyInterstitialAdListener의 onReceiveInterstitialAd에서 처리한다.
    interstial.requestInterstitialAd(this);
    // 전면 광고 노출 플래그 활성화
    showInterstitial = true;
}

// - 전면 광고 노출 취소 버튼
public void onCancelInterstitial(View button) {
    // 전면 광고 노출 플래그 비활성화
    showInterstitial = false;
}

// CaulyInterstitialAdListener
//전면 광고의 경우, 광고 수신 후 자동으로 노출되지 않으므로,
//반드시 onReceiveInterstitialAd 메소드에서 노출 처리해 주어야 한다.
@Override
public void onReceiveInterstitialAd(CaulyInterstitialAd ad, boolean isChargeableAd) {
    // 광고 수신 성공한 경우 호출됨.
    // 수신된 광고가 무료 광고인 경우 isChargeableAd 값이 false 임.
    if (isChargeableAd == false) {
        Log.d("CaulyExample", "free interstitial AD received.");
    } else {
        Log.d("CaulyExample", "normal interstitial AD received.");
    }
    // 노출 활성화 상태이면, 광고 노출
    if (showInterstitial)
        ad.show();
    else
        ad.cancel();
}

@Override
public void onFailedToReceiveInterstitialAd(CaulyInterstitialAd ad, int errorCode, String errorMsg) {
    // 전면 광고 수신 실패할 경우 호출됨.
    Log.d("CaulyExample", "failed to receive interstitial AD.");
}

@Override
public void onClosedInterstitialAd(CaulyInterstitialAd ad) {
    // 전면 광고가 닫힌 경우 호출됨.
    Log.d("CaulyExample", "interstitial AD closed.");
}

@Override
public void onLeaveInterstitialAd(CaulyInterstitialAd ad) {
    // 전면 광고를 클릭하여 앱을 벗어났을 경우 호출됨.
    Log.d("CaulyExample", "interstitial AD onLeaveInterstitialAd.");
}

@Override
public void onClickInterstitialAd(CaulyInterstitialAd ad) {
    // 전면 광고를 클릭할 경우 호출됨.
    Log.d("CaulyExample", "interstitial AD onClickInterstitialAd.");
}
```

## Close Ad Type 전면 광고

```java
public class JavaActivity extends Activity implements CaulyCloseAdListener {

    private static final String APP_CODE = "CAULY"; // 광고 요청을 위한 App Code
    CaulyCloseAd mCloseAd;                         // CloseAd광고 객체

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_java);
        //CloseAd 초기화 
        CaulyAdInfo closeAdInfo = new CaulyAdInfoBuilder(APP_CODE)
                          // statusbarHide(boolean) : 상태바 가리기 옵션(true : 상태바 가리기)
                            .build();	
        mCloseAd = new CaulyCloseAd();
                    
        /*  Optional
        //원하는 버튼의 문구를 설정 할 수 있다.  
        mCloseAd.setButtonText("취소", "종료");
        //원하는 텍스트의 문구를 설정 할 수 있다.  
        mCloseAd.setDescriptionText("종료하시겠습니까?");
                    */
        mCloseAd.setAdInfo(closeAdInfo);
        mCloseAd.setCloseAdListener(this); // CaulyCloseAdListener 등록
        // 종료광고 노출 후 back버튼 사용을 막기 원할 경우 disableBackKey();을 추가한다
        // mCloseAd.disableBackKey();	
    }

    @Override
    protected void onResume() {
        super.onResume();
        if (mCloseAd != null)
            mCloseAd.resume(this); // 필수 호출 
    }

    // Back Key가 눌러졌을 때, CloseAd 호출
    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if (keyCode == KeyEvent.KEYCODE_BACK) {
            // 앱을 처음 설치하여 실행할 때, 필요한 리소스를 다운받았는지 여부.
            if (mCloseAd.isModuleLoaded()) {
                mCloseAd.show(this);
            } else {
                // 광고에 필요한 리소스를 한번만  다운받는데 실패했을 때 앱의 종료팝업 구현
                showDefaultClosePopup();
            }
            return true;
        }
        return super.onKeyDown(keyCode, event);
    }

    private void showDefaultClosePopup() {
        new AlertDialog.Builder(this).setTitle("").setMessage("종료 하시겠습니까?")
                .setPositiveButton("예", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        finish();
                    }
                })
                .setNegativeButton("아니요", null)
                .show();
    }

    // CaulyCloseAdListener
    @Override
    public void onFailedToReceiveCloseAd(CaulyCloseAd ad, int errCode, String errMsg) {
    }

    // CloseAd의 광고를 클릭하여 앱을 벗어났을 경우 호출되는 함수이다. 
    @Override
    public void onLeaveCloseAd(CaulyCloseAd ad) {
    }

    // CloseAd의 request()를 호출했을 때, 광고의 여부를 알려주는 함수이다. 
    @Override
    public void onReceiveCloseAd(CaulyCloseAd ad, boolean isChargable) {

    }

    //왼쪽 버튼을 클릭 하였을 때, 원하는 작업을 수행하면 된다. 
    @Override
    public void onLeftClicked(CaulyCloseAd ad) {

    }

    //오른쪽 버튼을 클릭 하였을 때, 원하는 작업을 수행하면 된다. 
    //Default로는 오른쪽 버튼이 종료로 설정되어있다. 
    @Override
    public void onRightClicked(CaulyCloseAd ad) {
        finish();
    }

    @Override
    public void onShowedCloseAd(CaulyCloseAd ad, boolean isChargable) {
    }
    
    // CloseAd의 광고를 클릭할 경우 호출되는 함수이다.
    @Override
    public void onClickCloseAd(CaulyCloseAd ad) {
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
## FullScreen Type 전면 광고

```kotlin
// 아래와 같이 전면 광고 표시 여부 플래그를 사용하면, 전면 광고 수신 후, 노출 여부를 선택할 수 있다.
private var showInterstitial = false
private var interstitialAd: CaulyInterstitialAd? = null

// Activity 버튼 처리
// - 전면 광고 요청 버튼
fun onRequestInterstitial(button: View?) {

    // CaulyAdInfo 생성
    val adInfo = CaulyAdInfoBuilder(APP_CODE)
                      // statusbarHide(boolean) : 상태바 가리기 옵션(true : 상태바 가리기)        
                        .build()

    // 전면 광고 생성
    interstitialAd = CaulyInterstitialAd()
    interstitialAd!!.setAdInfo(adInfo)
    interstitialAd!!.setInterstialAdListener(this)

    // 전면광고 노출 후 back 버튼 사용을 막기 원할 경우 disableBackKey();을 추가한다
    // 단, requestInterstitialAd 위에서 추가되어야 합니다.
    // interstitialAd.disableBackKey()

    // 광고 요청. 광고 노출은 CaulyInterstitialAdListener의 onReceiveInterstitialAd에서 처리한다.
    interstitialAd!!.requestInterstitialAd(this)
    // 전면 광고 노출 플래그 활성화
    showInterstitial = true
}

// - 전면 광고 노출 취소 버튼
fun onCancelInterstitial(button: View?) {
    // 전면 광고 노출 플래그 비활성화
    showInterstitial = false
}

// CaulyInterstitialAdListener
//전면 광고의 경우, 광고 수신 후 자동으로 노출되지 않으므로,
//반드시 onReceiveInterstitialAd 메소드에서 노출 처리해 주어야 한다.
override fun onReceiveInterstitialAd(ad: CaulyInterstitialAd, isChargeableAd: Boolean) {
    // 광고 수신 성공한 경우 호출됨.
    // 수신된 광고가 무료 광고인 경우 isChargeableAd 값이 false 임.
    if (isChargeableAd == false) {
        Log.d("CaulyExample", "free interstitial AD received.")
    } else {
        Log.d("CaulyExample", "normal interstitial AD received.")
    }
    // 노출 활성화 상태이면, 광고 노출
    if (showInterstitial)
        ad!!.show()
    else
        ad!!.cancel()
}

override fun onFailedToReceiveInterstitialAd(ad: CaulyInterstitialAd, errorCode: Int, errorMsg: String) {
    // 전면 광고 수신 실패할 경우 호출됨.
    Log.d("CaulyExample", "failed to receive interstitial AD.")
}

override fun onClosedInterstitialAd(ad: CaulyInterstitialAd) {
    // 전면 광고가 닫힌 경우 호출됨.
    Log.d("CaulyExample", "interstitial AD closed.")
}

override fun onLeaveInterstitialAd(ad: CaulyInterstitialAd) {
    // 전면 광고를 클릭하여 앱을 벗어났을 경우 호출됨.
    Log.d("CaulyExample", "interstitial AD onLeaveInterstitialAd.")
}

override fun onClickInterstitialAd(ad: CaulyInterstitialAd) {
    // 전면 광고를 클릭할 경우 호출됨.
    Log.d("CaulyExample", "interstitial AD onClickInterstitialAd.");
}
```

## Close Ad Type 전면 광고

```kotlin
class KotlinActivity : AppCompatActivity(), CaulyCloseAdListener {

    companion object {
        // 광고 요청을 위한 App Code
        private const val APP_CODE = "CAULY"
    }

    var mCloseAd: CaulyCloseAd? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_kotlin)

        //CloseAd 초기화 
        val closeAdInfo = CaulyAdInfoBuilder(APP_CODE)
                          // statusbarHide(boolean) : 상태바 가리기 옵션(true : 상태바 가리기)        
                            .build()
        mCloseAd = CaulyCloseAd()
                    
        /*  Optional
        //원하는 버튼의 문구를 설정 할 수 있다.  
        mCloseAd.setButtonText("취소", "종료");
        //원하는 텍스트의 문구를 설정 할 수 있다.  
        mCloseAd.setDescriptionText("종료하시겠습니까?");
                    */
        mCloseAd!!.setAdInfo(closeAdInfo)
        mCloseAd!!.setCloseAdListener(this) // CaulyCloseAdListener 등록              
        // 종료광고 노출 후 back버튼 사용을 막기 원할 경우 disableBackKey();을 추가한다
        // mCloseAd!!.disableBackKey()
    }

    override fun onResume() {
        super.onResume()
        if (mCloseAd != null) mCloseAd!!.resume(this) // 필수 호출 
    }

    // Back Key가 눌러졌을 때, CloseAd 호출
    override fun onKeyDown(keyCode: Int, event: KeyEvent): Boolean {
        if (keyCode == KeyEvent.KEYCODE_BACK) {
            // 앱을 처음 설치하여 실행할 때, 필요한 리소스를 다운받았는지 여부.
            if (mCloseAd != null && mCloseAd!!.isModuleLoaded) {
                mCloseAd!!.show(this)
                //광고의 수신여부를 체크한 후 노출시키고 싶은 경우, show(this) 대신 request(this)를 호출.
                //onReceiveCloseAd에서 광고를 정상적으로 수신한 경우 , show(this)를 통해 광고 노출
            } else {
                // 광고에 필요한 리소스를 한번만 다운받는데 실패했을 때 앱의 종료팝업 구현
                showDefaultClosePopup()
            }
            return true
        }
        return super.onKeyDown(keyCode, event)
    }

    private fun showDefaultClosePopup() {
        AlertDialog.Builder(this).setTitle("").setMessage("종료 하시겠습니까?")
            .setPositiveButton("예") { dialog, which -> finish() }.setNegativeButton("아니요", null)
            .show()
    }


    // CaulyCloseAdListener
    override fun onFailedToReceiveCloseAd(ad: CaulyCloseAd, errCode: Int, errMsg: String) {
    }

    // CloseAd의 광고를 클릭하여 앱을 벗어났을 경우 호출되는 함수이다.
    override fun onLeaveCloseAd(ad: CaulyCloseAd) {
    }

    // CloseAd의 request()를 호출했을 때, 광고의 여부를 알려주는 함수이다.
    override fun onReceiveCloseAd(ad: CaulyCloseAd, isChargable: Boolean) {
   
    }

    // 왼쪽 버튼을 클릭 하였을 때, 원하는 작업을 수행하면 된다.
    override fun onLeftClicked(ad: CaulyCloseAd) {

    }

    // 오른쪽 버튼을 클릭 하였을 때, 원하는 작업을 수행하면 된다.
    // Default로는 오른쪽 버튼이 종료로 설정되어있다.
    override fun onRightClicked(ad: CaulyCloseAd) {
        finish()
    }

    override fun onShowedCloseAd(ad: CaulyCloseAd, isChargable: Boolean) {

    }

    // CloseAd의 광고를 클릭할 경우 호출되는 함수이다.
    override fun onClickCloseAd(ad: caulyCloseAd) {
    
    }
}
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Lifecycle에 따라 pause/resume/destroy API를 호출하지 않을 경우, 광고 수신에 불이익을 받을 수 있습니다.
{% endhint %}

