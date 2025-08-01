# 2. 프로젝트 빌드 설정 (build.gradle)

## Cauly SDK 저장소 경로 변경 및 버전 업데이트 안내

{% hint style="danger" %}
**꼭 확인해 주세요!**

Cauly SDK의 저장소 경로가 기존 **GitHub Packages** 방식에서 **GitHub Pages** 방식으로 변경되었습니다.

이에 따라, **기존 GitHub Packages 경로를 통한 SDK 추가는 더 이상 지원되지 않습니다.**

현재 **3.5.38 이하 버전을 사용중인 앱**의 경우, 향후 앱 업데이트 시 SDK 연결 과정에서 **예기치 않은 빌드 오류가 발생할 수 있습니다.** 안정적인 광고 연동과 빌드 오류 방지를 위해, 사전 대응 차원에서 **아래와 같은 조치를 권장**드립니다.

**조치 안내**

* 기존 build.gradle 내 SDK 경로 설정 코드 중 **GitHub Packages 관련 코드 블록 삭제**&#x20;

***

```clojure
// (Deprecated 됨) Cauly SDK Repository Public Access Info
maven {
    name = "GitHubPackages"
    url = uri("https://maven.pkg.github.com/cauly/Android-SDK/SDK")
    credentials {
        username = 'cauly'
        password = '...'
    }
}
```

* 코드블록 삭제 후, 이어서 를 [Cauly SDK 추가하기](project-build-setting.md#cauly-sdk-1)를 진행
* [dependencies 설정](project-build-setting.md#dependencies)을 참고하여 Cauly SDK를 **최신 버전(3.5.39 이상)** 으로 업데이트

삭제 후, 이어서 **Cauly SDK 추가하기를 진행**합니다.
{% endhint %}

## CAULY SDK 추가하기

프로젝트 단위의 "**build.gradle**" 설정에 다음 항목을 추가합니다.

```groovy
allprojects {
    repositories {
        google()
        jcenter()

        // Cauly SDK Repository Public Access Info
        maven { url 'https://cauly.github.io/cauly-sdk-android-maven/maven-repo' }
    }
}
```

## 최신 SDK Version : 3.5.39

## dependencies 설정

앱 단위의 "build.gradle" 설정에 다음 항목을 추가합니다.

{% hint style="warning" %}
기존에 Cauly SDK 3.5.38 이하를 사용했다면,

기존 **dependencies** 항목은 그대로 유지하며, SDK 버전이 **최신 버전**인지 확인합니다.
{% endhint %}

```groovy
dependencies {
    implementation 'com.google.android.gms:play-services-ads-identifier:17.0.0'
    implementation 'com.google.android.gms:play-services-appset:16.0.0'
    implementation 'com.fsn.cauly:cauly-sdk:3.5.39' 
}
```

{% hint style="danger" %}
#### SDK 및 Gradle 관련 오류 안내 <a href="#sdk-gradle" id="sdk-gradle"></a>

**1. 기존 Cauly SDK 3.5.38 이하를 사용하는 경우**

&#x20;**`build.gradle`** 경로를 변경하지 않고 **SDK 버전만 업데이트**할 경우, 아래와 같은 오류가 발생할 수 있습니다.

* **오류 예시**:\
  `Could not find com.fsn.cauly:cauly-sdk:3.5.34`

> **해결 방법**: `maven { url 'https://cauly.github.io/cauly-sdk-android-maven/maven-repo' }` 설정을 적용하고 최신 SDK 버전 **`3.5.39`**  이상을 사용하세요.

**2. Gradle Sync 및 빌드 관련 문제 해결 (1, 2 항목으로 해결 실패 시)**

위의 1, 2 항목에 대한 문제를 해결했음에도 Gradle Sync 또는 빌드 과정에서 문제가 발생한 경우, 아래 단계를 순차적으로 진행합니다.

1. **Android Studio 캐시 초기화 및 재빌드**
   * Android Studio 상단 메뉴에서 **File** → **Invalidate Caches / Restart...** → **Invalidate and Restart**를 선택하세요.
   * 캐시 초기화 후 문제가 해결되지 않으면, 다음 단계를 진행하세요.
2. **수동으로 Gradle 캐시 초기화 및 종속성 새로고침**
   * Android Studio의 Gradle 창에서 다음 명령어를 실행하세요:
     * `gradle --build-cache`
     * `gradle assemble --refresh-dependencies`
   * 그런 다음 Android Studio 상단 메뉴에서 **Build** → **Clean Project**를 실행한 후 **Rebuild Project**를 수행하세요.
   * 수동 SDK 삭제 후에도 에러 발생 시 다음 단계를 진행하세요.
3. **Gradle 캐시 파일 직접 삭제**
   *   아래 경로에서 Gradle 캐시 파일을 삭제한 후 프로젝트를 다시 빌드하세요:

       ```
       /Users/<YOUR_USER_NAME>/.gradle/caches/modules-2/files-2.1/com.fsn.cauly
       ```

       (운영 체제와 사용자 환경에 따라 경로가 다를 수 있습니다.)
{% endhint %}

