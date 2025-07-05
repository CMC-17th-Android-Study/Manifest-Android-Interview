https://lead-whistle-2dd.notion.site/Category-0-The-Android-Framework-2106d503b266809d8233dd71d5dbdfe7?source=copy_link

## Q) 0. What is Android?
- 안드로이드는 계층적 구조를 통해 앱 실행과 하드웨어 제어를 효율적으로 처리함.
- 앱은 안드로이드 프레임워크의 API를 통해 기능을 요청하고, 이 요청은 내부적으로 Native Library나 HAL을 거쳐 Linux 커널로 전달되어 실제 하드웨어를 제어함.
- 앱의 바이트 코드는 ART에서 JIT 또는 AOT 방식으로 실행되며, 이 과정에서 메모리 관리나 보안 등은 kernal이 담당
- 이처럼 각 계층이 역할을 분담하며 유기적으로 동작해 안정적이고 확장 가능한 플랫폼을 제공함.

## Q) 1. What is Intent?
1. **Intent는 안드로이드 컴포넌트 간 메시지 객체로**, Activity 전환, Service 실행, Broadcast 송신 등에 사용되며, 데이터 전달도 가능함.
2. **명시적 Intent**는 대상 컴포넌트를 직접 지정하며, **같은 앱 내 Activity 이동이나 Service 실행 시** 사용함.
3. **암시적 Intent**는 수행할 작업만 명시하고, **시스템이 처리 가능한 앱을 찾아 실행**하며, 외부 앱과의 연동에 적합함 (예: 웹 열기, 공유).
4. 암시적 인텐트 처리 시, 시스템은 **`intent-filter`와 인텐트의 action/data/category를 비교하여** 적절한 앱을 결정함.
5. 처리 가능한 앱이 없으면 **ActivityNotFoundException이 발생**하므로, 사전 확인을 위해 `resolveActivity()` 사용 권장.
6. **Intent Filter는 AndroidManifest.xml에 정의**되며, 앱이 어떤 인텐트를 처리할 수 있는지를 시스템에 알리는 역할을 함.

## Q) 2. What is the purpose of Pending Intent?
1. PendingIntent는 일반 Intent를 포장해 시스템이나 외부 앱이 나중에 대신 실행할 수 있도록 권할을 위임하는 객체입니다.
2. 이는 알림 클릭, 알람 예약, 백그라운드 작업 트리거 등 앱 외부에서 동작을 실행해야 할 때 필수적입니다.
3. Intent는 즉시 실행되지만, `PendingIntent`는 앱 종료 후에도 유지되어 지연된 실행이 가능하며, `FLAG_IMMUTABLE`과 같은 플래그로 보안을 강화할 수 있습니다.
4. 주요 생성 메서드에는 `getActivity()` , `getService()` , `getBroadcast()`

## Q) 3. What are the differences between Serializable and Parcelable
| 구분 | Serializable | Parcelable |
| --- | --- | --- |
| 종류 | 자바 표준 | Android 전용 |
| 직렬화 방식 | 리플렉션 기반 | 수동 구현(고성능) |
| 속도 | 느림 | 빠름 |
| 메모리 사용 | 높음 | 낮음 |
| 사용 시점 | 성능이 중요하지 않을 때 | Android 내 빠른 데이터 전달 필요 시 |
- 안드로이드에서는 성능 때문에 Parcelable을 주로 사용
- @Parcelable 플러그인으로 구현을 자동화 가능
- Parcel은 Android IPC를 위한 고성능 데이터 컨테이너로 객체를 Marshaling → 전송 → Unmarshaling 함.(파일 저장에는 사용X. 내부 구현 변경 가능성)
- kotlinx.serialization과 Parcelable의 차이는 용도 차이 ⇒ kotlinx는 외부 전송

## Q) 4. What is Context and what types of Context exist?
- Context는 안드로이드 앱에서 시스템 리소스, 서비스, 컴포넌트에 접근하기 위한 핵심 객체입니다.
- Applicatoin, Activity, Service, BroadcastReceiver 등 각 컴포넌트는 고유한 Context를 가집니다.
- ApplicatoinContext는 전역 작업에, ActivityContext는 UI 작업에 사용되며 잘못된 사용은 메모리 누수를 유발합니다.
- ContextWrapper는 기존 Context의 기능을 확장하거나 수정할 때 사용합니다.
- this는 현재 Activity의 Context, baseContext는 래핑되지 않은 원본 Context
- UI작업은 메인 스레드에서, 종료된 컴포넌트의 COntext는 절대 사용하지 말아야 함.
- 올바른 Context 선택과 관리가 앱의 안정성과 성능을 좌우함

## Q) 5. What is Application class?
- 앱 전체에서 공통적으로 필요한 리소스나 설정을 초기화하고 관리하는 전역 기반 클래스.
    - Database, Log 시스템, network 등
- 앱의 시작 시점에 단 한 번 초기화되어 생명주기 전체동안 유지됨.
- Activity와 달리 앱이 시작될 떄 생성되어 종료될 때까지 유지되며, scope가 앱 전역임.
- 공통 리소스를 초기화하거나 전역 설정할 때 사용함.

## Q) 6. What is the purpose of the AndroidManifest file?
- 앱의 구성 요소, 권한, 요구사항 등을 OS에 알려주는 핵심 설정 파일
- Activity, Service, BroadcastReceiver 등은 반드시 이 파일에 등록해야 사용 가능
- 권한 선언, 기능 요구사항, 인텐트 필터, 테마, API 레벨 등을 정의함.

## Q) 7. Describe the Activity lifecycle
- onCreate → onStart → onResume → onPause() → onStop() → onDestory()
- 액티비티가 중단되었다가 다시 시작될 때는 onRestart()를 거쳐 onStart()를 실행.
- 즉시 반응해야 할 작업은 onPause에서, 자원 절약 목적이면 onStop()에서 처리

## Q) 8. Describe the Fragment lifecycle
- Fragment는 Activity와 별도 생명주기를 가지며, View는 그보다 짧은 생명주기를 가짐.
- UI 초기화는 onCreateView(), 리소스 해제는 onDestroyView()에서 수행
- viewLifecycleOwner는 View 생명주기에 맞춰 LiveData를 안전하게 관찰 가능
- ViewBinding 사용 시 _binding = null로 명시적 해제 필요
- fragmentManager는 Activity 수준, childFragmentManager는 Fragment 내부에서 자식 Fragment 관리용
- 생명주기 구분을 통해 메모리 누수 방지 및 UI 일관성 유지 가능

## Q) 9. What is Service?
- `Service`는 UI 없이 백그라운드 작업을 수행하는 컴포넌트로, 음악 재생, 파일 다운로드 등에 사용됨.
- `Started Service`는 `startService()`로 시작되며, 명시적으로 `stopSelf()` 또는 `stopService()` 호출 전까지 계속 실행됨.
- `Bound Service`는 `bindService()`로 컴포넌트에 바인딩되어 동작하며, 모든 클라이언트가 연결 해제 시 자동 종료됨.
- `Foreground Service`는 알림(Notification)을 필수로 표시하며, 사용자에게 인지 가능한 작업(위치 추적 등)에 적합함.
- Started Service의 생명주기: `onCreate()` → `onStartCommand()` → `onDestroy()`
- Bound Service의 생명주기: `onCreate()` → `onBind()` → (선택) `onUnbind()` → `onDestroy()`
- WorkManager는 즉시 실행이 필요 없는 작업에 적합하며, 서비스보다 자원 효율적임.
- 서비스 종료 시에는 `onDestroy()`에서 리소스를 정리해야 메모리 누수를 방지할 수 있음.

## Q) 10. What is BroadcastReceiver?
- **BroadcastReceiver**: 시스템/앱 이벤트 수신용 컴포넌트
- **종류**:
  - System Broadcast (OS가 발생)
  - Custom Broadcast (앱이 직접 발생)
- **등록 방식**:
  - 정적 등록 → Manifest (앱 꺼져도 수신)
  - 동적 등록 → 코드 (앱 실행 중만 수신, 해제 필요)
- **주의사항**:
  - Android 8.0+ 백그라운드 제한
  - 동적 등록 시 `unregisterReceiver()` 필수
  - 민감 정보 포함 시 권한 설정 필요
- **사용 예**:
  - 네트워크 변화 감지
  - 배터리 상태, 충전 상태 반응
  - 커스텀 이벤트 처리 (알람 등)

## Q) 11. What is the purpose of ContentProvider, and how does it facilitate secure data sharing between applications?
- `ContentProvider`는 구조화된 데이터를 **앱 간 안전하게 공유**하기 위한 컴포넌트입니다.
- **URI(authority, path, id)**를 통해 데이터를 조회·삽입·수정·삭제합니다.
- `ContentResolver`가 ContentProvider와 상호작용하여 데이터에 접근합니다.
- 보통 SQLite, 파일, 네트워크 등 내부 데이터 소스를 추상화합니다.
- `Application.onCreate()`보다 먼저 `onCreate()`가 호출되어 **초기화 용도로도 활용**됩니다.
- 예: Firebase는 ContentProvider로 자동 초기화를 수행합니다.
- Jetpack App Startup도 `InitializationProvider`로 컴포넌트 초기화를 처리합니다.
- 보안 제어와 데이터 은닉이 가능하여 **파일 공유나 시스템 연동에도 사용**됩니다.

## Q) 12. How to handle configuration changes
- Android는 구성 변경 시 기본적으로 Activity를 재시작하여 UI 상태가 손실될 수 있음
- `onSaveInstanceState()`와 `ViewModel`을 사용하면 UI 상태를 안전하게 유지 가능
- `ViewModel`은 구성 변경에도 살아남아 데이터 손실 없이 유지됨
- `android:configChanges`를 사용하면 특정 구성 변경을 앱이 직접 처리할 수 있음
- 이때 `onConfigurationChanged()`를 오버라이드하여 UI를 수동으로 조정함
- Jetpack Compose에서는 `rememberSaveable`로 상태를 저장
- Navigation 컴포넌트를 쓰면 back stack 유지도 자동으로 처리됨
- 구성 의존 데이터는 피하고 ViewModel 등으로 분리하는 것이 권장됨

## Q) 13. How Android handles memory management, and how do you avoid memory leaks?
- Android는**GC(Dalvik/ART)**로 사용하지 않는 객체를 자동 회수해 메모리를 관리함
- 하지만 **남아 있는 참조**로 인해 메모리 누수가 발생할 수 있으므로 주의 필요
- **ViewModel, LiveData, Flow** 등 **Lifecycle-aware 컴포넌트**로 리소스를 안전하게 관리
- **Context나 View를 static으로 참조하지 말고**, ApplicationContext를 적절히 사용
- **리스너, 콜백, BroadcastReceiver**는 생명주기에 맞게 명시적으로 해제
- `Cursor`, `Stream`, `DB` 연결 등은 **명시적으로 close() 호출** 필요
- **LeakCanary**, **Android Studio Memory Profiler** 등을 활용해 누수 탐지 및 분석
- Fragment 간 참조는 `onDestroyView()`에서 정리하여 메모리 유지 방지

## Q) 14. What are the main causes of ANR erros, and how can you prevent them from occuring?
- **ANR**은 UI 스레드가 5초 이상 차단될 때 발생하며, 사용자 상태 손실과 앱 강제 종료로 이어짐
- 원인: **무거운 계산**, **네트워크/DB 작업**, **Thread.sleep()** 등 UI 스레드에서의 차단 작업
- 해결: **Coroutine, Executor, WorkManager** 등으로 작업을 **백그라운드로 이동**
- 데이터는 **Paging**으로 나눠 불러오고, 구성 변경 시 **ViewModel**로 상태 유지
- **StrictMode**, **Logcat**, **Android Vitals**, **Profiler**로 ANR 감지 및 진단 가능
- **Thread.sleep() 대신 Handler.postDelayed()**, **UI 블로킹 호출은 피하기**
- **Profiler 도구로 CPU, 메모리, 네트워크 병목 식별**

## Q) 15. How do you handle deep links?
- 딥링크(Deep Link)는 외부 URL 등을 통해 앱의 특정 화면으로 직접 이동할 수 있게 하는 기능입니다.
- `AndroidManifest.xml`에 intent-filter로 URL 패턴을 정의하고, Activity에서 `intent.data`를 처리해야 합니다.
- URL 쿼리 파라미터를 추출해 해당 ID나 상태에 따라 화면을 전환할 수 있습니다.
- `adb shell am start -a VIEW -d "..."` 명령으로 딥링크 동작을 시뮬레이션할 수 있습니다.
- **Custom Scheme**(예: `myapp://`)도 가능하지만, 호환성 측면에선 **HTTPS URL**이 권장됩니다.
- **App Links**를 설정하면 브라우저 대신 앱이 자동으로 실행되며, 도메인 검증이 필요합니다.
- **예외 처리 및 fallback**도 구현해 링크 데이터 누락/오류 시 앱이 안정적으로 동작하도록 해야 합니다.
- 테스트는 ADB, 브라우저, Deep Link Tester 앱, App Links Assistant 도구 등을 활용해 전방위적으로 확인합니다.

## Q) 16. What are tasks and back stack?
- **Task**는 여러 Activity로 구성된 작업 단위이며, **back stack**은 LIFO 구조로 Activity를 관리합니다.
- **standard**: 항상 새 인스턴스 생성.
- **singleTop**: top에 있으면 재사용 (onNewIntent 호출).
- **singleTask**: Task 내에 단 하나만 존재, 기존 인스턴스 앞으로 bring.
- **singleInstance**: 해당 Activity는 **독립적인 Task**에만 존재.
- **Intent Flag**로 런타임에 back stack 제어 가능:
  - `NEW_TASK`: 새 Task 생성
  - `CLEAR_TOP`: 해당 Activity 위의 것들 제거
  - `NO_HISTORY`: back stack에 추가하지 않음
- launchMode와 intent flag는 Activity 수명 및 내비게이션 흐름을 정밀하게 제어하는 핵심 도구

## Q) 17. What's the purpose of Bundle?
- Bundle은 컴포넌트 간 데이터를 전달하거나 일시적인 상태를 저장/복원하기 위해 사용되는 경량의 key-value 구조 데이터 객체
- 목적
  - 소량의 데이터를 효율적으로 전달(문자열, 숫자, Parcelable 등)
  - Activity, Fragment, Service 간 통신
  - 구성 변경(screen rotation 등) 시 UI 상태 보존
  - 직렬화 가능한 데이터 저장 및 복원

## Q) 18. How do you pass data between Activites or Fragments
- Activity 간 데이터 잔달 ⇒ Intent의 putExtra() / getIntent() 사용
- Frgament 간 데이터 전달 ⇒ arguments에 Bundle을 사용해 key-value 쌍 전달
- Jetpack Navigation + Safe Args ⇒ nav_graph.xml에 인자 정의, 생성된 direction 클래스로 type-safe하게 전달 및 수신
- Shared ViewModel (같은 Activity 내 Fragment 간) ⇒ activityViewModles()로 ViewModel 공유 → 생명주기 인식 + 느신한 결합 + 상태 보존
- Fragment Result API ⇒ Fragment 간 일회성(one-time) 데이터 전달에 적합, setFragmentResult(), setFragmentResultListener() 사용

## Q) 19. What happens to an Activity during configuration changes?
- Activity는 피괴 후 재생성 됨
- 이때 시스템은 리소스를 새롭게 로드하며, 데이터 손실을 방지하기 위해 상태 저장이 필요
- 일시적 상태 ⇒ onSaveInstanceState() 사용, 입력값과 스크롤 위치 등을 사용
- 지속적 상태 ⇒ ViewModel 또는 SavedStateHandle 사용, 로그인 상태 및 API 응답 결과 등

## Q) 20. What is Activity Manager?
- 시스템에서 실행 중인 **Activity, Task, Process, 메모리 상태**를 관리하는 서비스
- 대표 메서드: `getMemoryInfo()`, `getRunningAppProcesses()`, `killBackgroundProcesses()`, `clearApplicationUserData()`
- **메모리 누수 감지 도구**로, `ActivityManager`를 활용해 **메모리 상태 추적**함
- 시스템이 **저메모리 상태**(`lowMemory == true`)일 때 대비 가능
- 사용처: 캐시 제거, 백그라운드 작업 정리, UI 간소화 등 리소스 해제
- `onTrimMemory()`와 함께 사용하면 더욱 효과적