# 카테고리 0: 안드로이드 프레임워크

안드로이드 프레임워크는 안드로이드 애플리케이션을 구축하는 데 필요한 기본 지식입니다. 이는 안드로이드 SDK, 내부 구조, 안드로이드 런타임 시스템 등을 포함합니다. 이러한 구성 요소들은 안드로이드의 기본 시스템에 깊게 통합되어 있어, 안드로이드 개발자가 되기 위한 첫 번째 단계로 프레임워크를 이해하는 것이 필수적입니다.

이 카테고리에서는 Activity, Fragment, 안드로이드 UI를 포함한 안드로이드 SDK의 핵심 측면과 함께, 현대적인 안드로이드 개발에 유용한 Jetpack 라이브러리와 같은 추가 지식 영역을 다룰 것입니다. 또한 실제 환경에서 안드로이드 애플리케이션을 실행하는 데 필수적인 안드로이드 시스템과 비즈니스 로직에 대해서도 배울 것입니다.

이 카테고리는 전 세계의 모든 가능한 면접 질문을 다루는 것을 목표로 하지 않습니다. 마찬가지로 제공된 답변도 모든 면접관이 기대하는 결정적이거나 보편적인 답변으로 취급되어서는 안 됩니다. 대신 이를 통찰력을 얻고 안드로이드 프레임워크에 대한 더 깊은 이해를 향한 학습 여정을 안내하는 참고 자료로 여기십시오.

## Q) 0. 안드로이드란 무엇인가?

안드로이드는 주로 스마트폰과 태블릿과 같은 모바일 기기를 위해 설계된 오픈 소스 운영체제입니다. 구글에서 개발하고 유지 관리하며, 리눅스 커널을 기반으로 합니다. 안드로이드는 다양한 하드웨어 구성과 기기를 지원하는 견고하고 유연한 플랫폼을 제공합니다.

### 안드로이드 OS의 주요 특징

1. **오픈 소스 및 커스터마이징 가능**: 안드로이드는 오픈 소스(안드로이드 오픈 소스 프로젝트)로, 개발자와 제조업체가 필요에 따라 수정하고 커스터마이징할 수 있습니다. 이러한 유연성은 웨어러블, TV, IoT 기기 등 다양한 기기에서의 광범위한 채택과 혁신에 기여했습니다.
2. **SDK를 활용한 애플리케이션 개발**: 안드로이드 앱은 주로 Java 또는 Kotlin과 안드로이드 소프트웨어 개발 키트(SDK)를 사용하여 구축됩니다. 개발자는 안드로이드 스튜디오와 같은 도구를 사용하여 플랫폼용 애플리케이션을 설계, 개발, 디버그할 수 있습니다.
3. **풍부한 앱 생태계**: 구글 플레이 스토어는 안드로이드의 공식 앱 배포 플랫폼으로, 게임부터 생산성 도구까지 다양한 카테고리의 수백만 개의 앱을 제공합니다. 개발자는 또한 서드파티 스토어나 직접 다운로드를 통해 앱을 독립적으로 배포할 수도 있습니다.
4. **멀티태스킹 및 리소스 관리**: 안드로이드는 멀티태스킹을 지원하여 사용자가 여러 앱을 동시에 실행할 수 있습니다. 관리되는 메모리 시스템과 효율적인 가비지 컬렉션을 사용하여 다양한 기기에서 성능을 최적화합니다.
5. **다양한 하드웨어 지원**: 안드로이드는 저가형 폰부터 프리미엄 플래그십 모델까지 광범위한 기기를 지원하며, 다양한 화면 크기, 해상도, 하드웨어 구성과의 광범위한 호환성을 제공합니다.

## 안드로이드 아키텍처

안드로이드의 플랫폼 아키텍처는 모듈식이고 계층화되어 있으며, 여러 구성 요소로 이루어져 있습니다:
![image](https://github.com/user-attachments/assets/563ca18f-28e2-4fad-908e-cb99528f6b82)

### 아키텍처 구성 요소

![image](https://github.com/user-attachments/assets/63ff226f-e574-44e9-9291-798c2a64eaf1)

- **리눅스 커널**: 리눅스 커널은 안드로이드 운영체제의 기반을 형성합니다. 하드웨어 추상화를 처리하여 소프트웨어와 하드웨어 간의 원활한 상호작용을 보장합니다. 주요 책임에는 메모리 및 프로세스 관리, 보안 강화, Wi-Fi, 블루투스, 디스플레이와 같은 하드웨어 구성 요소의 장치 드라이버 관리가 포함됩니다.

![image](https://github.com/user-attachments/assets/878d364b-0af6-468e-8323-87e33aacd8dd)

- **하드웨어 추상화 계층(HAL)**: 하드웨어 추상화 계층(HAL)은 안드로이드의 Java API 프레임워크를 기기 하드웨어에 연결하는 표준 인터페이스를 제공합니다. 카메라나 블루투스와 같은 특정 하드웨어 구성 요소에 맞춰진 라이브러리 모듈로 구성됩니다. 프레임워크 API가 하드웨어 액세스를 요청할 때, 안드로이드 시스템은 요청을 처리하기 위해 해당 HAL 모듈을 동적으로 로드합니다.

![image](https://github.com/user-attachments/assets/2ec49b69-837b-4245-a272-4d7b135209fb)

- **안드로이드 런타임(ART) 및 핵심 라이브러리**: 안드로이드 런타임(ART)은 Kotlin 또는 Java에서 컴파일된 바이트코드를 사용하여 애플리케이션을 실행합니다. ART는 최적화된 성능을 위해 AOT(Ahead-of-Time) 및 JIT(Just-in-Time) 컴파일을 지원합니다. 핵심 라이브러리는 데이터 구조, 파일 조작, 스레딩 등을 위한 필수 API를 제공하여 앱 개발을 위한 포괄적인 환경을 제공합니다.

<aside>

Java 같은 컴파일 언어는 개발자가 작성한 원시 코드를 통으로 기계어(어셈블리어)로 변환한 뒤에 기계를 통해서 실행하는 방식이다. Java는 bytecode로 compile되기 때문에 이를 실행하기 위해서는 JVM이 필요하게 된다. 그러나, 라이선스 문제로 인해서 Android는 JVM을 사용하지 않고 Dalvik과 ART를 별도로 개발하여 적용했다. ART는 DEX파일(Android용으로 특별히 설계된 바이트코드형식)을 실행하여 저용량 메모리 기기에서 여러 VM을 실행하도록 작성되었다. Android 5(API 21) 이전에는 Dalvik이라는 VM을 사용했다고 한다. 하지만, 성능과 배터리에 악영향 문제로 Dalvik VM을 폐지하고 지금의 ART로 완전히 대체했다고 한다.

</aside>

![image](https://github.com/user-attachments/assets/af0685b4-3908-4c46-8691-1007ce524774)

- **네이티브 C/C++ 라이브러리**: 안드로이드는 중요한 기능을 지원하기 위해 C와 C++로 작성된 네이티브 라이브러리 세트를 포함합니다. OpenGL과 같은 라이브러리는 그래픽 렌더링을 관리하고, SQLite는 데이터베이스 작업을 가능하게 하며, WebKit은 웹 콘텐츠 표시를 용이하게 합니다. 이러한 라이브러리는 안드로이드 프레임워크와 성능 집약적인 작업을 위한 애플리케이션에서 직접 사용됩니다.

![image](https://github.com/user-attachments/assets/a3b73250-e12f-4474-b5ea-175a2df3a99f)

- **안드로이드 프레임워크(API)**: 애플리케이션 프레임워크 계층은 앱 개발을 위한 상위 수준 서비스와 API를 제공합니다. ActivityManager, NotificationManager, Content Provider 등을 포함하여 개발자가 안드로이드 애플리케이션을 구축할 수 있게 합니다. 이 계층은 개발자가 안드로이드 시스템 기능을 효율적으로 활용할 수 있도록 지원합니다.

![image](https://github.com/user-attachments/assets/ae27e611-cd35-4875-a560-7207f6a52bc9)

- **애플리케이션**: 최상위 계층은 시스템 앱(예: 연락처, 설정)과 안드로이드 SDK를 사용하여 생성된 서드파티 앱을 포함한 모든 사용자 대면 앱을 포함합니다. 이러한 앱은 사용자에게 기능을 원활하게 제공하기 위해 하위 계층에 의존합니다.

## 요약

안드로이드는 전 세계적으로 가장 널리 사용되는 모바일 운영체제로, 압도적인 시장 점유율을 보유하고 있습니다. 혁신을 촉진하고 개발자가 수십억 명의 사용자를 위한 애플리케이션을 만들 수 있는 기회를 제공합니다. 적응성과 오픈 소스 특성으로 인해 안드로이드는 다양한 시장에서 번영할 수 있었고, 스마트폰을 넘어선 수많은 기기의 기반이 되었습니다.

## 실습 질문

**Q) 안드로이드의 플랫폼 아키텍처는 리눅스 커널, 안드로이드 런타임(ART), 하드웨어 추상화 계층(HAL)을 포함한 여러 계층으로 구성되어 있습니다. 이러한 구성 요소들이 어떻게 함께 작동하여 애플리케이션 실행과 하드웨어 상호작용을 보장하는지 설명할 수 있나요?**

---

### 용어 설명

- **AOT(Ahead-of-Time) 컴파일**: 런타임 이전에 코드를 기계 코드로 컴파일하는 과정으로, 실행 중 JIT(Just-In-Time) 컴파일의 필요성을 제거합니다. 이 방식은 최적화된 사전 컴파일된 바이너리를 생성하여 성능을 향상시키고 런타임 오버헤드를 줄입니다.
- **JIT(Just-In-Time) 컴파일**: 바이트코드가 실행 직전에 기계 코드로 동적으로 번역되는 런타임 프로세스입니다. 이를 통해 런타임 환경이 실제 실행 패턴을 기반으로 코드를 최적화할 수 있어, 자주 사용되는 코드 경로의 성능이 향상됩니다.

[Platform architecture  |  Android Developers](https://developer.android.com/guide/platform/index.html)

## Q) 1. 인텐트(Intent)란 무엇인가?

인텐트(Intent)는 수행될 작업에 대한 추상적인 설명입니다. 액티비티, 서비스, 브로드캐스트 리시버 간의 통신을 가능하게 하는 메시징 객체 역할을 합니다. 인텐트는 일반적으로 액티비티를 시작하거나, 브로드캐스트를 전송하거나, 서비스를 시작하는 데 사용됩니다. 또한 구성 요소 간에 데이터를 전달할 수 있어 안드로이드의 컴포넌트 기반 아키텍처의 기본 요소입니다.

안드로이드에는 두 가지 주요 인텐트 유형이 있습니다: 명시적 인텐트와 암시적 인텐트입니다.

### 1. 명시적 인텐트(Explicit Intent)

- **정의**: 명시적 인텐트는 직접 이름을 지정하여 호출할 정확한 구성 요소(액티비티 또는 서비스)를 지정합니다.
- **사용 사례**: 명시적 인텐트는 대상 구성 요소를 알고 있을 때 사용됩니다(예: 앱 내에서 특정 액티비티 시작).
- **시나리오**: 같은 앱 내에서 한 액티비티에서 다른 액티비티로 전환할 때 명시적 인텐트를 사용합니다.

아래 코드와 같이 명시적 인텐트를 사용할 수 있습니다:

```kotlin
val intent = Intent(this, TargetActivity::class.java)
startActivity(intent)
```

### 2. 암시적 인텐트(Implicit Intent)

- **정의**: 암시적 인텐트는 특정 구성 요소를 지정하지 않고 수행할 일반적인 작업을 선언합니다. 시스템은 작업, 카테고리, 데이터를 기반으로 인텐트를 처리할 수 있는 구성 요소를 결정합니다.
- **사용 사례**: 암시적 인텐트는 다른 앱이나 시스템 구성 요소가 처리할 수 있는 작업을 수행하려는 경우에 유용합니다(예: URL 열기 또는 콘텐츠 공유).
- **시나리오**: 브라우저에서 웹 페이지를 열거나 다른 앱과 콘텐츠를 공유할 때 암시적 인텐트를 사용합니다. 시스템이 인텐트를 처리할 앱을 결정합니다.

아래 예시와 같이 암시적 인텐트를 사용할 수 있습니다:

```kotlin
val intent = Intent(Intent.ACTION_VIEW)
intent.data = Uri.parse("https://www.example.com")
startActivity(intent)
```

### 요약

명시적 인텐트는 대상 구성 요소를 알고 있는 내부 앱 네비게이션에 사용됩니다. 반면 암시적 인텐트는 대상을 직접 지정하지 않고 외부 앱이나 다른 구성 요소가 처리할 수 있는 작업에 사용됩니다. 이를 통해 안드로이드 생태계가 더욱 유연해지고 앱들이 원활하게 상호작용할 수 있습니다.

## 실습 질문

**Q) 명시적 인텐트와 암시적 인텐트의 주요 차이점은 무엇이며, 각각을 어떤 시나리오에서 사용하나요?**

**Q) 안드로이드 시스템은 암시적 인텐트를 처리할 앱을 어떻게 결정하며, 적절한 애플리케이션이 없는 경우 어떻게 되나요?**

## 숙련도 향상 팁: 인텐트 필터란?

인텐트 필터(Intent Filter)는 안드로이드에서 앱 구성 요소가 링크 열기나 브로드캐스트 처리와 같은 특정 인텐트에 응답할 수 있는 방법을 정의합니다. 액티비티, 서비스, 브로드캐스트 리시버가 처리할 수 있는 인텐트 유형을 선언하는 필터 역할을 하며, AndroidManifest.xml 파일에 지정됩니다.

각 인텐트 필터는 들어오는 인텐트를 정확히 매칭하기 위해 액션, 카테고리, 데이터 유형을 포함할 수 있습니다. 인텐트 필터를 적절히 정의하면 앱이 다른 앱 및 시스템 구성 요소와 원활하게 상호작용하여 기능을 향상시킬 수 있습니다.

암시적 인텐트가 전송되면, 안드로이드 시스템은 인텐트의 속성을 설치된 앱의 매니페스트 파일에 정의된 인텐트 필터와 매칭하여 적절한 구성 요소를 시작할 구성 요소를 결정합니다. 매칭이 발견되면 시스템은 해당 구성 요소를 시작하고 Intent 객체를 전달합니다. 여러 구성 요소가 인텐트와 매칭되는 경우, 시스템은 사용자에게 선택기 대화상자를 제시하여 작업을 처리할 선호하는 앱을 선택할 수 있게 합니다.

<aside>
<img src="notion://custom_emoji/6cc65820-7843-4ec7-86ce-56f4cc79a1f9/139ad32d-fc35-80ec-881d-007ae6418e85" alt="notion://custom_emoji/6cc65820-7843-4ec7-86ce-56f4cc79a1f9/139ad32d-fc35-80ec-881d-007ae6418e85" width="40px" />

Intent를 통해서 데이터를 전달할 때, 실제로 Binder라는 IPC 메커니즘을 사용한다고 한다. 컴포넌트 간에 인텐트 전달을 위해 시스템을 거칠 경우 전달되는 argument나 값들은 Binder Transaction Buffer를 거치게 되는데, 해당 버퍼의 크기는 1MB로 제한되어있다. 문제는 해당 버퍼가 현재 수행중인 모든 트랜잭션을 공유해 사용하기 때문에 대용량 데이터는 전달하기 힘들다. 제한을 초과할 경우 TransactionTooLargeException을 뱉어내면서 ANR이 발생할 수도 있다.

</aside>

<aside>
<img src="notion://custom_emoji/6cc65820-7843-4ec7-86ce-56f4cc79a1f9/139ad32d-fc35-80ec-881d-007ae6418e85" alt="notion://custom_emoji/6cc65820-7843-4ec7-86ce-56f4cc79a1f9/139ad32d-fc35-80ec-881d-007ae6418e85" width="40px" />

안드로이드 시스템에서 컴포넌트 간 통신을 위한 메시징 객체

</aside>

## Q) 2. 펜딩 인텐트(Pending Intent)의 목적은 무엇인가?

펜딩 인텐트(PendingIntent)는 다른 애플리케이션이나 시스템 구성 요소가 나중에 애플리케이션을 대신하여 미리 정의된 인텐트를 실행할 수 있는 능력을 부여하는 특별한 종류의 인텐트입니다. 이는 알림이나 서비스와의 상호작용과 같이 앱의 생명주기 밖에서 트리거되어야 하는 작업에 특히 유용합니다.

### 펜딩 인텐트의 주요 특징

펜딩 인텐트는 일반 인텐트의 래퍼 역할을 하여 앱의 생명주기를 넘어서 지속될 수 있도록 합니다. 앱과 동일한 권한으로 다른 앱이나 시스템 서비스에 인텐트 실행을 위임합니다. 펜딩 인텐트는 액티비티, 서비스, 브로드캐스트 리시버에 대해 생성할 수 있습니다.

펜딩 인텐트는 세 가지 주요 형태로 사용할 수 있습니다:

- **Activity**: 액티비티를 실행합니다.
- **Service**: 서비스를 시작합니다.
- **Broadcast**: 브로드캐스트를 전송합니다.

`PendingIntent.getActivity()`, `PendingIntent.getService()`, `PendingIntent.getBroadcast()`와 같은 팩토리 메서드를 사용하여 펜딩 인텐트를 생성할 수 있습니다.

### 알림을 위한 펜딩 인텐트 생성 예시

```kotlin
val intent = Intent(this, MyActivity::class.java)
val pendingIntent = PendingIntent.getActivity(
    this,
    0,
    intent,
    PendingIntent.FLAG_UPDATE_CURRENT
)
val notification = NotificationCompat.Builder(this, CHANNEL_ID)
    .setContentTitle("제목")
    .setContentText("내용")
    .setSmallIcon(R.drawable.ic_notification)
    .setContentIntent(pendingIntent)// 알림을 탭할 때 트리거
    .build()
NotificationManagerCompat.from(this).notify(NOTIFICATION_ID, notification)
```

### 펜딩 인텐트 플래그

펜딩 인텐트는 동작과 시스템 또는 다른 구성 요소와의 상호작용을 제어하는 다양한 플래그를 지원합니다:

- **FLAG_UPDATE_CURRENT**: 기존 펜딩 인텐트를 새 데이터로 업데이트합니다.
- **FLAG_CANCEL_CURRENT**: 새 펜딩 인텐트를 생성하기 전에 기존 펜딩 인텐트를 취소합니다.
- **FLAG_IMMUTABLE**: 펜딩 인텐트를 불변으로 만들어 수신자가 수정할 수 없게 합니다.
- **FLAG_ONE_SHOT**: 펜딩 인텐트가 한 번만 사용될 수 있도록 보장합니다.

### 사용 사례

1. **알림**: 사용자가 알림을 탭할 때 액티비티 열기와 같은 작업을 허용합니다.
2. **알람**: AlarmManager를 사용하여 작업을 예약합니다.
3. **서비스**: 백그라운드 작업을 위해 ForegroundService나 BroadcastReceiver에 작업을 위임합니다.

### 보안 고려사항

악성 앱이 기본 인텐트를 수정하는 것을 방지하기 위해 펜딩 인텐트에 항상 FLAG_IMMUTABLE을 설정하세요. 이는 특히 안드로이드 12(API 레벨 31) 이후부터 특정 시나리오에서 FLAG_IMMUTABLE이 필수인 경우에 매우 중요합니다.

### 요약

펜딩 인텐트는 앱이 활발히 실행되지 않을 때에도 앱과 시스템 구성 요소 또는 다른 앱 간의 원활한 통신을 가능하게 하는 안드로이드의 핵심 메커니즘입니다. 플래그와 권한을 신중하게 관리함으로써 지연된 작업의 안전하고 효율적인 실행을 보장할 수 있습니다.

## 실습 질문

**Q) 펜딩 인텐트란 무엇이며, 일반 인텐트와 어떻게 다른가요? 펜딩 인텐트 사용이 필요한 시나리오를 제공할 수 있나요?**

# Serializable과 Parcelable의 차이점

안드로이드에서 Serializable과 Parcelable은 모두 서로 다른 컴포넌트(액티비티나 프래그먼트 등) 간에 데이터를 전달하는 데 사용되는 메커니즘이지만, 성능과 구현 방식에서 차이가 있습니다.

## Serializable

### 특징

- **Java 표준 인터페이스**: Java의 표준 인터페이스로, 객체를 바이트 스트림으로 변환하여 액티비티 간에 전달하거나 디스크에 저장할 수 있습니다.
- **리플렉션 기반**: Java 리플렉션을 사용하여 런타임에 클래스와 필드를 동적으로 검사하여 객체를 직렬화합니다.
- **성능**: 리플렉션이 느린 프로세스이고 직렬화 과정에서 많은 임시 객체를 생성하여 메모리 오버헤드가 증가하므로 Parcelable보다 느립니다.
- **사용 사례**: 성능이 중요하지 않은 상황이나 안드로이드 특화되지 않은 코드베이스를 다룰 때 유용합니다.

## Parcelable

### 특징

- **안드로이드 전용 인터페이스**: 안드로이드 컴포넌트 내에서 고성능 프로세스 간 통신(IPC)을 위해 특별히 설계된 안드로이드 전용 인터페이스입니다.
- **성능**: 안드로이드에 최적화되어 있고 리플렉션에 의존하지 않기 때문에 Serializable보다 빠릅니다. 많은 임시 객체 생성을 피해 가비지 컬렉션을 최소화합니다.
- **사용 사례**: 성능이 중요한 안드로이드에서 데이터를 전달할 때, 특히 IPC나 액티비티/서비스 간 데이터 전달 시 선호됩니다.

## 현대적인 접근법: kotlin-parcelize

현대 안드로이드 개발에서는 `kotlin-parcelize` 플러그인이 Parcelable 객체 생성 과정을 단순화합니다. 이 플러그인은 구현을 자동으로 생성하여 이전의 수동 메커니즘보다 효율적입니다.

### 사용 예시

```kotlin

kotlin
import kotlinx.parcelize.Parcelize
import android.os.Parcelable

@Parcelize
class User(val firstName: String, val lastName: String, val age: Int) : Parcelable

```

이 설정으로 `writeToParcel` 메서드를 오버라이드하거나 `CREATOR`를 구현할 필요가 없어져 보일러플레이트 코드가 크게 줄어들고 가독성이 향상됩니다.

## 주요 차이점 비교

```
구분SerializableParcelable타입Java 표준 인터페이스안드로이드 전용 인터페이스성능느림 (리플렉션 사용)빠름 (안드로이드 최적화)가비지 생성더 많은 가비지 생성적은 가비지 생성 (효율적)사용 사례일반 Java 사용에 적합안드로이드, 특히 IPC에 선호
```

## 권장사항

일반적으로 안드로이드 애플리케이션에서는 대부분의 사용 사례에서 더 나은 성능을 제공하는 **Parcelable이 권장**됩니다.

### 언제 Serializable을 사용할까?

- 간단한 경우나 성능이 중요하지 않은 작업
- 안드로이드 특화되지 않은 코드를 다룰 때

### 언제 Parcelable을 사용할까?

- 성능이 중요한 안드로이드 특화 컴포넌트에서 작업할 때
- 안드로이드의 IPC 메커니즘에서 훨씬 더 효율적이므로

## Parcel과 Parcelable 심화 이해

### Parcel이란?

- 안드로이드에서 애플리케이션의 다양한 컴포넌트(액티비티, 서비스, 브로드캐스트 리시버) 간 고성능 프로세스 간 통신(IPC)을 가능하게 하는 컨테이너 클래스
- 데이터를 마샬링(평면화)하고 언마샬링(역평면화)하여 안드로이드의 IPC 경계를 넘나들 수 있게 함
- 평면화된 데이터와 살아있는 IBinder 객체에 대한 참조를 모두 전송할 수 있는 컨테이너
- 고성능 IPC 전송을 위해 설계되어 객체가 효율적으로 직렬화되고 컴포넌트 간에 전달될 수 있게 함

### 주의사항

- Parcel은 범용 직렬화 도구가 아니며 지속적인 저장소에 사용해서는 안 됩니다
- 기본 구현이 변경될 수 있어 이전 데이터를 읽을 수 없게 만들 수 있습니다

### Parcelable 인터페이스

- 객체가 Parcel을 통해 전달될 수 있도록 직렬화하는 데 사용되는 안드로이드 전용 인터페이스
- Parcelable을 구현하는 객체는 Parcel에 쓰여지고 복원될 수 있어 안드로이드 컴포넌트 간 복잡한 데이터 전달에 적합

## 요약

- **Parcel**: IPC를 사용하여 컴포넌트 간 데이터 전송을 위한 컨테이너로, 다양한 데이터 타입을 지원
- **Parcelable**: 객체가 효율적인 전송을 위해 Parcel로 평면화될 수 있게 하는 인터페이스

Parcel의 실제 구현과 작동 메커니즘을 더 깊이 이해하려면 AOSP의 Parcel.java 소스 코드를 탐색해볼 수 있습니다.

# Context와 Context의 타입들

## Context란?

Context는 애플리케이션의 환경이나 상태를 나타내며, 애플리케이션 특화 리소스와 클래스에 접근할 수 있도록 합니다. 앱과 안드로이드 시스템 간의 다리 역할을 하여 컴포넌트들이 리소스, 데이터베이스, 시스템 서비스 등에 접근할 수 있게 합니다. Context는 액티비티 실행, 에셋 접근, 레이아웃 인플레이션 등의 작업에 필수적입니다.

## Context의 타입들

### 1. Application Context

**Application Context**는 애플리케이션의 생명주기와 연결되어 있습니다. 현재 Activity나 Fragment와 독립적인 전역적이고 오래 지속되는 컨텍스트가 필요할 때 사용됩니다. `getApplicationContext()`를 호출하여 얻을 수 있습니다.

### Application Context 사용 사례:

- 공유 환경설정이나 데이터베이스 같은 애플리케이션 전역 리소스 접근
- 전체 앱 생명주기에 걸쳐 유지되어야 하는 브로드캐스트 리시버 등록
- 앱 생명주기 전반에 걸쳐 존재하는 라이브러리나 컴포넌트 초기화

### 2. Activity Context

**Activity Context** (`Activity`의 `this` 인스턴스)는 Activity의 생명주기와 연결되어 있습니다. 리소스 접근, 다른 액티비티 실행, Activity에 특화된 레이아웃 인플레이션에 사용됩니다.

### Activity Context 사용 사례:

- UI 컴포넌트 생성 또는 수정
- 다른 액티비티 시작
- 현재 액티비티에 범위가 지정된 리소스나 테마 접근

### 3. Service Context

**Service Context**는 Service의 생명주기와 연결되어 있습니다. 네트워크 작업이나 음악 재생과 같은 백그라운드에서 실행되는 작업에 주로 사용됩니다. Service에서 필요한 시스템 레벨 서비스에 접근할 수 있게 합니다.

### 4. Broadcast Context

**Broadcast Context**는 BroadcastReceiver가 호출될 때 제공됩니다. 짧은 생명주기를 가지며 일반적으로 특정 브로드캐스트에 응답하는 데 사용됩니다. 이 컨텍스트로 장시간 실행되는 작업을 수행하는 것은 피해야 합니다.

## Context의 일반적인 사용 사례

### 1. 리소스 접근

Context는 `getString()`이나 `getDrawable()` 같은 메서드를 사용하여 문자열, 드로어블, 치수 같은 리소스에 접근할 수 있게 합니다.

### 2. 레이아웃 인플레이션

LayoutInflater를 사용하여 XML 레이아웃을 뷰로 인플레이션할 때 컨텍스트를 사용합니다.

### 3. 액티비티와 서비스 시작

액티비티를 시작(`startActivity()`)하고 서비스를 시작(`startService()`)하는 데 Context가 필요합니다.

### 4. 시스템 서비스 접근

Context는 `getSystemService()`를 통해 ClipboardManager나 ConnectivityManager 같은 시스템 레벨 서비스에 접근할 수 있게 합니다.

### 5. 데이터베이스 및 SharedPreferences 접근

SQLite 데이터베이스나 공유 환경설정 같은 영구 저장 메커니즘에 접근하는 데 컨텍스트를 사용합니다.

## 요약

Context는 안드로이드에서 앱과 시스템 리소스 간의 상호작용을 가능하게 하는 핵심 컴포넌트입니다. Application Context, Activity Context, Service Context 등 서로 다른 목적을 가진 여러 타입의 컨텍스트가 존재합니다. 적절한 컨텍스트 사용은 효율적인 리소스 관리를 보장하고 메모리 누수나 크래시를 방지하므로, 올바른 컨텍스트를 선택하고 불필요하게 보유하지 않는 것이 중요합니다.

## 실용적인 질문

**Q) 안드로이드 애플리케이션에서 올바른 타입의 Context를 사용하는 것이 왜 중요하며, Activity Context에 대한 오래 지속되는 참조를 보유하는 것의 잠재적 위험은 무엇인가?**

## Context 사용 시 주의사항

Context는 안드로이드에서 편리한 메커니즘이지만, 부적절한 사용은 메모리 누수, 크래시, 비효율적인 리소스 처리와 같은 심각한 문제를 야기할 수 있습니다. 이러한 함정을 피하려면 컨텍스트를 효과적으로 사용하기 위한 미묘한 차이점과 모범 사례를 이해하는 것이 필수적입니다.

### 주요 주의사항:

### 1. 메모리 누수 방지

- **Activity Context를 오래 보유하지 마세요**: Activity Context를 정적 변수나 싱글톤에 저장하면 Activity가 파괴된 후에도 메모리에 남아 메모리 누수가 발생합니다.
- **Application Context 사용**: 오래 지속되는 작업이나 싱글톤 객체에는 Application Context를 사용하세요.

### 2. 적절한 Context 선택

- **UI 작업**: Activity Context 사용
- **애플리케이션 전역 작업**: Application Context 사용
- **서비스 작업**: Service Context 사용

### 3. WeakReference 사용

오래된 참조가 필요한 경우 WeakReference를 사용하여 메모리 누수를 방지하세요.

```kotlin

kotlin
class MyManager {
    private var contextRef: WeakReference<Context>? = null

    fun setContext(context: Context) {
        contextRef = WeakReference(context)
    }

    fun doSomething() {
        contextRef?.get()?.let { context ->
// context 사용
        }
    }
}

```

### 4. Context 타입별 제한사항 이해

- **Application Context**는 Activity의 테마나 스타일에 접근할 수 없습니다
- **Broadcast Context**는 `bindService()`를 호출할 수 없습니다
- **Service Context**는 UI 관련 작업에 적합하지 않습니다

올바른 Context 사용은 안드로이드 앱의 안정성과 성능에 매우 중요합니다.

## Context 사용 시 흔한 실수와 해결책

### 1. Context 참조를 생명주기 이상으로 보유하기

가장 흔한 문제 중 하나는 Context, 특히 Activity나 Fragment context에 대한 참조를 생명주기를 넘어서 보유하는 것입니다. 이는 가비지 컬렉터가 컨텍스트나 관련 리소스의 메모리를 회수할 수 없게 하여 메모리 누수를 야기합니다.

### 잘못된 예시:

```kotlin

kotlin
object Singleton {
    var context: Context? = null// 컨텍스트를 보유하여 메모리 누수 발생
}

```

### 올바른 해결책:

```kotlin

kotlin
object Singleton {
    lateinit var applicationContext: Context
}

```

오래 지속되는 객체에 컨텍스트가 필요한 경우 Application context를 사용하세요.

### 2. 잘못된 Context 타입 사용

다른 컨텍스트 타입들은 서로 다른 목적을 가지고 있습니다. 잘못된 타입을 사용하면 예상치 못한 동작이 발생할 수 있습니다:

- **Activity Context**: 레이아웃 인플레이션이나 다이얼로그 표시 같은 UI 관련 작업에 사용
- **Application Context**: 라이브러리 초기화 같은 UI 생명주기와 독립적인 작업에 사용

### 잘못된 예시:

```kotlin

kotlin
val dialog = AlertDialog.Builder(context.applicationContext)// 잘못됨

```

### 올바른 해결책:

```kotlin

kotlin
val dialog = AlertDialog.Builder(activityContext)// 올바름

```

적절한 테마 적용을 위해 Activity Context를 사용하세요.

### 3. 파괴된 컴포넌트의 Context 사용

연관된 컴포넌트(Activity나 Fragment)가 파괴된 후에 Context를 사용하는 것을 피해야 합니다. 파괴된 컴포넌트와 연결된 Context에 접근하면 해당 컨텍스트와 연결된 리소스가 더 이상 존재하지 않을 수 있어 크래시나 정의되지 않은 동작을 야기할 수 있습니다.

### 잘못된 예시:

```kotlin

kotlin
val button = Button(activity)
activity.finish()// 액티비티가 파괴되었지만 버튼이 여전히 참조를 보유

```

### 4. 백그라운드 스레드에서 Context 사용 피하기

Context는 특히 리소스 접근이나 UI와의 상호작용을 위해 메인 스레드용으로 설계되었습니다. 백그라운드 스레드에서 사용하면 예상치 못한 크래시나 스레딩 문제가 발생할 수 있습니다.

### 올바른 해결책:

```kotlin

kotlin
viewModelScope.launch {
    val data = fetchData()
    withContext(Dispatchers.Main) {
        Toast.makeText(context, "Data fetched", Toast.LENGTH_SHORT).show()
    }
}

```

UI 관련 컨텍스트 리소스와 상호작용하기 전에 메인 스레드로 전환하세요.

## ContextWrapper란?

**ContextWrapper**는 안드로이드에서 Context 객체를 래핑하여 래핑된 Context에 호출을 위임할 수 있는 기능을 제공하는 기본 클래스입니다. 원본 Context의 동작을 수정하거나 확장하기 위한 중간 계층 역할을 합니다. ContextWrapper를 사용하면 기본 Context를 변경하지 않고도 기능을 사용자 정의할 수 있습니다.

### ContextWrapper의 목적

ContextWrapper는 기존 Context의 특정 동작을 향상시키거나 오버라이드해야 할 때 사용됩니다. 원본 Context에 대한 호출을 가로채서 추가 기능이나 사용자 정의된 동작을 제공할 수 있게 합니다.

### 사용 사례

1. **사용자 정의 Context**: 다른 테마 적용이나 특수한 방식으로 리소스 처리 같은 특정 목적을 위한 사용자 정의 Context를 생성할 때
2. **동적 리소스 처리**: 문자열, 치수, 스타일 같은 리소스를 동적으로 제공하거나 수정하기 위해 Context를 래핑할 때
3. **의존성 주입**: Dagger나 Hilt 같은 라이브러리가 의존성 주입을 위해 컴포넌트에 사용자 정의 컨텍스트를 연결하기 위해 ContextWrapper를 생성할 때

### ContextWrapper 예시

사용자 정의 테마를 적용하기 위해 ContextWrapper를 사용하는 방법을 보여주는 코드입니다:

```kotlin

kotlin
class CustomThemeContextWrapper(base: Context) : ContextWrapper(base) {
    override fun getTheme(): Resources.Theme {
        val theme = super.getTheme()
        theme.applyStyle(R.style.CustomTheme, true)// 사용자 정의 테마 적용
        return theme
    }
}

```

Activity에서 이 사용자 정의 래퍼를 사용할 수 있습니다:

```kotlin

kotlin
class MyActivity : AppCompatActivity() {
    override fun attachBaseContext(newBase: Context) {
        super.attachBaseContext(CustomThemeContextWrapper(newBase))
    }
}

```

이 예시에서 `CustomThemeContextWrapper`는 기본 컨텍스트의 기본 동작을 오버라이드하여 Activity에 특정 테마를 적용합니다.

## Activity에서 `this`와 `baseContext`의 차이점

Activity에서 `this`와 `baseContext` 모두 Context에 대한 접근을 제공하지만, 서로 다른 목적을 가지며 안드로이드 컨텍스트 계층구조의 서로 다른 레벨을 나타냅니다. 언제 어떤 것을 사용할지 아는 것은 코드에서 혼란이나 잠재적인 문제를 피하는 데 중요합니다.

### Activity에서 `this`

Activity에서 `this` 키워드는 Activity 클래스의 현재 인스턴스를 참조합니다. Activity는 ContextWrapper의 하위 클래스(따라서 간접적으로 Context의 하위 클래스)이므로, `this`는 생명주기 관리와 UI와의 상호작용 같은 추가 기능을 포함하여 액티비티의 특정 컨텍스트에 접근할 수 있게 합니다.

Activity에서 `this`를 사용하면 종종 액티비티의 현재 컨텍스트를 가리키며, 해당 Activity에 특정한 메서드를 호출할 수 있게 합니다.

### 예시:

```kotlin

kotlin
val intent = Intent(this, AnotherActivity::class.java)
startActivity(intent)

val dialog = AlertDialog.Builder(this)
    .setTitle("Example")
    .setMessage("This dialog is tied to this Activity instance.")
    .show()

```

### Activity에서 `baseContext`

`baseContext`는 Activity가 기반으로 하는 기초적이거나 "기본" 컨텍스트를 나타냅니다. Activity가 확장하는 ContextWrapper 클래스의 일부입니다. `baseContext`는 일반적으로 Context 메서드의 핵심 구현을 제공하는 ContextImpl 인스턴스입니다.

`baseContext`는 일반적으로 `getBaseContext()` 메서드를 통해 접근됩니다. 직접 사용하는 경우는 드물지만, 사용자 정의 ContextWrapper 구현을 작업할 때나 래핑된 것 뒤의 원본 컨텍스트를 참조해야 할 때 관련성이 있습니다.

### 예시:

```kotlin

kotlin
val systemService = baseContext.getSystemService(Context.LAYOUT_INFLATER_SERVICE)

```

### `this`와 `baseContext`의 주요 차이점

1. **범위**: `this`는 현재 Activity 인스턴스와 그 생명주기를 나타내는 반면, `baseContext`는 Activity가 기반으로 하는 하위 레벨 컨텍스트를 참조합니다.
2. **사용법**: `this`는 일반적으로 액티비티 시작이나 다이얼로그 표시 같은 액티비티의 생명주기나 UI와 연결된 작업에 사용됩니다. `baseContext`는 일반적으로 Context의 핵심 구현과 상호작용할 때, 특히 사용자 정의 ContextWrapper 시나리오에서 사용됩니다.
3. **계층구조**: `baseContext`는 Activity의 기초 컨텍스트입니다. `baseContext`에 접근하면 Activity가 ContextWrapper로서 제공하는 추가 기능을 우회합니다.

### 예시: 사용자 정의 Context 래핑

사용자 정의 ContextWrapper를 작업할 때 `this`와 `baseContext`의 구별이 중요해집니다. `this` 키워드는 여전히 Activity 자체를 참조하는 반면, `baseContext`는 원본의 수정되지 않은 컨텍스트에 접근할 수 있게 합니다.

```kotlin

kotlin
class CustomContextWrapper(base: Context) : ContextWrapper(base) {
    override fun getSystemService(name: String): Any? {
// 예시: LayoutInflater 수정
        if (name == Context.LAYOUT_INFLATER_SERVICE) {
            val inflater = super.getSystemService(name) as LayoutInflater
            return inflater.cloneInContext(this)
        }
        return super.getSystemService(name)
    }
}

override fun attachBaseContext(newBase: Context) {
    super.attachBaseContext(CustomContextWrapper(newBase))
}

```

## 최종 요약

### Context 사용의 핵심 원칙

Context를 효과적으로 사용하려면 일반적인 함정을 피하기 위한 신중한 고려가 필요합니다. Activity나 Fragment 컨텍스트를 생명주기를 넘어서 보유하는 것은 메모리 누수로 이어질 수 있으므로 피해야 합니다. 항상 특정 작업에 맞는 올바른 Context 타입을 선택하고, 백그라운드 스레드에서 사용하거나 연관된 컴포넌트가 파괴된 후에 사용하는 것을 피하세요.

### ContextWrapper의 활용

ContextWrapper는 원본 Context를 직접 수정하지 않고도 호출을 가로채고 수정할 수 있게 하여 안드로이드에서 Context 동작을 사용자 정의하는 유연하고 재사용 가능한 도구입니다. 이는 모듈화되고 적응 가능한 애플리케이션을 구축하는 데 필수적인 클래스입니다.

### `this` vs `baseContext`

Activity에서 `this`는 현재 액티비티 인스턴스를 참조하며 생명주기와 UI 특화 기능을 가진 상위 레벨 컨텍스트를 제공합니다. 반면 `baseContext`는 액티비티가 기반으로 하는 기초 컨텍스트를 나타내며, 주로 사용자 정의 ContextWrapper 구현 같은 고급 시나리오에서 사용됩니다. `this`가 안드로이드 개발에서 가장 일반적으로 사용되지만, `baseContext`를 이해하면 디버깅과 모듈화된 재사용 가능한 컴포넌트 생성에 도움이 됩니다.

Context의 적절한 관리는 효율적인 리소스 사용을 보장하고 메모리 누수나 애플리케이션 크래시를 방지하는 데 도움이 됩니다.

# Application 클래스란 무엇인가?

Android의 Application 클래스는 전역 애플리케이션 상태와 생명주기를 유지하기 위한 기본 클래스 역할을 합니다. 이는 앱의 진입점 역할을 하며, 액티비티, 서비스, 브로드캐스트 리시버와 같은 다른 컴포넌트들보다 먼저 초기화됩니다. Application 클래스는 앱의 생명주기 전반에 걸쳐 사용할 수 있는 컨텍스트를 제공하여 공유 리소스를 초기화하는 데 이상적입니다.

## Application 클래스의 목적

Application 클래스는 전역 상태를 보유하고 애플리케이션 전반의 초기화를 수행하도록 설계되었습니다. 개발자들은 종종 이 클래스를 오버라이드하여 의존성을 설정하고, 라이브러리를 구성하며, 액티비티와 서비스 전반에 걸쳐 지속되어야 하는 리소스를 관리합니다. 기본적으로 모든 Android 애플리케이션은 AndroidManifest.xml 파일에 사용자 정의 클래스가 지정되지 않는 한 Application 클래스의 기본 구현을 사용합니다.

## Application 클래스의 주요 메서드

### 1. onCreate()

onCreate() 메서드는 앱 프로세스가 생성될 때 호출됩니다. 여기서 일반적으로 데이터베이스 인스턴스, 네트워크 라이브러리, 분석 도구와 같은 애플리케이션 전반의 의존성을 초기화합니다. 애플리케이션 생명주기 동안 한 번만 호출됩니다.

### 2. onTerminate()

이 메서드는 에뮬레이터 환경에서 애플리케이션이 종료될 때 호출됩니다. 실제 기기의 프로덕션 환경에서는 Android가 이 메서드의 호출을 보장하지 않으므로 호출되지 않습니다.

### 3. onLowMemory()와 onTrimMemory()

이러한 메서드들은 시스템이 메모리 부족 상황을 감지할 때 트리거됩니다. onLowMemory()는 이전 API 레벨에서 사용되며, onTrimMemory()는 앱의 현재 메모리 상태에 따라 더 세밀한 제어를 제공합니다.

## Application 클래스 사용 방법

사용자 정의 Application 클래스를 정의하려면 Application 클래스를 상속하고 AndroidManifest.xml 파일의 `<application>` 태그에서 이를 지정해야 합니다.

**CustomApplication.kt**

```kotlin

kotlin
class CustomApplication : Application() {

    override fun onCreate() {
        super.onCreate()
// 전역 의존성 초기화
        initializeDatabase()
        initializeAnalytics()
    }

    private fun initializeDatabase() {
// 데이터베이스 인스턴스 설정
    }

    private fun initializeAnalytics() {
// 분석 추적 구성
    }
}

```

**AndroidManifest.xml**

```xml

xml
<application
    android:name=".CustomApplication"
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher">
    ...
</application>

```

## Application 클래스의 사용 사례

### 1. 전역 리소스 관리

데이터베이스, 공유 환경설정, 네트워크 클라이언트와 같은 리소스를 한 번 설정하고 재사용할 수 있습니다.

### 2. 컴포넌트 초기화

Firebase Analytics, Timber 등과 같은 도구들을 애플리케이션 시작 시 적절히 초기화하여 앱의 생명주기 전반에 걸쳐 원활한 기능을 보장해야 합니다.

### 3. 의존성 주입

Dagger나 Hilt와 같은 프레임워크를 초기화하여 앱 전반에 걸쳐 의존성을 제공할 수 있습니다.

## 모범 사례

1. 앱 실행 지연을 방지하기 위해 onCreate()에서 장시간 실행되는 작업을 수행하지 마세요.
2. Application 클래스를 관련 없는 로직의 저장소로 사용하지 마세요. 전역 초기화와 리소스 관리에 집중하세요.
3. Application 클래스에서 공유 리소스를 관리할 때 스레드 안전성을 보장하세요.

## 요약

Application 클래스는 전체 앱에서 사용할 수 있어야 하는 리소스를 초기화하고 관리하는 중심 장소입니다. 전역 구성을 위한 중요하고 기본적인 API이지만, 명확성을 유지하고 불필요한 복잡성을 피하기 위해 진정으로 전역적인 작업으로 사용을 제한해야 합니다.

## 실용적인 질문

**Q) Application 클래스의 목적은 무엇이며, 생명주기와 리소스 관리 측면에서 Activity와 어떻게 다른가요?**

Application 클래스는 애플리케이션 전체의 생명주기 동안 지속되며 앱이 실행되는 동안 한 번만 생성됩니다. 반면 Activity는 사용자 상호작용에 따라 여러 번 생성되고 소멸될 수 있습니다. 리소스 관리 측면에서 Application 클래스는 전역 리소스와 앱 전반에 걸친 설정을 관리하는 반면, Activity는 특정 화면이나 사용자 인터페이스와 관련된 리소스를 관리합니다.
