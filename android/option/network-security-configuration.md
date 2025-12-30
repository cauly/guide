# 네트워크 보안 구성

{% hint style="info" %}
**Android 9** **(API 28)부터 강화된 네트워크 보안정책으로 인해 application에 cleartext를 삽입합니다.**&#x20;

**광고 이미지 Url Load, 트래킹요소, Resource가 HTTP 구성될 수 있으므로 해당옵션을 삽입합니다.**
{% endhint %}

```html
<application android:usesCleartextTraffic="true" />
```

{% embed url="https://developer.android.com/privacy-and-security/security-config?hl=ko" %}
