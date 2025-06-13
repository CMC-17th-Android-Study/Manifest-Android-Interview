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