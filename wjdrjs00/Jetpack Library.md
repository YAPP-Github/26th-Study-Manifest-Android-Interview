# Category 2: Jetpack Library

## 🙋🏻 Practical Questions (49 ~ 58)

### Q) 54. What is Jetpack ViewModel?

> Q) ViewModel은 구성 변경 시 데이터를 어떻게 유지하며, onSaveInstanceState()를 사용하는 것과 어떤 차이가 있나요?

jetpack viewmodel은 viewModelStore를 기반으로 동작 → 이 Store는 구성 변경이 발생해도 메모리에 그대로 유지된다.  
따라서 Activity나 fragment가 파괴 → 재생성 되어도 viewmodel은 같은 인스턴스를 재사용하게 되어 데이터가 소실되지 않는다.

더 디테일한 학습을 위한 레퍼런스
- [How Android ViewModel works under the hood to survive to configuration change](https://proandroiddev.com/how-viewmodel-works-under-the-hood-52a4f1ff64cf)
- [viewmodel이 구성변경에 어떻게-살아남는지 알아보자](https://medium.com/@vov3616/viewmodel%EC%9D%B4-%EA%B5%AC%EC%84%B1%EB%B3%80%EA%B2%BD%EC%97%90-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%82%B4%EC%95%84%EB%82%A8%EB%8A%94%EC%A7%80-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-66d28ca915e3)
- [ViewModelStore, ViewModelProvider 분석](https://rccode.tistory.com/380)

<br>

`onSaveInstanceState()`를 사용하는 것과의 차이점은?  

ViewModel
- 메모리에 데이터를 보관한다.
- 복잡한 객체도 데이터 저장 가능하다.
- 메모리에 데이터를 보관하기 때문에 UI 데이터를 빠르게 읽고 쓸 수 있음
- 메모리 부족 등의 액티비티 종료시 폐기된다. (UI 상태 유지 X)

onSaveInstanceState():
- 디스크에서 데이터 관리한다.
- 원시(Primitive) 유형 및 문자열과 같은 단순하고 작은 객체만 저장 가능하다.
- 직렬화/역직렬화 및 디스크 엑세스로 데이터 읽고 쓰기에 느림
- Bundle은 onSaveInstanceState()에서 데이터를 저장하는 과정 중 직렬화 과정을 거친다.
메인 쓰레드에서 동작하기 때문에 여기서 데이터를 저장하는데 시간을 많이 사용하게 되면 UI 버벅거림이 발생할 수 있다.
- 메모리 부족 등의 액티비티 종료 시 저장 가능하다(UI 상태 유지 O)  
<br>  

> Q) ViewModelStoreOwner의 목적은 무엇이며, 하나의 액티비티 내에서 여러 프래그먼트가 ViewModel을 공유하려면 어떻게 해야 하나요?

ViewModelStoreOwner의 목적  

ViewModelStoreOwner는 ViewModel의 생명주기를 관리하는 책임자다. 
ViewModel은 ViewModelStoreOwner 의해 생성되고, ViewModelStoreOwner가 유지되는 동안 ViewModel 인스턴스도 함께 유지된다.  
- Activity, Fragment, Navigation Graph 등은 모두 ViewModelStoreOwner를 구현함
- 이를 통해 각 컴포넌트는 자신만의 ViewModel 저장소(ViewModelStore)를 가지게 되며, 해당 저장소에 ViewModel을 등록하고 관리한다.
- ViewModel은 이 저장소에 저장되어 구성 변경이 발생해도 자동으로 재사용된다.

여러 프래그먼트가 ViewModel을 공유하려면, requireActivity()를 통해 액티비티 범위(ViewModelStoreOwner)를 명시한다.  
<br>  


> Q) ViewModel 내에서 UI 상태를 관리할 때 StateFlow나 LiveData를 사용하는 것의 장점과 잠재적인 단점은 무엇인가요?


공통적인 장점 (StateFlow & LiveData)
1.	관찰 기반 상태 관리
둘 다 옵저버블(observable) 형태로 동작하여, UI는 ViewModel의 상태 변경에 반응형으로 업데이트됩니다.
2.	UI 생명주기에 맞는 자동 관리
LiveData는 생명주기 인식이 내장되어 있어, UI 컴포넌트가 활성 상태일 때만 업데이트를 전달합니다.
StateFlow도 repeatOnLifecycle()과 함께 사용하면 유사한 생명주기 제어가 가능합니다.
3.	구성 변경에 강한 ViewModel 연동
ViewModel이 구성 변경에도 살아남기 때문에, 상태 데이터 역시 함께 유지되며 데이터 재요청 없이 UI 복원이 가능합니다.  
<br>


StateFlow의 장점  
- Kotlin Coroutines 기반으로 collect()를 통해 비동기 처리에 자연스럽게 통합됨.
- 항상 최신 값을 유지 (state.value)하며, UI 상태 표현에 적합.
- combine, map, flatMapLatest 등 리액티브 연산자 지원이 풍부하여 복잡한 상태 흐름을 깔끔하게 표현할 수 있음.
- Flow API들과의 일관성 있는 통합이 가능.  
<br>


LiveData의 장점
- 생명주기 자동 인식(Lifecycle-aware): 따로 생명주기를 관리하지 않아도 안전하게 UI와 연결됨.
- Jetpack 공식 컴포넌트와의 높은 호환성: XML에서의 DataBinding과 쉽게 통합 가능.
- Android 앱에 이미 널리 쓰이고 있어 진입 장벽이 낮음.  
<br>

StateFlow의 단점
- 생명주기 자동 관리가 내장되어 있지 않기 때문에, UI에서 사용 시 lifecycleScope + repeatOnLifecycle() 같은 적절한 코루틴 스코프 설정이 필수임.
- XML 기반 앱에서는 DataBinding과 직접 연동이 불편함.  
<br>

LiveData의 단점
- Kotlin의 Flow 또는 Coroutines와 완전한 통합이 어렵고 어색할 수 있음.
- 연산자 기능이 제한적이며, 복잡한 상태 조합이나 변환 처리 시 코드가 지저분해질 수 있음.
- UI 외의 계층(예: Repository)에서는 사용이 권장되지 않음 → 비동기 흐름 통합에는 부적합.


---

### Q) 55. Jetpack Navigation 라이브러리는 무엇인가요?

> Q) Jetpack Navigation 라이브러리는 백 스택을 어떻게 처리하며, NavController를 통해 이를 프로그래밍 방식으로 어떻게 조작할 수 있나요?

Jetpack Navigation 라이브러리는 백 스택을 자동으로 관리하여 사용자가 뒤로 가기를 누른 경우 적절한 이전 화면으로 돌아가도록 도와준다.  
각 목적지(destination)는 내비게이션 그래프에 따라 백 스택에 순차적으로 쌓이고, 사용자가 이동할 때마다 해당 목적지가 백스택에 추가된다.  
→ 이 덕분에 복잡한 수동 백 스택 처리를 하지 않아도 일관된 내비게이션 흐름을 유지할 수 있다고 한다.


NavController를 사용하여 백 스택을 프로그래밍 방식으로 조작이 가능한데, 주요 메서드는 다음과 같다.  
- `popBackStack()`: 가장 최근의 목적지를 백 스택에서 제거하고 이전 화면으로 이동한다.
- `popBackStack(destinationId, inclusive)`: 특정 목적지까지 백 스택을 되돌리며, inclusive가 true일 경우 해당 목적지도 제거한다.
- `navigate(R.id.destination)`: 새로운 목적지로 이동하며 백 스택에 해당 목적지를 추가한다.

이 중에서 `popBackStack(destinationId, inclusive)`는 어떤 경우에 사용할 수 있을까?  
예를 들어 [스플래쉬 → 로그인 → 홈]의 흐름을 가진 경우에 사용할 수 있다.  
로그인 성공 후 이동한 홈 화면에서 뒤로가기 시 다시 로그인 화면으로 이동하는 경우를 방지하기 위해서 해당 메소드를 사용한다.  
<br>


> Q) Safe Args란 무엇이며, Jetpack Navigation 구성 요소에서 목적지 간 데이터 전달 시 타입 안전성을 어떻게 향상시키나요?

Safe Args는 Jetpack Navigation 라이브러리에서 제공하는 Gradle 플러그인으로, 타입 안전성이 보장된 인자 전달 코드를 자동 생성해준다.  

타입 안정성을 향상시키는 방법
1. 전달할 인자의 타입을 명시적으로 정의한다.
   ```xml
   <argument
    android:name="itemId"
    app:argType="integer" />
   ```
2. Gradle 빌드 시 타입 안전한 코드를 자동 생성한다.  
   Safe Args는 이 정보를 기반으로 Directions 클래스와 Args 클래스를 자동 생성한다. 이 클래스들은 전달할 데이터의 타입을 함수 시그니처에 반영한다.
   ```kotlin
   val action = HomeFragmentDirections.actionHomeToDetails(itemId = 42) // 타입 명확
   ```

3. 컴파일 시 타입 오류를 감지한다.  
   못된 타입을 전달하거나 인자를 빠뜨리면 컴파일 에러가 발생한다.
   ```kotlin
   val action = HomeFragmentDirections.actionHomeToDetails(itemId = "wrongType") // 컴파일 에러
   ```

그럼 compose에서는? compose에서도 TypeSafety를 지원한다.    
사실 정확한 이해 없이 사용했었는데, 이번 기회에 확실하게 숙지해봐야겠다.   
참고할 만한 레퍼런스  
- [공식문서](https://developer.android.com/guide/navigation/design/type-safety?hl=ko)
- [Type safe navigation for Compose](https://medium.com/androiddevelopers/type-safe-navigation-for-compose-105325a97657)
- [[Android] Type-Safe Compose Navigation 적용 - 중첩(Nested) 네비게이션 구조](https://velog.io/@mraz3068/Android-Type-Safe-Compose-Navigation-Applying-in-Nested-Navigation)
