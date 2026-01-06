# XCFramework 연동

본 페이지에서는 **수동 다운로드 방식 중 XCFramework 방식**으로\
Cauly iOS SDK를 연동하는 방법을 안내합니다.

XCFramework 방식은 Apple이 권장하는 최신 SDK 배포 방식으로,\
**단일 파일 추가만으로 연동이 가능**하며\
별도의 헤더 파일 또는 Bridging Header 설정이 필요하지 않습니다.

***

### XCFramework 방식 개요

XCFramework 방식에서는\
아래 파일 **하나만** 프로젝트에 추가합니다.

* `CaulySDK.xcframework`

> XCFramework 방식은\
> **Swift / Objective-C 프로젝트 모두에서 사용 가능합니다.**

***

### 1. CaulySDK.xcframework 추가

1. Xcode 프로젝트를 엽니다.
2. **Targets → General → Frameworks, Libraries, and Embedded Content**로 이동합니다.
3. `CaulySDK.xcframework` 파일을 추가합니다.

추가 후, Embed 설정은 **기본값(Embed & Sign)** 을 유지합니다.

***

### 2. 시스템 Framework 추가

Cauly iOS SDK는 내부적으로 iOS 시스템 Framework를 사용합니다.\
XCFramework 방식에서도 아래 Framework를 프로젝트에 추가해야 합니다.

#### 필수 Framework

다음 Framework를 추가합니다.

* `AVKit.framework`
* `UIKit.framework`
* `Foundation.framework`
* `CoreGraphics.framework`
* `QuartzCore.framework`
* `SystemConfiguration.framework`
* `MediaPlayer.framework`
* `CFNetwork.framework`

***

#### Optional 설정이 필요한 Framework

아래 Framework는 추가 후\
**Status를 `Required` → `Optional`로 변경합니다.**

* `MessageUI.framework`
* `EventKit.framework`
* `AdSupport.framework`

***

### 3. Import 및 사용 방법

XCFramework 방식에서는\
**헤더 파일 import 또는 Bridging Header 설정이 필요하지 않습니다.**

{% tabs %}
{% tab title="Swift" %}
```swift
import CaulySDK
```
{% endtab %}

{% tab title="Objective-C" %}
```objc
@import CaulySDK;
```
{% endtab %}
{% endtabs %}

***

### 4. XCFramework 방식에서 불필요한 설정

XCFramework 방식에서는\
아래 설정을 **진행하지 않아야 합니다.**

* ❌ Cauly SDK 헤더 파일 (`*.h`) 추가
* ❌ Bridging Header 생성 및 설정
* ❌ `libCauly.a`, `libCauly-universal.a` 추가

> ⚠️ Static Library 방식과 혼용할 경우\
> 중복 심볼 또는 링크 오류가 발생할 수 있습니다.

***

### 주의 사항

* XCFramework 방식과 Static Library 방식은\
  **동시에 사용할 수 없습니다.**
* 설치 방식에 따른\
  **광고 기능, API, 동작 차이는 없습니다.**
* 최신 SDK Version은\
  **모든 설치 방식에 공통 적용**됩니다.
