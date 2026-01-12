# SDK 구성

본 페이지에서는 **수동 다운로드 방식**으로 제공되는\
Cauly iOS SDK의 구성 파일과 설치 방식별 사용 기준을 안내합니다.

수동 다운로드 방식은 프로젝트 환경에 따라\
**XCFramework 방식** 또는 **Static Library 방식** 중\
하나를 선택하여 연동할 수 있습니다.

***

### SDK 구성 개요

수동 다운로드 패키지에는 **두 가지 설치 방식이 함께 포함**되어 있습니다.

* **XCFramework 방식**
* **Static Library 방식**

{% hint style="warning" %}
**프로젝트에서는 반드시 하나의 설치 방식만 선택하여 연동해야 합니다.**

Static Library와 XCFramework를 **동시에 추가하면 빌드 오류가 발생할 수 있습니다.**
{% endhint %}

***

### 설치 방식별 포함 파일

#### 1. XCFramework 방식

XCFramework 방식은 Apple이 권장하는 최신 프레임워크 배포 방식입니다.

**포함 파일**

* `CaulySDK.xcframework`

**포함하지 않는 파일**

* Cauly SDK 헤더 파일 (`*.h`)
* `libCauly.a`
* `libCauly-universal.a`

{% hint style="info" %}
XCFramework 방식에서는

**`CaulySDK.xcframework` 파일만 추가하면 되며,**

**별도의 헤더 파일 추가는 필요하지 않습니다.**
{% endhint %}

***

#### 2. Static Library 방식

Static Library 방식은 기존 레거시 프로젝트 호환을 위해 제공됩니다.

**포함 파일**

* `libCauly.a` **또는**
* `libCauly-universal.a`
* Cauly SDK 헤더 파일 (`*.h`)

**포함하지 않는 파일**

* `CaulySDK.xcframework`

{% hint style="info" %}
**Static Library 방식으로 연동하는 경우**

**`CaulySDK.xcframework`는 반드시 제외해야 합니다.**
{% endhint %}

***

### Static Library 파일 선택 기준

Static Library 방식 사용 시, 프로젝트 환경에 따라 `.a` 파일을 선택해 주세요.

| 파일명                    | 설명                             |
| ---------------------- | ------------------------------ |
| `libCauly.a`           | iOS 디바이스 전용 Static Library     |
| `libCauly-universal.a` | 시뮬레이터 + 디바이스 통합 Static Library |

{% hint style="info" %}
일반적인 개발 환경에서는

**`libCauly-universal.a` 사용을 권장**합니다.
{% endhint %}

***

### Cauly SDK 헤더 파일

Static Library 방식 사용 시 아래 헤더 파일들이 함께 제공됩니다.

| 파일명                     | 설명                       |
| ----------------------- | ------------------------ |
| `Cauly.h`               | Cauly SDK 공용 헤더 파일       |
| `CaulyAdSetting.h`      | 광고 설정 클래스 헤더 파일          |
| `CaulyAdView.h`         | 배너 광고 클래스 및 프로토콜 헤더 파일   |
| `CaulyInterstitialAd.h` | 전면 광고 클래스 및 프로토콜 헤더 파일   |
| `CaulyNativeAd.h`       | 네이티브 광고 클래스 및 프로토콜 헤더 파일 |
| `CaulyNativeAdItem.h`   | 네이티브 광고 아이템 헤더 파일        |

***

### SDK 라이브러리 파일 정리

| 파일명                    | 설치 방식          | 설명                |
| ---------------------- | -------------- | ----------------- |
| `CaulySDK.xcframework` | XCFramework    | 단일 파일 연동 (헤더 불필요) |
| `libCauly.a`           | Static Library | 디바이스 전용           |
| `libCauly-universal.a` | Static Library | 시뮬레이터 + 디바이스 통합   |

***

### 주의 사항 (중요)

* **XCFramework 방식**
  * `CaulySDK.xcframework` 단일 파일만 사용
  * 헤더 파일 및 Bridging Header 설정 불필요
* **Static Library 방식**
  * `.a` 파일 + 헤더 파일 사용
  * Swift 프로젝트의 경우 Bridging Header 설정 필요
  * `CaulySDK.xcframework`는 사용하지 않음
* 설치 방식에 따른 **광고 기능, API, 동작 차이는 없습니다.**
* 최신 SDK Version은 **모든 설치 방식에 공통 적용**됩니다.
