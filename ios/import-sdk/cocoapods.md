# CocoaPods

[CocoaPods 사용](https://guides.cocoapods.org/using/using-cocoapods)에서 Podfile을 만들고 사용하는 방법을 참고하세요.

{% hint style="info" %}
Cauly iOS SDK 업데이트에 따라 아래 내용이 변경될 수 있습니다.
{% endhint %}

1. 프로젝트의 `Podfile`을 열고 다음 항목을 추가합니다.

```
source 'https://github.com/CocoaPods/Specs.git'
source 'https://github.com/cauly/CaulySDK_iOS.git'

pod 'CaulySDK', '3.1.22'

# Xcode15.0 이상 버전에서 TOOL CHAIN 관련 빌드 에러가 발생하는 경우, 아래 코드를 추가합니다.
post_install do |installer|
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      xcconfig_path = config.base_configuration_reference.real_path
      xcconfig = File.read(xcconfig_path)
      xcconfig_mod = xcconfig.gsub(/DT_TOOLCHAIN_DIR/, "TOOLCHAIN_DIR")
      File.open(xcconfig_path, "w") { |file| file << xcconfig_mod }
    end
  end
end
```

2. pod을 설치하고 `.xcworkspace` 파일을 엽니다.

```
pod install --repo-update
```

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
