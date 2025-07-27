# **Q) 31. 어플리케이션 크기를 어떻게 줄이나요?**

Android 애플리케이션의 크기를 최적화하는 것은 사용자 경험을 개선하는 데 필수적입니다. 특히 제한된 기기 저장공간이나 느린 인터넷 연결을 가진 사용자들에게 더욱 그렇습니다. 기능을 손상시키지 않으면서 애플리케이션 크기를 줄이기 위해 여러 전략을 사용할 수 있습니다.

**사용하지 않는 리소스 제거**
사용하지 않는 리소스(이미지, 레이아웃, 문자열 등)는 APK나 AAB 크기를 불필요하게 증가시킵니다. Android Studio의 Lint와 같은 도구가 이러한 리소스를 식별하는 데 도움이 됩니다. 사용하지 않는 리소스를 제거한 후, build.gradle 파일에서 shrinkResources를 활성화하여 빌드 과정에서 자동으로 사용하지 않는 리소스를 제거합니다.

```kotlin
android {
    buildTypes {
        release {
            minifyEnabled true
            shrinkResources true
        }
    }
}
```

**R8을 통한 코드 축소 활성화**
R8은 Android의 기본 코드 축소기이자 최적화 도구로, 사용하지 않는 클래스와 메서드를 제거합니다. 또한 코드를 난독화하여 더 컴팩트하게 만듭니다. 적절한 ProGuard 규칙을 통해 중요한 코드나 리플렉션 기반 라이브러리가 제거되지 않도록 보장합니다.

```kotlin
android {
    buildTypes {
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}
```

**리소스 최적화 사용**
이미지와 XML 파일 등의 리소스를 최적화하면 앱 크기를 크게 줄일 수 있습니다:

- **벡터 드로어블**: 래스터 이미지(PNG, JPEG 등)를 공간을 덜 차지하는 확장 가능한 그래픽용 벡터 드로어블로 대체
- **이미지 압축**: TinyPNG나 ImageMagick 같은 도구를 사용하여 눈에 띄는 품질 손실 없이 래스터 이미지 압축
- **WebP 형식**: PNG나 JPEG보다 더 나은 압축을 제공하는 WebP 형식으로 이미지 변환

```kotlin
android {
    defaultConfig {
        vectorDrawables.useSupportLibrary = true
    }
}
```

**Android App Bundles (AAB) 사용**
Android App Bundle (AAB) 형식으로 전환하면 Google Play가 개별 기기에 맞춘 최적화된 APK를 제공할 수 있습니다. 이는 특정 구성(화면 밀도, CPU 아키텍처, 언어 등)에 필요한 리소스와 코드만 포함하여 앱 크기를 줄입니다.

```kotlin
android {
    bundle {
        density {
            enableSplit true
        }
        abi {
            enableSplit true
        }
        language {
            enableSplit true
        }
    }
}
```

**불필요한 종속성 제거**
프로젝트의 종속성을 검토하고 사용하지 않거나 중복된 라이브러리를 제거합니다. Android Studio의 Gradle Dependency Analyzer를 사용하여 무거운 라이브러리와 전이 종속성을 식별할 수 있습니다.

**네이티브 라이브러리 최적화**
앱이 네이티브 라이브러리를 포함하는 경우, 다음 전략을 사용하여 영향을 줄입니다:

- **사용하지 않는 아키텍처 제외**: build.gradle 파일의 abiFilters 옵션을 사용하여 필요한 ABI만 포함
- **디버그 심볼 제거**: stripDebugSymbols를 사용하여 네이티브 라이브러리에서 디버깅 심볼 제거

```kotlin
android {
    defaultConfig {
        ndk {
            abiFilters "armeabi-v7a", "arm64-v8a" // 필요한 ABI만 포함
        }
    }
    packagingOptions {
        exclude "**/lib/**/*.so.debug"
    }
}
```

**ProGuard 규칙 구성으로 디버그 정보 줄이기**
디버깅 메타데이터는 최종 APK나 AAB에 불필요한 용량을 추가합니다. proguard-rules.pro 파일을 구성하여 이러한 정보를 제거합니다.

- `dontwarn com.example.unusedlibrary.**
-keep class com.example.important.** { *; }`

**동적 기능 사용**
동적 기능 모듈을 사용하면 덜 자주 사용되는 기능을 온디맨드 모듈로 분리하여 앱을 모듈화할 수 있습니다. 이는 초기 다운로드 크기를 줄입니다.

gradle

`dynamicFeatures = [":feature1", ":feature2"]`

**인앱 대용량 자산 피하기**

- 비디오나 고해상도 이미지 같은 대용량 자산을 콘텐츠 전송 네트워크(CDN)에 호스팅하고 런타임에 동적으로 로드
- 앱에 번들링하는 대신 미디어 콘텐츠에 스트리밍 사용

**요약**
Android 애플리케이션 크기를 줄이는 것은 사용하지 않는 리소스 제거, 코드 축소를 위한 R8 활성화, 리소스 최적화, App Bundle 같은 현대적 형식 활용을 포함한 전략들의 조합입니다. 또한 종속성 검토, 네이티브 라이브러리 최적화, 기능 모듈화는 앱 크기를 더욱 최소화할 수 있습니다. 이러한 관행들은 우수한 사용자 경험을 제공하는 가볍고 성능이 좋은 애플리케이션을 보장합니다.

**실전 질문**

**Q) 앱에 APK/AAB 크기를 크게 증가시키는 고해상도 이미지가 포함되어 있습니다. 시각적 품질을 유지하면서 이미지 리소스를 최적화하는 방법과 최대 효율성을 위해 어떤 형식을 사용할지 설명해주세요.**

**Q) 애플리케이션에 여러 기능이 포함되어 있지만, 일부는 대부분의 사용자가 거의 사용하지 않습니다. 필요할 때 해당 기능들을 사용할 수 있게 하면서도 초기 앱 크기를 줄이는 솔루션을 어떻게 구현하시겠습니까?**

# **Q) 32. Android 애플리케이션에서 프로세스란 무엇이며, Android 운영 체제는 이를 어떻게 관리합니까?**

Android에서 프로세스는 애플리케이션의 구성 요소가 실행되는 실행 환경을 나타냅니다. 각 Android 앱은 단일 실행 스레드를 가진 자체 프로세스에서 작동하며, 다른 앱과 격리되어 시스템 보안, 메모리 관리, 내결함성을 보장합니다. Android 프로세스는 Linux 커널을 사용하여 운영 체제에 의해 관리되며 엄격한 생명주기 규칙을 따릅니다. 기본적으로 동일한 애플리케이션의 모든 구성 요소는 메인 스레드라고 불리는 동일한 프로세스와 스레드에서 실행됩니다.

**Android에서 프로세스 작동 방식**
Android 애플리케이션이 시작되면, 운영 체제는 Linux fork() 시스템 호출을 사용하여 새 프로세스를 생성합니다. 각 프로세스는 Dalvik 또는 ART(Android Runtime) 가상 머신의 고유한 인스턴스에서 실행되어 안전하고 독립적인 실행을 보장합니다. Android는 각 프로세스에 고유한 Linux 사용자 ID(UID)를 할당하여 권한 제어와 파일 시스템 격리를 포함한 엄격한 보안 경계를 강제합니다.

**애플리케이션 구성 요소와 프로세스 연결**
기본적으로 Android 애플리케이션의 모든 구성 요소는 동일한 프로세스 내에서 실행되며, 대부분의 애플리케이션이 이 표준을 따릅니다. 그러나 개발자는 AndroidManifest.xml 파일의 android:process 속성을 사용하여 프로세스 할당을 사용자 정의할 수 있습니다. 이 속성은 `<activity>`, `<service>`, `<receiver>`, `<provider>` 같은 구성 요소에 적용될 수 있어, 구성 요소가 별도의 프로세스에서 실행되거나 선택적으로 프로세스를 공유할 수 있게 합니다. `<application>` 요소도 이 속성을 지원하여 모든 구성 요소의 기본 프로세스를 정의합니다.

```kotlin
<service
    android:name=".MyService"
    android:process=":remote" />
```

이 예제에서 MyService 서비스는 :remote라는 별도의 프로세스에서 실행되어 독립적인 작동과 증가된 내결함성을 가능하게 합니다.

또한 다른 애플리케이션의 구성 요소들도 동일한 Linux 사용자 ID를 가지고 동일한 인증서로 서명된 경우 같은 프로세스를 공유할 수 있습니다. Android는 시스템 리소스 요구에 따라 동적으로 프로세스를 관리하여 필요시 낮은 우선순위 프로세스를 종료합니다. 더 이상 보이지 않는 액티비티를 호스팅하는 프로세스는 가시적인 구성 요소를 호스팅하는 프로세스에 비해 종료될 가능성이 높습니다. Android 시스템은 관련 구성 요소가 작업을 수행해야 할 때 프로세스를 재시작하여 최적의 시스템 성능과 사용자 경험을 보장합니다.

**프로세스와 앱 생명주기**
Android는 시스템 메모리와 앱의 현재 상태를 기반으로 엄격한 우선순위 계층을 따라 프로세스와 앱 생명주기를 관리합니다:

1. **포어그라운드 프로세스**: 적극적으로 실행되고 사용자와 상호작용하는 프로세스. 최고 우선순위 프로세스이며 거의 종료되지 않습니다.
2. **가시적 프로세스**: 사용자에게 보이지만 적극적으로 상호작용하지 않는 프로세스(예: 대화상자 뒤의 액티비티)
3. **서비스 프로세스**: 데이터 동기화나 음악 재생 같은 작업을 수행하는 백그라운드 서비스를 실행하는 프로세스
4. **캐시된 프로세스**: 더 빠른 재시작을 위해 메모리에 유지되는 유휴 프로세스. 가장 낮은 우선순위를 가지며 메모리가 부족할 때 먼저 종료됩니다.

Android 시스템은 메모리를 확보하고 시스템 안정성을 유지하기 위해 자동으로 낮은 우선순위 프로세스를 종료합니다.

**보안과 권한**
각 Android 프로세스는 Linux 보안 모델을 사용하여 샌드박스화되어 엄격한 권한 기반 액세스 제어를 강제합니다. 이러한 격리는 Android 권한 시스템을 통해 명시적으로 권한이 부여되지 않는 한, 애플리케이션이 다른 프로세스의 데이터에 액세스할 수 없도록 보장합니다. 이 보안 모델은 Android의 멀티태스킹 환경에 기본적이며, 시스템 안정성과 데이터 프라이버시를 모두 지원합니다.

**요약**
Android의 프로세스는 애플리케이션 구성 요소의 실행 환경 역할을 하여 격리되고, 안전하며, 효율적인 앱 작동을 보장합니다. Android 시스템에 의해 관리되는 프로세스는 메모리 제약, 사용자 활동, 애플리케이션 우선순위에 따라 생성, 스케줄링, 종료됩니다. 개발자는 매니페스트 파일의 구성과 Android의 권한 관리 시스템을 통해 프로세스 동작을 추가로 제어할 수 있어 강력하고 확장 가능한 애플리케이션 개발을 가능하게 합니다.

**실전 질문**

**Q) 다른 Android 구성 요소들이 별도의 프로세스에서 실행되어야 하는 애플리케이션을 개발하고 있습니다. AndroidManifest에서 이를 어떻게 구성하고, 다중 프로세스 사용의 잠재적 단점은 무엇입니까?**

**Q) Android는 메모리가 부족할 때 어떤 프로세스를 종료할지 결정하기 위해 우선순위 기반 프로세스 관리 시스템을 사용합니다. 시스템이 프로세스의 우선순위를 매기는 방법과 중요한 프로세스가 종료되지 않도록 개발자가 따라야 할 전략을 설명할 수 있습니까?**

---

## 실전 질문 답변

### Q1) 고해상도 이미지 최적화 방법

**답변:**

```kotlin
*// 1. WebP 형식 사용 (PNG 대비 25-35% 용량 절약)// build.gradle에서 WebP 지원 활성화*
android {
    defaultConfig {
        minSdkVersion 18 *// WebP 완전 지원*
    }
}

*// 2. 벡터 드로어블 사용// res/drawable에서 SVG를 벡터 드로어블로 변환*
<vector android:width="24dp" android:height="24dp"
        android:viewportWidth="24" android:viewportHeight="24">
    <path android:fillColor="#FF000000"
          android:pathData="M12,2l3.09,6.26L22,9.27l-5,4.87 1.18,6.88L12,17.77l-6.18,3.25L7,14.14 2,9.27l6.91,-1.01L12,2z"/>
</vector>

*// 3. 적응형 이미지 로딩*
class ImageOptimizer {
    fun loadOptimizedImage(imageView: ImageView, url: String) {
        val density = imageView.context.resources.displayMetrics.density
        val targetWidth = (imageView.width * density).toInt()
        
        Glide.with(imageView.context)
            .load(url)
            .override(targetWidth, Target.SIZE_ORIGINAL)
            .format(DecodeFormat.PREFER_RGB_565) *// 메모리 절약*
            .diskCacheStrategy(DiskCacheStrategy.RESOURCE)
            .into(imageView)
    }
}
```

### Q2) 동적 기능 모듈 구현

**답변:**

```kotlin
*// app/build.gradle*
android {
    dynamicFeatures = [":feature_premium", ":feature_analytics"]
}

*// feature_premium/build.gradle*
apply plugin: 'com.android.dynamic-feature'
android {
    compileSdkVersion 34
}
dependencies {
    implementation project(':app')
}

*// 동적 기능 로드 관리자*
class DynamicFeatureManager(private val context: Context) {
    private val splitInstallManager = SplitInstallManagerFactory.create(context)
    
    fun installFeature(moduleName: String, callback: (Boolean) -> Unit) {
        val request = SplitInstallRequest.newBuilder()
            .addModule(moduleName)
            .build()
            
        splitInstallManager.startInstall(request)
            .addOnSuccessListener { sessionId ->
                callback(true)
            }
            .addOnFailureListener { exception ->
                callback(false)
            }
    }
    
    fun isFeatureInstalled(moduleName: String): Boolean {
        return splitInstallManager.installedModules.contains(moduleName)
    }
}

*// 사용 예시*
class MainActivity : AppCompatActivity() {
    private val featureManager = DynamicFeatureManager(this)
    
    fun loadPremiumFeature() {
        if (featureManager.isFeatureInstalled("feature_premium")) {
            *// 이미 설치됨 - 바로 사용*
            startPremiumActivity()
        } else {
            *// 다운로드 후 사용*
            showDownloadDialog()
            featureManager.installFeature("feature_premium") { success ->
                if (success) startPremiumActivity()
            }
        }
    }
}
```

### Q3) 다중 프로세스 구성과 단점

**답변:**

```kotlin
*<!-- AndroidManifest.xml -->*
<application android:name=".MainApplication">
    *<!-- 메인 프로세스에서 실행 -->*
    <activity android:name=".MainActivity" />
    
    *<!-- 별도 프로세스에서 실행 -->*
    <service 
        android:name=".NetworkService"
        android:process=":network" />
    
    *<!-- 다른 앱과 공유 가능한 프로세스 -->*
    <service 
        android:name=".SharedService"
        android:process="com.company.shared" />
        
    *<!-- UI와 분리된 백그라운드 작업 -->*
    <service 
        android:name=".DataSyncService"
        android:process=":background" />
</application>
```

**단점들:**

1. **메모리 사용량 증가**: 각 프로세스마다 별도의 가상머신 인스턴스
2. **IPC 오버헤드**: 프로세스 간 통신 비용
3. **복잡한 데이터 공유**: Singleton 패턴이 프로세스별로 분리됨
4. **디버깅 어려움**: 여러 프로세스 동시 디버깅 필요

### Q4) 프로세스 우선순위와 생존 전략

**답변:**

```kotlin
class ProcessSurvivalManager {
    
    *// 1. Foreground Service로 우선순위 높이기*
    fun startForegroundService() {
        val notification = createNotification()
        startForeground(NOTIFICATION_ID, notification)
    }
    
    *// 2. Persistent Notification*
    private fun createNotification(): Notification {
        return NotificationCompat.Builder(this, CHANNEL_ID)
            .setContentTitle("앱이 백그라운드에서 실행 중")
            .setSmallIcon(R.drawable.ic_notification)
            .setPriority(NotificationCompat.PRIORITY_LOW)
            .setOngoing(true) *// 사용자가 제거할 수 없음*
            .build()
    }
    
    *// 3. 시스템 이벤트 수신으로 재시작*
    class SystemEventReceiver : BroadcastReceiver() {
        override fun onReceive(context: Context, intent: Intent) {
            when (intent.action) {
                Intent.ACTION_BOOT_COMPLETED,
                Intent.ACTION_MY_PACKAGE_REPLACED -> {
                    *// 중요한 서비스 재시작*
                    context.startService(Intent(context, CriticalService::class.java))
                }
            }
        }
    }
    
    *// 4. JobScheduler로 안정적인 백그라운드 작업*
    @TargetApi(Build.VERSION_CODES.LOLLIPOP)
    class BackgroundJobService : JobService() {
        override fun onStartJob(params: JobParameters): Boolean {
            *// 중요한 백그라운드 작업 수행*
            doImportantWork()
            return true
        }
        
        override fun onStopJob(params: JobParameters): Boolean {
            *// 작업이 중단되면 재스케줄링*
            scheduleJob()
            return true
        }
    }
}
```

**생존 전략:**

1. **Foreground Service 사용**: 사용자에게 알림을 표시하되 중요한 작업임을 명시
2. **적절한 Service 타입 선택**: IntentService보다 JobIntentService 사용
3. **시스템 이벤트 활용**: BOOT_COMPLETED, PACKAGE_REPLACED 등으로 재시작
4. **메모리 효율적 구현**: 불필요한 객체 참조 제거로 GC 압박 감소
5. **배터리 최적화 예외**: 사용자에게 배터리 최적화에서 앱 제외 요청

# 카테고리 1: Android UI - Views

Android UI는 Android 개발의 핵심으로, 애플리케이션의 구조와 상호작용을 형성하는 화면, 레이아웃, 위젯, 다양한 구성 요소를 설계하는 도구를 제공합니다. 개발의 다른 측면들도 중요하지만, UI 시스템은 첫인상을 정의하고 의미 있는 사용자 상호작용을 촉진하므로 중요한 위치를 차지합니다. Android UI에 대한 깊은 이해는 시각적으로 매력적이고 반응성이 좋은 애플리케이션을 만드는 데 필수적입니다.

요즘 Jetpack Compose 생태계는 빠르게 성장했으며 현재 Android 애플리케이션에서 프로덕션 준비가 된 UI를 구축하는 데 널리 채택되고 있습니다. Jetpack Compose가 Android UI 개발의 미래를 나타낸다고 말해도 안전합니다. Android를 시작하는 새로운 개발자라면 전통적인 View 시스템을 먼저 배울 필요가 없습니다. 바로 1장: Jetpack Compose 인터뷰 질문으로 뛰어들 수 있습니다.

그렇긴 하지만, 일부 대기업들은 여전히 Android View 시스템에 크게 의존하고 있습니다. Jetpack Compose로 마이그레이션하는 것이 어려울 수 있고 단기 전략과 맞지 않기 때문입니다. 그런 회사들의 기술 면접을 준비하고 있다면, 전통적인 View 시스템에 대한 확실한 이해가 여전히 필수적일 수 있습니다.

Android Views에서는 모든 UI 요소가 기본적으로 메인 스레드에서 실행되므로 고성능 애플리케이션을 구축하기 위해 View 생명주기와 일반적으로 사용되는 UI 구성 요소에 대한 확실한 이해가 필수적입니다. 또한 Window나 텍스트 단위와 같은 Android UI 시스템의 핵심 원리를 이해하면 개발자가 애플리케이션을 올바르게 구축하기 위한 정보에 입각한 결정을 내리는 데 도움이 됩니다.

많은 경우, 디자인 팀의 복잡한 디자인 명세를 충족하기 위해 커스텀 뷰를 만드는 것이 필요합니다. 따라서 Android UI 시스템을 깊이 이해하는 것은 효율적으로 개발하기 위한 핵심 기술이며 Android 프레임워크를 따라 좋은 Android 개발자가 되기 위한 다음 단계입니다.

# Q) 33. View 생명주기를 설명하세요

Android에서 View 생명주기는 View(TextView나 Button 같은)가 생성되고, 액티비티나 프래그먼트에 연결되고, 화면에 표시되고, 결국 파괴되거나 분리되면서 겪는 생명주기 이벤트를 의미합니다. View 생명주기를 이해하면 개발자가 View의 초기화, 렌더링, 해체를 관리하고, 사용자 행동, 시스템 이벤트에 대응하여 View 생명주기에 따라 커스텀 뷰를 구현하거나 적절한 순간에 리소스를 해제하는 데 도움이 됩니다.

### View 생명주기 단계:

1. **View 생성 (onAttachedToWindow)**: View가 프로그래밍 방식으로 또는 XML 레이아웃을 인플레이트하여 인스턴스화되는 단계입니다. 리스너 설정과 데이터 바인딩 같은 초기 설정 작업이 여기서 수행됩니다. onAttachedToWindow() 메서드는 View가 부모에 추가되고 화면에 렌더링될 준비가 되었을 때 트리거됩니다.
2. **레이아웃 단계 (onMeasure, onLayout)**: 이 단계에서는 View의 크기와 위치가 계산됩니다. onMeasure() 메서드는 레이아웃 매개변수와 부모 제약 조건을 기반으로 View의 너비와 높이를 결정합니다. 측정이 완료되면 onLayout() 메서드가 View를 부모 내에서 위치시켜 화면에 나타날 위치를 최종 결정합니다.
3. **그리기 단계 (onDraw)**: 크기와 위치가 최종 결정된 후, onDraw() 메서드가 텍스트나 이미지 같은 View의 내용을 Canvas에 렌더링합니다. 커스텀 View는 이 메서드를 오버라이드하여 커스텀 그리기 로직을 정의할 수 있습니다.
4. **이벤트 처리 (onTouchEvent, onClick)**: 상호작용 가능한 View는 이 단계에서 터치 이벤트, 클릭, 제스처 같은 사용자 상호작용을 처리합니다. onTouchEvent()와 onClick() 같은 메서드를 사용하여 이러한 이벤트를 처리하고 사용자 입력에 대한 View의 응답을 정의합니다.
5. **View 분리 (onDetachedFromWindow)**: View가 화면과 부모 ViewGroup에서 제거될 때(예: 액티비티나 프래그먼트 파괴 중), onDetachedFromWindow() 메서드가 호출됩니다. 이 단계는 리소스 정리나 리스너 분리에 이상적입니다.
6. **View 파괴**: View가 더 이상 사용되지 않으면 가비지 컬렉션됩니다. 개발자는 메모리 누수를 피하고 성능을 최적화하기 위해 이벤트 리스너나 백그라운드 작업 같은 모든 리소스가 적절히 해제되도록 해야 합니다.

### 요약

View의 생명주기는 생성, 측정, 레이아웃, 그리기, 이벤트 처리, 최종 분리를 포함하며, Android 애플리케이션 내에서 표시되고 사용되는 동안 거치는 단계를 반영합니다.

### 실전 질문

**Q) 이미지 로딩이나 애니메이션 설정 같은 비용이 많이 드는 초기화를 수행해야 하는 커스텀 View를 만들고 있습니다. View 생명주기의 어느 시점에서 이러한 리소스를 초기화해야 하며, 메모리 누수를 피하기 위해 적절한 정리를 어떻게 보장하시겠습니까?**

**Q) 애플리케이션에 성능 문제를 겪는 동적으로 생성된 View들이 있는 복잡한 UI가 있습니다. 반응성을 유지하면서 렌더링 효율성을 개선하기 위해 onMeasure()와 onLayout() 메서드를 어떻게 최적화하시겠습니까?**

### 마스터리를 위한 프로 팁: View의 findViewTreeLifecycleOwner() 함수는 무엇인가요?

findViewTreeLifecycleOwner() 함수는 View 클래스의 일부입니다. 이 함수는 View 트리 계층을 위로 탐색하여 View 트리에 연결된 가장 가까운 LifecycleOwner를 찾아 반환합니다. LifecycleOwner는 호스팅 구성 요소의 생명주기 범위를 나타내며, 일반적으로 Activity, Fragment, 또는 LifecycleOwner를 구현하는 어떤 커스텀 구성 요소입니다. LifecycleOwner가 발견되지 않으면 함수는 null을 반환합니다.

### findViewTreeLifecycleOwner()를 사용하는 이유?

이 함수는 LiveData, ViewModel, 또는 LifecycleObserver 같은 생명주기 인식 요소와 상호작용해야 하는 커스텀 View나 서드파티 구성 요소와 작업할 때 특히 유용합니다. 이를 통해 View가 호스팅 Activity나 Fragment에 대한 명시적 종속성을 요구하지 않고도 관련된 생명주기에 액세스할 수 있습니다.

findViewTreeLifecycleOwner()를 사용하면 다음을 보장할 수 있습니다:

- 생명주기 인식 구성 요소가 올바른 생명주기에 적절히 바인딩됨
- 생명주기가 끝날 때 관찰자가 지워져 메모리 누수를 방지함

LifecycleObserver 인스턴스를 바인딩해야 하는 커스텀 View를 고려해보세요. findViewTreeLifecycleOwner()를 사용하여 올바른 생명주기에 관찰을 바인딩할 수 있습니다.

```kotlin
class CustomView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null
) : LinearLayout(context, attrs) {

    fun bindObserver(observer: LifecycleObserver) {
        *// View 트리에서 가장 가까운 LifecycleOwner 찾기*
        val lifecycleOwner = findViewTreeLifecycleOwner()

        lifecycleOwner?.lifecycle?.addObserver(observer) ?: run {
            Log.e("CustomView", "No LifecycleOwner found for the View")
        }
    }
}
```

여기서 커스텀 CustomView는 View 트리에서 가장 가까운 LifecycleOwner에 동적으로 바인딩하여 LifecycleObserver 관찰이 적절한 생명주기에 연결되도록 보장합니다.

### 주요 사용 사례

1. **커스텀 View**: 커스텀 View 내의 생명주기 인식 구성 요소가 LifecycleObserver, LiveData를 관찰하거나 리소스를 관리할 수 있게 함
2. **서드파티 라이브러리**: 재사용 가능한 UI 구성 요소가 명시적 생명주기 관리를 요구하지 않고 생명주기 인식 리소스와 상호작용할 수 있게 함
3. **로직 분리**: View가 View 트리에서 LifecycleOwner를 독립적으로 발견할 수 있게 하여 결합도를 줄이는 데 도움

### 제한사항

findViewTreeLifecycleOwner()는 유용한 유틸리티이지만 View 트리에 LifecycleOwner가 있는지에 의존합니다. 그런 소유자가 없으면 함수는 null을 반환하므로, 크래시나 예상치 못한 동작을 피하기 위해 이 경우를 우아하게 처리해야 합니다.

### 요약

View의 findViewTreeLifecycleOwner() 함수는 View 트리에서 가장 가까운 LifecycleOwner를 검색하는 유용한 유틸리티입니다. 커스텀 View나 서드파티 라이브러리에서 생명주기 인식 구성 요소와 작업하는 것을 단순화하여 적절한 생명주기 관리를 보장하고 View와 호스팅 구성 요소 간의 결합도를 줄입니다.

---

## 실전 질문 답변

### Q1) 비용이 많이 드는 초기화 최적화

**답변:**

```kotlin
class CustomImageView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : View(context, attrs, defStyleAttr) {
    
    private var imageLoader: ImageLoader? = null
    private var animationSet: AnimatorSet? = null
    private var isResourcesInitialized = false
    
    *// onAttachedToWindow에서 리소스 초기화*
    override fun onAttachedToWindow() {
        super.onAttachedToWindow()
        initializeExpensiveResources()
    }
    
    private fun initializeExpensiveResources() {
        if (isResourcesInitialized) return
        
        *// 이미지 로더 초기화*
        imageLoader = ImageLoader.Builder(context)
            .memoryCache {
                MemoryCache.Builder(context)
                    .maxSizePercent(0.25)
                    .build()
            }
            .diskCache {
                DiskCache.Builder()
                    .directory(context.cacheDir.resolve("image_cache"))
                    .maxSizePercent(0.02)
                    .build()
            }
            .build()
            
        *// 애니메이션 설정*
        setupAnimations()
        isResourcesInitialized = true
    }
    
    private fun setupAnimations() {
        val scaleX = ObjectAnimator.ofFloat(this, "scaleX", 1f, 1.1f, 1f)
        val scaleY = ObjectAnimator.ofFloat(this, "scaleY", 1f, 1.1f, 1f)
        
        animationSet = AnimatorSet().apply {
            playTogether(scaleX, scaleY)
            duration = 300
            interpolator = DecelerateInterpolator()
        }
    }
    
    *// onDetachedFromWindow에서 리소스 정리*
    override fun onDetachedFromWindow() {
        super.onDetachedFromWindow()
        cleanupResources()
    }
    
    private fun cleanupResources() {
        *// 애니메이션 정리*
        animationSet?.apply {
            if (isRunning) cancel()
            removeAllListeners()
        }
        animationSet = null
        
        *// 이미지 로더 정리*
        imageLoader?.shutdown()
        imageLoader = null
        
        isResourcesInitialized = false
    }
    
    *// 메모리 누수 방지를 위한 약한 참조 사용*
    private class WeakReferenceCallback(view: CustomImageView) : Runnable {
        private val viewRef = WeakReference(view)
        
        override fun run() {
            viewRef.get()?.handleCallback()
        }
    }
    
    fun loadImage(imageUrl: String) {
        if (!isResourcesInitialized) {
            *// 아직 초기화되지 않았다면 대기 후 재시도*
            postDelayed({ loadImage(imageUrl) }, 100)
            return
        }
        
        val request = ImageRequest.Builder(context)
            .data(imageUrl)
            .target(this as ImageView)
            .placeholder(R.drawable.placeholder)
            .error(R.drawable.error_image)
            .build()
            
        imageLoader?.enqueue(request)
    }
}
```

**핵심 포인트:**

- **onAttachedToWindow()**: View가 윈도우에 연결된 후 초기화 수행
- **onDetachedFromWindow()**: View가 분리될 때 리소스 정리
- **약한 참조 사용**: 메모리 누수 방지
- **초기화 상태 관리**: 중복 초기화 방지

### Q2) onMeasure()와 onLayout() 최적화

**답변:**

```kotlin
class OptimizedCustomViewGroup @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : ViewGroup(context, attrs, defStyleAttr) {
    
    *// 측정 결과 캐싱*
    private val measureCache = mutableMapOf<Pair<Int, Int>, MeasureResult>()
    private var lastWidthMeasureSpec = -1
    private var lastHeightMeasureSpec = -1
    
    data class MeasureResult(
        val measuredWidth: Int,
        val measuredHeight: Int,
        val childMeasures: List<Pair<Int, Int>>
    )
    
    override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
        *// 캐시된 결과가 있는지 확인*
        val specPair = Pair(widthMeasureSpec, heightMeasureSpec)
        val cachedResult = measureCache[specPair]
        
        if (cachedResult != null && 
            widthMeasureSpec == lastWidthMeasureSpec && 
            heightMeasureSpec == lastHeightMeasureSpec) {
            setMeasuredDimension(cachedResult.measuredWidth, cachedResult.measuredHeight)
            applyCachedChildMeasures(cachedResult.childMeasures)
            return
        }
        
        *// 실제 측정 수행*
        performMeasurement(widthMeasureSpec, heightMeasureSpec)
        
        lastWidthMeasureSpec = widthMeasureSpec
        lastHeightMeasureSpec = heightMeasureSpec
    }
    
    private fun performMeasurement(widthMeasureSpec: Int, heightMeasureSpec: Int) {
        val widthMode = MeasureSpec.getMode(widthMeasureSpec)
        val widthSize = MeasureSpec.getSize(widthMeasureSpec)
        val heightMode = MeasureSpec.getMode(heightMeasureSpec)
        val heightSize = MeasureSpec.getSize(heightMeasureSpec)
        
        var totalWidth = 0
        var totalHeight = 0
        val childMeasures = mutableListOf<Pair<Int, Int>>()
        
        *// 자식 View들을 배치로 측정하여 성능 개선*
        val childCount = childCount
        val batchSize = 10 *// 배치 크기*
        
        for (i in 0 until childCount step batchSize) {
            val endIndex = minOf(i + batchSize, childCount)
            measureChildrenBatch(i, endIndex, widthMeasureSpec, heightMeasureSpec, childMeasures)
        }
        
        *// 최종 크기 계산*
        totalWidth = childMeasures.maxOfOrNull { it.first } ?: 0
        totalHeight = childMeasures.sumOf { it.second }
        
        val finalWidth = when (widthMode) {
            MeasureSpec.EXACTLY -> widthSize
            MeasureSpec.AT_MOST -> minOf(totalWidth, widthSize)
            else -> totalWidth
        }
        
        val finalHeight = when (heightMode) {
            MeasureSpec.EXACTLY -> heightSize
            MeasureSpec.AT_MOST -> minOf(totalHeight, heightSize)
            else -> totalHeight
        }
        
        setMeasuredDimension(finalWidth, finalHeight)
        
        *// 결과 캐싱*
        val result = MeasureResult(finalWidth, finalHeight, childMeasures.toList())
        measureCache[Pair(widthMeasureSpec, heightMeasureSpec)] = result
        
        *// 캐시 크기 제한*
        if (measureCache.size > 20) {
            measureCache.remove(measureCache.keys.first())
        }
    }
    
    private fun measureChildrenBatch(
        startIndex: Int,
        endIndex: Int,
        widthMeasureSpec: Int,
        heightMeasureSpec: Int,
        childMeasures: MutableList<Pair<Int, Int>>
    ) {
        for (i in startIndex until endIndex) {
            val child = getChildAt(i)
            if (child.visibility != GONE) {
                val lp = child.layoutParams
                val childWidthSpec = getChildMeasureSpec(widthMeasureSpec, 0, lp.width)
                val childHeightSpec = getChildMeasureSpec(heightMeasureSpec, 0, lp.height)
                
                child.measure(childWidthSpec, childHeightSpec)
                childMeasures.add(Pair(child.measuredWidth, child.measuredHeight))
            }
        }
    }
    
    override fun onLayout(changed: Boolean, l: Int, t: Int, r: Int, b: Int) {
        *// 레이아웃 변경이 없고 캐시된 위치 정보가 있다면 스킵*
        if (!changed && !needsLayout) return
        
        layoutChildrenOptimized()
        needsLayout = false
    }
    
    private var needsLayout = true
    private val childPositions = mutableMapOf<View, Rect>()
    
    private fun layoutChildrenOptimized() {
        var currentY = paddingTop
        val leftPadding = paddingLeft
        val rightPadding = paddingRight
        val availableWidth = width - leftPadding - rightPadding
        
        *// 가시적인 자식들만 레이아웃*
        val visibleChildren = (0 until childCount)
            .map { getChildAt(it) }
            .filter { it.visibility != GONE }
        
        visibleChildren.forEach { child ->
            val childWidth = child.measuredWidth
            val childHeight = child.measuredHeight
            
            val left = leftPadding + (availableWidth - childWidth) / 2
            val top = currentY
            val right = left + childWidth
            val bottom = top + childHeight
            
            *// 위치가 실제로 변경된 경우에만 레이아웃*
            val currentRect = childPositions[child]
            val newRect = Rect(left, top, right, bottom)
            
            if (currentRect != newRect) {
                child.layout(left, top, right, bottom)
                childPositions[child] = newRect
            }
            
            currentY = bottom + getChildVerticalSpacing()
        }
    }
    
    private fun getChildVerticalSpacing(): Int = 8 *// dp를 px로 변환 필요*
    
    private fun applyCachedChildMeasures(childMeasures: List<Pair<Int, Int>>) {
        childMeasures.forEachIndexed { index, (width, height) ->
            if (index < childCount) {
                val child = getChildAt(index)
                child.setMeasuredDimension(width, height)
            }
        }
    }
    
    *// 메모리 정리*
    override fun onDetachedFromWindow() {
        super.onDetachedFromWindow()
        measureCache.clear()
        childPositions.clear()
    }
}
```

**최적화 기법:**

1. **측정 결과 캐싱**: 동일한 MeasureSpec에 대한 결과 재사용
2. **배치 처리**: 자식 View들을 배치로 처리하여 성능 개선
3. **지연 레이아웃**: 실제 변경이 있을 때만 레이아웃 수행
4. **가시성 필터링**: GONE 상태의 View는 처리하지 않음
5. **위치 캐싱**: 실제 위치 변경이 있을 때만 layout() 호출
6. **메모리 관리**: 캐시 크기 제한 및 정리

# Q) 34. View와 ViewGroup의 차이점은 무엇인가요?

View와 ViewGroup은 UI 구성 요소를 구현하는 데 있어 기본적인 개념입니다. 둘 다 android.view 패키지의 일부이지만, UI 계층구조에서 서로 다른 목적을 가지고 있습니다.

### View란 무엇인가요?

View는 화면에 표시되는 단일하고 직사각형의 UI 요소를 나타냅니다. Button, TextView, ImageView, EditText와 같은 모든 UI 구성 요소의 기본 클래스입니다. 각 View는 화면에 그리기와 터치나 키 이벤트 같은 사용자 상호작용을 처리합니다.

```kotlin
val textView = TextView(context).apply {
    text = "Hello, World!"
    textSize = 16f
    setTextColor(Color.BLACK)
}
```

**추가 팁**: View 시스템은 Android 개발의 핵심 기반 중 하나로, 전체 UI 프레임워크의 뼈대 역할을 합니다. UI 구성 요소의 렌더링, 업데이트, 사용자 상호작용을 가능하게 하는 콜백 시스템 관리를 담당합니다. 기본 버튼부터 복잡한 레이아웃까지 모든 UI 요소는 View 클래스 위에 구축됩니다.

AOSP의 View.java 내부 구현을 살펴보면 34,000줄 이상의 코드로 구성되어 있습니다. 이러한 복잡성은 View 인스턴스를 생성하고 관리하는 데 상당한 처리 오버헤드가 수반됨을 보여줍니다. 결과적으로 불필요한 View 생성은 애플리케이션의 성능에 영향을 미쳐 메모리 사용량을 증가시키고 렌더링을 느리게 할 수 있습니다.

성능을 최적화하려면 가능한 한 불필요한 View 인스턴스를 피해야 합니다. 또한 깊게 중첩된 뷰가 measure, layout, draw 패스 시간을 증가시킬 수 있으므로 레이아웃 트리의 깊이를 줄이는 것이 중요합니다. UI 계층구조를 얕고 효율적으로 유지하면 더 부드러운 성능, 더 나은 반응성, 더 낮은 리소스 소비를 보장합니다.

### ViewGroup이란 무엇인가요?

ViewGroup은 여러 View나 다른 ViewGroup 요소를 담는 컨테이너입니다. LinearLayout, RelativeLayout, ConstraintLayout, FrameLayout과 같은 레이아웃의 기본 클래스입니다. ViewGroup은 자식 뷰들의 레이아웃과 위치를 관리하며, 화면에서 어떻게 측정되고 그려질지를 정의합니다.

```kotlin
val linearLayout = LinearLayout(context).apply {
    orientation = LinearLayout.VERTICAL
    addView(TextView(context).apply { text = "Child 1" })
    addView(TextView(context).apply { text = "Child 2" })
}
```

**추가 팁**: ViewGroup 클래스는 View를 확장하고 ViewParent와 ViewManager 인터페이스를 모두 구현합니다. ViewGroup은 다른 View 객체들의 컨테이너 역할을 하므로, 독립적인 View보다 본질적으로 더 복잡하고 리소스 집약적입니다. LinearLayout, RelativeLayout, ConstraintLayout과 같은 레이아웃들은 모두 ViewGroup 구현의 예시이며, ViewGroup 인스턴스의 과도한 중첩은 렌더링 성능에 부정적인 영향을 미칠 수 있습니다.

ViewParent 인터페이스는 View 객체들의 부모 역할을 하는 클래스의 책임을 정의하며, 레이아웃 측정, 터치 이벤트 처리, 그리기 순서를 관리합니다. 반면 ViewManager 인터페이스는 ViewGroup 계층구조 내에서 자식 뷰를 동적으로 추가하고 제거하는 메서드를 제공합니다. ViewGroup은 추가적인 레이아웃 계산을 수행하고 여러 자식 뷰를 관리해야 하므로, 불필요한 중첩을 줄이는 것이 성능 최적화와 부드러운 UI 렌더링을 보장하는 데 필수적입니다.

### View와 ViewGroup의 주요 차이점

1. **목적**
• View는 콘텐츠를 표시하거나 사용자와 상호작용하도록 설계된 단일 UI 요소입니다.
• ViewGroup은 여러 자식 뷰를 구성하고 관리하는 컨테이너입니다.
2. **계층구조**
• View는 UI 계층구조의 리프 노드로, 다른 뷰를 포함할 수 없습니다.
• ViewGroup은 여러 자식 뷰나 다른 ViewGroup 요소를 포함할 수 있는 브랜치 노드입니다.
3. **레이아웃 동작**
• View는 레이아웃 매개변수에 의해 정의되는 자체 크기와 위치를 가집니다.
• ViewGroup은 LinearLayout이나 ConstraintLayout에서 정의되는 특정 레이아웃 규칙을 사용하여 자식 뷰들의 크기와 위치를 정의합니다.
4. **상호작용 처리**
• View는 자체 터치 및 키 이벤트를 처리합니다.
• ViewGroup은 onInterceptTouchEvent 같은 메서드를 사용하여 자식들의 이벤트를 가로채고 관리할 수 있습니다.
5. **성능 고려사항**
• ViewGroup은 계층적 구조로 인해 렌더링에 복잡성을 추가합니다. 중첩된 ViewGroup을 과도하게 사용하면 렌더링 시간 증가와 UI 업데이트 속도 저하 같은 성능 문제가 발생할 수 있습니다.

### 요약

View는 모든 UI 요소의 기반이며, ViewGroup은 여러 View 객체를 구성하고 관리하는 컨테이너 역할을 합니다. 둘이 함께 복잡한 Android 사용자 인터페이스를 구축하는 빌딩 블록을 형성합니다. 이들의 역할과 차이점을 이해하는 것은 레이아웃을 최적화하고 반응성이 좋은 사용자 경험을 보장하는 데 필수적입니다.

### 실전 질문

**Q) View 생명주기에서 requestLayout(), invalidate(), postInvalidate()가 어떻게 작동하는지 설명하고, 각각을 언제 사용해야 하는지 설명하세요.**

**Q) View 생명주기가 Activity 생명주기와 어떻게 다른지, 그리고 효율적인 UI 렌더링을 위해 둘 다 이해하는 것이 왜 중요한지 설명하세요.**

### Q) 35. ViewStub을 사용해본 적이 있나요? 이를 사용하여 UI 성능을 어떻게 최적화하나요?

ViewStub은 명시적으로 필요할 때까지 레이아웃의 인플레이션을 지연시키는 데 사용되는 가볍고 보이지 않는 플레이스홀더 뷰입니다. 앱의 생명주기 동안 즉시 또는 전혀 필요하지 않을 수 있는 뷰들의 인플레이션 오버헤드를 피함으로써 성능을 개선하는 데 자주 사용됩니다.

### ViewStub의 주요 특성

1. **가벼움**: ViewStub은 인플레이션될 때까지 레이아웃 공간을 차지하지 않고 리소스를 소비하지 않으므로 최소한의 메모리 사용량을 가진 극도로 가벼운 뷰입니다.
2. **지연된 인플레이션**: ViewStub에 지정된 실제 레이아웃은 inflate() 메서드가 호출되거나 ViewStub이 보이게 될 때만 인플레이션됩니다.
3. **일회용**: 인플레이션되면 ViewStub은 뷰 계층구조에서 인플레이션된 레이아웃으로 대체되며 재사용될 수 없습니다.

### ViewStub의 일반적인 사용 사례

1. **조건부 레이아웃**: ViewStub은 에러 메시지, 진행 표시줄, 선택적 UI 요소와 같이 조건부로 표시되는 레이아웃에 이상적입니다.
2. **초기 로드 시간 단축**: 복잡하거나 리소스가 많이 드는 뷰의 인플레이션을 지연시킴으로써 ViewStub은 액티비티나 프래그먼트의 초기 렌더링 시간을 개선하는 데 도움이 됩니다.
3. **동적 UI 요소**: 필요할 때만 화면에 동적 콘텐츠를 추가하는 데 사용될 수 있어 메모리 사용량을 최적화합니다.

### ViewStub 사용 방법

ViewStub은 인플레이션해야 할 레이아웃 리소스를 가리키는 속성과 함께 XML 레이아웃에서 정의됩니다.

**activity_main.xml**

```kotlin
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    *<!-- 일반 Views -->*
    <TextView
        android:id="@+id/title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Main Content" />

    *<!-- 플레이스홀더 ViewStub -->*
    <ViewStub
        android:id="@+id/viewStub"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout="@layout/optional_content" />
</LinearLayout>
```

**ViewStub 인플레이션**

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val viewStub = findViewById<ViewStub>(R.id.viewStub)

        *// 필요할 때 레이아웃 인플레이션*
        val inflatedView = viewStub.inflate()

        *// 인플레이션된 레이아웃의 뷰에 액세스*
        val optionalTextView = inflatedView.findViewById<TextView>(R.id.optionalText)
        optionalTextView.text = "Inflated Content"
    }
}
```

### ViewStub의 장점

1. **최적화된 성능**: 항상 보이지 않을 수 있는 뷰의 생성을 지연시켜 메모리 사용량을 줄이고 초기 렌더링 성능을 개선합니다.
2. **단순화된 레이아웃 관리**: 프로그래밍 방식으로 뷰를 수동으로 추가하거나 제거하지 않고도 선택적 UI 요소를 쉽게 관리할 수 있습니다.
3. **사용 편의성**: 간단한 API와 XML 통합으로 ViewStub을 개발자에게 편리한 도구로 만듭니다.

### ViewStub의 제한사항

1. **일회용**: 인플레이션되면 ViewStub은 뷰 계층구조에서 제거되며 재사용될 수 없습니다.
2. **제한된 제어**: 플레이스홀더이므로 인플레이션될 때까지 사용자 상호작용을 처리하거나 복잡한 작업을 수행할 수 없습니다.

### 요약

ViewStub은 필요할 때까지 레이아웃의 인플레이션을 지연시켜 성능을 최적화하는 Android의 유용한 도구입니다. 조건부 레이아웃이나 항상 필요하지 않을 수 있는 뷰에 특히 유용하며, 메모리 사용량을 줄이고 앱 반응성을 개선하는 데 도움이 됩니다. ViewStub의 적절한 사용은 더 효율적이고 간소화된 사용자 경험으로 이어질 수 있습니다.

### 실전 질문

**Q) ViewStub이 인플레이션될 때 무엇이 일어나는지, 그리고 레이아웃 성능과 메모리 사용량 측면에서 뷰 계층구조에 어떤 영향을 미치는지 설명하세요.**

---

## 실전 질문 답변

### Q1) requestLayout(), invalidate(), postInvalidate() 동작 원리와 사용 시점

**답변:**

```kotlin
class CustomOptimizedView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : View(context, attrs, defStyleAttr) {
    
    private var customWidth = 0
    private var customHeight = 0
    private var backgroundColor = Color.BLUE
    private var textContent = "Sample Text"
    
    *// 1. requestLayout() - 크기나 위치가 변경될 때 사용*
    fun updateDimensions(newWidth: Int, newHeight: Int) {
        if (customWidth != newWidth || customHeight != newHeight) {
            customWidth = newWidth
            customHeight = newHeight
            
            *// 크기 변경 시 measure와 layout 과정을 다시 수행*
            requestLayout() *// onMeasure() -> onLayout() -> onDraw() 순서로 실행*
        }
    }
    
    *// 2. invalidate() - UI 스레드에서 외관만 변경될 때 사용*
    fun updateBackgroundColor(newColor: Int) {
        if (backgroundColor != newColor) {
            backgroundColor = newColor
            
            *// 크기는 변경 없이 다시 그리기만 필요*
            invalidate() *// onDraw()만 호출됨*
        }
    }
    
    *// 3. postInvalidate() - 백그라운드 스레드에서 UI 업데이트가 필요할 때 사용*
    fun updateTextFromBackground(newText: String) {
        *// 백그라운드 스레드에서 실행되는 작업*
        Thread {
            *// 시간이 오래 걸리는 작업 시뮬레이션*
            Thread.sleep(1000)
            
            textContent = newText
            
            *// 백그라운드 스레드에서는 invalidate() 대신 postInvalidate() 사용*
            postInvalidate() *// 메인 스레드로 invalidate() 요청을 전달*
        }.start()
    }
    
    override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec)
        
        *// requestLayout()이 호출되었을 때만 실행됨*
        val desiredWidth = if (customWidth > 0) customWidth else suggestedMinimumWidth
        val desiredHeight = if (customHeight > 0) customHeight else suggestedMinimumHeight
        
        setMeasuredDimension(
            resolveSize(desiredWidth, widthMeasureSpec),
            resolveSize(desiredHeight, heightMeasureSpec)
        )
        
        Log.d("CustomView", "onMeasure called - Size: ${measuredWidth}x${measuredHeight}")
    }
    
    override fun onLayout(changed: Boolean, left: Int, top: Int, right: Int, bottom: Int) {
        super.onLayout(changed, left, top, right, bottom)
        
        *// requestLayout()이 호출되었을 때만 실행됨*
        Log.d("CustomView", "onLayout called - Position: ($left,$top,$right,$bottom)")
    }
    
    override fun onDraw(canvas: Canvas) {
        super.onDraw(canvas)
        
        *// requestLayout(), invalidate(), postInvalidate() 모두에서 호출됨*
        canvas.drawColor(backgroundColor)
        
        val paint = Paint().apply {
            color = Color.WHITE
            textSize = 48f
            textAlign = Paint.Align.CENTER
        }
        
        canvas.drawText(
            textContent,
            width / 2f,
            height / 2f,
            paint
        )
        
        Log.d("CustomView", "onDraw called")
    }
    
    *// 사용 예시*
    companion object {
        fun demonstrateUsage(view: CustomOptimizedView) {
            *// 크기 변경 - requestLayout() 사용*
            view.updateDimensions(300, 200)
            
            *// 색상 변경 - invalidate() 사용*
            view.updateBackgroundColor(Color.RED)
            
            *// 백그라운드에서 텍스트 변경 - postInvalidate() 사용*
            view.updateTextFromBackground("Updated Text")
        }
    }
}

*// 최적화된 ViewGroup에서의 사용 예시*
class OptimizedViewGroup @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : ViewGroup(context, attrs, defStyleAttr) {
    
    private var needsLayout = false
    private var layoutRequestCount = 0
    
    override fun requestLayout() {
        *// 중복 requestLayout 호출 방지*
        if (!needsLayout) {
            needsLayout = true
            layoutRequestCount++
            super.requestLayout()
            
            Log.d("OptimizedViewGroup", "requestLayout called - Count: $layoutRequestCount")
        }
    }
    
    override fun onLayout(changed: Boolean, l: Int, t: Int, r: Int, b: Int) {
        needsLayout = false
        
        *// 자식 뷰들 레이아웃*
        for (i in 0 until childCount) {
            val child = getChildAt(i)
            child.layout(0, i * 100, child.measuredWidth, (i + 1) * 100)
        }
    }
    
    override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
        measureChildren(widthMeasureSpec, heightMeasureSpec)
        setMeasuredDimension(
            MeasureSpec.getSize(widthMeasureSpec),
            childCount * 100
        )
    }
}
```

**사용 시점 정리:**

1. **requestLayout()**:
    - View의 크기나 위치가 변경될 때
    - 레이아웃 파라미터가 수정될 때
    - measure → layout → draw 전체 과정 실행
2. **invalidate()**:
    - UI 스레드에서 View의 외관만 변경될 때
    - 색상, 텍스트, 이미지 등의 변경
    - draw 과정만 실행
3. **postInvalidate()**:
    - 백그라운드 스레드에서 UI 업데이트가 필요할 때
    - 내부적으로 Handler를 통해 메인 스레드로 전달
    - 스레드 안전한 invalidate() 호출

### Q2) View 생명주기와 Activity 생명주기의 차이점

**답변:**

```kotlin
class LifecycleAwareCustomView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : View(context, attrs, defStyleAttr), LifecycleObserver {
    
    private var activityLifecycleState = "UNKNOWN"
    private var viewLifecycleState = "CREATED"
    private var animationRunning = false
    
    init {
        *// Activity 생명주기 관찰*
        findViewTreeLifecycleOwner()?.lifecycle?.addObserver(this)
    }
    
    *// Activity 생명주기 콜백들*
    @OnLifecycleEvent(Lifecycle.Event.ON_CREATE)
    fun onActivityCreate() {
        activityLifecycleState = "CREATED"
        Log.d("Lifecycle", "Activity Created, View State: $viewLifecycleState")
    }
    
    @OnLifecycleEvent(Lifecycle.Event.ON_START)
    fun onActivityStart() {
        activityLifecycleState = "STARTED"
        Log.d("Lifecycle", "Activity Started, View State: $viewLifecycleState")
    }
    
    @OnLifecycleEvent(Lifecycle.Event.ON_RESUME)
    fun onActivityResume() {
        activityLifecycleState = "RESUMED"
        startOptimizedAnimations()
        Log.d("Lifecycle", "Activity Resumed, View State: $viewLifecycleState")
    }
    
    @OnLifecycleEvent(Lifecycle.Event.ON_PAUSE)
    fun onActivityPause() {
        activityLifecycleState = "PAUSED"
        pauseAnimations()
        Log.d("Lifecycle", "Activity Paused, View State: $viewLifecycleState")
    }
    
    @OnLifecycleEvent(Lifecycle.Event.ON_DESTROY)
    fun onActivityDestroy() {
        activityLifecycleState = "DESTROYED"
        cleanupResources()
        Log.d("Lifecycle", "Activity Destroyed, View State: $viewLifecycleState")
    }
    
    *// View 생명주기 콜백들*
    override fun onAttachedToWindow() {
        super.onAttachedToWindow()
        viewLifecycleState = "ATTACHED"
        Log.d("Lifecycle", "View Attached, Activity State: $activityLifecycleState")
        
        *// View가 윈도우에 연결된 후에만 애니메이션 시작*
        if (activityLifecycleState == "RESUMED") {
            startOptimizedAnimations()
        }
    }
    
    override fun onDetachedFromWindow() {
        super.onDetachedFromWindow()
        viewLifecycleState = "DETACHED"
        stopAllAnimations()
        Log.d("Lifecycle", "View Detached, Activity State: $activityLifecycleState")
    }
    
    override fun onVisibilityChanged(changedView: View, visibility: Int) {
        super.onVisibilityChanged(changedView, visibility)
        
        when (visibility) {
            VISIBLE -> {
                viewLifecycleState = "VISIBLE"
                if (activityLifecycleState == "RESUMED") {
                    resumeAnimations()
                }
            }
            INVISIBLE, GONE -> {
                viewLifecycleState = "HIDDEN"
                pauseAnimations()
            }
        }
        
        Log.d("Lifecycle", "View Visibility Changed: $visibility")
    }
    
    override fun onWindowFocusChanged(hasWindowFocus: Boolean) {
        super.onWindowFocusChanged(hasWindowFocus)
        
        if (hasWindowFocus && activityLifecycleState == "RESUMED" && viewLifecycleState == "VISIBLE") {
            resumeAnimations()
        } else {
            pauseAnimations()
        }
        
        Log.d("Lifecycle", "Window Focus Changed: $hasWindowFocus")
    }
    
    *// 효율적인 리소스 관리*
    private fun startOptimizedAnimations() {
        if (!animationRunning && 
            activityLifecycleState == "RESUMED" && 
            viewLifecycleState in listOf("ATTACHED", "VISIBLE")) {
            
            animationRunning = true
            *// 애니메이션 시작 로직*
            Log.d("Lifecycle", "Starting animations - optimized timing")
        }
    }
    
    private fun pauseAnimations() {
        if (animationRunning) {
            animationRunning = false
            *// 애니메이션 일시정지 로직*
            Log.d("Lifecycle", "Pausing animations to save resources")
        }
    }
    
    private fun resumeAnimations() {
        if (!animationRunning && shouldStartAnimations()) {
            startOptimizedAnimations()
        }
    }
    
    private fun stopAllAnimations() {
        animationRunning = false
        *// 모든 애니메이션 중지*
        Log.d("Lifecycle", "Stopping all animations")
    }
    
    private fun shouldStartAnimations(): Boolean {
        return activityLifecycleState == "RESUMED" && 
               viewLifecycleState in listOf("ATTACHED", "VISIBLE") &&
               hasWindowFocus()
    }
    
    private fun cleanupResources() {
        stopAllAnimations()
        *// 기타 리소스 정리*
        findViewTreeLifecycleOwner()?.lifecycle?.removeObserver(this)
    }
    
    override fun onDraw(canvas: Canvas) {
        super.onDraw(canvas)
        
        *// 상태에 따른 최적화된 렌더링*
        if (viewLifecycleState == "VISIBLE" && activityLifecycleState == "RESUMED") {
            drawFullContent(canvas)
        } else {
            drawMinimalContent(canvas)
        }
    }
    
    private fun drawFullContent(canvas: Canvas) {
        *// 전체 콘텐츠 렌더링 (비용이 많이 드는 작업)*
        canvas.drawColor(Color.BLUE)
        *// 복잡한 그리기 작업들...*
    }
    
    private fun drawMinimalContent(canvas: Canvas) {
        *// 최소한의 콘텐츠만 렌더링 (성능 최적화)*
        canvas.drawColor(Color.GRAY)
    }
}

*// 사용 예시*
class MainActivity : AppCompatActivity() {
    private lateinit var customView: LifecycleAwareCustomView
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        customView = LifecycleAwareCustomView(this)
        setContentView(customView)
    }
}
```

**주요 차이점:**

1. **범위**: Activity는 전체 화면, View는 특정 UI 요소
2. **타이밍**: View 생명주기는 Activity 생명주기 내에서 독립적으로 실행
3. **최적화 포인트**: 두 생명주기를 모두 고려해야 최적의 성능 달성 가능

### Q3) ViewStub 인플레이션 시 발생하는 변화와 성능 영향

**답변:**

```kotlin
class ViewStubOptimizationDemo : AppCompatActivity() {
    
    private var viewStub: ViewStub? = null
    private var inflatedView: View? = null
    private var isInflated = false
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        viewStub = findViewById(R.id.viewStub)
        
        *// ViewStub의 상태 모니터링*
        monitorViewStubState()
        
        *// 조건부 인플레이션 데모*
        findViewById<Button>(R.id.btnInflate).setOnClickListener {
            inflateViewStubSafely()
        }
        
        findViewById<Button>(R.id.btnToggle).setOnClickListener {
            toggleContentVisibility()
        }
    }
    
    private fun monitorViewStubState() {
        Log.d("ViewStub", "Initial state:")
        Log.d("ViewStub", "ViewStub exists: ${viewStub != null}")
        Log.d("ViewStub", "ViewStub parent: ${viewStub?.parent}")
        Log.d("ViewStub", "ViewStub layout params: ${viewStub?.layoutParams}")
        
        *// 초기 메모리 사용량 측정*
        logMemoryUsage("Before inflation")
    }
    
    private fun inflateViewStubSafely() {
        if (!isInflated && viewStub != null) {
            val startTime = System.currentTimeMillis()
            
            try {
                *// ViewStub 인플레이션 실행*
                inflatedView = viewStub!!.inflate()
                isInflated = true
                
                val inflationTime = System.currentTimeMillis() - startTime
                Log.d("ViewStub", "Inflation completed in ${inflationTime}ms")
                
                *// 인플레이션 후 상태 확인*
                analyzePostInflationState()
                
                *// 인플레이션된 뷰 설정*
                setupInflatedView()
                
            } catch (e: Exception) {
                Log.e("ViewStub", "Inflation failed", e)
            }
        } else {
            Log.w("ViewStub", "Already inflated or ViewStub is null")
        }
    }
    
    private fun analyzePostInflationState() {
        Log.d("ViewStub", "Post-inflation analysis:")
        
        *// ViewStub 상태*
        Log.d("ViewStub", "ViewStub after inflation: ${viewStub}")
        Log.d("ViewStub", "ViewStub parent after inflation: ${viewStub?.parent}")
        
        *// 인플레이션된 뷰 상태*
        inflatedView?.let { view ->
            Log.d("ViewStub", "Inflated view type: ${view.javaClass.simpleName}")
            Log.d("ViewStub", "Inflated view parent: ${view.parent}")
            Log.d("ViewStub", "Inflated view layout params: ${view.layoutParams}")
            Log.d("ViewStub", "Inflated view size: ${view.width}x${view.height}")
            
            *// 자식 뷰 개수 확인*
            if (view is ViewGroup) {
                Log.d("ViewStub", "Child views count: ${view.childCount}")
                analyzeChildViews(view)
            }
        }
        
        *// 메모리 사용량 변화 측정*
        logMemoryUsage("After inflation")
        
        *// ViewStub이 뷰 계층구조에서 제거되었는지 확인*
        checkViewHierarchyChanges()
    }
    
    private fun analyzeChildViews(viewGroup: ViewGroup) {
        for (i in 0 until viewGroup.childCount) {
            val child = viewGroup.getChildAt(i)
            Log.d("ViewStub", "Child $i: ${child.javaClass.simpleName}")
        }
    }
    
    private fun checkViewHierarchyChanges() {
        val rootView = findViewById<ViewGroup>(android.R.id.content)
        val viewCount = countAllViews(rootView)
        Log.d("ViewStub", "Total views in hierarchy: $viewCount")
    }
    
    private fun countAllViews(view: View): Int {
        return if (view is ViewGroup) {
            1 + (0 until view.childCount).sumOf { countAllViews(view.getChildAt(it)) }
        } else {
            1
        }
    }
    
    private fun setupInflatedView() {
        inflatedView?.let { view ->
            *// 인플레이션된 뷰의 구성 요소들 설정*
            val textView = view.findViewById<TextView>(R.id.dynamicText)
            val imageView = view.findViewById<ImageView>(R.id.dynamicImage)
            val button = view.findViewById<Button>(R.id.dynamicButton)
            
            textView?.text = "동적으로 로드된 콘텐츠"
            
            *// 이미지 로딩 (Glide 등 사용)*
            imageView?.let { 
                loadImageOptimized(it, "https://example.com/image.jpg")
            }
            
            button?.setOnClickListener {
                Log.d("ViewStub", "Dynamic button clicked")
            }
            
            *// 애니메이션 적용*
            animateInflatedView(view)
        }
    }
    
    private fun loadImageOptimized(imageView: ImageView, url: String) {
        *// 이미지 로딩 최적화*
        Glide.with(this)
            .load(url)
            .placeholder(R.drawable.placeholder)
            .error(R.drawable.error_image)
            .diskCacheStrategy(DiskCacheStrategy.ALL)
            .into(imageView)
    }
    
    private fun animateInflatedView(view: View) {
        *// 부드러운 등장 애니메이션*
        view.alpha = 0f
        view.translationY = 100f
        
        view.animate()
            .alpha(1f)
            .translationY(0f)
            .setDuration(300)
            .setInterpolator(DecelerateInterpolator())
            .start()
    }
    
    private fun toggleContentVisibility() {
        inflatedView?.let { view ->
            if (view.visibility == View.VISIBLE) {
                *// 숨기기 (하지만 메모리에는 유지)*
                view.animate()
                    .alpha(0f)
                    .setDuration(200)
                    .withEndAction {
                        view.visibility = View.GONE
                    }
                    .start()
            } else {
                *// 보이기*
                view.visibility = View.VISIBLE
                view.animate()
                    .alpha(1f)
                    .setDuration(200)
                    .start()
            }
        } ?: run {
            Log.w("ViewStub", "Cannot toggle - view not inflated yet")
        }
    }
    
    private fun logMemoryUsage(phase: String) {
        val runtime = Runtime.getRuntime()
        val usedMemory = runtime.totalMemory() - runtime.freeMemory()
        val maxMemory = runtime.maxMemory()
        
        Log.d("Memory", "$phase:")
        Log.d("Memory", "Used: ${usedMemory / 1024 / 1024}MB")
        Log.d("Memory", "Max: ${maxMemory / 1024 / 1024}MB")
        Log.d("Memory", "Free: ${(maxMemory - usedMemory) / 1024 / 1024}MB")
    }
    
    override fun onDestroy() {
        super.onDestroy()
        
        *// ViewStub 관련 리소스 정리*
        inflatedView = null
        viewStub = null
        
        Log.d("ViewStub", "Activity destroyed - resources cleaned up")
    }
}

*// 고급 ViewStub 관리 유틸리티*
class ViewStubManager {
    private val inflatedViews = mutableMapOf<String, View>()
    private val inflationTimes = mutableMapOf<String, Long>()
    
    fun inflateViewStub(
        context: Context,
        viewStub: ViewStub?,
        identifier: String,
        onInflated: (View) -> Unit = {}
    ): View? {
        if (inflatedViews.containsKey(identifier)) {
            Log.w("ViewStubManager", "ViewStub '$identifier' already inflated")
            return inflatedViews[identifier]
        }
        
        return viewStub?.let { stub ->
            val startTime = System.currentTimeMillis()
            
            try {
                val inflatedView = stub.inflate()
                val inflationTime = System.currentTimeMillis() - startTime
                
                inflatedViews[identifier] = inflatedView
                inflationTimes[identifier] = inflationTime
                
                Log.d("ViewStubManager", "ViewStub '$identifier' inflated in ${inflationTime}ms")
                
                onInflated(inflatedView)
                inflatedView
                
            } catch (e: Exception) {
                Log.e("ViewStubManager", "Failed to inflate ViewStub '$identifier'", e)
                null
            }
        }
    }
    
    fun getInflatedView(identifier: String): View? {
        return inflatedViews[identifier]
    }
    
    fun isInflated(identifier: String): Boolean {
        return inflatedViews.containsKey(identifier)
    }
    
    fun getInflationTime(identifier: String): Long? {
        return inflationTimes[identifier]
    }
    
    fun getTotalInflationTime(): Long {
        return inflationTimes.values.sum()
    }
    
    fun clearInflatedView(identifier: String) {
        inflatedViews.remove(identifier)?.let { view ->
            if (view.parent is ViewGroup) {
                (view.parent as ViewGroup).removeView(view)
            }
        }
        inflationTimes.remove(identifier)
    }
    
    fun clearAll() {
        inflatedViews.clear()
        inflationTimes.clear()
    }
    
    *// 성능 분석 리포트*
    fun generatePerformanceReport(): String {
        return buildString {
            appendLine("=== ViewStub 성능 리포트 ===")
            appendLine("총 인플레이션된 ViewStub 수: ${inflatedViews.size}")
            appendLine("총 인플레이션 시간: ${getTotalInflationTime()}ms")
            appendLine("평균 인플레이션 시간: ${if (inflationTimes.isNotEmpty()) getTotalInflationTime() / inflationTimes.size else 0}ms")
            appendLine()
            appendLine("개별 ViewStub 성능:")
            inflationTimes.forEach { (id, time) ->
                appendLine("- $id: ${time}ms")
            }
        }
    }
}

*// 메모리 효율적인 ViewStub 사용 패턴*
class MemoryEfficientViewStubActivity : AppCompatActivity() {
    
    private val viewStubManager = ViewStubManager()
    private val conditionalViews = mutableMapOf<String, ViewStub>()
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_memory_efficient)
        
        *// 조건부 ViewStub들 등록*
        registerConditionalViews()
        
        *// 사용자 상태에 따른 조건부 UI 로드*
        loadUIBasedOnUserState()
    }
    
    private fun registerConditionalViews() {
        conditionalViews["premium_features"] = findViewById(R.id.stub_premium)
        conditionalViews["debug_panel"] = findViewById(R.id.stub_debug)
        conditionalViews["tutorial_overlay"] = findViewById(R.id.stub_tutorial)
        conditionalViews["error_feedback"] = findViewById(R.id.stub_error)
    }
    
    private fun loadUIBasedOnUserState() {
        *// 프리미엄 사용자인 경우에만 프리미엄 기능 UI 로드*
        if (UserPreferences.isPremiumUser()) {
            loadPremiumFeatures()
        }
        
        *// 디버그 빌드인 경우에만 디버그 패널 로드*
        if (BuildConfig.DEBUG) {
            loadDebugPanel()
        }
        
        *// 첫 실행 시에만 튜토리얼 로드*
        if (UserPreferences.isFirstLaunch()) {
            loadTutorial()
        }
    }
    
    private fun loadPremiumFeatures() {
        viewStubManager.inflateViewStub(
            this,
            conditionalViews["premium_features"],
            "premium_features"
        ) { view ->
            setupPremiumFeatures(view)
        }
    }
    
    private fun loadDebugPanel() {
        viewStubManager.inflateViewStub(
            this,
            conditionalViews["debug_panel"],
            "debug_panel"
        ) { view ->
            setupDebugPanel(view)
        }
    }
    
    private fun loadTutorial() {
        viewStubManager.inflateViewStub(
            this,
            conditionalViews["tutorial_overlay"],
            "tutorial_overlay"
        ) { view ->
            setupTutorial(view)
        }
    }
    
    private fun setupPremiumFeatures(view: View) {
        *// 프리미엄 기능 설정*
        val premiumButton = view.findViewById<Button>(R.id.btn_premium_feature)
        premiumButton.setOnClickListener {
            *// 프리미엄 기능 실행*
        }
    }
    
    private fun setupDebugPanel(view: View) {
        *// 디버그 패널 설정*
        val memoryInfo = view.findViewById<TextView>(R.id.tv_memory_info)
        updateMemoryInfo(memoryInfo)
    }
    
    private fun setupTutorial(view: View) {
        *// 튜토리얼 설정*
        val skipButton = view.findViewById<Button>(R.id.btn_skip_tutorial)
        skipButton.setOnClickListener {
            viewStubManager.clearInflatedView("tutorial_overlay")
            UserPreferences.setFirstLaunchCompleted()
        }
    }
    
    private fun updateMemoryInfo(textView: TextView) {
        val runtime = Runtime.getRuntime()
        val memoryInfo = "메모리 사용량: ${(runtime.totalMemory() - runtime.freeMemory()) / 1024 / 1024}MB"
        textView.text = memoryInfo
    }
    
    override fun onDestroy() {
        super.onDestroy()
        
        *// 성능 리포트 출력*
        Log.d("Performance", viewStubManager.generatePerformanceReport())
        
        *// 리소스 정리*
        viewStubManager.clearAll()
        conditionalViews.clear()
    }
}

*// 사용자 설정 관련 유틸리티 (예시)*
object UserPreferences {
    fun isPremiumUser(): Boolean = true *// 예시*
    fun isFirstLaunch(): Boolean = false *// 예시*
    fun setFirstLaunchCompleted() {} *// 예시*
}
```

**ViewStub 인플레이션 시 발생하는 주요 변화:**

1. **뷰 계층구조 변화:**
    - ViewStub이 뷰 트리에서 완전히 제거됨
    - 인플레이션된 뷰가 ViewStub의 위치에 삽입됨
    - ViewStub의 layoutParams가 인플레이션된 뷰로 전달됨
2. **메모리 사용량 변화:**
    - ViewStub 자체는 매우 가벼운 뷰 (거의 메모리 사용 없음)
    - 인플레이션 후 실제 레이아웃의 모든 뷰가 메모리에 로드됨
    - 복잡한 레이아웃일수록 메모리 사용량 급증
3. **성능 최적화 효과:**
    - 초기 액티비티 로딩 시간 단축
    - 불필요한 뷰 생성 방지로 배터리 수명 향상
    - 조건부 UI로 인한 앱 용량 효율성 증대
4. **주의사항:**
    - 인플레이션 후 ViewStub 참조는 null이 됨
    - 재사용 불가능 (일회성)
    - 인플레이션 타이밍 최적화 필요
