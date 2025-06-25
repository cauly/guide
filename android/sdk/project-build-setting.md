# 2. 프로젝트 빌드 설정 (build.gradle)

## GitHub Packages 변경사항 안내

{% hint style="danger" %}
Cauly SDK의 저장소 경로가 **SDK 버전 3.5.34** 부터 **S3 저장소에서 GitHub Packages로 변경**되었습니다.

GitHub Packages를 사용하는 경우 반드시 Cauly SDK 버전을 3.5.34 이상 사용해야 정상적으로 빌드할 수 있습니다.
{% endhint %}

## CAULY SDK 추가

프로젝트 단위의 "**build.gradle**" 설정에 다음 항목을 추가합니다.

{% hint style="warning" %}
기존에 **S3 저장소 설정**을 추가했다면 해당 코드를 삭제하고 아래 코드를 적용합니다.

***

Cauly SDK를 사용하려면 **username과 password 값이 필수**이며, 반드시 **`가이드에서 제공된 값을 그대로 사용`**&#xD574;야 합니다.

제공된 값 이외에 개인 GitHub 계정 정보나 다른 값을 입력하면 인증이 실패할 수 있습니다.
{% endhint %}

```groovy
allprojects {
    repositories {
        google()
        jcenter()

        // Cauly SDK Repository Public Access Info
        maven {
            name = "GitHubPackages"
            url = uri("https://maven.pkg.github.com/cauly/Android-SDK/SDK")
            credentials {
                username = 'cauly'
                password = 'SDK 부록페이지 참고(https://cauly.gitbook.io/cauly/sdk-android)'
            }
        }
    }
}
```

## 최신 SDK Version : 3.5.39



앱 단위의 "build.gradle" 설정에 다음 항목을 추가합니다.

{% hint style="warning" %}
기존 **S3 저장소에서 GitHub Packages로 전환**하는 경우, 기존 **dependencies** 항목은 그대로 유지하며, SDK 버전이 **최신 버전**인지 확인합니다.
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

**1. 기존에 S3 저장소를 사용한 경우 발생할 수 있는 오류**

기존 S3 저장소를 사용 중인 경우 **`build.gradle`** 경로를 변경하지 않고 **SDK 버전만 업데이트**할 경우, 아래와 같은 오류가 발생할 수 있습니다.

* **오류 예시**:\
  `Could not find com.fsn.cauly:cauly-sdk:3.5.34`

> **해결 방법**: **GitHub Packages** 설정을 적용하고 **`최신 SDK 버전`** 을 사용하세요.

**2. GitHub Packages 사용 시 발생할 수 있는 오류**

1. 신규 **GitHub Packages** 설정을 적용한 경우 **지원되지 않는 SDK 버전**을 빌드하려고 시도하면 아래와 같은 오류가 발생할 수 있습니다.
   * **오류 예시**:\
     `Could not find com.fsn.cauly:cauly-sdk:3.5.32`

> **해결 방법**: GitHub Packages에서 지원하는 최소 SDK 버전은 **`3.5.34`** 입니다. 해당 버전 이상을 사용하세요.

2.  가이드에서 제공된 username과 password가 아닌 임의의 값을 입력하고 빌드를 시도하면 아래와 같은 오류가 발생할 수 있습니다.

    * 오류 예시:\
      `Received status code 401 from server: Unauthorized`

    > **해결 방법**: 가이드에 제공된 username과 password 값을 정확히 입력했는지 확인하세요.

**3. Gradle Sync 및 빌드 관련 문제 해결 (1, 2 항목으로 해결 실패 시)**

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

