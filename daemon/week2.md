## Q6. AndroidManifest 파일의 목적은 무엇인가요?

AndroidManifest.xml 파일은 Android 프로젝트에서 중요한 구성 파일로, 애플리케이션에 대한 필수 정보를 Android 운영체제에 정의합니다. 이는 애플리케이션과 OS 간의 다리 역할을 하며, 앱의 구성 요소, 권한, 하드웨어 및 소프트웨어 기능 등에 대해 OS에 알려줍니다.

### AndroidManifest.xml의 주요 기능

1. **애플리케이션 구성 요소 선언**: Activity, Service, Broadcast Receiver, Content Provider와 같은 필수 구성 요소를 등록하여 Android 시스템이 이들을 시작하거나 상호작용하는 방법을 알 수 있게 합니다.
2. **권한**: 앱이 필요로 하는 권한(INTERNET, ACCESS_FINE_LOCATION, READ_CONTACTS 등)을 선언하여 사용자가 앱이 접근할 리소스를 알고 이러한 권한을 허용하거나 거부할 수 있게 합니다.
3. **하드웨어 및 소프트웨어 요구사항**: 앱이 의존하는 기능(카메라, GPS, 특정 화면 크기 등)을 명시하여 Play Store가 이러한 요구사항을 충족하지 않는 기기를 필터링할 수 있게 도와줍니다.
4. **앱 메타데이터**: 앱의 패키지 이름, 버전, 최소 및 타겟 API 레벨, 테마, 스타일과 같은 필수 정보를 제공하여 시스템이 앱 설치 및 실행에 사용할 수 있게 합니다.
5. **Intent Filter**: 구성 요소(예: 액티비티)에 대한 Intent Filter를 정의하여 링크 열기나 콘텐츠 공유와 같이 어떤 종류의 인텐트에 응답할 수 있는지 명시하여 다른 앱이 여러분의 앱과 상호작용할 수 있게 합니다.
6. **앱 구성 및 설정**: 메인 런처 액티비티 설정, 백업 동작 구성, 테마 지정 등의 구성을 포함하여 앱의 동작과 표시 방식을 제어하는 데 도움을 줍니다.

### 요약

AndroidManifest.xml 파일은 모든 Android 앱의 기본이 되며, Android OS가 앱의 생명주기, 권한, 상호작용을 관리하는 데 필요한 세부 정보를 제공합니다. 이는 본질적으로 앱의 구조와 요구사항을 정의하는 청사진 역할을 합니다. GitHub의 Pokedex 프로젝트에서 AndroidManifest.xml의 실제 구현을 살펴볼 수 있습니다.

### 실습 질문

**Q) AndroidManifest에서 intent filter가 앱 상호작용을 어떻게 가능하게 하며, 액티비티 클래스가 AndroidManifest에 등록되지 않으면 어떤 일이 발생하나요?**

## Q) 7. Activity 생명주기를 설명하세요

Android Activity 생명주기는 액티비티가 생성부터 소멸까지 생애 동안 거치는 다양한 상태들을 설명합니다. 이러한 상태들을 이해하는 것은 리소스를 효과적으로 관리하고, 사용자 입력을 처리하며, 원활한 사용자 경험을 보장하는 데 매우 중요합니다. Activity 생명주기의 주요 단계들은 다음과 같습니다:

**Figure 21. activity-lifecycle**

1. **onCreate()**: 액티비티가 생성될 때 호출되는 첫 번째 메서드입니다. 여기서 액티비티를 초기화하고, UI 구성 요소를 설정하며, 저장된 인스턴스 상태를 복원합니다. 액티비티가 소멸되고 재생성되지 않는 한 생명주기 동안 한 번만 호출됩니다.
2. **onStart()**: 액티비티가 사용자에게 보이지만 아직 상호작용할 수 없는 상태입니다. onCreate() 이후이고 onResume() 이전에 호출됩니다.
3. **onRestart()**: 액티비티가 중지되었다가 다시 시작되는 경우(예: 사용자가 다시 돌아올 때) onStart() 이전에 이 메서드가 호출됩니다.
4. **onResume()**: 액티비티가 전면에 있고 사용자가 상호작용할 수 있는 상태입니다. 여기서 일시 중지된 UI 업데이트, 애니메이션 또는 입력 리스너를 재개합니다.
5. **onPause()**: 다른 액티비티에 의해 액티비티가 부분적으로 가려질 때(예: 대화상자) 호출됩니다. 액티비티는 여전히 보이지만 포커스를 잃은 상태입니다. 애니메이션, 센서 업데이트 일시 중지 또는 데이터 저장과 같은 작업에 자주 사용됩니다.
6. **onStop()**: 액티비티가 더 이상 사용자에게 보이지 않을 때(예: 다른 액티비티가 전면에 올 때) 호출됩니다. 백그라운드 작업이나 무거운 객체와 같이 액티비티가 중지된 동안 필요하지 않은 리소스를 해제해야 합니다.
7. **onDestroy()**: 액티비티가 완전히 소멸되고 메모리에서 제거되기 전에 호출됩니다. 남은 모든 리소스를 해제하는 최종 정리 메서드입니다.

### 요약

액티비티는 사용자 상호작용과 Android 시스템의 앱 리소스 관리에 따라 이러한 메서드들을 거칩니다. 개발자들은 이러한 콜백을 사용하여 전환을 관리하고, 리소스를 절약하며, 사용자에게 원활한 경험을 제공합니다. 자세한 내용은 Android 공식 문서¹⁶를 확인하세요.

### 실습 질문

**Q) onPause()와 onStop()의 차이점은 무엇이며, 리소스 집약적인 작업을 처리할 때 각각 어떤 시나리오에서 사용해야 하나요?**

### 마스터를 위한 전문가 팁: 여러 Activity 간의 생명주기 변화

Activity 생명주기에 대한 일반적인 후속 질문은 다음과 같습니다: "Activity A를 실행한 다음 Activity B를 실행하고, 마지막으로 Activity A로 순차적으로 돌아올 때 발생하는 생명주기 전환을 설명할 수 있나요?" 이 시나리오는 Android 시스템이 여러 액티비티 상태를 어떻게 관리하는지에 대한 이해를 테스트하는 데 도움이 됩니다.

두 액티비티인 Activity A와 Activity B 사이를 탐색할 때, 각 액티비티의 Android 생명주기 콜백이 특정 순서로 호출됩니다. 이 시나리오에 대한 생명주기 전환을 단계별로 분석해보겠습니다:

### Activity A와 Activity B의 완전한 순차 생명주기 흐름:

- **Activity A의 초기 실행**:
    - Activity A: 초기 실행 시 onCreate() -> onStart() -> onResume() 순서. 사용자가 Activity A와 상호작용합니다.
- **Activity A에서 Activity B로 이동**:
    - Activity A: onPause(), UI를 일시 중지하고 보이는 상태와 관련된 리소스를 해제합니다.
    - Activity B: onCreate() -> onStart() -> onResume(), 포커스를 얻고 전면 액티비티가 됩니다.
    - Activity A: onStop(), Activity B가 Activity A를 완전히 덮는 경우 선택적으로 호출됩니다.
- **Activity B에서 Activity A로 돌아가기**:
    - Activity B: onPause()
    - Activity A: onRestart() -> onStart() -> onResume(), 포커스를 다시 얻고 전면으로 돌아옵니다.
    - Activity B: onStop() -> onDestroy()

### 요약

두 액티비티 간 전환 시, 전면에 있는 액티비티는 백그라운드로 이동하기 전에 항상 onPause() 생명주기를 거칩니다. 실행되는 새 액티비티는 onCreate()부터 시작하는 생명주기로 포커스를 차지합니다. 이전 액티비티로 돌아갈 때는 onRestart() 또는 onResume()을 사용하여 일시 중지된 상태에서 재개되며, 나가는 액티비티는 작업에 따라 중지되거나 소멸됩니다. 이러한 생명주기 전환을 이해하면 적절한 리소스 관리와 원활한 사용자 경험을 보장할 수 있습니다.

### 마스터를 위한 전문가 팁: Activity의 생명주기 인스턴스란 무엇인가?

모든 Activity는 Lifecycle 인스턴스와 연결되어 있으며, 이는 Activity의 생명주기 이벤트를 관찰하고 반응할 수 있는 방법을 제공합니다. 생명주기 인스턴스는 Jetpack Lifecycle 라이브러리¹⁷의 일부이며, 개발자가 Activity의 생명주기 변화에 대응하여 코드를 깔끔하고 구조적인 방식으로 관리할 수 있게 해줍니다.

lifecycle 속성은 모든 ComponentActivity 하위 클래스에서 노출되는 Lifecycle 클래스의 인스턴스입니다. 이는 Activity의 현재 생명주기 상태를 나타내며, 해당 메서드를 직접 오버라이드하지 않고도 onCreate, onStart, onResume 등과 같은 생명주기 이벤트를 관찰할 수 있는 방법을 제공합니다. 이는 UI 업데이트, 리소스 정리 또는 생명주기 인식 방식으로 LiveData 관찰에 특히 유용합니다.

### 생명주기 인스턴스 사용 방법

생명주기 인스턴스를 사용하면 특정 생명주기 이벤트에 응답하는 LifecycleObserver 또는 DefaultLifecycleObserver 객체를 추가할 수 있습니다. 예를 들어, onStart와 onStop을 수신하려면 해당 콜백을 처리하는 관찰자를 등록할 수 있습니다:

**Figure 22. 생명주기 이벤트 관찰**

```kotlin

kotlin
class MyObserver : DefaultLifecycleObserver {

    override fun onStart(owner: LifecycleOwner) {
        super.onStart(owner)
    }

    override fun onStop(owner: LifecycleOwner) {
        super.onStop(owner)
    }
}

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        lifecycle.addObserver(MyObserver())
    }
}

```

이 예제에서 MyObserver 클래스는 MainActivity 인스턴스의 생명주기 변화를 관찰합니다. 액티비티가 STARTED 또는 STOPPED 상태에 들어가면 해당 메서드가 호출됩니다.

### 생명주기 인스턴스 사용의 이점

1. **생명주기 인식**: 생명주기 인스턴스를 사용하면 구성 요소가 Activity의 생명주기 상태를 인식할 수 있어, Activity가 원하는 상태에 있지 않을 때 불필요하거나 잘못된 작업을 방지할 수 있습니다.
2. **관심사의 분리**: 생명주기 의존적 로직을 Activity 클래스 밖으로 이동시켜 가독성과 유지보수성을 향상시킬 수 있습니다.
3. **Jetpack 라이브러리와의 통합**: LiveData와 ViewModel과 같은 라이브러리는 생명주기 인스턴스와 원활하게 작동하도록 설계되어 반응형 프로그래밍과 효율적인 리소스 관리를 가능하게 합니다.

### 요약

Activity의 생명주기 인스턴스는 Android의 현대적 아키텍처의 핵심 구성 요소로, 개발자가 구조적이고 재사용 가능한 방식으로 생명주기 이벤트를 처리할 수 있게 해줍니다. LifecycleObserver와 기타 Jetpack 구성 요소를 활용하여 더욱 견고하고 유지보수 가능한 애플리케이션을 만들 수 있습니다.

---

¹⁶ https://developer.android.com/reference/android/app/Activity
¹⁷ https://developer.android.com/jetpack/androidx/releases/lifecycle

## Q) 8. Fragment 생명주기를 설명하세요

각 Fragment 인스턴스는 연결된 액티비티의 생명주기와는 별개로 자체적인 생명주기를 가지고 있습니다. 사용자가 앱과 상호작용할 때, 프래그먼트는 추가되거나 제거되거나 화면에 나타났다 사라질 때와 같이 다양한 생명주기 상태를 거치며 전환됩니다. 이러한 생명주기 단계는 생성, 시작, 가시화, 활성화를 포함하며, 더 이상 필요하지 않을 때 중지나 소멸과 같은 상태로 전환됩니다. 이러한 전환을 관리하면 프래그먼트가 리소스를 효과적으로 처리하고, UI 일관성을 유지하며, 사용자 작업에 원활하게 응답할 수 있습니다.

Android의 프래그먼트 생명주기는 액티비티 생명주기와 매우 유사하지만 프래그먼트 고유의 추가적인 메서드와 동작을 도입합니다.

**Figure 23. fragment-lifecycle**

1. **onAttach()**: 프래그먼트가 부모 액티비티와 연결될 때 호출되는 첫 번째 콜백입니다. 프래그먼트가 이제 연결되어 액티비티 컨텍스트와 상호작용할 수 있습니다.
2. **onCreate()**: 프래그먼트를 초기화하기 위해 호출됩니다. 이 시점에서 프래그먼트는 생성되었지만 UI는 아직 생성되지 않았습니다. 여기서 일반적으로 필수 구성 요소를 초기화하거나 저장된 상태를 복원합니다.
3. **onCreateView()**: 프래그먼트의 UI가 처음으로 그려질 때 호출됩니다. 이 메서드에서 프래그먼트 레이아웃의 루트 뷰를 반환합니다. 여기서 LayoutInflater를 사용하여 프래그먼트의 레이아웃을 인플레이트합니다.
4. **onViewStateRestored()**: 프래그먼트의 뷰 계층이 생성되고 저장된 상태가 뷰에 복원된 후 호출됩니다.
5. **onViewCreated()**: 이 메서드는 프래그먼트의 뷰가 생성된 후 호출됩니다. UI 구성 요소와 사용자 상호작용을 처리하는 데 필요한 로직을 설정하는 데 자주 사용됩니다.
6. **onStart()**: 프래그먼트가 사용자에게 보이기 시작합니다. 이는 액티비티의 onStart() 콜백과 동일하며, 프래그먼트가 이제 활성화되었지만 아직 전면에 있지는 않은 상태입니다.
7. **onResume()**: 프래그먼트가 이제 완전히 활성화되어 전면에서 실행되고 있으며, 이는 상호작용 가능함을 의미합니다. 이 메서드는 프래그먼트의 UI가 완전히 보이고 사용자가 상호작용할 수 있을 때 호출됩니다.
8. **onPause()**: 프래그먼트가 더 이상 전면에 있지 않지만 여전히 보이는 상태일 때 호출됩니다. 프래그먼트가 포커스를 잃으려고 하며, 프래그먼트가 전면에 있지 않을 때 계속되어서는 안 되는 작업을 일시 중지해야 합니다.
9. **onStop()**: 프래그먼트가 더 이상 보이지 않을 때 호출됩니다. 여기서 프래그먼트가 화면 밖에 있는 동안 계속될 필요가 없는 작업을 중지합니다.
10. **onSaveInstanceState()**: 프래그먼트가 소멸되기 전에 UI 관련 상태 데이터를 저장하여 나중에 복원할 수 있도록 호출됩니다.
11. **onDestroyView()**: 프래그먼트의 뷰 계층이 제거될 때 호출됩니다. 메모리 누수를 방지하기 위해 어댑터 지우기나 참조 무효화와 같은 뷰와 관련된 리소스를 정리해야 합니다.
12. **onDestroy()**: 프래그먼트 자체가 소멸될 때 호출됩니다. 이 시점에서 모든 리소스를 정리해야 하지만, 프래그먼트는 여전히 부모 액티비티에 연결되어 있습니다.
13. **onDetach()**: 프래그먼트가 부모 액티비티에서 분리되어 더 이상 연결되지 않습니다. 이는 최종 콜백이며, 프래그먼트의 생명주기가 완료됩니다.

### 요약

프래그먼트 생명주기를 이해하는 것은 리소스를 효과적으로 관리하고, 구성 변경을 처리하며, Android 앱에서 원활한 사용자 경험을 보장하는 데 매우 중요합니다. 자세한 내용은 Android 공식 문서¹⁸를 확인하세요.

### 실습 질문

**Q) onCreateView()와 onDestroyView()의 목적은 무엇이며, 이러한 메서드에서 뷰 관련 리소스를 적절히 처리하는 것이 왜 중요한가요?**

**답변:**

**onCreateView()의 목적:**

- 프래그먼트의 UI 레이아웃을 생성하고 반환하는 역할을 합니다
- LayoutInflater를 사용하여 XML 레이아웃을 View 객체로 인플레이트합니다
- 프래그먼트의 시각적 구성 요소를 초기화하는 시작점입니다
- 이 메서드에서 반환된 View가 프래그먼트의 UI 계층의 루트가 됩니다

**onDestroyView()의 목적:**

- 프래그먼트의 뷰 계층이 제거될 때 호출됩니다
- 뷰와 관련된 리소스를 정리하고 메모리 누수를 방지하는 역할을 합니다
- View Binding이나 Data Binding 참조를 null로 설정해야 합니다
- 애니메이션, 리스너, 어댑터 등 뷰에 연결된 리소스를 해제합니다

**적절한 처리가 중요한 이유:**

1. **메모리 누수 방지**: 뷰 참조를 적절히 해제하지 않으면 메모리 누수가 발생할 수 있습니다
2. **생명주기 불일치 해결**: 프래그먼트 자체의 생명주기와 뷰의 생명주기가 다르기 때문에, 뷰가 소멸된 후에도 프래그먼트가 살아있을 수 있습니다
3. **크래시 방지**: 소멸된 뷰에 접근하려고 시도하면 앱이 크래시될 수 있습니다
4. **리소스 효율성**: 불필요한 뷰 리소스를 적절히 해제함으로써 앱의 성능을 향상시킵니다

### 마스터를 위한 전문가 팁: fragmentManager와 childFragmentManager의 차이점

Android에서 fragmentManager와 childFragmentManager는 프래그먼트 관리에 필수적이지만, 서로 다른 목적을 가지고 있으며 별개의 범위에서 작동합니다.

### fragmentManager

fragmentManager는 FragmentActivity나 Fragment 자체와 연결되어 있으며 액티비티 레벨에서 프래그먼트를 관리하는 역할을 합니다. 여기에는 부모 액티비티에 직접 연결된 프래그먼트를 추가, 교체 또는 제거하는 것이 포함됩니다.

액티비티에서 supportFragmentManager를 호출하면 이 fragmentManager에 액세스합니다. fragmentManager에 의해 관리되는 프래그먼트들은 형제 관계이며 동일한 계층 레벨에서 작동합니다.

```kotlin

kotlin
// 액티비티 레벨에서 프래그먼트 관리
supportFragmentManager.beginTransaction()
    .replace(R.id.container, ExampleFragment())
    .commit()

```

이는 일반적으로 액티비티의 주요 네비게이션이나 UI 구조의 일부인 프래그먼트에 사용됩니다.

### childFragmentManager

childFragmentManager는 Fragment에 특화되어 있으며 해당 프래그먼트의 자식 프래그먼트들을 관리합니다. 이를 통해 프래그먼트가 다른 프래그먼트들을 호스팅할 수 있어 중첩된 프래그먼트 구조를 만들 수 있습니다.

childFragmentManager를 사용할 때, 부모 프래그먼트의 생명주기 내에서 프래그먼트들을 정의합니다. 이는 프래그먼트 내에서 UI와 로직을 캡슐화하는 데 유용하며, 특히 프래그먼트가 액티비티의 프래그먼트 생명주기와 독립적인 자체 중첩 프래그먼트 세트가 필요할 때 유용합니다.

**Figure 24. childFragmentManager Example.kt**

```kotlin

kotlin
// 부모 프래그먼트 내에서 자식 프래그먼트 관리
childFragmentManager.beginTransaction()
    .replace(R.id.child_container, ChildFragment())
    .commit()

```

childFragmentManager에 의해 관리되는 자식 프래그먼트들은 부모 프래그먼트로 범위가 제한되며, 이는 그들의 생명주기가 부모 프래그먼트에 연결되어 있음을 의미합니다. 예를 들어, 부모 프래그먼트가 소멸될 때 그 자식 프래그먼트들도 함께 소멸됩니다.

### 주요 차이점

1. **범위**:
• fragmentManager는 액티비티 레벨에서 작동하며, 액티비티에 직접 연결된 프래그먼트들을 관리합니다.
• childFragmentManager는 프래그먼트 내에서 작동하며, 부모 프래그먼트 내에 중첩된 프래그먼트들을 관리합니다.
2. **사용 사례**:
• 액티비티의 주요 UI 구성 요소를 형성하는 프래그먼트에는 fragmentManager를 사용합니다.
• 프래그먼트가 자체 중첩 프래그먼트를 관리해야 할 때는 childFragmentManager를 사용하여 더 모듈화되고 재사용 가능한 UI 구성 요소를 만들 수 있습니다.
3. **생명주기**:
• fragmentManager에 의해 관리되는 프래그먼트들은 액티비티의 생명주기를 따릅니다.
• childFragmentManager에 의해 관리되는 프래그먼트들은 부모 프래그먼트의 생명주기를 따릅니다.

### 요약

fragmentManager와 childFragmentManager 사이의 선택은 UI의 계층 구조에 따라 달라집니다. 액티비티 레벨의 프래그먼트 관리에는 fragmentManager를 사용하고, 부모 프래그먼트 내에서 프래그먼트를 중첩시킬 때는 childFragmentManager를 선택하세요. 이들의 범위와 생명주기를 이해하면 Android 애플리케이션의 더 나은 구성과 모듈화를 보장할 수 있습니다.

### 마스터를 위한 전문가 팁: Fragment의 viewLifecycleOwner 인스턴스란 무엇인가?

Android 개발에서 Fragment는 호스팅 액티비티에 연결된 자체 생명주기를 가지고 있지만, Fragment의 View 계층은 별도의 생명주기를 가집니다. 이러한 구분은 Fragment에서 LiveData와 같은 구성 요소를 관리하거나 생명주기 인식 데이터 소스를 관찰할 때 매우 중요해집니다. viewLifecycleOwner 인스턴스는 이러한 미묘한 차이를 효과적으로 관리하는 데 도움을 줍니다.

### viewLifecycleOwner란 무엇인가?

viewLifecycleOwner는 Fragment의 View 계층과 연결된 LifecycleOwner입니다. 이는 Fragment의 onCreateView가 호출될 때 시작되고 onDestroyView가 호출될 때 끝나는 Fragment View의 생명주기를 나타냅니다. 이를 통해 UI 관련 데이터나 리소스를 Fragment View의 생명주기에 특별히 바인딩할 수 있어 메모리 누수와 같은 문제를 방지할 수 있습니다.

Fragment View 계층의 생명주기는 Fragment 자체의 생명주기보다 짧습니다. Fragment의 생명주기(this as LifecycleOwner)를 사용하여 데이터나 생명주기 이벤트를 관찰하면, View가 소멸된 후에도 View에 접근할 위험이 있습니다. 이는 크래시나 예상치 못한 동작을 유발할 수 있습니다.

viewLifecycleOwner를 사용하면 관찰자나 생명주기 인식 구성 요소가 View의 생명주기에 연결되어, View가 소멸될 때 안전하게 업데이트를 중지할 수 있습니다.

다음은 메모리 누수를 피하면서 Fragment에서 LiveData를 관찰하는 예제입니다:

**Figure 25. viewLifecycleOwner와 LiveData 사용**

```kotlin

kotlin
class MyFragment : Fragment(R.layout.fragment_example) {

    private val viewModel: MyViewModel by viewModels()

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

// viewLifecycleOwner를 사용하여 LiveData 관찰
        viewModel.data.observe(viewLifecycleOwner) { data ->
// 새 데이터로 UI 업데이트
            textView.text = data
        }
    }
}

```

이 예제에서 viewLifecycleOwner는 Fragment 자체가 여전히 살아있더라도 Fragment의 View가 소멸될 때 관찰자가 자동으로 제거되도록 보장합니다.

### lifecycleOwner와 viewLifecycleOwner의 차이점

- **lifecycleOwner (Fragment의 생명주기)**: Fragment의 전체 생명주기를 나타내며, 이는 더 길고 호스팅 Activity에 연결되어 있습니다.
- **viewLifecycleOwner (Fragment View의 생명주기)**: Fragment View의 생명주기를 나타내며, onCreateView에서 시작되고 onDestroyView에서 끝납니다.

### 요약

viewLifecycleOwner 사용은 LiveData 관찰이나 View에 연결된 리소스 관리와 같이 View의 생명주기를 존중해야 하는 UI 관련 작업에서 특히 유용합니다.

---

¹⁸ https://developer.android.com/guide/fragments/lifecycle

## Q) 9. Service란 무엇인가요?

Service는 사용자 상호작용과 독립적으로 오래 실행되는 작업을 수행할 수 있게 해주는 백그라운드 구성 요소입니다. 액티비티와 달리 서비스는 사용자 인터페이스를 가지지 않으며 앱이 전면에 있지 않을 때도 계속 실행될 수 있습니다. 파일 다운로드, 음악 재생, 데이터 동기화, 네트워크 작업 처리와 같은 백그라운드 작업에 일반적으로 사용됩니다.

## 1. Started Service

서비스는 애플리케이션 구성 요소가 `startService()`를 호출할 때 시작됩니다. `stopSelf()`를 사용하여 스스로 중지하거나 `stopService()`를 사용하여 명시적으로 중지될 때까지 백그라운드에서 무한정 실행됩니다.

사용 예시:

- 백그라운드 음악 재생
- 파일 업로드 또는 다운로드

Figure 26. Started Service

```kotlin

kotlin
class MyService : Service() {
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
// 백그라운드에서 오래 실행되는 작업 수행
        return START_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? = null
}

```

## 2. Bound Service

바운드 서비스는 구성 요소가 `bindService()`를 사용하여 바인딩할 수 있게 해줍니다. 바인딩된 클라이언트가 있는 동안 활성 상태를 유지하며 모든 클라이언트가 연결을 끊으면 자동으로 중지됩니다.

사용 예시:

- 원격 서버에서 데이터 가져오기
- 백그라운드 블루투스 연결 관리

Figure 27. Bound Service

```kotlin

kotlin
class BoundService : Service() {
    private val binder = LocalBinder()

    inner class LocalBinder : Binder() {
        fun getService(): BoundService = this@BoundService
    }

    override fun onBind(intent: Intent?): IBinder = binder
}

```

## 3. Foreground Service

포어그라운드 서비스는 지속적인 알림을 표시하면서 활성 상태를 유지하는 특별한 유형의 서비스입니다. 음악 재생, 내비게이션, 위치 추적과 같이 지속적인 사용자 인식이 필요한 작업에 사용됩니다.

Figure 28. Foreground Service

```kotlin

kotlin
class ForegroundService : Service() {
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        val notification = createNotification()
        startForeground(1, notification)
        return START_STICKY
    }

    private fun createNotification(): Notification {
        return NotificationCompat.Builder(this, "channel_id")
            .setContentTitle("Foreground Service")
            .setContentText("Running...")
            .setSmallIcon(R.drawable.ic_notification)
            .build()
    }
}

```

## 서비스 유형 간의 주요 차이점

```
서비스 유형백그라운드 실행자동 중지UI 알림 필요Started Service예아니오아니오Bound Service예예 (모든 클라이언트가 바인딩 해제 시)아니오Foreground Service예아니오예
```

## 서비스 사용 모범 사례

- 즉시 실행이 필요하지 않은 백그라운드 작업에는 서비스 대신 Jetpack WorkManager¹⁹를 사용하세요.
- 불필요한 리소스 소비를 방지하기 위해 작업이 완료되면 서비스를 중지하세요.
- 투명성을 위해 명확한 알림을 제공하여 포어그라운드 서비스를 책임감 있게 사용하세요.
- 메모리 누수를 방지하기 위해 서비스 생명주기 변경을 적절히 처리하세요.

## 요약

Service는 사용자 상호작용 없이 백그라운드 처리를 가능하게 합니다. Started 서비스는 수동으로 중지될 때까지 실행되고, Bound 서비스는 다른 구성 요소와 상호작용하며, Foreground 서비스는 지속적인 알림과 함께 활성 상태를 유지합니다. 서비스를 적절히 관리하면 효율적인 시스템 리소스 사용과 원활한 사용자 경험을 보장할 수 있습니다.

## 실습 질문

Q) Android에서 started 서비스와 bound 서비스의 차이점은 무엇이며, 각각 언제 사용해야 하나요?

### 답변:

**Started Service와 Bound Service의 주요 차이점:**

**Started Service:**

- `startService()`로 시작되며 독립적으로 실행됩니다
- 호출한 구성 요소와 상관없이 계속 실행됩니다
- `stopSelf()` 또는 `stopService()`로 명시적으로 중지해야 합니다
- 구성 요소와 직접적인 통신 인터페이스를 제공하지 않습니다
- 한 번 시작되면 작업이 완료될 때까지 실행됩니다

**Bound Service:**

- `bindService()`로 바인딩되며 클라이언트에 의존적입니다
- 하나 이상의 클라이언트가 바인딩되어 있는 동안만 실행됩니다
- 모든 클라이언트가 바인딩 해제되면 자동으로 중지됩니다
- `IBinder` 인터페이스를 통해 클라이언트와 직접 통신할 수 있습니다
- 클라이언트-서버 관계를 형성합니다

**사용 시기:**

**Started Service를 사용하는 경우:**

- 백그라운드 음악 재생
- 파일 업로드/다운로드
- 데이터 동기화
- 로그 기록
- 클라이언트와의 상호작용이 필요 없는 독립적인 작업

**Bound Service를 사용하는 경우:**

- 원격 서버에서 데이터 가져오기
- 계산 결과를 반환해야 하는 작업
- 실시간 데이터 스트림 제공
- 클라이언트가 서비스의 메서드를 직접 호출해야 하는 경우
- 여러 구성 요소가 같은 서비스 인스턴스를 공유해야 하는 경우

## 마스터를 위한 전문가 팁: 포어그라운드 서비스를 어떻게 처리하나요?

포어그라운드 서비스는 사용자에게 눈에 띄는 작업을 수행하는 특별한 유형의 서비스입니다. 알림 표시줄에 지속적인 알림을 표시하여 사용자가 그 작업을 인식할 수 있도록 보장합니다. 포어그라운드 서비스는 미디어 재생, 위치 추적, 파일 업로드와 같은 높은 우선순위 작업에 사용됩니다.

일반 서비스와의 주요 차이점은 포어그라운드 서비스가 시작 즉시 `startForeground()`를 호출하고 알림을 표시해야 한다는 것입니다.

Figure 29. ForegroundService.kt

```kotlin

kotlin
class ForegroundService : Service() {

    override fun onCreate() {
        super.onCreate()
// 리소스 초기화
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        val notification = createNotification()
        startForeground(1, notification)// 서비스를 포어그라운드로 시작// 작업 수행
        return START_STICKY
    }

    private fun createNotification(): Notification {
        val notificationChannelId = "ForegroundServiceChannel"
        val channel = NotificationChannel(
            notificationChannelId,
            "Foreground Service",
            NotificationManager.IMPORTANCE_DEFAULT
        )
        getSystemService(NotificationManager::class.java).createNotificationChannel(channel)
        return NotificationCompat.Builder(this, notificationChannelId)
            .setContentTitle("Foreground Service")
            .setContentText("Running in the foreground")
            .setSmallIcon(R.drawable.ic_notification)
            .build()
    }

    override fun onDestroy() {
        super.onDestroy()
// 리소스 정리
    }

    override fun onBind(intent: Intent?): IBinder? = null
}

```

## Service와 Foreground Service의 주요 차이점

1. **사용자 인식**: 일반 서비스는 사용자가 알아채지 못하게 백그라운드에서 실행될 수 있지만, 포어그라운드 서비스는 알림이 필요하여 그 작업이 사용자에게 보입니다.
2. **우선순위**: 포어그라운드 서비스는 더 높은 우선순위를 가지며 메모리 부족 상황에서 일반 서비스에 비해 시스템에 의해 종료될 가능성이 낮습니다.
3. **사용 사례**: 서비스는 가벼운 백그라운드 작업에 이상적이며, 포어그라운드 서비스는 지속적이고 사용자가 알아차릴 수 있는 작업에 적합합니다.

## 서비스 사용 모범 사례

1. 작업이 완료되면 시스템 리소스를 절약하기 위해 항상 서비스를 중지하세요.
2. 즉시 실행이 필요하지 않은 백그라운드 작업에는 WorkManager를 사용하세요.
3. 포어그라운드 서비스의 경우, 투명성을 유지하기 위해 알림이 사용자 친화적이고 정보를 제공하도록 하세요.

## 요약

Android 서비스는 효율적인 백그라운드 작업 실행을 가능하게 하며, 포어그라운드 서비스는 사용자 가시성과 함께 지속적인 작업이 필요한 작업에 사용됩니다. 둘 다 시스템 리소스를 효율적으로 관리하면서 원활한 사용자 경험을 유지하는 앱을 만드는 데 중요한 역할을 합니다.

## 마스터를 위한 전문가 팁: 서비스의 생명주기는 무엇인가요?

앞서 배운 바와 같이, 서비스는 두 가지 모드로 작동할 수 있습니다:

1. **Started Service**: `startService()`를 사용하여 시작되며 `stopSelf()` 또는 `stopService()`를 사용하여 명시적으로 중지될 때까지 계속 실행됩니다.
2. **Bound Service**: `bindService()`를 사용하여 하나 이상의 구성 요소에 연결되며 바인딩되어 있는 동안 존재합니다.

생명주기는 `onCreate()`, `onStartCommand()`, `onBind()`, `onDestroy()`와 같은 메서드를 통해 관리됩니다.

### Started Service의 생명주기 메서드

1. **onCreate()** 서비스가 처음 생성될 때 호출됩니다. 서비스에 필요한 리소스를 초기화하는 데 사용됩니다.
2. **onStartCommand()** `startService()`로 서비스가 시작될 때 트리거됩니다. 이 메서드는 실제 작업 실행을 처리하고 반환 값을 사용하여 서비스가 종료될 경우의 재시작 동작을 결정합니다(예: START_STICKY, START_NOT_STICKY).
3. **onDestroy()** `stopSelf()` 또는 `stopService()`를 사용하여 서비스가 중지될 때 호출됩니다. 리소스 해제나 스레드 중지와 같은 정리 작업에 사용됩니다.

Figure 31. SimpleStartedService.kt

```kotlin

kotlin
class SimpleStartedService : Service() {
    override fun onCreate() {
        super.onCreate()
// 리소스 초기화
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
// 오래 실행되는 작업 수행
        return START_STICKY// 서비스가 종료되면 재시작
    }

    override fun onDestroy() {
        super.onDestroy()
// 리소스 정리
    }

    override fun onBind(intent: Intent?): IBinder? = null// Started 서비스에서는 사용하지 않음
}

```

### Bound Service의 생명주기 메서드

1. **onCreate()** Started 서비스와 유사하게, 서비스가 생성될 때 리소스를 초기화합니다.
2. **onBind()** `bindService()`를 사용하여 구성 요소가 서비스에 바인딩될 때 호출됩니다. 이 메서드는 서비스에 대한 인터페이스(IBinder)를 제공합니다.
3. **onUnbind()** 마지막 클라이언트가 서비스에서 바인딩을 해제할 때 호출됩니다. 바인딩된 클라이언트와 관련된 리소스를 정리하는 곳입니다.
4. **onDestroy()** 서비스가 종료될 때 호출됩니다. 리소스 정리와 진행 중인 작업 중지를 처리합니다.

Figure 32. SimpleBoundService.kt

```kotlin

kotlin
class SimpleBoundService : Service() {
    private val binder = LocalBinder()

    override fun onCreate() {
        super.onCreate()
// 리소스 초기화
    }

    override fun onBind(intent: Intent?): IBinder {
        return binder// 바운드 서비스를 위한 인터페이스 반환
    }

    override fun onUnbind(intent: Intent?): Boolean {
// 바인딩된 클라이언트가 없을 때 정리
        return super.onUnbind(intent)
    }

    override fun onDestroy() {
        super.onDestroy()
// 리소스 정리
    }

    inner class LocalBinder : Binder() {
        fun getService(): SimpleBoundService = this@SimpleBoundService
    }
}

```

### Started Service와 Bound Service 생명주기의 주요 차이점

1. **Started Service**: 구성 요소와 독립적이며 명시적으로 중지될 때까지 실행됩니다.
2. **Bound Service**: 최소 하나의 클라이언트에 바인딩되어 있는 동안만 존재합니다.

## 요약

서비스 생명주기를 이해하는 것은 효율적이고 신뢰할 수 있는 백그라운드 작업을 구현하는 데 중요합니다. Started 서비스는 독립적인 작업을 수행하고 중지될 때까지 지속되며, Bound 서비스는 클라이언트 상호작용을 위한 인터페이스를 제공하고 바인딩이 해제되면 종료됩니다. 이러한 생명주기를 적절히 관리하면 최적의 리소스 활용을 보장하고 메모리 누수를 방지할 수 있습니다.

¹⁹https://developer.android.com/jetpack/androidx/releases/work

## Q) 10. BroadcastReceiver란 무엇인가?

BroadcastReceiver는 앱이 시스템 전체 브로드캐스트 메시지나 앱별 브로드캐스트를 수신하고 응답할 수 있게 해주는 컴포넌트입니다. 이러한 브로드캐스트는 일반적으로 시스템이나 다른 애플리케이션에 의해 트리거되어 배터리 상태 변경, 네트워크 연결 업데이트, 앱 내에서 전송되는 사용자 정의 인텐트와 같은 다양한 이벤트를 알려줍니다. BroadcastReceiver는 동적인 시스템 또는 앱 수준 이벤트에 반응하는 반응형 애플리케이션을 구축하기 위한 유용한 메커니즘입니다.

## BroadcastReceiver의 목적

BroadcastReceiver는 액티비티나 서비스의 생명주기와 직접적으로 연결되지 않을 수 있는 이벤트를 처리하는 데 사용됩니다. 앱이 백그라운드에서 지속적으로 실행되지 않고도 변경사항에 반응할 수 있게 해주는 메시징 시스템 역할을 하여 리소스를 절약합니다.

## 브로드캐스트의 유형

1. **시스템 브로드캐스트**: Android 운영체제에서 배터리 레벨 변경, 시간대 업데이트, 네트워크 연결 변경과 같은 시스템 이벤트에 대해 앱에 알리기 위해 전송됩니다.
2. **사용자 정의 브로드캐스트**: 애플리케이션에서 앱 내부 또는 앱 간에 특정 정보나 이벤트를 통신하기 위해 전송됩니다.

## BroadcastReceiver 선언

BroadcastReceiver를 생성하려면 BroadcastReceiver 클래스를 확장하고 onReceive 메서드를 오버라이드해야 합니다. 여기서 브로드캐스트를 처리하는 로직을 정의합니다.

**그림 33. MyBroadcastReceiver.kt**

```kotlin

kotlin
class MyBroadcastReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        val action = intent.action
        if (action == Intent.ACTION_BATTERY_LOW) {
// 배터리 부족 이벤트 처리
            Log.d("MyBroadcastReceiver", "배터리가 부족합니다!")
        }
    }
}

```

## BroadcastReceiver 등록

BroadcastReceiver를 등록하는 방법은 두 가지입니다:

### 1. 정적 등록 (매니페스트를 통한 방법)

앱이 실행되지 않을 때도 처리해야 하는 이벤트에 사용합니다. AndroidManifest.xml 파일에 `<intent-filter>`를 추가합니다.

**그림 34. AndroidManifest.xml**

```xml

xml
<receiver android:name=".MyBroadcastReceiver">
    <intent-filter>
        <action android:name="android.intent.action.BATTERY_LOW" />
    </intent-filter>
</receiver>

```

### 2. 동적 등록 (코드를 통한 방법)

앱이 활성 상태이거나 특정 상태일 때만 처리해야 하는 이벤트에 사용합니다.

**그림 35. Activity에서의 동적 등록**

```kotlin

kotlin
val receiver = MyBroadcastReceiver()
val intentFilter = IntentFilter(Intent.ACTION_BATTERY_LOW)
registerReceiver(receiver, intentFilter)

```

## 중요한 고려사항

- **생명주기 관리**: 동적 등록을 사용하는 경우, 메모리 누수를 방지하기 위해 `unregisterReceiver`를 사용하여 리시버를 등록 해제해야 합니다.
- **백그라운드 실행 제한**: Android 8.0 (API 레벨 26)부터 백그라운드 앱은 암시적 브로드캐스트 예외를 제외하고 브로드캐스트 수신에 제한이 있습니다. 이러한 경우를 처리하기 위해 `Context.registerReceiver`나 `JobScheduler`를 사용하세요.
- **보안**: 민감한 정보가 포함된 브로드캐스트의 경우 권한으로 보호하여 무단 액세스를 방지하세요.

## BroadcastReceiver 사용 사례

- 네트워크 연결 변경 모니터링
- SMS나 통화 이벤트에 응답
- 충전 상태와 같은 시스템 이벤트에 반응하여 UI 업데이트
- 사용자 정의 브로드캐스트로 작업이나 알람 스케줄링

## 요약

BroadcastReceiver는 특히 OS 시스템과 상호작용하는 반응형 애플리케이션을 구축하는 데 필수적인 컴포넌트입니다. 앱이 시스템이나 앱 이벤트를 효율적으로 수신하고 응답할 수 있게 해줍니다. 특히 생명주기를 고려한 등록과 최신 Android 제한 사항 준수를 통한 적절한 사용은 앱을 견고하고 리소스 효율적으로 유지하는 데 도움이 됩니다.

---

## 실습 질문

**Q) 브로드캐스트의 다양한 유형은 무엇이며, 시스템 브로드캐스트와 사용자 정의 브로드캐스트가 기능과 사용법 측면에서 어떻게 다른가요?**

### 답안:

브로드캐스트는 크게 두 가지 유형으로 나뉩니다:

### 1. 시스템 브로드캐스트 (System Broadcasts)

**기능:**

- Android 운영체제에서 자동으로 전송
- 시스템 수준의 이벤트나 상태 변경을 알림
- 모든 앱이 수신할 수 있는 전역적 특성

**주요 예시:**

- `ACTION_BATTERY_LOW`: 배터리 부족 상태
- `ACTION_BOOT_COMPLETED`: 기기 부팅 완료
- `CONNECTIVITY_ACTION`: 네트워크 연결 상태 변경
- `ACTION_SCREEN_OFF/ON`: 화면 꺼짐/켜짐
- `ACTION_TIME_CHANGED`: 시간 변경

**사용법:**

```kotlin
kotlin
// 매니페스트에서 정적 등록
<receiver android:name=".SystemEventReceiver">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED" />
    </intent-filter>
</receiver>

// 동적 등록
val filter = IntentFilter().apply {
    addAction(Intent.ACTION_BATTERY_LOW)
    addAction(ConnectivityManager.CONNECTIVITY_ACTION)
}
registerReceiver(systemReceiver, filter)
```

### 2. 사용자 정의 브로드캐스트 (Custom Broadcasts)

**기능:**

- 개발자가 직접 정의하고 전송
- 앱 내부 또는 앱 간 통신에 사용
- 특정 비즈니스 로직이나 이벤트를 처리

**사용 방식:**

```kotlin
// 사용자 정의 브로드캐스트 전송
val customIntent = Intent("com.myapp.CUSTOM_ACTION").apply {
    putExtra("data", "중요한 데이터")
}
sendBroadcast(customIntent)

// 사용자 정의 브로드캐스트 수신
class CustomBroadcastReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        when (intent.action) {
            "com.myapp.CUSTOM_ACTION" -> {
                val data = intent.getStringExtra("data")
// 사용자 정의 로직 처리
            }
        }
    }
}
```

### 주요 차이점 비교

```
구분시스템 브로드캐스트사용자 정의 브로드캐스트발신자Android 시스템개발자/앱범위시스템 전체앱 내부 또는 지정된 앱제어시스템이 자동 관리개발자가 완전 제어보안시스템에서 보장개발자가 직접 구현 필요성능 영향시스템 최적화됨구현 방식에 따라 달라짐권한시스템 권한 필요할 수 있음사용자 정의 권한 설정 가능
```

### 실제 사용 시나리오

**시스템 브로드캐스트 활용:**

```kotlin
class NetworkMonitorReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        val connectivityManager = context.getSystemService(Context.CONNECTIVITY_SERVICE)
            as ConnectivityManager
        val networkInfo = connectivityManager.activeNetworkInfo

        if (networkInfo?.isConnected == true) {
// 네트워크 연결됨 - 대기 중인 작업 실행
            startPendingTasks(context)
        } else {
// 네트워크 연결 끊어짐 - 작업 일시정지
            pauseNetworkTasks(context)
        }
    }
}
```

**사용자 정의 브로드캐스트 활용:**

```kotlin
// 데이터 동기화 완료 알림
class DataSyncManager {
    fun notifySyncComplete(context: Context, syncResult: SyncResult) {
        val intent = Intent("com.myapp.DATA_SYNC_COMPLETE").apply {
            putExtra("sync_result", syncResult)
            putExtra("timestamp", System.currentTimeMillis())
        }
        context.sendBroadcast(intent)
    }
}

// UI에서 동기화 결과 수신
class SyncStatusReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        val syncResult = intent.getParcelableExtra<SyncResult>("sync_result")
// UI 업데이트 또는 사용자 알림
        updateSyncStatus(syncResult)
    }
}
```

이러한 차이점을 이해하고 적절히 활용하면 효율적이고 반응형인 Android 애플리케이션을 개발할 수 있습니다.
