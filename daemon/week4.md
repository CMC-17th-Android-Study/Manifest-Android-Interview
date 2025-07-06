## Q) 16. 태스크와 백스택이란 무엇인가?

- *태스크(Task)**는 특정 목표를 달성하기 위해 사용자가 상호작용하는 액티비티들의 모음입니다. 태스크는 백스택으로 구성되며, 이는 후입선출(LIFO) 구조로 액티비티가 실행될 때 추가되고 사용자가 뒤로 탐색하거나 시스템이 리소스를 회수할 때 제거됩니다.

### 태스크(Tasks)

태스크는 액티비티가 시작될 때 생성되며, 일반적으로 런처나 인텐트를 통해 시작됩니다. 태스크는 인텐트와 액티비티 실행 모드 설정에 따라 여러 애플리케이션과 그 액티비티들을 포함할 수 있습니다. 예를 들어, 이메일 앱에서 링크를 클릭하면 같은 태스크의 일부로 브라우저가 열릴 수 있습니다. 태스크는 관련된 액티비티가 소멸될 때까지 활성 상태를 유지합니다.

### 백스택(Back Stack)

백스택은 태스크 내에서 액티비티의 히스토리를 관리합니다. 사용자가 새로운 액티비티로 이동할 때, 현재 액티비티는 스택에 푸시됩니다. 뒤로 버튼을 누르면 맨 위의 액티비티가 스택에서 팝되고, 그 아래의 액티비티가 재개됩니다. 이 메커니즘은 직관적인 탐색과 사용자 워크플로우의 연속성을 보장합니다.

태스크와 백스택은 액티비티 실행 모드와 인텐트 플래그의 영향을 받습니다. 실행 모드와 인텐트 플래그는 태스크와 백스택 내에서 액티비티의 동작을 제어하는 메커니즘입니다. 이러한 구성을 통해 개발자는 액티비티가 실행되는 방식과 다른 액티비티와의 상호작용을 정의할 수 있습니다.

### 실행 모드(Launch Modes)

실행 모드는 액티비티가 백스택에서 어떻게 인스턴스화되고 처리되는지를 결정합니다. Android에는 네 가지 주요 실행 모드가 있습니다:

1. **standard**: 기본 실행 모드입니다. 인스턴스가 이미 존재하더라도 액티비티가 실행될 때마다 새로운 인스턴스가 생성되어 백스택에 추가됩니다.
2. **singleTop**: 액티비티의 인스턴스가 이미 백스택의 맨 위에 있으면 새로운 인스턴스가 생성되지 않습니다. 대신 기존 인스턴스가 onNewIntent()에서 인텐트를 처리합니다.
3. **singleTask**: 태스크에서 액티비티의 인스턴스는 하나만 존재합니다. 인스턴스가 이미 존재하면 앞으로 가져오고 onNewIntent()가 호출됩니다. 이는 앱의 진입점 역할을 하는 액티비티에 유용합니다.
4. **singleInstance**: singleTask와 유사하지만, 액티비티는 다른 액티비티와 분리된 자체 태스크에 배치됩니다. 이는 다른 액티비티가 같은 태스크의 일부가 될 수 없음을 보장합니다.

### 인텐트 플래그(Intent Flags)

인텐트 플래그는 액티비티가 실행되는 방식이나 인텐트가 전송될 때 백스택이 동작하는 방식을 수정하는 데 사용됩니다. 일반적으로 사용되는 플래그들:

- **FLAG_ACTIVITY_NEW_TASK**: 새로운 태스크에서 액티비티를 시작하거나, 이미 존재하는 경우 태스크를 앞으로 가져옵니다.
- **FLAG_ACTIVITY_CLEAR_TOP**: 액티비티가 이미 백스택에 있으면 그 위의 모든 액티비티를 정리하고, 기존 인스턴스가 인텐트를 처리합니다.
- **FLAG_ACTIVITY_SINGLE_TOP**: 액티비티가 백스택의 맨 위에 있으면 새로운 인스턴스가 생성되지 않도록 보장합니다. 다른 플래그와 함께 사용되는 경우가 많습니다.
- **FLAG_ACTIVITY_NO_HISTORY**: 액티비티가 백스택에 추가되지 않도록 하여 종료 후 지속되지 않게 합니다.

### 사용 사례

- 실행 모드는 주로 AndroidManifest.xml 파일의 `<activity>` 태그 아래에 선언되어 액티비티의 기본 동작을 설정할 수 있습니다.
- 인텐트 플래그는 인텐트를 생성할 때 프로그래밍적으로 적용되어 특정 시나리오에 더 많은 유연성을 제공합니다.

### 예제

```kotlin
val intent = Intent(this, SecondActivity::class.java).apply {
    flags = Intent.FLAG_ACTIVITY_NEW_TASK or Intent.FLAG_ACTIVITY_CLEAR_TOP
}
startActivity(intent)
```

이 예제에서 SecondActivity는 아직 존재하지 않는 경우 새로운 태스크에서 실행됩니다. 존재하는 경우 그 위의 모든 액티비티가 정리됩니다.

### 요약

태스크와 백스택은 Android의 탐색 모델의 핵심으로, 액티비티의 생명주기와 탐색 히스토리를 관리하여 사용자 친화적인 워크플로우를 가능하게 합니다. 실행 모드는 액티비티가 태스크 내에서 실행되고 관리되는 방식의 기본 동작을 정의하며, 인텐트 플래그는 유사한 동작에 대한 런타임 제어를 제공합니다. 함께 사용하면 액티비티 생명주기와 백스택 탐색의 정밀한 관리가 가능합니다.

---

## Q) 17. Bundle의 목적은 무엇인가?

**Bundle**은 액티비티, 프래그먼트, 서비스 등의 컴포넌트 간에 데이터를 전달하는 데 사용되는 키-값 쌍 데이터 구조입니다. 앱 내에서 소량의 데이터를 효율적으로 전송하는 데 일반적으로 사용됩니다. Bundle은 경량이며 Android 운영체제가 쉽게 관리하고 전송할 수 있는 형태로 데이터를 직렬화하도록 설계되었습니다.

### Bundle의 일반적인 사용 사례

1. **액티비티 간 데이터 전달**: 새로운 액티비티를 시작할 때 Bundle을 Intent에 첨부하여 대상 액티비티로 데이터를 전달할 수 있습니다.
2. **프래그먼트 간 데이터 전달**: 프래그먼트 트랜잭션에서 Bundle은 setArguments()와 getArguments()와 함께 사용되어 프래그먼트 간에 데이터를 전송합니다.
3. **인스턴스 상태 저장 및 복원**: Bundle은 onSaveInstanceState()와 onRestoreInstanceState()와 같은 생명주기 메서드에서 구성 변경 중 임시 UI 상태를 저장하고 복원하는 데 사용됩니다.
4. **서비스로 데이터 전달**: Bundle은 서비스를 시작하거나 바인드된 서비스로 데이터를 전달할 때 데이터를 담을 수 있습니다.

### Bundle의 작동 방식

Bundle은 데이터를 키-값 구조로 직렬화하여 작동합니다. 키는 문자열이고, 값은 원시 타입, Serializable, Parcelable 객체, 또는 다른 Bundle이 될 수 있습니다. 이를 통해 효율적인 데이터 저장과 전송이 가능합니다.

### 예제: 액티비티 간 데이터 전달

```kotlin
// Activity A에서 데이터 전송
val intent = Intent(this, ActivityB::class.java).apply {
    putExtra("user_name", "John Doe")
    putExtra("user_age", 25)
}
startActivity(intent)

// Activity B에서 데이터 받기
val name = intent.getStringExtra("user_name")
val age = intent.getIntExtra("user_age", -1)
```

이 예제에서 데이터는 Intent.putExtra()를 통해 내부적으로 Bundle에 패키지됩니다.

### 예제: 프래그먼트 간 데이터 전달

```kotlin
// 프래그먼트로 데이터 전송
val fragment = MyFragment().apply {
    arguments = Bundle().apply {
        putString("user_name", "Jane Doe")
        putInt("user_age", 30)
    }
}

// 프래그먼트에서 데이터 받기
val name = arguments?.getString("user_name")
val age = arguments?.getInt("user_age")
```

### 예제: 상태 저장 및 복원

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    outState.putString("user_input", editText.text.toString())
}

override fun onRestoreInstanceState(savedInstanceState: Bundle) {
    super.onRestoreInstanceState(savedInstanceState)
    val userInput = savedInstanceState.getString("user_input")
    editText.setText(userInput)
}
```

이 경우 Bundle은 화면 회전과 같은 구성 변경 중에 사용자 입력이 보존되도록 보장합니다.

### 요약

Bundle은 컴포넌트 간 및 생명주기 이벤트 전반에 걸쳐 데이터를 효율적으로 전달하고 보존하는 Android의 중요한 컴포넌트입니다. 경량이고 유연한 구조로 인해 애플리케이션 상태와 데이터 전송을 관리하는 필수 도구입니다.

---

## 실전 질문 답변

### Q) singleTask와 singleInstance 실행 모드의 차이점은 무엇이며, 각각 어떤 시나리오에서 사용해야 하는가?

**singleTask와 singleInstance의 주요 차이점:**

**singleTask:**

- 액티비티의 인스턴스가 하나의 태스크에 단 하나만 존재
- 다른 액티비티들과 같은 태스크 내에서 공존 가능
- 인스턴스가 이미 존재하면 그 위의 모든 액티비티를 제거하고 해당 액티비티를 맨 위로 가져옴
- onNewIntent() 호출됨

**singleInstance:**

- 액티비티가 완전히 독립적인 태스크에 단독으로 존재
- 다른 액티비티가 같은 태스크에 배치될 수 없음
- 시스템 전체에서 해당 액티비티의 인스턴스가 하나만 존재

**사용 시나리오:**

**singleTask 사용 시나리오:**

- 앱의 메인 액티비티나 홈 화면
- 로그인 액티비티 (한 번 로그인하면 재사용)
- 설정 화면 (여러 개가 필요 없음)

**singleInstance 사용 시나리오:**

- 전화 앱의 통화 화면
- 알람 앱의 알람 화면
- 카메라 앱의 촬영 화면
- 다른 앱에서 공유되어 사용되는 액티비티

### Q) 다양한 액티비티 실행 모드와 그들이 태스크와 백스택 동작에 미치는 영향은?

**1. standard (기본 모드):**

- 호출될 때마다 새로운 인스턴스 생성
- 백스택에 계속 쌓임
- 일반적인 액티비티에 적합

**2. singleTop:**

- 맨 위에 있으면 새 인스턴스 생성 안 함
- 푸시 알림을 처리하는 액티비티에 적합
- 중복 인스턴스 방지

**3. singleTask:**

- 태스크당 하나의 인스턴스만 존재
- 루트 액티비티로 동작
- 앱의 메인 진입점에 적합

**4. singleInstance:**

- 독립적인 태스크에 단독 존재
- 다른 앱과 공유되는 액티비티에 적합

### Q) onSaveInstanceState()가 Bundle을 사용하여 구성 변경 중 UI 상태를 보존하는 방법과 Bundle에 저장할 수 있는 데이터 타입은?

**onSaveInstanceState() 동작 방식:**

1. **호출 시점:** 액티비티가 소멸되기 전 (화면 회전, 언어 변경 등)
2. **Bundle 저장:** 임시 UI 상태를 Bundle에 저장
3. **복원:** onCreate() 또는 onRestoreInstanceState()에서 Bundle로부터 상태 복원

**Bundle에 저장 가능한 데이터 타입:**

**기본 타입:**

- String, int, long, float, double, boolean
- 배열: IntArray, StringArray 등

**객체 타입:**

- Parcelable 인터페이스를 구현한 객체
- Serializable 인터페이스를 구현한 객체
- Bundle (중첩 Bundle)

**컬렉션:**

- ArrayList<String>, ArrayList<Integer>
- ArrayList<Parcelable>

**예제 코드:**

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)

// 기본 타입 저장
    outState.putString("text_input", editText.text.toString())
    outState.putInt("selected_position", spinner.selectedItemPosition)
    outState.putBoolean("checkbox_state", checkbox.isChecked)

// 객체 저장 (Parcelable)
    outState.putParcelable("user_data", userData)

// 컬렉션 저장
    outState.putStringArrayList("item_list", itemList)
}

override fun onRestoreInstanceState(savedInstanceState: Bundle) {
    super.onRestoreInstanceState(savedInstanceState)

// 데이터 복원
    editText.setText(savedInstanceState.getString("text_input"))
    spinner.setSelection(savedInstanceState.getInt("selected_position"))
    checkbox.isChecked = savedInstanceState.getBoolean("checkbox_state")

    userData = savedInstanceState.getParcelable("user_data")
    itemList = savedInstanceState.getStringArrayList("item_list") ?: arrayListOf()
}
```

**주의사항:**

- Bundle은 1MB 제한이 있으므로 큰 데이터는 저장하지 않는 것이 좋음
- 복잡한 객체는 Parcelable을 구현하여 효율적으로 직렬화
- 임시 UI 상태만 저장하고, 영구적인 데이터는 SharedPreferences나 Database 사용

---

## Q) 18. 액티비티나 프래그먼트 간에 데이터를 전달하는 방법은?

액티비티나 프래그먼트 간의 데이터 전달은 대화형이고 동적인 화면을 만들기 위해 중요합니다. Android는 앱의 아키텍처를 준수하면서 원활한 통신을 보장하는 다양한 메커니즘을 제공합니다.

### 액티비티 간 데이터 전달

한 액티비티에서 다른 액티비티로 데이터를 전달하려면 **Intent**가 가장 일반적으로 사용되는 메커니즘입니다. 데이터는 키-값 쌍(putExtra())을 사용하여 Intent에 추가되고, 수신 액티비티는 getIntent()를 사용하여 데이터를 검색합니다.

```kotlin
// 전송 액티비티
val intent = Intent(this, SecondActivity::class.java).apply {
    putExtra("USER_NAME", "John Doe")
    putExtra("USER_AGE", 25)
}
startActivity(intent)

// 수신 액티비티
class SecondActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_second)

        val userName = intent.getStringExtra("USER_NAME")
        val userAge = intent.getIntExtra("USER_AGE", 0)
        Log.d("SecondActivity", "User Name: $userName, Age: $userAge")
    }
}
```

### 프래그먼트 간 데이터 전달

프래그먼트 간 통신을 위해서는 **Bundle**을 사용할 수 있습니다. 전송 프래그먼트가 키-값 쌍으로 Bundle을 생성하고 arguments를 통해 수신 프래그먼트에 전달합니다.

```kotlin
// 전송 프래그먼트
val fragment = SecondFragment().apply {
    arguments = Bundle().apply {
        putString("USER_NAME", "John Doe")
        putInt("USER_AGE", 25)
    }
}
parentFragmentManager.beginTransaction()
    .replace(R.id.fragment_container, fragment)
    .commit()

// 수신 프래그먼트
class SecondFragment : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        val view = inflater.inflate(R.layout.fragment_second, container, false)

        val userName = arguments?.getString("USER_NAME")
        val userAge = arguments?.getInt("USER_AGE")
        Log.d("SecondFragment", "User Name: $userName, Age: $userAge")

        return view
    }
}
```

### Jetpack Navigation 라이브러리를 사용한 프래그먼트 간 데이터 전달

**Safe Args** 플러그인과 함께 Jetpack Navigation 라이브러리를 사용하면 목적지 간 타입 안전한 네비게이션을 가능하게 하는 direction과 argument 클래스를 생성할 수 있습니다.

**1. 네비게이션 그래프에서 인수 정의**

nav_graph.xml에서:

```xml
<fragment
    android:id="@+id/secondFragment"
    android:name="com.example.SecondFragment">
    <argument
        android:name="username"
        app:argType="string" />
</fragment>
```

**2. 소스 프래그먼트에서 데이터 전달**

Safe Args 플러그인은 컴파일 시 목적지 객체와 빌더 클래스를 생성하여 안전하고 명시적으로 인수를 전달할 수 있게 합니다:

```kotlin
// FirstFragment.kt
val action = FirstFragmentDirections
    .actionFirstFragmentToSecondFragment(username = "skydoves")
findNavController().navigate(action)
```

**3. 목적지 프래그먼트에서 데이터 검색**

전달된 인수에서 데이터를 검색할 수 있습니다:

```kotlin
// SecondFragment.kt
val username = arguments?.let {
    SecondFragmentArgs.fromBundle(it).username
}
```

Safe Args를 사용하면 강력한 타입의 인수를 정의하고 검색할 수 있어 런타임 오류를 줄이고 프래그먼트 간의 가독성을 향상시킬 수 있습니다.

### 공유 ViewModel 사용

프래그먼트가 같은 액티비티 내에서 통신해야 할 때 **공유 ViewModel**이 권장되는 접근 방식입니다. 공유 ViewModel은 같은 액티비티 내의 여러 프래그먼트 간에 공유되는 ViewModel 인스턴스를 의미합니다. 이는 Jetpack의 androidx.fragment:fragment-ktx 패키지에서 제공하는 activityViewModels() 메서드를 사용하여 달성됩니다. 이 메서드는 ViewModel을 액티비티에 범위를 지정하여 프래그먼트가 ViewModel의 동일한 인스턴스에 액세스하고 공유할 수 있게 합니다. 이 방법은 프래그먼트 간의 긴밀한 결합을 방지하고 생명주기를 인식하는 반응형 데이터 공유를 가능하게 합니다.

```kotlin
// 공유 ViewModel
class SharedViewModel: ViewModel() {
    private val _userData = MutableStateFlow<User?>(null)
    val userData : StateFlow<User?> = _userData

    fun setUserData(user: User) {
        _userData.value = user
    }
}

// Fragment A (데이터 전송)
class FirstFragment : Fragment() {
    private val sharedViewModel: SharedViewModel by activityViewModels()

    fun updateUser(user: User) {
        sharedViewModel.setUserData(user)
    }
}

// Fragment B (다른 프래그먼트에서 데이터 수신)
class SecondFragment : Fragment() {
    private val sharedViewModel: SharedViewModel by activityViewModels()

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        lifecycleScope.launch {
            viewLifecycleOwner.repeatOnLifecycle(Lifecycle.State.RESUMED) {
                sharedViewModel.userData.collectLatest { user ->
// 사용자 데이터 처리
                }
            }
        }
    }
}

// Activity (액티비티에서 데이터 수신)
class MainActivity : ComponentActivity() {
    private val sharedViewModel: SharedViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        lifecycleScope.launch {
            lifecycle.repeatOnLifecycle(Lifecycle.State.RESUMED) {
                sharedViewModel.userData.collectLatest { user ->
// 사용자 데이터 처리
                }
            }
        }
    }
}
```

### 요약

1. **Intent**는 putExtra()와 getIntent()를 사용하여 액티비티 간에 데이터를 전달하는 데 사용됩니다.
2. **Bundle**은 arguments 속성을 통해 프래그먼트 간에 데이터를 전달하는 데 일반적으로 사용됩니다.
3. **Safe Args 플러그인**과 함께 Jetpack Navigation을 사용하여 생성된 direction과 argument 클래스를 통해 프래그먼트 간 타입 안전한 인수 전달이 가능합니다.
4. 같은 액티비티 내의 프래그먼트 간 데이터 공유를 위해 **공유 ViewModel**이 생명주기를 인식하고 분리된 솔루션을 제공합니다.

각 방법은 고유한 사용 사례가 있으며, 선택은 애플리케이션의 특정 요구사항에 따라 달라집니다.

---

## Fragment Result API (고급 팁)

경우에 따라 프래그먼트는 다른 프래그먼트나 프래그먼트와 호스트 액티비티 사이에 일회성 값을 전달해야 합니다. 예를 들어, QR 코드 스캔 프래그먼트가 스캔된 데이터를 이전 프래그먼트로 다시 보내야 할 수 있습니다.

Fragment 버전 1.3.0+부터 각 FragmentManager는 FragmentResultOwner를 구현하여 프래그먼트가 서로 직접 참조할 필요 없이 결과 리스너를 통해 통신할 수 있습니다. 이는 느슨한 결합을 유지하면서 데이터 전달을 단순화합니다.

Fragment B(송신자)에서 Fragment A(수신자)로 데이터를 전달하려면 다음 단계를 따르세요:

1. Fragment A(결과를 받는 프래그먼트)에서 결과 리스너를 설정합니다.
2. 동일한 requestKey를 사용하여 Fragment B에서 결과를 전송합니다.

### Fragment A에서 결과 리스너 설정

Fragment A는 setFragmentResultListener()를 사용하여 리스너를 등록하여 STARTED 상태가 될 때 결과를 받을 수 있도록 해야 합니다.

```kotlin
// FragmentA.kt
class FragmentA : Fragment() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

// 결과를 받기 위한 리스너 등록
        setFragmentResultListener("requestKey") { _, bundle ->
            val result = bundle.getString("bundleKey")
// 받은 결과 처리
        }
    }
}
```

setFragmentResultListener("requestKey")는 특정 요청 키에 대한 리스너를 등록합니다. 콜백은 프래그먼트가 STARTED 상태에 진입할 때 실행됩니다.

### Fragment B에서 결과 전송

Fragment B는 setFragmentResult()를 사용하여 결과를 전송하여 Fragment A가 활성화될 때 데이터를 검색할 수 있도록 합니다.

```kotlin
// FragmentB.kt
class FragmentB : Fragment() {
    private lateinit var button: Button

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        button = view.findViewById(R.id.button)
        button.setOnClickListener {
            val result = "result"
// FragmentA로 결과 전송
            setFragmentResult("requestKey", bundleOf("bundleKey" to result))
        }
    }
}
```

setFragmentResult("requestKey", bundleOf("bundleKey" to result))는 지정된 키를 사용하여 FragmentManager에 결과를 저장합니다. Fragment A가 활성화되지 않은 경우 Fragment A가 재개되고 리스너를 등록할 때까지 결과가 저장됩니다.

### Fragment Result의 동작

- **키당 단일 리스너**: 각 키는 한 번에 하나의 리스너와 하나의 결과만 가질 수 있습니다.
- **대기 중인 결과는 덮어쓰기됨**: 리스너가 활성화되기 전에 여러 결과가 설정되면 최신 결과만 저장됩니다.
- **결과는 소비된 후 정리됨**: 프래그먼트가 결과를 받고 처리하면 결과가 FragmentManager에서 제거됩니다.
- **백스택의 프래그먼트는 결과를 받지 않음**: 프래그먼트가 백스택에서 팝되고 STARTED 상태가 되어야 결과를 받을 수 있습니다.
- **STARTED 상태의 리스너는 즉시 트리거됨**: Fragment B가 결과를 설정할 때 Fragment A가 이미 활성화되어 있으면 리스너가 즉시 실행됩니다.

### 요약

Fragment Result API는 직접 참조 없이 프래그먼트 간 일회성 값 전달을 단순화합니다. FragmentManager를 활용하여 수신 프래그먼트가 활성화될 때까지 결과를 안전하게 저장하여 분리되고 생명주기를 인식하는 통신 메커니즘을 보장합니다. 이 접근 방식은 QR 코드 스캔, 사용자 입력 대화상자, 폼 제출 등 다양한 시나리오에서 유용하며 프래그먼트 기반 네비게이션을 더 효율적이고 유지보수하기 쉽게 만듭니다.

---

## 실전 질문 답변

### Q) 공유 ViewModel이 같은 액티비티 내 프래그먼트 간 통신을 어떻게 촉진하며, Bundle이나 직접 프래그먼트 트랜잭션을 사용하는 것에 비해 어떤 장점이 있는가?

**공유 ViewModel의 통신 방식:**

공유 ViewModel은 `activityViewModels()` 델리게이트를 통해 액티비티 범위에서 ViewModel 인스턴스를 생성하고 공유합니다. 모든 프래그먼트가 동일한 ViewModel 인스턴스에 액세스할 수 있어 데이터 일관성을 보장합니다.

**공유 ViewModel의 장점:**

**1. 생명주기 인식:**

- ViewModel은 액티비티의 생명주기를 따라 데이터를 보존
- 화면 회전이나 구성 변경 시에도 데이터가 유지됨
- 프래그먼트가 소멸되고 재생성되어도 데이터 손실 없음

**2. 반응형 데이터 공유:**

- LiveData나 StateFlow를 통한 관찰 가능한 데이터 스트림
- 데이터 변경 시 모든 구독자에게 자동으로 알림
- UI 업데이트가 자동으로 처리됨

**3. 느슨한 결합:**

- 프래그먼트 간 직접 참조 불필요
- 각 프래그먼트가 독립적으로 ViewModel과 상호작용
- 코드의 유지보수성과 테스트 용이성 향상

**4. 타입 안전성:**

- 강타입 데이터 전달
- 컴파일 타임에 타입 검사
- 런타임 오류 방지

**Bundle 대비 장점:**

```kotlin
// Bundle 방식 (제한적)
val bundle = Bundle().apply {
    putString("data", "value")
// 타입 안전성 부족, 키 문자열 하드코딩
}

// 공유 ViewModel 방식 (권장)
class SharedViewModel : ViewModel() {
    private val _data = MutableStateFlow("")
    val data: StateFlow<String> = _data

    fun updateData(newData: String) {
        _data.value = newData
    }
}
```

**직접 프래그먼트 트랜잭션 대비 장점:**

```kotlin

// 직접 참조 방식 (권장하지 않음)
val fragment = supportFragmentManager.findFragmentByTag("tag") as? MyFragment
fragment?.updateData("data")// 타이트한 결합, null 안전성 문제// 공유 ViewModel 방식 (권장)
sharedViewModel.updateData("data")// 느슨한 결합, 안전함
```

**5. 데이터 지속성:**

- 프래그먼트 교체나 백스택 네비게이션에도 데이터 유지
- 복잡한 UI 상태 관리 용이
- 사용자 입력 데이터 보존

**6. 테스트 용이성:**

- ViewModel은 UI와 분리되어 단위 테스트 가능
- 비즈니스 로직을 별도로 테스트할 수 있음
- Mock 객체 사용이 간편함

**실제 사용 예시:**

```kotlin
// 쇼핑카트 시나리오
class ShoppingCartViewModel : ViewModel() {
    private val _cartItems = MutableStateFlow<List<Item>>(emptyList())
    val cartItems: StateFlow<List<Item>> = _cartItems

    private val _totalPrice = MutableStateFlow(0.0)
    val totalPrice: StateFlow<Double> = _totalPrice

    fun addItem(item: Item) {
        val currentItems = _cartItems.value.toMutableList()
        currentItems.add(item)
        _cartItems.value = currentItems
        calculateTotal()
    }

    private fun calculateTotal() {
        _totalPrice.value = _cartItems.value.sumOf { it.price }
    }
}

// 상품 목록 프래그먼트
class ProductListFragment : Fragment() {
    private val cartViewModel: ShoppingCartViewModel by activityViewModels()

    private fun onAddToCart(item: Item) {
        cartViewModel.addItem(item)
    }
}

// 장바구니 프래그먼트
class CartFragment : Fragment() {
    private val cartViewModel: ShoppingCartViewModel by activityViewModels()

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        lifecycleScope.launch {
            cartViewModel.cartItems.collect { items ->
                updateCartUI(items)
            }
        }

        lifecycleScope.launch {
            cartViewModel.totalPrice.collect { price ->
                updatePriceUI(price)
            }
        }
    }
}
```

**결론:**
공유 ViewModel은 Bundle이나 직접 프래그먼트 참조에 비해 생명주기 인식, 반응형 데이터 공유, 느슨한 결합, 타입 안전성, 데이터 지속성, 테스트 용이성 등의 장점을 제공합니다. 특히 복잡한 UI 상태 관리나 여러 프래그먼트 간 데이터 동기화가 필요한 경우 공유 ViewModel이 최적의 선택입니다.

## Q) 19. 구성 변경 시 Activity에 무슨 일이 일어나는가?

Android에서 구성 변경이 발생할 때 (예: 화면 회전, 테마 변경, 폰트 크기 조정, 언어 업데이트), 시스템은 새로운 구성을 적용하기 위해 현재 Activity를 파괴하고 다시 생성할 수 있습니다. 이 동작은 앱의 리소스가 업데이트된 구성을 반영하도록 다시 로드되도록 보장합니다.

### 구성 변경 시 기본 동작

1. **Activity 파괴 및 재생성**: 구성 변경이 발생하면 Activity가 파괴되고 다시 생성됩니다. 이 과정은 다음 단계를 포함합니다:
    - 시스템이 현재 Activity의 `onPause()`, `onStop()`, `onDestroy()` 메서드를 호출합니다.
    - 새로운 구성으로 `onCreate()` 메서드를 호출하여 Activity를 다시 생성합니다.
2. **리소스 재로딩**: 시스템은 새로운 구성을 기반으로 리소스(레이아웃, 드로어블, 문자열 등)를 다시 로드하여 앱이 화면 방향, 테마, 로케일 등의 변경사항에 적응할 수 있도록 합니다.
3. **데이터 손실 방지**: 재생성 중 데이터 손실을 방지하기 위해 개발자는 `onSaveInstanceState()`와 `onRestoreInstanceState()` 메서드를 사용하거나 ViewModel을 활용하여 인스턴스 상태를 저장하고 복원할 수 있습니다.

### SavedInstanceState를 이용한 구성 변경 처리

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    outState.putString("user_input", editText.text.toString())
}

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    val restoredInput = savedInstanceState?.getString("user_input")
    editText.setText(restoredInput)
}
```

### 재생성을 유발하는 주요 구성 변경사항

1. **화면 회전**: 화면의 방향이 세로와 가로 사이에서 변경되어 새로운 크기에 맞게 레이아웃이 다시 로드됩니다.
2. **다크/라이트 테마 변경**: 사용자가 다크 모드와 라이트 모드를 전환할 때 앱은 테마별 리소스(색상, 스타일 등)를 다시 로드합니다.
3. **폰트 크기 변경**: 기기의 폰트 크기 설정 조정으로 새로운 크기를 반영하기 위해 텍스트 리소스가 다시 로드됩니다.
4. **언어 변경**: 시스템 언어 업데이트는 지역화된 리소스(다른 언어의 문자열 등) 로딩을 유발합니다.

### Activity 재생성 방지

Activity를 재시작하지 않고 구성 변경을 처리하려면 매니페스트에서 `android:configChanges` 속성을 사용할 수 있습니다. 이 접근 방식은 변경사항을 프로그래밍적으로 처리할 책임을 개발자에게 위임합니다.

```xml
<activity
    android:name=".MainActivity"
    android:configChanges="orientation|screenSize|keyboardHidden" />
```

이 시나리오에서 시스템은 Activity를 파괴하고 다시 생성하지 않습니다. 대신 `onConfigurationChanged()` 메서드가 호출되어 개발자가 변경사항을 수동으로 처리할 수 있습니다.

### 구성 변경 수동 처리

```kotlin
override fun onConfigurationChanged(newConfig: Configuration) {
    super.onConfigurationChanged(newConfig)

    if (newConfig.orientation == Configuration.ORIENTATION_LANDSCAPE) {
// 가로 방향 특정 변경사항 처리
    } else if (newConfig.orientation == Configuration.ORIENTATION_PORTRAIT) {
// 세로 방향 특정 변경사항 처리
    }
}
```

### 요약

구성 변경이 발생할 때 기본 동작은 Activity를 파괴하고 다시 생성하여 새로운 구성에 적응하도록 리소스를 다시 로드하는 것입니다. 개발자는 임시 UI 상태를 보존하기 위해 `onSaveInstanceState()`를 사용하거나 비-UI 상태를 위해 ViewModel을 사용할 수 있습니다. 재생성을 방지하려면 매니페스트에서 `android:configChanges` 속성을 사용하여 변경사항을 처리할 책임을 개발자에게 위임할 수 있습니다.

---

## Q) 20. ActivityManager란 무엇인가?

ActivityManager는 Android의 시스템 서비스로, 기기에서 실행 중인 액티비티, 작업, 프로세스에 대한 정보를 제공하고 관리합니다. 이는 Android 프레임워크의 일부로, 개발자가 앱 생명주기, 메모리 사용량, 작업 관리의 측면과 상호작용하고 제어할 수 있도록 합니다.

### ActivityManager의 주요 기능

1. **작업 및 액티비티 정보**: ActivityManager는 실행 중인 작업, 액티비티, 스택 상태에 대한 세부사항을 검색할 수 있습니다. 이는 개발자가 앱 동작과 시스템 리소스 사용량을 모니터링하는 데 도움이 됩니다.
2. **메모리 관리**: 앱별 메모리 소비와 시스템 전체 메모리 상태를 포함한 시스템 전반의 메모리 사용량에 대한 정보를 제공합니다. 개발자는 이를 사용하여 앱 성능을 최적화하고 저메모리 상황을 처리할 수 있습니다.
3. **앱 프로세스 관리**: ActivityManager는 실행 중인 앱 프로세스와 서비스에 대한 세부사항을 쿼리할 수 있습니다. 개발자는 이 정보를 사용하여 앱 상태를 감지하거나 프로세스 수준 변경사항에 응답할 수 있습니다.
4. **디버깅 및 진단**: 힙 덤프 생성이나 앱 프로파일링과 같은 디버깅 도구를 제공하여 성능 병목현상이나 메모리 누수를 식별하는 데 도움이 됩니다.

### ActivityManager의 일반적인 메서드

- `getRunningAppProcesses()`: 현재 기기에서 실행 중인 프로세스 목록을 반환합니다.
- `getMemoryInfo(ActivityManager.MemoryInfo memoryInfo)`: 사용 가능한 메모리, 임계값 메모리, 기기가 저메모리 상태인지 여부와 같은 시스템에 대한 상세한 메모리 정보를 검색합니다. 저메모리 상황에서 앱 동작을 최적화하는 데 유용합니다.
- `killBackgroundProcesses(String packageName)`: 시스템 리소스를 확보하기 위해 지정된 앱의 백그라운드 프로세스를 종료합니다. 테스트나 리소스 집약적인 앱 관리에 유용합니다.
- `isLowRamDevice()`: 기기가 저RAM으로 분류되는지 확인하여 앱이 저메모리 기기에 맞게 리소스 사용량을 최적화하는 데 도움이 됩니다.
- `appNotResponding(String message)`: 테스트 목적으로 앱이 응답하지 않음(ANR) 이벤트를 시뮬레이션합니다. ANR 상황에서 앱이 어떻게 동작하거나 응답하는지 이해하기 위해 디버깅 중에 사용할 수 있습니다.
- `clearApplicationUserData()`: 파일, 데이터베이스, 공유 환경설정을 포함한 애플리케이션과 관련된 모든 사용자별 데이터를 지웁니다. 공장 초기화나 앱을 기본 상태로 재설정하는 경우에 자주 사용됩니다.

### 사용 예시

다음 코드는 ActivityManager를 사용하여 메모리 정보를 가져오는 방법을 보여줍니다:

```kotlin
val activityManager = getSystemService(Context.ACTIVITY_SERVICE) as ActivityManager
val memoryInfo = ActivityManager.MemoryInfo()
activityManager.getMemoryInfo(memoryInfo)

Log.d(TAG, "Low memory state: ${memoryInfo.lowMemory}")
Log.d(TAG, "Threshold memory: ${memoryInfo.threshold / (1024 * 1024)} MB")

val processes = activityManager.runningAppProcesses
Log.d(TAG, "Process name: ${processes.first().processName}")

// 앱이 시스템에 중단되었다고 알리고 ANR을 트리거하는 메서드
activityManager.appNotResponding("Pokedex is not responding")

// 애플리케이션이 디스크에서 자체 데이터를 지울 수 있게 하는 메서드
activityManager.clearApplicationUserData()
```

### LeakCanary에서의 ActivityManager

LeakCanary는 Block에서 유지관리하는 Android 애플리케이션용 오픈소스 메모리 누수 감지 라이브러리입니다. 개발 중 앱의 메모리 누수를 자동으로 모니터링하고 감지하여 누수를 효율적으로 수정할 수 있는 상세한 분석과 실행 가능한 인사이트를 제공합니다. 메모리 상태와 정보를 추적하기 위해 ActivityManager를 활용합니다.

### 요약

ActivityManager는 시스템 수준 관리, 성능 튜닝, 앱 동작 모니터링을 위한 도구입니다. 최신 Android에서는 그 기능이 더 특화된 API로 부분적으로 대체되었지만, Android 애플리케이션에서 리소스 사용량을 관리하고 최적화하는 도구로 남아있습니다. 개발자는 의도하지 않은 시스템 성능 영향을 피하기 위해 이를 책임감 있게 사용할 수 있습니다.

---

## 실전 질문에 대한 답변

### Q) 구성 변경으로 인한 액티비티 재생성 시 데이터 손실을 방지하는 방법과 임시 상태와 지속적 상태를 처리하는 방법은 무엇인가?

**답변:**

구성 변경 시 데이터 손실을 방지하기 위한 주요 방법들:

### 1. 임시 상태 처리 방법

**SavedInstanceState 사용:**

```kotlin
// 상태 저장
override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    outState.putString("user_input", editText.text.toString())
    outState.putInt("counter", currentCounter)
    outState.putParcelable("user_data", userData)
}

// 상태 복원
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    savedInstanceState?.let {
        val restoredInput = it.getString("user_input")
        val restoredCounter = it.getInt("counter")
        val restoredUserData = it.getParcelable<UserData>("user_data")

        editText.setText(restoredInput)
        currentCounter = restoredCounter
        userData = restoredUserData
    }
}
```

**onRestoreInstanceState 사용:**

```kotlin
override fun onRestoreInstanceState(savedInstanceState: Bundle) {
    super.onRestoreInstanceState(savedInstanceState)
// onCreate 이후에 호출되므로 뷰가 완전히 초기화된 상태
    editText.setText(savedInstanceState.getString("user_input"))
}
```

### 2. 지속적 상태 처리 방법

**ViewModel 사용 (권장):**

```kotlin
class MainViewModel : ViewModel() {
    private val _userData = MutableLiveData<UserData>()
    val userData: LiveData<UserData> = _userData

    private val _networkData = MutableLiveData<List<Item>>()
    val networkData: LiveData<List<Item>> = _networkData

    fun loadData() {
// 네트워크 요청이나 비즈니스 로직
        viewModelScope.launch {
            val data = repository.fetchData()
            _networkData.value = data
        }
    }
}

// Activity에서 사용
class MainActivity : AppCompatActivity() {
    private lateinit var viewModel: MainViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        viewModel = ViewModelProvider(this)[MainViewModel::class.java]

        viewModel.userData.observe(this) { data ->
// UI 업데이트
        }

        viewModel.networkData.observe(this) { items ->
// 리스트 업데이트
        }
    }
}
```

### 3. 데이터 지속성 방법

**SharedPreferences (간단한 설정값):**

```kotlin
private fun saveToPreferences(key: String, value: String) {
    val prefs = getSharedPreferences("app_prefs", MODE_PRIVATE)
    prefs.edit().putString(key, value).apply()
}

private fun loadFromPreferences(key: String): String? {
    val prefs = getSharedPreferences("app_prefs", MODE_PRIVATE)
    return prefs.getString(key, null)
}
```

**Room Database (복잡한 데이터):**

```kotlin
@Entity
data class UserData(
    @PrimaryKey val id: Int,
    val name: String,
    val email: String
)

@Dao
interface UserDao {
    @Query("SELECT * FROM userdata")
    fun getAll(): LiveData<List<UserData>>

    @Insert
    suspend fun insert(userData: UserData)
}
```

### 4. 구성 변경 처리 전략

**상황별 권장사항:**

- **UI 상태 (텍스트 입력, 체크박스 상태)**: SavedInstanceState
- **비즈니스 로직 데이터**: ViewModel
- **사용자 설정**: SharedPreferences
- **대용량 데이터**: Room Database
- **임시 파일**: 내부 저장소

### Q) ActivityManager.getMemoryInfo()를 사용하여 앱 성능을 최적화하는 방법과 시스템이 저메모리 상태가 될 때 개발자가 취해야 할 조치는 무엇인가?

**답변:**

### 1. ActivityManager.getMemoryInfo() 활용 방법

```kotlin
class MemoryManager {
    private val activityManager = getSystemService(Context.ACTIVITY_SERVICE) as ActivityManager

    fun checkMemoryStatus(): MemoryStatus {
        val memoryInfo = ActivityManager.MemoryInfo()
        activityManager.getMemoryInfo(memoryInfo)

        return MemoryStatus(
            availableMemory = memoryInfo.availMem,
            totalMemory = memoryInfo.totalMem,
            threshold = memoryInfo.threshold,
            isLowMemory = memoryInfo.lowMemory,
            isLowRamDevice = activityManager.isLowRamDevice()
        )
    }

    fun optimizeBasedOnMemory() {
        val memoryStatus = checkMemoryStatus()

        when {
            memoryStatus.isLowMemory -> {
// 저메모리 상태 처리
                handleLowMemoryState()
            }
            memoryStatus.availableMemory < memoryStatus.threshold * 1.5 -> {
// 메모리 부족 임계점 접근
                handleMemoryPressure()
            }
            memoryStatus.isLowRamDevice -> {
// 저사양 기기 최적화
                optimizeForLowRamDevice()
            }
        }
    }
}
```

### 2. 저메모리 상태 처리 방법

**메모리 해제 전략:**

```kotlin
class LowMemoryHandler {

    fun handleLowMemoryState() {
// 1. 캐시 정리
        clearCaches()

// 2. 이미지 리소스 최적화
        optimizeImageResources()

// 3. 백그라운드 작업 중단
        cancelNonEssentialTasks()

// 4. 데이터 구조 최적화
        optimizeDataStructures()
    }

    private fun clearCaches() {
// 이미지 캐시 정리
        Glide.get(context).clearMemory()

// 커스텀 캐시 정리
        AppCache.getInstance().clearMemoryCache()

// 시스템 캐시 정리
        val cacheDir = context.cacheDir
        cacheDir.deleteRecursively()
    }

    private fun optimizeImageResources() {
// 이미지 품질 낮추기
        val options = BitmapFactory.Options().apply {
            inSampleSize = 2// 이미지 크기 절반으로
            inPreferredConfig = Bitmap.Config.RGB_565// 메모리 사용량 줄이기
        }

// 불필요한 이미지 뷰 해제
        recycleImageViews()
    }

    private fun cancelNonEssentialTasks() {
// 백그라운드 작업 취소
        backgroundExecutor.shutdown()

// 네트워크 요청 취소
        networkManager.cancelNonEssentialRequests()

// 애니메이션 중단
        animationManager.pauseAnimations()
    }
}
```

### 3. 메모리 모니터링 및 최적화

**실시간 메모리 모니터링:**

```kotlin
class MemoryMonitor {
    private val handler = Handler(Looper.getMainLooper())
    private val monitoringRunnable = object : Runnable {
        override fun run() {
            checkMemoryAndOptimize()
            handler.postDelayed(this, 30000)// 30초마다 체크
        }
    }

    fun startMonitoring() {
        handler.post(monitoringRunnable)
    }

    fun stopMonitoring() {
        handler.removeCallbacks(monitoringRunnable)
    }

    private fun checkMemoryAndOptimize() {
        val memoryInfo = ActivityManager.MemoryInfo()
        (getSystemService(Context.ACTIVITY_SERVICE) as ActivityManager)
            .getMemoryInfo(memoryInfo)

        val usedMemoryPercent = (memoryInfo.totalMem - memoryInfo.availMem) * 100 / memoryInfo.totalMem

        when {
            usedMemoryPercent > 90 -> {
// 즉시 메모리 해제
                aggressiveMemoryCleanup()
            }
            usedMemoryPercent > 75 -> {
// 점진적 메모리 최적화
                moderateMemoryOptimization()
            }
            usedMemoryPercent > 60 -> {
// 예방적 최적화
                preventiveOptimization()
            }
        }
    }
}
```

### 4. 저사양 기기 최적화

```kotlin
class LowRamDeviceOptimizer {

    fun optimizeForLowRamDevice() {
// 1. 기능 제한
        disableNonEssentialFeatures()

// 2. UI 최적화
        optimizeUI()

// 3. 데이터 로딩 최적화
        optimizeDataLoading()

// 4. 백그라운드 작업 제한
        limitBackgroundTasks()
    }

    private fun disableNonEssentialFeatures() {
// 애니메이션 비활성화
        animationsEnabled = false

// 고해상도 이미지 비활성화
        highResolutionImagesEnabled = false

// 복잡한 UI 효과 비활성화
        complexUIEffectsEnabled = false
    }

    private fun optimizeUI() {
// RecyclerView 최적화
        recyclerView.apply {
            setHasFixedSize(true)
            recycledViewPool.setMaxRecycledViews(0, 0)
            setItemViewCacheSize(0)
        }

// 이미지 로딩 최적화
        Glide.with(context)
            .setDefaultRequestOptions(
                RequestOptions()
                    .diskCacheStrategy(DiskCacheStrategy.RESOURCE)
                    .skipMemoryCache(false)
                    .downsample(DownsampleStrategy.AT_MOST)
            )
    }

    private fun optimizeDataLoading() {
// 페이징 크기 줄이기
        pagingConfig = PagingConfig(
            pageSize = 10,// 기본값보다 작게
            prefetchDistance = 3,
            enablePlaceholders = false
        )

// 데이터 미리 로딩 제한
        preFetchingEnabled = false
    }
}
```

### 5. 종합적인 메모리 관리 전략

**Application 클래스에서 전역 메모리 관리:**

```kotlin
class MyApplication : Application() {

    override fun onCreate() {
        super.onCreate()
        setupMemoryManagement()
    }

    private fun setupMemoryManagement() {
// 메모리 임계값 설정
        val memoryManager = MemoryManager()

// 메모리 상태에 따른 초기 최적화
        memoryManager.optimizeBasedOnMemory()

// 메모리 모니터링 시작
        MemoryMonitor().startMonitoring()
    }

    override fun onLowMemory() {
        super.onLowMemory()
// 시스템 저메모리 콜백 처리
        LowMemoryHandler().handleLowMemoryState()
    }

    override fun onTrimMemory(level: Int) {
        super.onTrimMemory(level)
        when (level) {
            TRIM_MEMORY_RUNNING_MODERATE -> {
// 메모리 부족 시작
                moderateMemoryTrim()
            }
            TRIM_MEMORY_RUNNING_LOW -> {
// 메모리 부족 심각
                aggressiveMemoryTrim()
            }
            TRIM_MEMORY_RUNNING_CRITICAL -> {
// 메모리 부족 위험
                criticalMemoryTrim()
            }
        }
    }
}
```

이러한 전략들을 통해 앱의 메모리 사용량을 효과적으로 관리하고, 저메모리 상황에서도 안정적인 사용자 경험을 제공할 수 있습니다.
