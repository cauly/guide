# 아동 대상 서비스 취급용 '태그' 설정

{% hint style="info" %}
아동대상 콘텐츠로 지정한 경우 관심 기반 광고 및 리마케팅 광고 등이 필터링 됩니다.

* google families policy : [https://play.google.com/about/families/#!?zippy\_activeEl=designed-for-families#designed-for-families](https://play.google.com/about/families/#!?zippy_activeEl=designed-for-families#designed-for-families)
* coppa : [https://www.ftc.gov/tips-advice/business-center/privacy-and-security/children's-privacy](https://www.ftc.gov/tips-advice/business-center/privacy-and-security/children's-privacy)
{% endhint %}

**COPPA에 따라 콘텐츠를 아동 대상으로 지정하려면 'tagForChildDirectedTreatment(true)' 로 호출 한다.**

```java
 CaulyAdInfo adInfo = new CaulyAdInfoBuilder(APPCODE).
 	tagForChildDirectedTreatment(true).
	build();
```

**COPPA에 따라 콘텐츠를 아동 대상으로 지정하지 않으려면 'tagForChildDirectedTreatment(false)' 로 호출 한다.**

```java
 CaulyAdInfo adInfo = new CaulyAdInfoBuilder(APPCODE).
	 tagForChildDirectedTreatment(false).
 	 build();
```

{% hint style="warning" %}
tagForChildDirectedTreatment을 호출하지 않으면 아동 대상 콘텐츠가 아닌 것으로 간주 합니다.
{% endhint %}
