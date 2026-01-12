# 8. 주요공지 및 안내사항

### 기본 안내사항

* **Cauly Android SDK 최신 버전 사용을 권장**합니다.
* **최신 버전의 Android Studio 사용을 권장**하며, Eclipse에 대한 기술 지원은 제공하지 않습니다.
* Cauly Android SDK는 **Android 4.0 (API Level 14) 이상** 환경에서 동작합니다.
* Activity / Fragment Lifecycle에 따라\
  `pause / resume / destroy` API를 적절히 호출하지 않을 경우\
  **광고 수신에 불이익이 발생할 수 있습니다.**

***

### 공지사항 (필독)

#### 2025/01/09

**GitHub Packages 변경 안내**

* Cauly Android SDK의 저장소 경로가 **S3 저장소에서 GitHub Packages로 변경**되었습니다.
* 변경된 연동 방식 및 설정 방법은\
  [Android SDK 연동 가이드](../) 를 참고 부탁드립니다.

***

#### 2022/01/03

**Google Play 정책 대응 필독 안내 (Android 12 / 13)**

Google Play 정책 변경에 따라,\
**Android 12 이상 (Android 13 포함)** 환경에서는\
Google 광고 ID(GAID) 수집을 위해 아래 사항을 반드시 확인해 주셔야 합니다.

***

#### 1) Google 광고 ID (GAID) 수집을 위한 퍼미션 추가

Android 12 이상에서 광고 ID를 획득하기 위해\
`AndroidManifest.xml`에 아래 퍼미션을 반드시 추가해야 합니다.

```xml
<uses-permission android:name="com.google.android.gms.permission.AD_ID"/>
```

***

#### 2) Cauly Android SDK 최신 버전 업데이트

* **SDK 3.5.21 이상 버전**으로 업데이트해야\
  Android 12 환경에서 정상적인 광고 ID 수집이 가능합니다.
* Google 광고 ID(GAID)가 없는 경우,\
  [**App Set ID**](https://developer.android.com/identity/app-set-id)를 활용하여 분석 및 Fraud 방어가 정상적으로 수행됩니다.
* [**로컬 SDK를 수동 Import하여 사용하는 경우**](https://github.com/cauly/Android-SDK/tree/master/CaulyLibs)\
  아래 dependency를 `build.gradle`에 추가합니다.

```gradle
implementation 'com.google.android.gms:play-services-ads-identifier:17.0.0'
implementation 'com.google.android.gms:play-services-appset:16.0.0'
```

***

#### 3) 개인정보 처리방침 업데이트 안내

* 3rd Party SDK 수집 항목에\
  **디바이스 레벨의 App Set ID**가 포함됩니다.
* 귀사 서비스의 개인정보 처리방침에 해당 항목 추가가 필요한 경우\
  반드시 업데이트해 주시기 바랍니다.

***

### Google Play 데이터 보안 정책 안내

Google Play 데이터 보안 정책 업데이트에 따라,\
앱 개발사는 **앱에서 수집하는 데이터의 종류 및 사용 목적**을\
Play Console의 데이터 보안 설문 양식을 통해 제출해야 합니다.

#### 주요 일정

* **2022년 4월 말**: Google Play 스토어에 보안 섹션 사용자 노출
* **2022년 7월 20일**: 데이터 보안 양식 제출 및 개인정보 처리방침 승인 기한\
  (미제출 또는 문제 발생 시 신규 앱 등록 및 업데이트가 거부될 수 있습니다)

***

### Cauly Android SDK 데이터 수집 안내

Cauly SDK는 광고 및 분석 목적으로 아래 데이터를 수집 및 공유합니다.

| 카테고리         | 데이터 유형             |
| ------------ | ------------------ |
| 기기 또는 기타 식별자 | Android 광고 ID      |
| 앱 활동         | 설치된 앱 정보           |
| 앱 정보 및 성능    | 오류 로그, 기타 앱 진단 데이터 |

* 모든 데이터는 **전송 중 암호화**됩니다.
* 사용자가 개별 데이터 삭제를 요청할 수 있는 기능은 제공하지 않습니다.

{% hint style="info" %}
Google Play 데이터 보안 섹션 양식 작성과 관련된 상세 내용은\
[가이드 문서](https://github.com/cauly/Android-SDK/blob/master/GooglePlay_%E1%84%87%E1%85%A9%E1%84%8B%E1%85%A1%E1%86%AB%E1%84%89%E1%85%A6%E1%86%A8%E1%84%89%E1%85%A7%E1%86%AB_%E1%84%8B%E1%85%A3%E1%86%BC%E1%84%89%E1%85%B5%E1%86%A8_%E1%84%8C%E1%85%A1%E1%86%A8%E1%84%89%E1%85%A5%E1%86%BC_%E1%84%80%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%83%E1%85%B3.pdf)를 화인 부탁드립니다.
{% endhint %}

***

### 문의

Cauly Android SDK 연동 및 정책 관련 문의는\
아래 고객센터로 연락해 주시기 바랍니다.

* **고객센터**: 1544-8867
* **이메일**: cauly\_sdk@fsn.co.kr

***

### 매체 운영 가이드

* [https://www.cauly.net/index.html#/apps/etc/guide](https://www.cauly.net/index.html#/apps/etc/guide)
