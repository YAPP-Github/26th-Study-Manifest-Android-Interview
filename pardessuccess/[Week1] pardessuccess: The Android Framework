## Android 아키텍처

- Linux 커널
- 하드웨어 추상화 계층
- Android Runtime 및 핵심 라이브러리
- 네이티브 C/C++ 라이브러리
- Android 프레임워크
- 애플리케이션

## Intent
Activity, Service, BroadcastReceiver 간의 통신을 가능하게 해주는 메시징 객체, 액티비티 시작, 브로드캐스트 전송, 서비스 시작 등에 사용

### 명시적 Intent
호출하려는 컴포넌트를 직접 명시하여 실행, 특정 Activity, Service 시작
### 암시적 Intent
특정 컴포넌트를 지정하지 않고, 일반적인 작업을 정의, 시스템은 작업, 카테고리, 데이터 등을 기반으로 어떤 컴포넌트가 이 Intent를 처리할 수 있는지 판단
### Intent Filter
앱 컴포넌트가 어떤 종류의 Intent를 처리할 수 있는지를 정의하는 역할을 함.
AndroidManifest.xml에 선언되며, action, category, data 타입 등을 조합하여 특정 Intent 에 반응하도록 구성
- **Explicit Intent (명시적 인텐트)**는 **대상 컴포넌트(예: 특정 Activity, Service 등)를 명확하게 지정**합니다.
- **Implicit Intent (암시적 인텐트)**는 **수행할 작업의 의도(Action, Data 등)를 전달**할 뿐, **구체적인 컴포넌트를 지정하지 않습니다**.

## PendingIntent
현재 앱이 아닌 다른 앱이나 시스템 컴포넌트가 나중에 정의된 Intent를 대신 실행할 수 있도록 권한을 위임하는 객체. 앱이 생명주기를 벗어난 시점에도 작업이 수행되도록 할 수 있게 해준다.
PendingIntent.getActivity(), getService(), getBroadcast()

### 주요 플래그
- FLAG_UPDATE_CURRENT: 기존 Intent를 새 데이터로 갱신
- FLAG_CANCEL_CURRENT: 기존 PendingIntent를 취소하고 새로 생성
- FLAG_IMMUTABLE: 수신자가 Intent 내용을 수정하지 못하게 함
- FLAG_ONE_SHOT: 한 번만 사용할 수 있도록 설정

### 2. Intent와의 차이점

| 항목 | Intent | PendingIntent |
| --- | --- | --- |
| 실행 주체 | 현재 앱이 직접 실행 | 다른 앱이나 시스템이 대신 실행 |
| 실행 시점 | 즉시 실행 | 나중에 실행될 수 있음 (예약, 알람 등) |
| 권한 | 현재 앱 권한으로만 실행 | **앱의 권한을 위임받아** 실행 가능 |
| 용도 | 컴포넌트 직접 호출 | 시스템/타 앱에 Intent 실행을 위임 |

## Serializable & Parcelable
### Serializable
- Java 표준 인터페이스
- 리플렉션 기반으로 객체의 필드를 런타임에 탐색하여 직렬화함
- 성능이 느림, 많은 임시 객체 생성으로 메모리 오버헤드가 큼
- 용도: 성능이 크게 중요하지 않거나, Android 외 일반 Java환경에서도 재사용이 필요한 경우

### Parcelable
- Android 전용 인터페이스, IPC 성능을 위해 최적화됨
- 리플렉션을 사용하지 않음. 빠르고 가비지 생성을 최소화
- 용도: Activity 간 데이터 전달 등 성능이 중요한 Android 환경에 적합

| 항목 | Serializable | Parcelable |
| --- | --- | --- |
| 종류 | Java 표준 | Android 전용 |
| 성능 | 느림 | 빠름 |
| 가비지 생성 | 많음 | 적음 |
| 사용처 | 단순, 범용 | Android IPC, 성능 중요 시 |

다만, **Parcel은 범용 직렬화 도구가 아니며, 영속적인 저장 용도로 사용해서는 안 됩니다.**

왜냐하면 Parcel의 내부 구현은 향후 변경될 수 있으며, 이로 인해 이전 버전의 데이터를 읽을 수 없게 될 수 있기 때문입니다.

### Context
애플리케이션의 상태나 환경을 나타내며, 앱의 리소스 및 클래스에 접근할 수 있게 해주는 역할을 한다. Context는 안드로이드 시스템과 애플리케이션 컴포넌트 사이의 다리 역할을 하며, 리소스 접근, 데이터베이스 연결, 시스템 서비스 이용 등 다양한 작업을 가능하게 한다.
Activity를 시작하거나 레이아웃을 인플레이트하고, 시스템 리소스에 접근하는 대부분의 작업에 Context가 필요하다.
### Application Context
애플리케이션 전체 생명주기에 걸쳐 존재하며, 현재의 Activity나 Fragment에 종속되지 않는다.
**사용 예시**
- 앱 전역에서 사용하는 데이터베이스나 SharedPreferences 접근
- 앱 전체에 걸쳐 동작해야 하는 BroadcastReceiver 등록
- 앱 초기화 시 사용하는 라이브러리나 컴포넌트 설정
### Activity Context
특정 Activity의 생명주기에 연결된 Context이며, UI와 밀접한 작업에 사용된다.
**사용 예시**
- 버튼, 다이얼로그 등 UI 생성 및 변경
- 다른 Activity 시작
- 현재 Activity에 종속된 리소스나 테마 접근
### Service Context
- Service 컴포넌트의 생명주기에 연결된 Context이다. 백그라운드 작업에 주로 사용된다.
**사용 예시**
- 음악 재생, 네트워크 통신 등 비동기 작업
- 시스템 서비스 접근
### Broadcast Context
- BroadcastReceiver가 호출될 때 제공되는 Context이며, 수명이 매우 짧다.
**사용 예시**
- 알림 표시, 간단한 로직 실행
- 긴 작업은 별도의 스레드나 WorkManager로 위임 필요
### Context의 일반적인 활용 예시
1. **리소스 접근**
    - `getString()`, `getDrawable()` 등을 통해 문자열, 이미지, 치수 등 리소스 획득
2. **레이아웃 인플레이트**
    - XML 레이아웃을 View 객체로 변환할 때 사용
3. **Activity/Service 시작**
    - `startActivity()`, `startService()` 등
4. **시스템 서비스 접근**
    - `getSystemService()`를 통해 ClipboardManager, ConnectivityManager 등 획득
5. **데이터 저장소 접근**
    - SharedPreferences, SQLite 등 영속적인 데이터 저장
### ContextWrapper란 무엇인가?

ContextWrapper는 Android에서 사용되는 기본 클래스이며, Context 객체를 감쌀 수 있도록 설계되어 있다. 이 클래스는 원래의 Context에 대한 호출을 위임하면서, 그 중간에서 동작을 수정하거나 확장할 수 있게 해준다. ContextWrapper를 사용하면, 원래의 Context를 변경하지 않고 기능을 커스터마이징할 수 있다.

### 주요 장점
- **재사용성**: 커스텀 로직을 Wrapper 클래스에 캡슐화하여 여러 컴포넌트에서 재사용할 수 있다.
- **캡슐화**: 원래의 Context 구현을 변경하지 않고 동작을 확장하거나 수정할 수 있다.
- **호환성**: 기존 Context 객체와 원활하게 작동하며, 하위 호환성을 유지할 수 있다.

### Activity에서 this와 baseContext의 차이점은?
Activity에서 this는 현재 Activity 인스턴스를 가리키며, 생명주기 및 UI 기능이 포함된 고수준의 Context를 제공한다. 반면 baseContext는 Activity가 기반하고 있는 저수준 Context이며, 커스텀 ContextWrapper와 같은 고급 기능 구현 시 유용하다. 일반적인 Android 개발에서는 this를 더 자주 사용하지만, baseContext에 대한 이해는 디버깅과 모듈화된 컴포넌트 구현에 도움이 된다.

---
## Application 클래스
전역 애플리케이션 상태와 생명주기를 유지하기 위한 기본 클래스 역할을 한다. 이는 애플리케이션의 진입점(entry point)으로 동작하며, Activity, Service, Broadcast Receiver 같은 다른 컴포넌트보다 먼저 초기화된다. Application 클래스는 애플리케이션 전체 생명주기 동안 사용할 수 있는 Context를 제공하므로, 공유 리소스를 초기화하는 데 적합하다.
### Application 클래스의 활용 사례
1. **전역 리소스 관리**: 데이터베이스, SharedPreferences, 네트워크 클라이언트 등 리소스를 한 번만 설정하고 전체에서 재사용
2. **컴포넌트 초기화**: Firebase Analytics, Timber 등 다양한 도구를 앱 시작 시 설정하여 전체 생명주기 동안 사용할 수 있게 함
3. **의존성 주입**: Dagger, Hilt 등의 프레임워크를 초기화하여 앱 전반에 걸쳐 의존성 주입을 적용

---

## AndroidManifest
AndroidManifest.xml 파일은 Android 프로젝트에서 필수적인 구성 파일로, 운영체제에 애플리케이션에 대한 중요한 정보를 전달한다. 이 파일은 애플리케이션과 운영체제 사이의 다리 역할을 하며, 컴포넌트, 권한, 하드웨어/소프트웨어 요구사항 등을 명시한다.

### AndroidManifest.xml의 주요 기능

1. **애플리케이션 컴포넌트 등록**: Activity, Service, BroadcastReceiver, ContentProvider 같은 필수 컴포넌트를 등록하여 시스템이 이들을 인식하고 실행하거나 상호작용할 수 있게 한다.
2. **권한 선언**: 애플리케이션이 접근하려는 권한을 선언한다. 예를 들어, `INTERNET`, `ACCESS_FINE_LOCATION`, `READ_CONTACTS` 등과 같이, 앱이 접근하려는 자원에 대해 사용자에게 설명하고 허용받도록 한다.
3. **하드웨어 및 소프트웨어 요구사항 명시**: 카메라, GPS, 특정 화면 크기 등의 기능 의존성을 정의하여, Play 스토어에서 해당 기능을 지원하지 않는 기기를 필터링할 수 있게 한다.
4. **애플리케이션 메타데이터 제공**: 패키지 이름, 버전, 최소/목표 API 레벨, 테마 및 스타일 등 설치 및 실행 시 필요한 정보를 제공한다.
5. **Intent 필터 정의**: 특정 컴포넌트가 어떤 종류의 Intent에 반응할 수 있는지를 지정한다. 예: 링크 열기, 콘텐츠 공유 등. 이를 통해 다른 앱과 상호작용할 수 있다.
6. **앱 구성 및 설정**: 앱의 메인 런처 액티비티 설정, 백업 동작 구성, 테마 지정 등 애플리케이션의 동작 방식과 표현을 제어할 수 있다.

## Activity 생명주기
1. **onCreate()**: Activity가 처음 생성될 때 호출되는 첫 번째 메서드이다. 여기서는 Activity를 초기화하고, UI 컴포넌트를 설정하며, 저장된 인스턴스 상태를 복원한다. 이 메서드는 Activity 생명주기 동안 단 한 번만 호출되며, Activity가 완전히 파괴되어 재생성될 경우 다시 호출된다.
2. **onStart()**: Activity가 사용자에게 **보이지만 아직 상호작용은 불가능한** 상태가 된다. onCreate() 이후, onResume() 이전에 호출된다.
3. **onRestart()**: Activity가 **중지되었다가 다시 시작될 때** 호출된다. 예를 들어, 사용자가 다른 Activity에서 다시 돌아올 경우, onStart() 전에 호출된다.
4. **onResume()**: Activity가 전면에 나타나고, 사용자와 **상호작용이 가능한** 상태가 된다. UI 업데이트, 애니메이션 재개, 입력 리스너 복구 등의 작업을 여기에 수행한다.
5. **onPause()**: Activity가 **다른 Activity나 다이얼로그 등에 의해 가려질 때** 호출된다. 이때 Activity는 여전히 보이지만 포커스를 잃은 상태이다. 애니메이션, 센서 업데이트 등을 일시 중지하거나 데이터를 저장하는 데 적합하다.
6. **onStop()**: Activity가 더 이상 **사용자에게 보이지 않게 될 때** 호출된다. 예: 다른 Activity가 전면에 나타난 경우. 백그라운드 작업이나 무거운 리소스를 해제하는 데 사용된다.
7. **onDestroy()**: Activity가 완전히 **종료되고 메모리에서 제거되기 전**에 호출된다. 모든 남아 있는 리소스를 정리하는 최종 정리 단계이다.

### 여러 Activity 간 생명주기 변화

Activity 생명주기에 대한 흔한 후속 질문은 다음과 같다:

**"Activity A를 실행한 뒤, Activity B를 실행하고, 다시 Activity A로 돌아올 때 발생하는 생명주기 전환을 설명할 수 있는가?"**

이 질문은 Android 시스템이 여러 Activity 상태를 어떻게 관리하는지를 이해하고 있는지를 평가하기 위함이다.

### Activity A와 B의 전체 순차 생명주기 흐름:

1. **Activity A 최초 실행**
    - 호출 순서: `onCreate()` → `onStart()` → `onResume()`
    - 사용자와 상호작용이 가능한 상태
2. **Activity A에서 B로 전환**
    - Activity A: `onPause()` 호출 → UI를 일시 중지하고 시각적 리소스를 해제
    - Activity B: `onCreate()` → `onStart()` → `onResume()` → 포커스를 가져옴
    - Activity A: `onStop()` 호출 (Activity B가 완전히 A를 가릴 경우)
3. **Activity B에서 다시 A로 돌아옴**
    - Activity B: `onPause()` 호출
    - Activity A: `onRestart()` → `onStart()` → `onResume()` 호출 → 다시 전면으로 복귀
    - Activity B: `onStop()` → `onDestroy()` 호출 → 메모리에서 제거
### Fragment 생명주기 흐름
1. **onAttach()**
    
    Fragment가 자신의 부모 Activity에 연결되었을 때 호출되는 첫 번째 콜백이다. 이 시점에서 Fragment는 Activity Context에 접근할 수 있다.
    
2. **onCreate()**
    
    Fragment가 초기화될 때 호출된다. 아직 UI는 생성되지 않았으며, 이 단계에서는 주요 구성요소를 초기화하거나 저장된 상태를 복원하는 작업이 이루어진다.
    
3. **onCreateView()**
    
    Fragment의 UI가 처음으로 그려질 때 호출된다. 이 메서드에서는 Fragment의 레이아웃을 `LayoutInflater`를 사용해 인플레이트하고, 루트 뷰를 반환해야 한다.
    
4. **onViewStateRestored()**
    
    Fragment의 뷰 계층 구조가 생성되고 나서, 저장된 상태가 뷰에 복원된 이후에 호출된다.
    
5. **onViewCreated()**
    
    Fragment의 뷰가 생성된 직후 호출된다. 보통 이 메서드 안에서 UI 컴포넌트를 설정하거나 사용자 인터랙션을 처리하는 로직을 구현한다.
    
6. **onStart()**
    
    Fragment가 사용자에게 보이게 되는 시점이다. Activity의 `onStart()`에 대응되며, 아직 전면 인터랙션은 불가능하다.
    
7. **onResume()**
    
    Fragment가 전면에서 완전히 활성화되어 사용자와 상호작용이 가능한 상태가 된다. UI가 완전히 보이고, 사용자 입력을 받을 수 있다.
    
8. **onPause()**
    
    Fragment가 전면이 아니게 되었지만 여전히 화면에 보이는 경우 호출된다. 포커스를 잃기 직전이며, 이 시점에는 계속해서 수행되면 안 되는 작업을 일시 중단해야 한다.
    
9. **onStop()**
    
    Fragment가 사용자에게 더 이상 보이지 않게 되었을 때 호출된다. 이 메서드에서는 화면 밖에서 계속될 필요가 없는 작업을 중단해야 한다.
    
10. **onSaveInstanceState()**
    
    Fragment가 파괴되기 전에 UI 상태 데이터를 저장할 때 호출된다. 나중에 복원할 수 있도록 상태를 저장한다.
11. **onDestroyView()**
    Fragment의 뷰 계층이 제거될 때 호출된다. 이 시점에서는 어댑터 제거, 뷰 참조 제거 등과 같이 뷰 관련 리소스를 정리해야 한다.
    
12. **onDestroy()**
    Fragment 자체가 파괴될 때 호출된다. 이 단계에서는 모든 리소스를 정리해야 하지만, Fragment는 여전히 부모 Activity에 연결된 상태일 수 있다.
    
13. **onDetach()**
    Fragment가 부모 Activity로부터 분리될 때 호출된다. 이 단계가 끝나면 Fragment 생명주기는 종료된다.
    
fragmentManager와 childFragmentManager의 선택은 UI의 계층 구조에 따라 달라진다. Activity에서 전체 UI를 구성할 경우 fragmentManager를 사용하고, Fragment 내부에서 독립적인 구조가 필요할 경우 childFragmentManager를 사용한다. 각 범위와 생명주기를 명확히 이해함으로써 Android 애플리케이션의 구조를 더 잘 모듈화할 수 있다.

### Service란 무엇인가?

Service는 사용자 인터페이스와 독립적으로 장시간 실행되는 작업을 수행할 수 있게 해주는 안드로이드의 백그라운드 컴포넌트이다.
### 1. Started Service (시작된 서비스)

Started Service는 애플리케이션 컴포넌트가 `startService()`를 호출할 때 시작된다. 이 서비스는 명시적으로 `stopSelf()` 또는 `stopService()`를 호출할 때까지 백그라운드에서 계속 실행된다.
**예시 사용처**
- 백그라운드 음악 재생
- 파일 업로드 및 다운로드

### 2. Bound Service (바인드된 서비스)

Bound Service는 다른 컴포넌트가 `bindService()`를 통해 서비스에 바인딩할 때 활성화된다. 하나 이상의 클라이언트가 바인딩된 동안 서비스는 활성 상태이며, 모든 클라이언트가 연결을 해제하면 자동으로 종료된다.

**예시 사용처**

- 원격 서버에서 데이터 수집
- 백그라운드 블루투스 연결 관리

### 3. Foreground Service (포그라운드 서비스)

Foreground Service는 지속적인 알림을 표시하면서 실행되는 특수한 형태의 서비스이다. 음악 재생, 내비게이션, 위치 추적 등 사용자가 항상 인지하고 있어야 하는 작업에 적합하다.

### Service vs Foreground Service의 주요 차이점

1. **사용자 인식 가능성(User Awareness)**
    
    일반 서비스는 백그라운드에서 사용자에게 보이지 않게 실행될 수 있지만, 포그라운드 서비스는 반드시 알림을 통해 사용자에게 표시되어야 한다.
    
2. **우선순위(Priority)**
    
    포그라운드 서비스는 시스템 메모리가 부족할 때 일반 서비스보다 종료 우선순위가 낮다. 즉, 잘 종료되지 않음.
    
3. **사용 사례**
    
    일반 서비스는 가벼운 백그라운드 작업에 적합하고, 포그라운드 서비스는 장기 실행되며 사용자에게 인식되어야 하는 작업에 적합하다.
    

---

### Service 사용 시 모범 사례

1. 작업이 끝나면 서비스를 반드시 종료하여 시스템 자원을 절약한다.
2. 즉각 실행이 필요 없는 백그라운드 작업은 WorkManager를 사용하는 것이 더 적절하다.
3. 포그라운드 서비스를 사용할 때는 사용자에게 친절하고 명확한 알림을 제공해 투명성을 유지해야 한다.

---

### BroadcastReceiver란 무엇인가?

**BroadcastReceiver**는 앱이 **시스템 전역 브로드캐스트 메시지** 또는 **앱 내부 브로드캐스트**를 수신하고 이에 응답할 수 있도록 해주는 컴포넌트다. 이 브로드캐스트는 시스템이나 다른 앱이 다양한 이벤트를 알릴 때 전송되며, 예를 들어 배터리 상태 변화, 네트워크 연결 상태 변경, 또는 앱 내부에서 정의한 커스텀 인텐트 등이 있다.

BroadcastReceiver는 시스템 또는 앱 수준의 동적 이벤트에 반응할 수 있는 **반응형 애플리케이션**을 구성하는 데 유용하다.

### 브로드캐스트의 종류

1. **System Broadcasts (시스템 브로드캐스트)**
    
    Android 운영체제가 시스템 이벤트를 앱에 전달하기 위해 전송한다. 예: 배터리 부족, 시간대 변경, 네트워크 연결 상태 변화 등
    
2. **Custom Broadcasts (커스텀 브로드캐스트)**
    
    앱이 특정 이벤트나 데이터를 앱 내부 또는 다른 앱과 통신하기 위해 직접 전송하는 브로드캐스트
    
## ContentProvider
**ContentProvider**는 구조화된 데이터를 관리하고, **앱 간에 데이터를 공유할 수 있도록 표준화된 인터페이스**를 제공하는 컴포넌트이다. ContentProvider는 앱 또는 외부 앱이 데이터를 **조회(query)**, **삽입(insert)**, **수정(update)**, **삭제(delete)**할 수 있게 해주는 중앙 저장소 역할을 하며, **데이터의 일관성과 보안**을 유지한다.

특히 여러 앱이 동일한 데이터에 접근할 필요가 있을 때, 또는 앱 내부의 DB나 파일 시스템 구조를 외부에 노출하지 않고 데이터를 제공하고자 할 때 유용하다.

ContentProvider의 주 목적은 데이터 접근 로직을 캡슐화하여, 앱 간 데이터 공유를 더 쉽고 안전하게 만드는 것이다. SQLite 데이터베이스, 파일 시스템, 네트워크 기반 데이터 등 다양한 소스를 추상화하고, 이들에 접근하기 위한 통일된 API를 제공한다.

### ContentProvider의 핵심 구성 요소

ContentProvider는 데이터를 접근할 수 있는 **URI (Uniform Resource Identifier)**를 사용한다. URI는 다음과 같이 구성된다:

1. **Authority**: ContentProvider를 식별하는 고유 문자열 (예: `com.example.myapp.provider`)
2. **Path**: 데이터의 유형 또는 테이블 (예: `/users`, `/products`)
3. **ID (선택사항)**: 특정 항목을 가리키는 식별자 (예: `/users/42`)

### ContentProvider의 사용 사례

- 앱 간 데이터 공유 (예: 연락처, 미디어 파일)
- 앱 실행 시 초기 리소스나 설정 초기화
- 구조화된 데이터(예: DB)의 외부 접근 제공
- Android 시스템 기능(예: 연락처 앱, 파일 선택기 등)과 통합
- 데이터 접근 권한을 세밀하게 제어

## 구성 변경 (Configure Changes)
구성 변경을 올바르게 처리하는 것은 화면 회전, 언어 변경, 다크/라이트 모드 전환, 글꼴 크기 조정 등의 이벤트 중에도 끊김 없는 사용자 경험을 유지하는 데 매우 중요하다. 안드로이드 시스템은 이러한 변경이 발생하면 기본적으로 Activity를 종료 후 재생성하며, 이로 인해 일시적인 UI 상태가 손실될 수 있다.

### 1. onSaveInstanceState(), onREstoreInstanceState()
### 2. Jetpack ViewModel 
### 3. 구성 변경 수동 처리 (android:configChanges)
### 4. Jetpack Compose에서 rememberSaveable
### 추가 팁
- **Navigation 구성 유지**: Jetpack Navigation을 사용하면 구성 변경에도 back stack이 유지되어 화면 이동 이력을 잃지 않는다.
- **구성에 의존하는 데이터 지양**: 텍스트 크기나 방향에 따라 값이 달라지는 데이터를 직접 UI에 보관하기보다는 ViewModel 등을 통해 보존하는 것이 바람직하다.

## 안드로이드 메모리 누수
안드로이드는 **가비지 컬렉션(Garbage Collection)** 메커니즘을 통해 메모리를 관리한다. 이는 사용되지 않는 메모리를 자동으로 회수하여, 앱과 서비스에 필요한 메모리가 효율적으로 할당되도록 한다.

안드로이드의 런타임 환경(Dalvik 또는 ART)은 메모리 사용을 모니터링하면서 참조되지 않는 객체를 정리하며, 메모리 과다 소비를 방지한다. 시스템 메모리가 부족할 경우, **Low-Memory Killer**가 백그라운드 프로세스를 종료시켜 포그라운드 앱의 안정적인 동작을 보장한다.

### 메모리 누수(Memory Leak)란?

메모리 누수란 앱이 **더 이상 필요하지 않은 객체에 대한 참조를 계속 유지**하고 있어, 가비지 컬렉터가 메모리를 회수하지 못하는 상황을 말한다. 이는 **메모리 부족, 성능 저하, 앱 강제 종료**로 이어질 수 있다.

### 메모리 누수 방지를 위한 모범 사례

1. **Lifecycle-Aware 컴포넌트 사용**
    - ViewModel, LiveData, Coroutine의 `collectAsStateWithLifecycle()` 등을 사용하여 컴포넌트 생명주기에 맞게 자동으로 자원을 해제하도록 한다.
2. **Context 참조 피하기**
    - 장시간 유지되는 객체(예: 싱글톤, static 변수)에 Activity나 Context를 참조하지 말 것. 필요한 경우 `applicationContext`를 사용한다.
3. **리스너/콜백 해제**
    - BroadcastReceiver, View.OnClickListener, LiveData Observer 등은 반드시 `onPause()` 또는 `onStop()` 등 적절한 시점에 `unregister` 또는 `removeObserver()`로 해제해야 한다.
4. **WeakReference 사용**
    - 꼭 강한 참조가 필요하지 않은 객체는 `WeakReference`를 사용해 GC의 회수를 허용한다.
5. **툴 사용: LeakCanary**
    - LeakCanary는 메모리 누수를 자동으로 감지하고 추적해주는 오픈소스 도구로, 객체가 왜 메모리에서 해제되지 않았는지를 시각적으로 보여준다.
    - Android Studio의 **Memory Profiler**를 함께 사용하면 할당량, GC 타이밍 등을 시각적으로 확인 가능하다.
6. **View의 static 참조 금지**
    - View는 절대 static 변수에 저장해서는 안 된다. 이는 Activity Context를 간접적으로 참조하게 되어 메모리 누수로 이어진다.
7. **리소스 명시적 해제**
    - File, Cursor, InputStream 등은 사용 후 `close()`를 반드시 호출한다.
8. **Fragment & Activity 간 순환 참조 주의**
    - Fragment를 ViewPager 등에서 사용할 경우, `onDestroyView()` 또는 `onDetach()`에서 자원을 명시적으로 해제해야 한다.
    
## ANR(Application Not Responding)

ANR은 "Application Not Responding"의 약자로, **안드로이드 앱의 메인 스레드(UI 스레드)가 너무 오랫동안 차단**되었을 때 발생한다. 보통 5초 이상 응답이 없으면 시스템은 **ANR 대화상자**를 띄워 사용자에게 앱 종료 또는 대기 여부를 묻는다.

ANR은 사용자 경험을 심각하게 저하시킬 수 있으며, 아래와 같은 상황에서 발생할 수 있다:

- 메인 스레드에서 5초 이상 걸리는 **무거운 연산**
- **오래 걸리는 네트워크** 또는 **데이터베이스 작업**
- UI 스레드에서 실행되는 **차단(blocking) 연산** (예: 동기식 요청)

### ANR을 방지하는 방법
ANR을 방지하려면 메인(UI) 스레드를 **항상 응답 가능한 상태**로 유지해야 하며, 연산이 오래 걸리는 작업은 백그라운드로 옮겨야 한다.

### 1. 무거운 작업은 반드시 메인 스레드에서 분리

- 파일 입출력, 네트워크 요청, DB 쿼리 등의 작업은 `AsyncTask`, `Executors`, `Thread` 등을 사용해 백그라운드에서 처리해야 한다.
- **권장 방법**: Kotlin Coroutine을 사용해 `Dispatchers.IO`에서 처리하도록 한다.
### 2. 지속적인 백그라운드 작업에는 WorkManager 사용

- 데이터 동기화처럼 반드시 수행되어야 하는 작업은 `WorkManager`를 통해 예약 및 실행한다.
- WorkManager는 시스템 상태에 따라 **지속적이고 안정적인 실행**을 보장한다.
### 3. 대용량 데이터는 Paging으로 나눠서 처리

- RecyclerView 등에 대량의 데이터를 한 번에 로딩하지 말고, `Paging` 라이브러리를 통해 **부분적 로딩**을 구현한다.
### 4. 구성 변경 시 UI 작업 최소화
- 화면 회전 등의 이벤트 발생 시 UI를 재로드하지 않도록 `ViewModel`을 통해 상태를 유지한다.
- `rememberSaveable`도 좋은 선택지다.

## Deep Link
딥 링크는 외부 소스(예: URL, 알림, 이메일 등)로부터 앱의 특정 화면이나 기능으로 **직접 이동**할 수 있도록 해주는 메커니즘이다. 이를 통해 사용자 경험을 개선하고 앱 내 특정 콘텐츠로 빠르게 접근하게 할 수 있다.

딥 링크를 처리하려면 **AndroidManifest.xml에 정의**하고, **해당 Activity에서 인텐트를 처리**해야 한다.

## Task, Back Stack
### Task란?

Task는 일반적으로 사용자가 앱 런처에서 Activity를 실행할 때 생성된다. 하나의 Task는 여러 앱에 걸쳐 존재할 수 있으며, Intent와 LaunchMode 설정에 따라 Task 간 Activity 이동이 가능하다.

예를 들어, 이메일 앱에서 링크를 클릭해 브라우저를 열면, 해당 브라우저 Activity는 같은 Task 안에 포함될 수도 있다. Task는 포함된 모든 Activity가 종료될 때까지 **유지된다**.

### Back Stack이란?

Back Stack은 Task 내 Activity들의 실행 이력을 유지한다. 사용자가 새 Activity로 이동하면 현재 Activity는 Stack에 푸시되고, 새 Activity가 최상단에 위치한다.

사용자가 **뒤로가기 버튼**을 누르면 가장 위의 Activity가 제거되고, 그 아래에 있던 Activity가 다시 보여진다. 이러한 방식은 사용자에게 **직관적인 내비게이션 흐름과 연속성 있는 경험**을 제공한다.

## Task와 Back Stack의 동작에 영향을 주는 요소들

Android에서 Task와 Back Stack의 동작은 다음 두 가지 요소에 의해 결정된다:

1. **Launch Mode (런치 모드)**
2. **Intent Flags (인텐트 플래그)**

---

### Launch Mode의 종류

Launch Mode는 Activity가 어떻게 인스턴스화되고 Back Stack에 추가되는지를 결정한다. Android에는 다음과 같은 네 가지 주요 Launch Mode가 있다:

1. **standard**
    - 기본값. 호출될 때마다 **새 Activity 인스턴스**가 생성되고 Back Stack에 추가된다.
    - 동일한 Activity가 여러 번 생성될 수 있다.
2. **singleTop**
    - Back Stack 최상단에 해당 Activity가 이미 존재하면, **새 인스턴스를 생성하지 않고 onNewIntent()로 처리**한다.
    - 그렇지 않으면 새로 생성된다.
3. **singleTask**
    - Task 내에서 해당 Activity 인스턴스가 **하나만 존재**한다.
    - 이미 존재하면 **해당 인스턴스를 최상단으로 가져오고**, 그 위의 Activity들은 **모두 제거된다**.
    - 일반적으로 앱의 **메인 진입점 Activity**에 적합하다.
4. **singleInstance**
    - `singleTask`와 유사하나, **해당 Activity는 독립적인 Task에 배치**된다.
    - 이 Task에는 다른 Activity가 함께 들어올 수 없다.
    - 보안상 고립된 환경이 필요한 경우에 적합하다.
### Intent Flag의 종류

Intent Flag는 런타임에서 Intent에 설정되어 Activity 실행 방식과 Back Stack 동작을 제어하는 플래그이다.

대표적인 플래그는 다음과 같다:

- `FLAG_ACTIVITY_NEW_TASK`
    - 새 Task에서 Activity를 시작하거나, 이미 존재하는 Task를 앞으로 가져온다.
- `FLAG_ACTIVITY_CLEAR_TOP`
    - Back Stack에 해당 Activity가 존재하면, **그 위의 모든 Activity를 제거**하고 해당 인스턴스를 재사용하며 `onNewIntent()` 호출.
- `FLAG_ACTIVITY_SINGLE_TOP`
    - `singleTop` Launch Mode와 유사. 최상단에 있을 경우 재사용한다.
- `FLAG_ACTIVITY_NO_HISTORY`
    - 실행된 Activity가 Back Stack에 추가되지 않는다. 뒤로가기 시 바로 사라진다.
### 요약

**Task**와 **Back Stack**은 안드로이드의 내비게이션 모델의 핵심이다.

- **Task**는 목적 중심의 Activity 모음이며
- **Back Stack**은 그 실행 이력을 LIFO 방식으로 유지한다.

`Launch Mode`와 `Intent Flag`를 적절히 설정하면, 복잡한 내비게이션 흐름에서도 **일관성 있고 예측 가능한 사용자 경험**을 제공할 수 있다.

### Bundle
Bundle은 Android에서 컴포넌트 간 데이터를 전달할 때 사용하는 키-값 쌍의 자료 구조이다. 이는 Activity, Fragment, Service 등 다양한 컴포넌트 간에 소량의 데이터를 효율적으로 전달할 수 있도록 설계되었다. Bundle은 가볍고 직렬화가 가능하여, Android 운영체제가 데이터를 저장하고 전달하는 데 적합하다.

### Bundle의 일반적인 사용 사례
- Activity 간 데이터 전달
- Fragment 간 데이터 전달
- UI 상태 저장 및 복원
- Service로 데이터 전달

### Activity 또는 Fragment 간 데이터를 어떻게 전달하나요?
### Activity 간 데이터 전달
Activity 간 데이터 전달에는 보통 **Intent**를 사용한다. 데이터를 `putExtra()`로 추가하고, 수신 Activity에서는 `getIntent()`로 값을 꺼낸다.
### Fragment 간 데이터 전달 (Bundle 사용)
Fragment 간에는 보통 **Bundle**을 사용하여 데이터를 전달한다. 전달 Fragment에서 `arguments`에 Bundle을 설정하고, 수신 Fragment에서 `getArguments()`로 값을 읽는다.
### Jetpack Navigation + Safe Args 사용

Jetpack Navigation을 사용할 경우 **Safe Args 플러그인**을 적용하면 **타입 안전한 데이터 전달**이 가능하다.
### Shared ViewModel 사용 (Fragment 간 통신)

같은 Activity 내 여러 Fragment가 데이터를 공유해야 한다면 **Shared ViewModel**이 적합하다. `activityViewModels()`를 통해 ViewModel 인스턴스를 공유할 수 있다.

### ActivityManager란 무엇인가요?

**ActivityManager**는 Android의 시스템 서비스로, **디바이스에서 실행 중인 Activity, Task, 프로세스** 등의 정보를 제공하고 이를 관리할 수 있도록 한다. Android 프레임워크의 일부로, 개발자가 앱의 생명주기, 메모리 사용량, 작업(Task) 상태 등을 제어할 수 있게 한다.
### SparseArray를 사용하는 장점은 무엇인가요?

**SparseArray**는 `android.util` 패키지에 포함된 Android 전용 자료구조로, **정수형 키(int key)** 를 **객체 값(object value)** 에 매핑할 수 있는 구조이다. 이는 `HashMap<Integer, Object>`와 비슷한 기능을 하지만, **메모리 효율성이 뛰어나고 Android에 최적화**되어 있다.

### Looper, Handler, HandlerThread의 역할은 무엇인가요?

**Looper**, **Handler**, **HandlerThread**는 **스레드 관리와 비동기 작업 처리**를 위해 함께 작동하는 Android의 핵심 컴포넌트들이다.

이들은 **백그라운드 스레드에서 작업을 수행하면서**, UI 업데이트 등은 **메인 스레드와 안전하게 통신**할 수 있도록 해준다.

### Build Variant와 Product Flavor

- **Build Type**: 앱을 어떻게 빌드할지 정의 (예: Debug vs Release)
- **Product Flavor**: 앱의 어떤 버전을 만들지 정의 (예: 무료 vs 유료)
- **Build Variant**: 이 둘의 조합으로, 앱의 다양한 버전을 생성할 수 있도록 한다.

### Android Runtime (ART), Dalvik, Dex Compiler란 무엇인가요?

| 구분 | Dalvik | ART |
| --- | --- | --- |
| 컴파일 방식 | JIT (실행 중 컴파일) | AOT (설치 시 컴파일) |
| 실행 속도 | 느림 (매번 컴파일) | 빠름 (기계어 사전 생성) |
| CPU 사용량 | 높음 | 낮음 |
| 앱 시작 시간 | 지연 | 빠름 |
| 메모리 관리 | 기본 GC | 향상된 GC |
| 디버깅/분석 | 제한적 | 향상된 프로파일링 지원 |

- **ART**는 AOT 컴파일로 더 빠르고 효율적인 앱 실행 환경을 제공
- **Dalvik**은 JIT 기반으로 동작하나, 성능 한계로 인해 대체됨
- **Dex Compiler**는 Java/Kotlin 코드를 `.dex` 파일로 변환하여 ART 또는 Dalvik에서 실행 가능하게 만듦

### APK 파일과 AAB 파일의 차이점은 무엇인가요?
### APK (Android Package)

APK는 전통적인 앱 배포 형식으로, **설치 가능한 완전한 패키지**입니다.

- **모든 리소스와 코드, 메타데이터가 하나의 파일로 포함**되어 있음
- **기기 구성(screen density, CPU 아키텍처, 언어 등)에 관계없이 모든 리소스를 포함**하므로 **파일 크기가 큼**
- Google Play 외에 APK 파일을 직접 공유하거나 **수동 설치(sideloading)** 가능
- 다양한 기기에 맞춰 **개발자가 직접 리소스 구성 관리** 필요
- 일반적으로 테스트, 외부 QA, 배포 전 앱 설치 등에 사용

---

### AAB (Android App Bundle)

AAB는 Google에서 도입한 **퍼블리싱 전용 포맷**입니다.

직접 설치할 수 있는 형식이 아닌, **Google Play에서 처리 후 사용자 기기에 맞춘 APK로 변환되어 설치**됩니다.

- **모듈화된 구조**로 구성되어, 화면 크기, 아키텍처, 언어 등 **기기별 최적화된 APK 생성 가능**
- **실제 다운로드 시에만 필요한 리소스만 포함된 APK를 생성**
    
    → 사용자 입장에서는 설치 용량이 줄어듦
    
- 개발자가 **AAB 파일을 직접 배포할 수는 없고**, sideloading 시에는 `bundletool` 등의 도구 사용 필요
- Google Play가 **APK 생성, 서명, 최적화**를 자동으로 처리

---

| 항목 | APK | AAB |
| --- | --- | --- |
| **형식 목적** | 설치 가능 패키지 | 배포용 퍼블리싱 포맷 |
| **리소스 포함** | 모든 기기용 리소스를 포함 | 기기별 최적화된 리소스만 포함된 APK 생성 |
| **파일 크기** | 비교적 큼 | 사용자에게 전달되는 APK 크기 작음 |
| **배포 방식** | 직접 배포 및 sideloading 가능 | Google Play를 통해 최적화된 APK 생성 및 설치 |
| **설치 가능성** | 바로 설치 가능 | 직접 설치 불가, bundletool 필요 |
| **구성 관리** | 개발자가 수동 관리 | Google Play가 자동 구성 최적화 처리 |
| **호환성** | 모든 스토어 및 기기에서 사용 가능 | Google Play 중심, 타 스토어 미지원 가능성 있음 |

### R8
**R8**은 Android의 **빌드 프로세스에 내장된 코드 축소 및 최적화 도구**입니다.

이전의 **ProGuard**를 대체하며, **APK 또는 AAB 파일의 크기를 줄이고 실행 성능을 향상**시키기 위해 사용됩니다.

---

### R8의 주요 기능

1. **코드 축소 (Code Shrinking)**
    - 사용되지 않는 클래스, 메서드, 필드, 속성을 제거하여 APK/AAB 크기를 줄입니다.
2. **코드 최적화 (Optimization)**
    - 코드 구조를 간소화하고 실행 성능을 향상시키기 위해 메서드 인라이닝, 중복 제거, 불필요한 분기 제거 등을 수행합니다.
3. **난독화 (Obfuscation)**
    - 클래스, 메서드, 필드 이름을 의미 없는 짧은 이름으로 바꿔 리버스 엔지니어링을 어렵게 만듭니다.
4. **리소스 최적화 (Resource Optimization)**
    - 사용되지 않는 이미지, 레이아웃, 문자열 등의 리소스를 제거하여 앱 크기를 더욱 축소합니다.
### R8의 장점

- **빌드 시스템과 긴밀하게 통합**되어 별도 설정 없이 자동 적용 가능
- **ProGuard보다 더 빠르고 효율적** (한 번의 패스로 모든 작업 수행)
- **앱 크기 감소** 및 **실행 성능 향상**
- **보안성 증가** (난독화를 통한 코드 보호)

---

### R8의 단점

- **과도한 축소 위험**: Reflection 등으로 참조되는 코드를 제거할 수 있어 런타임 오류 발생 가능
- **설정 복잡성**: 복잡한 앱일수록 ProGuard 규칙 작성이 어려움
- **디버깅 어려움**: 난독화된 코드로 인해 스택 트레이스 해석이 어렵고 디버깅이 복잡해질 수 있음

### 앱 크기를 줄이는 방법은?
1. 사용되지 않는 리소스 제거
2. R8을 통한 코드 축소
3. 리소스 최적화
4. Android App Bundle(AAB) 활용
5. 불필요한 의존성 제거

### 안드로이드에서 프로세스란 무엇이며 운영체제가 이를 어떻게 관리하는가?
### 프로세스 우선순위 계층

1. **Foreground Process (전경 프로세스)**
    - 사용자와 상호작용 중인 컴포넌트가 포함된 프로세스
    - 가장 높은 우선순위, 시스템이 거의 종료하지 않음
2. **Visible Process (화면 표시 프로세스)**
    - 현재 사용자와 직접 상호작용하지 않지만, 화면에 일부 표시되는 프로세스 (예: 다이얼로그 뒤의 액티비티)
3. **Service Process (서비스 프로세스)**
    - 백그라운드 작업(음악 재생, 데이터 동기화 등)을 수행하는 `Service` 가 실행 중인 프로세스
4. **Cached Process (캐시된 프로세스)**
    - 현재는 사용되지 않지만 빠른 재시작을 위해 메모리에 남아 있는 프로세스
    - 메모리가 부족하면 **가장 먼저 종료 대상**

→ Android는 **낮은 우선순위의 프로세스를 먼저 종료**하여 시스템의 안정성과 성능을 유지합니다

**왜 Activity, Service, BroadcastReceiver, ContentProvider를 안드로이드의 4대 컴포넌트라고 부르나요?**

**Activity**, **Service**, **BroadcastReceiver**, **ContentProvider**는 Android의 **4대 구성요소(Four Major Components)** 로 불립니다.

그 이유는 이 네 가지가 Android 애플리케이션이 **시스템 및 다른 앱과 상호작용할 수 있게 해주는 핵심 구성 블록**이기 때문입니다.

이들은 앱의 **수명주기를 관리**하고, **행동을 정의**하며, **프로세스 간 통신(IPC)** 을 가능하게 함으로써

Android의 앱 실행 및 관리 모델과 **긴밀하게 연결되어 있는 핵심 요소**입니다.
