# Swift Package Manager

프로젝트에 [패키지 종속 항목을 추가](https://developer.apple.com/documentation/xcode/adding-package-dependencies-to-your-app#Add-a-package-dependency)하려면 다음 단계를 진행합니다.

{% hint style="info" %}
1. Xcode에서 **File(파일) > Add Package Dependencies(패키지 종속 항목 추가)...**&#xB85C; 이동하여 Cauly Swift 패키지를 설치합니다.
2.  표시되는 메시지에서 다음 Google 모바일 광고 Swift 패키지 GitHub 저장소를 검색합니다.

    ```
    ```
3. 사용할 Cauly Swift 패키지의 버전을 선택합니다. 새 프로젝트의 경우 **Up to Next Major Version(최대 다음 메이저 버전)**&#xC744; 사용하는 것이 좋습니다.
{% endhint %}

***

### Import 및 사용 방법

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
