# Activity Orientation 설정

{% hint style="info" %}
Activity 형식의 전체 화면 랜딩을 지원하기 위해서는 아래의 설정을 추가해야합니다.

만약 설정하지 않으면 화면 전환 시 마다 view가 초기화 됩니다.
{% endhint %}

```xml
<activity
    android:name=".MainActivity"
    android:configChanges="keyboard|keyboardHidden|orientation|screenSize">
</activity>
```

```xml
<activity
    android:name="com.fsn.cauly.blackdragoncore.LandingActivity"
    android:configChanges="keyboard|keyboardHidden|orientation|screenSize"> 
</activity>
```
