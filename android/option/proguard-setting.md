# 난독화 (Proguard) 설정

{% hint style="danger" %}
난독화 예외 설정 없이 앱을 난독화하면 앱의 빌드가 실패하거나, 앱 사용중 오류가 발생할 수 있습니다.
{% endhint %}

```groovy
-keep public class com.fsn.cauly.** {
      public protected *;
}
-keep public class com.trid.tridad.** {
        public protected *;
}
-dontwarn android.webkit.**

- Android Studio 3.4 이상 부터 (gradle build tool 3.4.0)는 아래 와 같이 설정 해야합니다.

-keep class com.fsn.cauly.** {
       public *; protected *;
}
-keep class com.trid.tridad.** {
        public *; protected *;
}
-dontwarn android.webkit.**
```
