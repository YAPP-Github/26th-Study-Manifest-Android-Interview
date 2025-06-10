# Category 3: Business Logic

## 🙋🏻 Practical Questions (59 ~ 66)

### Q) 59. How would you manage long-running background tasks?

> Q) Android 앱에서 수백 MB에 달하는 대용량 파일을 원격 서버에서 다운로드하는 기능을 구현해야 합니다. 다운로드는 앱이 종료되더라도 계속되어야 하며, 성능과 네트워크 조건 측면에서도 효율적이어야 합니다. 이 경우 WorkManager, Foreground Service, JobScheduler 중 어떤 백그라운드 실행 메커니즘을 선택하시겠습니까? 그리고 그 이유는 무엇인가요?

`Foreground Service`를 선택한다.  

이유:  

1. **작업 지속성 보장**  
   Foreground Service는 앱이 백그라운드로 전환되거나 종료 되더라도 작업을 계속 실행 할 수 있다.  
   → 따라서 다운로드와 같은 장시간 실행되는 작업에 적합하다.

2. **사용자 인식 및 시스템 우선순위 확보**  
   지속적인 알림을 통해 사용자에게 작업 진행 상황을 명확히 알릴 수 있다.  
   시스템에서 높은 우선순위로 인식되어 작업이 강제 종료될 가능성이 낮다.

3. **대역폭 및 상태 모니터링 용이**
   foreground Service 내에서 네트워크 연결 상태, 속도, 중단 처리 등 복잡한 로직을 자유롭게 구현할 수 있어 대용랼 파일 다운로드 시 안정적이 처리가 가능하다.

---

### Q) 60. How do you serialize Json format to object

> Q) API로부터 받은 JSON 응답을 Kotlin 데이터 클래스에 역직렬화하려면 어떻게 하시겠습니까? Kotlin 중심 프로젝트에서는 어떤 라이브러리를 선택하시겠으며, 그 이유는 무엇인가요?

`kotlin.serialization`을 선택한다.

이유:

- JetBrains에서 공식적으로 지원하며, Kotlin 언어와 가장 깊게 통합되어 있음.
- 빌드타임 코드생성 방식으로 리플렉션 의존 없이 성능 최적화가 가능함.

<br>

> Q) Kotlin 데이터 클래스에 정의되지 않은 필드가 JSON 객체에 포함되어 있거나 일부 필드가 누락된 경우, 이러한 상황을 어떻게 처리하시겠습니까?

- **정의되지 않은 필드가 JSON 객체에 포함되어 있는 경우**  
  
    데이터 클래스에 정의되어 있지 않은 필드는 역직렬화 시 자동으로 건너뛴다.  
    why?   
    → 대부분의 라이브러리(Gson, Moshi, kotlinx.serialization)는 이를 기본적으로 무시하기 때문이다.  
    `kotlinx.serialization`에서는 `ignoreUnknownKeys = true` 설정을 반드시 명시해주는 것이 안전하다고 한다.


- **JSON에 필요한 필드가 누락된 경우**  
  데이터 클래스에 기본값을 설정하여, 누락된 경우 기본값을 사용하는 방법으로 대응한다.

---


### Q) 61. How do you handle network requests to fetch data, and which libraries or techniques do you use for efficiency and reliability?

> Q) 앱에서 여러 API 요청을 동시에 수행하고, 그 결과를 합쳐 UI를 갱신해야 합니다. Retrofit과 코루틴을 사용해 이를 효율적으로 구현하려면 어떻게 해야 하나요?

coroutineScope 내에서 async를 사용해 병렬 요청을 수행한다.  
이후 await를 통해 각 결과를 수신하고, 필요한 방식으로 수신 결과를 겹합하여 UI를 업데이트 한다.


<br> 

> Q) API 요청이 실패할 경우, 재시도 로직을 어떻게 구현하시겠습니까?

API 요청 실패 시, OkHttp의 Interceptor를 활용해서 재시도 로직을 전역으로 처리할 수 있다.

ex) RetryInterceptor를 커스텀으로 구현해서, 네트워크 오류(IOException) 발생 시 최대 N번까지 재시도하게 만들 수 있다고 함.

재시도는 주로 다음 조건에 따라 진행한다:
- IOException 같은 네트워크 오류일 때만 재시도
- 응답 코드가 500번대(서버 오류)인 경우 재시도 고려
- 클라이언트 오류(400번대)나 인증 오류는 바로 실패 처리

재시도 간에 딜레이(backoff)를 둘 수도 있고, exponential backoff 적용도 가능하다.

특정 API에서만 재시도가 필요한 경우, Interceptor 대신 UseCase나 Repository에서 직접 retry 로직을 넣기도 한다고 함.  
ex) flow.retryWhen { ... } 사용해서 조건 맞을 때만 재시도

---


### Q) 64. How do you store and persist data locally?

> Q) 네트워크 API에서 받은 대용량 JSON 응답을 오프라인에서도 접근 가능하도록 저장해야 하는 상황이라면, 어떤 로컬 저장 방식을 선택하겠으며 그 이유는 무엇인가요?

`File Storage` 방식을 선택한다.

이유:  

1. **유연한 데이터 구조 저장**  
   JSON은 구조화된 텍스트 데이터이며, 고정된 스키마를 갖지 않기 때문에 정형 데이터베이스(Room 등)보다는 파일로 직렬화해 저장하는 방식이 적합하다고 함.

2. **대용량 처리에 용이**  
   File Storage는 데이터 크기에 제한이 없고, 순차적으로 읽고 쓰는 데 최적화되어 있어 대형 JSON 파일을 효율적으로 처리할 수 있다고 함.

3. **간단한 구현**  
   JSON 문자열을 그대로 파일로 저장하거나, 필요에 따라 GSON, Moshi 같은 라이브러리로 역직렬화하여 데이터를 재사용할 수 있어 구현이 비교적 간단하다고 함.

4. **오프라인 접근 용이성**  
   내부 저장소에 저장하면 외부 접근 없이 앱 내에서 빠르게 데이터를 읽어들일 수 있어 오프라인 기능 구현에 유리하다고 함.

---


### Q) 65. How do you handle offline-first features?

> Q) 네트워크가 사용 불가능한 상황에서도 원활한 사용자 경험을 제공하기 위해 Android 애플리케이션에서 오프라인 우선 기능을 어떻게 설계하시겠습니까?

다음과 같이 설계한다.  

1.	먼저 Room의 로컬 데이터를 UI에 즉시 보여준다.
2.	네트워크가 가능하면 최신 데이터를 패치하여 Room을 갱신하고, Flow를 통해 UI가 자동으로 업데이트되도록 한다.
3.	유저 입력(예: 글쓰기, 즐겨찾기 등)은 먼저 로컬 DB에 저장하고, WorkManager를 통해 네트워크 연결 시 서버에 전송되도록 합니다.
4.	이 과정에서 네트워크 상태는 ConnectivityManager로 모니터링하여, 오프라인 상태에선 API 요청을 생략하거나 대체 UI를 제공한다.


<br>

> Q) 로컬 Room 데이터베이스의 변경 사항을 원격 서버와 동기화하기 위해 어떤 전략을 사용하시겠으며, 로컬과 원격 데이터가 모두 수정된 경우 충돌을 어떻게 해결하시겠습니까?


Room 엔티티에 isSynced 같은 필드를 추가해서, 로컬에서 변경된 데이터는 플래그를 true로 설정한다.  
이후 WorkManager를 활용해 네트워크 연결 시점에 백그라운드 동기화를 수행한다.  
하지만 충돌이 발생할 수 있기 때문에, 각 데이터에 updatedAt 타임스탬프를 함께 저장해두고, 서버의 타임스탬프와 비교해 가장 최신 데이터를 우선으로 적용하는 Last-Write-Wins 전략을 사용한다.  


만약 동시 수정으로 인해 자동 머지가 어려운 경우, 로컬에 Conflict 상태로 표시한 뒤 사용자에게 머지 여부를 직접 선택할 수 있도록 UI로 대응할 수 있다고 함.

---

### Q) 66. Where do you launch tasks for loading the initial data? LaunchedEffect vs. ViewModel.init()

> Q) ViewModel.init()에서 초기 데이터를 로드하는 것과 Jetpack Compose의 LaunchedEffect에서 로드하는 것의 장단점은 무엇인가요? 어떤 상황에서 각각을 선택하시겠습니까? 만약 다른 방식을 선호한다면, 그것은 무엇이며 왜 그렇게 선택하셨나요?

- ViewModel.init()에서 초기 데이터 로드  
  - **장점**:  
      - **구성 변경(Configuration Changes)에 강함**: ViewModel은 생명주기와 독립적으로 유지되므로, 화면 회전 등에도 데이터를 유지할 수 있음
      - **UI와 비즈니스 로직의 분리**: UI 레이어에 의존하지 않고, 관심사의 분리를 잘 지킴
      - **예측 가능성**: ViewModel이 생성되는 타이밍에 데이터 로드가 일관되게 실행됨
  
  - **단점**:
    - **시점 제어 어려움**: 항상 ViewModel이 생성되자마자 실행되므로, 사용자의 특정 행동 이후에 로드해야 하는 경우 유연성이 떨어짐
    - **단위 테스트 어려움**: init() 내부에서 로직이 자동 실행되면, 테스트 시점 제어나 mocking이 어려움
    - **불필요한 실행 가능성**: 데이터가 필요하지 않은 상황에서도 로직이 실행될 수 있음

  -  **적합한 상황:**
      - 앱 실행 직후나 화면 진입 시 무조건 데이터를 가져와야 하는 경우
      - 데이터를 항상 ViewModel 단위로 유지하고 재사용해야 하는 구조


<br>

- Jetpack Compose의 LaunchedEffect에서 초기 데이터 로드
  - **장점**
    - **UI 이벤트 기반으로 동작 가능**: 특정 파라미터가 변경되었을 때만 데이터 로드를 트리거 가능
    - **사용자 인터랙션 기반 로직 구성 가능**: 뷰가 보일 때에만 작동하도록 만들 수 있음
    - **코드 가독성 향상**: Composable 내부에서의 데이터 흐름을 명확히 할 수 있음

  - **단점**
    - **재구성(recomposition)으로 인한 중복 실행 위험**: Key를 잘못 설정하면 같은 로직이 반복 실행될 수 있음
    - **Composable 생명주기 의존**: UI 상태 변화에 따라 예측 못 한 로딩이 발생할 수 있음
    - **관심사 분리 위반 우려**: UI 코드 안에 데이터 로딩 로직이 포함되어 관리 복잡도 증가

  -  **적합한 상황:**
      - 특정 Composable 진입 시마다 데이터를 다시 불러와야 할 때
      - 화면 전환, 스크롤 등 UI 기반 조건에 따라 로딩을 제어하고 싶을 때

<br>


> Q) cold flow를 활용한 지연 관찰(lazy observation) 방식은 ViewModel.init()이나 LaunchedEffect에 비해 초기 데이터 로딩에서 어떻게 효율성을 향상시킬 수 있나요? 해당 방식이 유리한 예시 시나리오를 함께 설명해주세요.


cold flow를 활용한 지연 관찰 방식은 다음과 같은 이유로 초기 데이터 로딩의 효율성을 극대화 한다고 한다.:
1. **불필요한 호출을 방지하여 리소스를 절약**
2. **실제 UI와 느슨하게 결합되어 관심사를 분리**
3. **캐시와 공유가 가능하여 중복 작업 방지**
4. **유연한 제어와 테스트 가능성 확보**

따라서 초기 데이터 로딩이 조건부로 필요하거나, 화면 진입 여부에 따라 동적으로 로딩해야 하는 상황에서는 cold flow 기반 접근이 가장 적합한 솔루션이라고 한다.  

ex) "상세 화면 진입 시 특정 ID에 해당하는 데이터만 조회해야 하는 경우"
``` kotlin
// ViewModel 내부
val selectedId = savedStateHandle.getStateFlow<Int?>("id", null)

val itemDetail: StateFlow<ItemDetail?> = selectedId
    .filterNotNull()
    .flatMapLatest { id ->
        repository.fetchItemDetail(id) // 네트워크/DB 호출
    }
    .stateIn(
        scope = viewModelScope,
        started = SharingStarted.WhileSubscribed(5_000),
        initialValue = null
    )
```

- **특징**:
    - 화면에 진입하고 실제로 itemDetail.collect()가 발생하기 전까지는 fetchItemDetail()이 실행되지 않음
    - 화면에서 벗어나면 collect가 중단되며, 다시 진입하면 기존 캐시된 값 또는 새로 갱신된 값만 전달됨
    - ViewModel.init()처럼 진입하지 않아도 무조건 호출되지 않음
    - LaunchedEffect처럼 재구성 시마다 반복 호출되는 문제도 없음

**👉 이 시나리오에서의 장점**

- 사용자가 **해당 화면에 진입하지 않으면 불필요한 네트워크 호출 없음**
- **ID가 바뀔 때마다 자동으로 최신 데이터로 갱신되며**, 이전 요청은 자동 취소 (flatMapLatest)
- **여러 컴포저블이나 다수의 UI 요소가 해당 데이터를 구독해도 하나의 인스턴스를 공유**
