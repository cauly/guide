# Class Reference

## Logger 로그 생성 클래스

| 데이터 형    |                                                     |
| -------- | --------------------------------------------------- |
| LogLevel | 로그 수준. enum Verbose, Debug, Info, Warn, Error, None |

| 메소드                   |          |
| --------------------- | -------- |
| setLogLevel(LogLevel) | 로그 수준 지정 |
| getLogLevel()         | 현재 로그 수준 |

## 배너 광고

| <p>CaulyAdView<br>[광고 뷰 클래스]</p>       |                        |
| -------------------------------------- | ---------------------- |
| setAdInfo(CaulyAdInfo)                 | 광고 정보 설정               |
| setAdViewListener(CaulyAdViewListener) | CaulyAdViewListener 지정 |
| setShowPreExpandableAd(boolean)        | P/E 광고 허용 여부 설정        |
| reload()                               | 광고 재요청                 |
| pause()                                | 광고 요청 중단               |
| resume()                               | 광고 요청 재개               |
| destroy()                              | 광고 소멸                  |

| CaulyAdViewListener                                              |                                                        |
| ---------------------------------------------------------------- | ------------------------------------------------------ |
| onReceiveAd(CaulyAdView, boolean isChargeableAd)                 | 광고 노출 성공 시 호출됨. 유,무료 광고 여부가 isChargeableAd 변수에 설정됨     |
| onFailedToReceiveAd(CaulyAdView, int errorCode, String errorMsg) | 광고 노출 실패 시 호출됨. 오류 코드와 내용이 errorCode, errorMsg 변수에 설정됨 |
| onShowLandingScreen(CaulyAdView)                                 | webView를 통해 랜딩 페이지가 열린 경우 호출됨                          |
| onCloseLandingScreen(CaulyAdView)                                | webView를 통해 열린 랜딩 페이지가 닫힌 경우 호출됨                       |
| onClickAd(CaulyAdView)                                           | 광고 클릭 시 호출됨                                            |

### 전면 광고\_풀스크린형

| CaulyInterstitialAd                                  |                                           |
| ---------------------------------------------------- | ----------------------------------------- |
| setAdInfo(CaulyAdInfo)                               | 광고 정보 설정                                  |
| setInterstialAdListener(CaulyInterstitialAdListener) | CaulyAdViewListener 지정                    |
| requestInterstitialAd(Activity) 전면                   | 광고 요청                                     |
| show()                                               | 수신한 전면 광고를 노출                             |
| show(Activity)                                       | 수신한 전면 광고를 노출 (요청과 다른 Activity에서 노출하는 경우) |
| cancel()                                             | 수신한 전면 광고를 폐기                             |
| disableBackKey()                                     | 전면광고 노출 후 back 버튼 막기                      |

| CaulyInterstitialAdListener                                                          |                                                        |
| ------------------------------------------------------------------------------------ | ------------------------------------------------------ |
| onReceiveInterstitialAd(CaulyInterstitialAd, boolean isChargeableAd)                 | 광고 노출 성공 시 호출됨. 유,무료 광고 여부가 isChargeableAd 변수에 설정됨     |
| onFailedToReceiveInterstitialAd(CaulyInterstitialAd, int errorCode, String errorMsg) | 광고 노출 실패 시 호출됨. 오류 코드와 내용이 errorCode, errorMsg 변수에 설정됨 |
| onClosedInterstitialAd(CaulyInterstitialAd)                                          | 전면 광고 페이지가 닫힌 경우 호출됨                                   |
| onLeaveInterstitialAd(CaulyInterstitialAd ad)                                        | 전면 광고를 클릭하여 앱을 벗어났을 경우 호출됨                             |
| onClickInterstitialAd(CaulyInterstitialAd ad)                                       | 전면 광고 클릭 시 호출됨                                         |

### 전면 광고\_플로팅형

| CaulyCloseAd                             |                         |
| ---------------------------------------- | ----------------------- |
| setAdInfo(CaulyAdInfo)                   | 광고 정보 설정                |
| setCloseAdListener(CaulyCloseAdListener) | CaulyCloseAdListener 지정 |
| Boolean isModuleLoaded()                 | 최초 광고 리소스를 다운 받았는지 여부.  |
| resume(Activity)                         | 광고 State 전달 (required)  |
| request (Activity)                       | 전면 광고 요청 (optional)     |
| show(Activity)                           | 수신한 광고를 노출 (required)   |
| setButtonText(String left, String right) | 버튼의 텍스트를 변경 (optional)  |
| setDescriptionText(String)               | 설명 텍스트를 변경 (optional)   |
| cancel()                                 | 수신한 광고를 폐기              |
| disableBackKey()                         | 전면광고 노출 후 back 버튼 막기    |

| CaulyCloseAdListener                                                   |                                                        |
| ---------------------------------------------------------------------- | ------------------------------------------------------ |
| onReceiveCloseAd(CaulyCloseAd, boolean isChargeableAd)                 | 광고 요청 성공 시 호출됨. 유,무료 광고 여부가 isChargeableAd 변수에 설정됨     |
| onFailedToReceiveCloseAd(CaulyCloseAd, int errorCode, String errorMsg) | 광고 노출 실패 시 호출됨. 오류 코드와 내용이 errorCode, errorMsg 변수에 설정됨 |
| onLeaveCloseAd(CaulyInterstitialAd)                                    | 광고가 클릭되어 앱을 벗어났을 경우 호출됨                                |
| onShowedClosedAd(CaulyCloseAd, boolean isChargable)                    | 광고가 보여졌을 때 호출됨                                         |
| onLeftClicked(CaulyCloseAd)                                            | 왼쪽 버튼이 클릭되었을 때, 호출됨                                    |
| onRightClicked(CaulyCloseAd)                                           | 오른쪽 버튼이 클릭되었을 때, 호출됨                                   |
| onClickCloseAd(CaulyCloseAd)                                           | 광고 클릭 시 호출됨                                            |

### 네이티브광고

| CaulyNativeAdView                            |                                 |
| -------------------------------------------- | ------------------------------- |
| setAdInfo(CaulyAdInfo)                       | 광고 정보 설정                        |
| setAdViewListener(CaulyNativeAdViewListener) | CaulyNativeAdViewListener 지정    |
| request (Activity)                           | 네이티브 광고 요청                      |
| attachToView(ViewGroup)                      | 원하는 위치(ViewGroup)에 수신한 광고를 붙인다. |
| isAttachedtoView()                           | 광고가 ViewGroup에 노출되었는지 여부        |
| destroy()                                    | 광고 소멸                           |

| <p>CaulyNativeAdHelper<br>[ListView 에서 노출하기 위한 Helper]</p> |                                              |
| ---------------------------------------------------------- | -------------------------------------------- |
| getInstance()                                              | Singleton 객체                                 |
| init()                                                     | 초기화                                          |
| add(Context, ViewGroup, int, CaulyNativeAdView)            | ListView의 지정한 위치에 광고를 등록한다.                  |
| remove(ViewGroup, int)                                     | ListView의 지정한 위치에 광고를 해지한다.                  |
| isAdPosiont(ViewGroup, int)                                | ListView의 지정한 위치가 네이티브광고인지 여부                |
| getView(ViewGroup,int,convertView)                         | BaseAdaper 의 getView에서 등록된 NativeAdView를 붙인다 |
| getSize(ViewGroup)                                         | 현재 등록된 네이티브애드의 사이즈를 반환한다.                    |
| destroy()                                                  | 광고 소멸 (필수호출)                                 |

| CaulyNativeAdViewListener                                                |                                                        |
| ------------------------------------------------------------------------ | ------------------------------------------------------ |
| onReceiveNativeAd(CaulyNativeAd, boolean isChargeableAd)                 | 광고 요청 성공 시 호출됨. 유,무료 광고 여부가 isChargeableAd 변수에 설정됨     |
| onFailedToReceiveNativeAd(CaulyNativeAd, int errorCode, String errorMsg) | 광고 노출 실패 시 호출됨. 오류 코드와 내용이 errorCode, errorMsg 변수에 설정됨 |
| onReceiveNativeAd(CaulyNativeAd)                                         | 광고 클릭 시 호출됨                                            |

### CaulyAdInfo\[광고 설정 클래스]

| <p>CaulyNativeAdInfoBuilder<br>[CaulyAdInfo 생성용 클래스]</p> |                                                   |
| -------------------------------------------------------- | ------------------------------------------------- |
| CaulyNativeAdInfoBuilder(Context, AttributeSet)          | 지정한 Context 및 AttributSet으로 CaulyAdInfoBuilder 생성 |
| CaulyAdInfoBuilder(String)                               | 지정한 App Code로 CaulyAdInfoBuilder 생성               |
| layoutID(int)                                            | 앱이 NativeAd로 사용할 XML Layout을 연결한다.                |
| mainImageID(int)                                         | Layout 중 메인 이미지가 나타날 영역을 지정한다.                    |
| adRatio(String)                                          | Layout 중 메인 이미지 사이즈를 요청한다 ex) adRatio("720x480"). |
| mainImageOrientation(Orientation)                        | 메인 이미지가 가로형인지 세로형인지 설정한다.                         |
| iconImageID(int)                                         | Layout 중 아이콘이미지가 나타날 영역을 지정한다.                    |
| sponsorVisible(boolean)                                  | 스폰서마크가 화면노출여부를 결정한다.                              |
| titleID(int)                                             | Layout 중 제목이 나타날 영역을 지정한다.                        |
| subtitleID(int)                                          | Layout 중 부제목이 나타날 영역을 지정한다.                       |
| textID(int)                                              | Layout 중 설명부분이 나타날 영역을 지정한다.                      |
| clickingView(int id)                                     | Layout 중 클릭영역이 나타날 영역을 지정한다.                      |
| build()                                                  | 설정한 정보에 따라 CaulyAdInfo 생성                         |
