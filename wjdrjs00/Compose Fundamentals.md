# Category 0: Compose Fundamentals

## 🙋🏻 Practical Questions (0 ~ 10)

### Q) 0. What is the structure of Jetpack Compose?

> Q) Compose Compiler의 역할은 무엇이며, 기존의 KAPT나 KSP와는 어떻게 다른가요?

Compose Compiler의 역할:  
Compose Compiler는 Jetpack Compose에서 @Composable 함수로 작성된 선언형 UI 코드를 분석하고, 이를 실행 가능한 최적화된 코드로 변환하는 컴파일러 플러그인이다.  
컴파일 시점에 UI의 상태 변화에 따른 재구성(recomposition) 로직을 자동으로 생성하며, 상태 관리, 코드 최적화, 람다 리프팅(lambda lifting) 등의 기능도 수행한다.

KAPT/KSP는 소스 코드에 주석을 기반으로 외부 코드를 생성하는 반면, Compose Compiler는 Kotlin 컴파일러의 FIR(Frontend Intermediate Representation) 단계에서 작동하여 훨씬 더 깊은 정적 분석과 직접적인 바이트코드 최적화를 수행할 수 있다.  
즉, 단순한 코드 생성기가 아니라 Kotlin 컴파일러와 긴밀히 통합된 고성능 변환기로서 작동한다고 함.


<br>

> Q) Compose Runtime은 재구성과 상태를 어떻게 관리하며, 내부적으로 어떤 자료 구조를 사용하나요?

상태(state)가 변경될 때 어떤 UI 요소를 다시 그려야 하는지를 추적하고, 해당 요소만 재구성(recomposition) 되도록 관리한다.   
→ 이를 통해 UI의 일부분만 효율적으로 갱신할 수 있다.

내부적으로 슬롯 테이블(slot table)이라는 자료 구조를 통해 이루어진다.  
→ 슬롯 테이블은 컴포지션의 상태를 메모이즈하고, UI 요소의 트리 구조와 상태 정보를 추적합니다.

---

### Q) 1. What are the Compose phases?

> Q) Composition 단계에서 어떤 일이 발생하며, 이것이 Recomposition(재구성)과 어떤 관련이 있나요?

Composition 단계에서는 @Composable 함수들을 실행하여 UI의 구조를 설명하는 UI 트리를 생성한다.  
→ 이 과정에서 Compose는 각 컴포저블의 계층 관계를 Slot Table 이라는 내부 데이터 구조에 기록한다고 함.

Recomposition은 상태(state)에 변화가 생겼을 때, 변화가 감지된 부분만 다시 실행하여 UI를 갱신하는 과정이다.
→ 즉, Composition 단계는 최초의 UI 구성을 만들고, 이후 상태 변화가 발생하면 해당 변경사항에 따라 필요한 부분만 Recomposition 하는 식으로 효율적으로 UI를 업데이트를 한다.

<br>

> Q) Layout 단계는 어떻게 동작하나요?

Layout 단계는 Composition 이후에 실행되며, 각 컴포저블이 자신의 크기(너비와 높이)를 결정하고, 자식 컴포저블을 측정한 후 배치한다.  
이 단계에서는 Compose가 부모로부터 전달받은 제약 조건(Constraints)을 기반으로 각 컴포저블이 얼마나 크게 그려질 수 있는지를 계산한다고 함.

각 컴포저블은 다음과 같은 순서로 처리된다:  
1. 자식 컴포저블들을 측정
2. 자신의 크기 결정
3. 자식들을 적절한 위치에 배치

---

### Q) 2. Why is Jetpack Compose a declarative UI framework?

> Q) Jetpack Compose의 선언형 특성은 기존 명령형 XML UI 개발 방식과 어떻게 다르며, 어떤 장점을 제공하나요?

Jetpack Compose는 선언형 UI 프레임워크로, 개발자가 앱의 상태에 따라 UI가 어떤 모습이어야 하는지를 정의하면, 프레임워크가 상태 변경 시 UI를 자동으로 갱신한다.  
반면 XML은 UI 구조와 속성을 XML에서 정의하고, 이에 따른 **상태 처리 및 UI 업데이트 로직은 Kotlin이나 Java 코드에서 수동으로 작성해야 한다.

Jetpack Compose의 선언형 접근은 다음과 같은 장점을 제공한다:
- UI와 상태가 하나의 Kotlin 코드로 통합되어 있어 가독성이 높고 유지보수가 용이함
- Recomposition을 통해 변경된 상태에 따라 UI가 자동으로 업데이트되어, 수동 UI 동기화 코드가 불필요함
- XML과 달리 UI 정의와 로직이 같은 레이어에 존재하므로, 더 적은 코드로 더 많은 기능 구현이 가능함
- 모듈화 및 재사용성이 높은 구조를 유도하여, 대규모 앱에서도 유연한 확장이 가능함

<br>

> Q) Jetpack Compose는 컴포저블에서 어떤 방식으로 멱등성을 달성하며, 선언형 UI 시스템에서 이것이 왜 중요한가요?

@Composable 함수는 동일한 입력 값이 주어졌을 때 항상 동일한 UI 출력을 생성한다. 
이를 멱등성(Idempotence)이라고 하며, Compose는 이 원칙을 다음과 같은 방식으로 딜성한다고 함:

- @Composable 함수는 입력 값(예: 상태)만을 기준으로 UI를 정의함.
- 상태가 변경되면 Compose는 해당 상태를 참조하는 컴포저블만 선택적으로 재구성(Recompose)함.
- 이 때 함수의 실행 횟수와 상관없이, 같은 입력에는 같은 출력이 보장되도록 설계되어 있다.

이러한 멱등성 선언형 UI 시스템에서이 중요한 이유:

- 예측 가능한 UI 동작을 보장하여, 디버깅과 테스트를 용이하게 만든다.
- 상태 기반 렌더링에서 불필요한 사이드 이펙트를 방지하고, 안정적이고 일관된 UI를 유지할 수 있다.
- 여러 번 recomposition이 발생하더라도, 컴포저블이 동일한 결과를 출력하므로 성능 최적화에 유리하다.

---

### Q) 4. How the composable function works internally?

> Q) 함수에 @Composable 어노테이션을 붙이면 무슨 일이 일어나나요?

함수에 @Composable 어노테이션을 붙이면, Compose 컴파일러는 해당 함수의 구조를 수정하여 **모든 컴포저블 함수에 암시적인 Composer 파라미터**를 추가한다.  
이 파라미터는 컴포저블 함수와 Compose 런타임 사이의 **브리지(다리)** 역할을 하며, UI 상태 관리, 리컴포지션, 기타 핵심 기능들을 효율적으로 처리할 수 있도록 한다.  
이러한 변환은 런타임이 입력 파라미터의 변화 여부를 감지하여 **리컴포지션이 필요한지 판단할 수 있는 훅(hook)** 을 삽입한다.  
이러한 구조 덕분에 Compose는 **변경되지 않은 UI 부분의 리컴포지션을 건너뛰어**, 성능을 최적화할 수 있습니다.

---

### Q) 5. What is stability in Jetpack Compose, and how does it relate to performance?

> Q) Compose 컴파일러는 파라미터가 안정한지 불안정한지를 어떻게 판단하며, 이 판단은 왜 재구성에 중요할까요?

Compose 컴파일러는 Composable 함수에 전달되는 파라미터의 타입을 분석하여 해당 값이 안정(stable)한지 불안정(unstable)한지를 분류한다.  
이 판단은 컴파일 타임에 이루어지며, 다음과 같은 기준이 적용된다고 함:

- 안정한(stable) 타입의 예:
  - **기본 타입** (Int, Boolean, String 등): 본질적으로 변경되지 않으므로 안정함.
  - **불변 프로퍼티만 가진 data class**: 모든 프로퍼티가 안정한 경우 전체 클래스도 안정함.
  - **람다 표현식**: 캡처하지 않거나 예측 가능한 경우 안정함.
  - **@Stable, @Immutable 어노테이션이 붙은 클래스**: 명시적으로 안정성을 표시함.


- 불안정한(unstable) 타입의 예:
  - **interface, abstract class, List, Map, Any 등**: 구체 구현이 불확실하여 컴파일러가 예측 불가능.
  - **mutable 프로퍼티를 가진 클래스**: 값이 언제든 변경될 수 있으므로 불안정으로 간주됨.

Compose는 Composable 함수의 **이전 파라미터 값과 새로운 값이 동일한지 비교**해보고, 안정한 타입이고 값도 같다면 **재구성을 건너뛴다(Smart Recomposition)**.  
반면, 파라미터가 **불안정**하면 값이 같더라도 **무조건 재구성**이 발생한다.  
→ 따라서 안정한 타입을 사용하는 것은 **불필요한 UI 업데이트를 줄이고 성능을 높이는 핵심**이라고 함.

> Q) @Stable 및 @Immutable 어노테이션은 Jetpack Compose에서 어떤 역할을 하며, 언제 사용하는 것이 적절할까요?

@Stable과 @Immutable은 개발자가 클래스의 안정성을 명시적으로 표시할 수 있도록 제공되는 Compose 어노테이션이다.  
이를 사용하면 컴파일러가 클래스의 안정성을 더 정확하게 추론할 수 있어, 재구성 최적화와 스마트 스킵(Smart Skipping)에 도움이 된다고 한다.

- `@Immutable`

  - 모든 프로퍼티가 **완전히 불변(immutable)**이어야 함
  - Compose는 해당 클래스의 **상태가 절대 바뀌지 않는다**고 간주함
  - 주로 **네트워크 응답, DB 엔티티 등 변경되지 않는 데이터 모델**에 사용
  - Kotlin data class에 특히 적합


- `@Stable`

  - 제어된 가변성(controlled mutability)**이 있는 경우 사용
  - 예측 가능한 방식으로 값이 바뀌며, 같은 입력에 대해 **일관된 결과를 보장**
  - 내부적으로 값을 바꿀 수 있지만 외부에서 볼 때는 안정된 상태 유지
  - 주로 **UI 상태 객체나 인터페이스, Modifier 확장 함수** 등에서 사용

---

### Q) 6. Have you ever had experience optimizing Compose performance by improving stabilities?

> Q) List를 파라미터로 받는 Composable 함수가 불필요한 재구성을 일으킬 때, 어떻게 최적화하시겠습니까?

불변 컬렉션을 사용한다.  
→ kotlinx.collections.immutable의 ImmutableList을 사용하여 Compose 컴파일러가 안정한 타입으로 인식하도록 한다.

---

### Q) 10. What Kotlin idioms frequrently used in Jetpack Compose?

> Q) 트레일링 람다(trailing lambdas)와 고차 함수(higher-order functions)는 컴포저블 함수 구조화에서 어떤 역할을 하나요?

고차 함수는 컴포저블이 동작과 구조를 외부에서 정의할 수 있도록 만들어주며, 트레일링 람다는 이러한 구조를 간결하고 읽기 쉽게 표현할 수 있도록 도와준다.  
이 두 요소는 Compose의 선언형 UI 철학에 맞게 함수 기반 UI 구성을 가능하게 하고, 코드를 간단하고 유지보수하기 쉬운 구조로 만드는데 필수적이라고 한다.
