# 더 안전한 구성요소 내보내기 설정

{% hint style="info" %}
Cauly SDK 동작을 위해서는 MainActivity (메인 액티비티 / Launch, Main) 설정이 필요합니다.

Android 12 (API Level31) 이상

intent-filter 를 사용하는 Activity를 포함하는 경우 exported=true 적용이 필요합니다.
{% endhint %}

```xml
<!-- AndroidManifest.xml -->
<application>
    ...
    <activity
        android:name=".MainActivity"
        android:exported="true">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />

            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>
    ...
</application>
```
