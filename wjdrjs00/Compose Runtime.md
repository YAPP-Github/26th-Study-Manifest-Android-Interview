# Category 1: Compose Runtime

## 🙋🏻 Practical Questions (11 ~ 25)

### Q) 11. What is State and which APIs are used to manage it?

> Q) 상태(State)는 리컴포지션과 어떤 관련이 있으며, 리컴포지션 시에는 어떤 일이 발생하나요?

State가 변경되면 해당 State를 참조하는 컴포저블만 리컴포지션이 일어나 UI를 최신 상태로 자동 반영한다.  
리컴포지션 시에는 변경된 State를 기반으로 관련 컴포저블 함수가 다시 실행되어 반영된 부분만 UI에 효율적으로 갱신하는 일이 발생한다.

---

### Q) 12. What are the advantages you can take from the state hoisting?

> Q) 어떤 상황에서는 상태 호이스팅을 피하고 상태를 컴포저블 내부에 유지하는 것이 좋을까요?

상태가 해당 컴포저블에만 국한되는 경우가 있다.  
-> 해당 상태가 오직 컴포저블 내부에서만 사용되고, 외부에서 참조하거나 제어할 필요가 없는 경우에는 내부 상태 관리가 더 간단하기 때.


### Q) 13. What are the differences between remember and rememberSaveale?

> Q) 어떤 상황에서 rememberSaveable을 remember보다 사용하는 것이 더 적절하며, 이때 고려해야 할 트레이드오프는 무엇인가요?

구성 변경(configuration changes) 이후에도 상태를 유지해야 할 때 remember보다 더 적절한 선택이다.  

고려해야 할 트레이드오프: 성능 오버헤드  
-> rememberSaveable은 내부적으로 Bundle을 사용하여 상태를 저장하기 때문에 remember보다 약간의 성능 및 메모리 오버헤드가 발생할 수 있다고 한다.

---

### Q) 14. How do you safely create a coroutine scope within composable functions?

> Q) 코루틴을 컴포저블 내부에서 직접 실행할 때 발생할 수 있는 위험은 무엇이며, 이를 어떻게 피할 수 있을까요?

메모리 누수:  
-> 컴포저블이 컴포지션에서 제거된 이후에도 코루틴이 계속 실행되면, 해당 컴포저블이 참조하고 있는 리소스가 해제되지 않아 메모리 누수가 발생할 수 있다.  

rememberCoroutineScope 사용해 이 문제를 피할 수 있다.  
-> 컴포지션에 따라 자동으로 관리되는 안전한 스코프를 생성하므로, 리소스 정리가 자동으로 이루어지고 메모리 누수를 방지할 수 있다.

---

### Q) 15. How do you handle side effects inside composable functions?




### Q) 16. What is the purpose of rememberUpdatedState, and how does it work?

> Q) LaunchedEffect를 사용해 지연된 동작을 트리거하는 컴포저블에서, 지연 후 최신 람다가 호출되도록 하려면 어떻게 해야 하나요?

`rememberUpdatedState`를 사용하여 지연 이후에도 최신 람다가 호출되도록 한다.

---

### Q) 17. What is the purpose of produceState, and how does it work?

> Q) 컴포저블 함수 내에서 코루틴 작업을 실행하고 그 결과를 상태로 관찰해야 하는 상황에서, LaunchedEffect나 rememberCoroutineScope를 사용하지 않고 이를 어떻게 구현하시겠습니까?

`produceState`를 사용해여 이를 구현할 수 있다고 한다.  
produceState는 코루틴을 실행하고 그 결과를 상태로 변환하여 Compose UI에 반영할 수 있도록 해준다고 한다.

---

### Q) 18. What is snapshotFlow and how does it work?

> Q) 어떤 상황에서 ViewModel의 Flow를 직접 관찰하는 것보다 snapshotFlow를 사용하는 것이 더 적절하며, 방출(Emission) 동작은 어떻게 최적화하시겠습니까?

Compose 내부의 상태나 동작(ex. remember, mutableStateOf, LazyListState 등)을 기반으로 반응해야 하는 경우가 있다고 함

방출(Emission) 동작 최적화 방법:
1. distinctUntilChanged() 사용
2. debounce() 또는 throttleLatest() 사용
3. 조건부 필터링 (filter)

---

### Q) 19. What is the purpose of derivedStateOf, and how does it help optimize recomposition?

> Q) 어떤 상황에서 derivedStateOf를 사용하는 것이 적절한가요? 그리고 다른 상태 변수들로부터 값을 계산하더라도 derivedStateOf 사용을 피해야 하는 상황은 언제인가요?

`derivedStateOf`는 리컴포지션 최적화가 실제로 필요한 경우에만 사용하는 것이 바람직하다고 한다.  
“파생된 값이 자주 바뀌는 상태에 의존하지만, 실제 결과는 자주 바뀌지 않는다”는 조건이 핵심이며, 단순한 연산이라면 일반 변수나 remember만으로 충분하다고 한다.

---

### Q) 20. What’s the lifecycle of composable functions or Composition?

> Q) 컴포저블 함수의 생명 주기 단계를 설명하고, 상태가 변경될 때 Compose가 재구성을 어떻게 처리하는지 설명하세요.

생명주기 단계: Initial Composition -> Recomposition -> Leaving the Composition  
Compose는 각 컴포저블이 어떤 상태에 의존하는지를 추적하고 있으며, 해당 상태가 변경되면 이를 감지하여 해당 컴포저블만 선택적으로 재구성한다.  

---

### Q) 21. What is SaveableStateHolder?

> Q) Jetpack Navigation 라이브러리를 사용하지 않고, 탭이 여러 개인 사용자 인터페이스(tab UI)에서 각 탭의 스크롤 위치나 입력 상태를 화면 전환 후에도 유지하려면 어떻게 구현하시겠습니까?

SaveableStateHolder와 rememberSaveable을 함께 사용하는 구조를 구현하면 이 문제를 해결할 수 있다고 한다.  
-> SaveableStateHolder를 활용하면 탭을 전환해도 이전의 스크롤 위치나 입력 상태를 그대로 복원할 수 있음.

---

### Q) 22. What’s the purpose of the snapshot system?

> Q) Snapshot.takeSnapshot()을 사용하여 스냅샷을 찍는 것이, 컴포저블에서 상태를 직접 관찰(observe)하는 것보다 더 적절한 상황은 어떤 경우인가요?

다음과 같은 상황들이 이에 해당한다고 한다.  
1. 디버깅 또는 상태 분석 도구 구현 시
2. 멀티스레드 환경에서 안전한 읽기 작업이 필요할 때
3. 일시적으로 캡처된 상태 기반으로 계산이나 연산을 수행할 때
4. 상태를 관찰하지 않고 단순히 캡처하고 유지하고자 할 때


### Q) 23. What are the mutableStateListOf and mutableStateMapOf

> Q) mutableStateOf로 감싼 일반 mutableListOf를 수정해도 왜 Compose에서 재구성이 발생하지 않나요? 이 문제를 어떻게 해결할 수 있을까요?

리스트 객체 자체의 변경만 감지하고 리스트 내부 항목의 추가,삭제와 같은 구조적 변경은 감지하지 못하기 때문에 항목을 추가해도 Compose는 상태가 변경되었다고 인식하지 않아 UI가 재구성되지 않는다.  
mutableStateListOf()를 사용해 내부 항목 변경도 감지할 수 있는 상태 인식 리스트를 사용하여 해결할 수 있다.


### Q) 24. How can you safely collect Kotlin’s Flow in composable functions while preventing memory leaks?

> Q) collectAsState는 생명 주기를 인식하지 못해, UI가 보이지 않아도 수집을 계속하며 메모리 누수로 이어질 수 있습니다. 이 문제를 어떻게 해결할 수 있을까요?

`collectAsStateWithLifecycle`을 사용하여 Android 생명 주기에 맞춰 Flow 수집을 자동으로 일시 중지하도록 처리한다.  
이 방식은 UI가 백그라운드로 전환되거나 비활성 상태일 때 불필요한 리소스 사용을 막아준다.

### Q) 25. What’s the role of the CompositionLocals?

> Q) CompositionLocal이란 무엇이며, 어떤 상황에서 파라미터 전달 대신 이를 사용하는 것이 좋을까요?

CompositionLocal:  
-> Jetpack Compose에서 데이터를 컴포지션 트리 하위로 암묵적으로 전달할 수 있는 메커니즘  

사용이 적절한 상황:  
-> 앱 전역에서 공통으로 사용되는 설정값이 있는 경우 (예: 테마, 색상, 폰트, 언어 등)
