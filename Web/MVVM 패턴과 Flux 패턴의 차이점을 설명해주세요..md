# MVVM 패턴과 Flux 패턴의 차이점을 설명해주세요.

## 면접용

네, MVVM 패턴과 Flux 패턴은 UI 상태를 관리하는 방법이지만, 구조와 데이터 흐름 방식에서 차이가 있습니다.

MVVM(Model-View-ViewModel) 패턴은 UI와 비즈니스 로직을 분리하면서도 양방향 데이터 바인딩을 제공하는 구조입니다. Model은 데이터와 비즈니스 로직을 담당하고, View는 사용자에게 보여지는 UI이며, ViewModel은 이 둘을 중간에서 연결하는 역할을 합니다. ViewModel은 Model의 데이터를 가공하여 View에 전달하고, View에서 발생한 사용자 입력을 Model로 전달하는 방식으로 동작합니다.

반면, Flux 패턴은 단방향 데이터 흐름을 기반으로 한 아키텍처입니다. 애플리케이션의 상태 관리를 예측 가능하게 만들기 위해 Action → Dispatcher → Store → View의 순서로 데이터가 흐릅니다. 사용자가 특정 동작을 하면 Action이 생성되고, 이 Action이 Dispatcher를 통해 Store로 전달되며, Store에서 상태를 업데이트한 후 변경된 상태가 View에 반영됩니다. Redux는 Flux 패턴을 기반으로 하는 대표적인 상태 관리 라이브러리입니다.

즉, MVVM은 ViewModel을 활용하여 UI와 데이터를 양방향으로 동기화하는 방식이고, Flux는 단방향 데이터 흐름을 통해 상태 변화를 명확히 추적하는 방식입니다. MVVM은 UI와 데이터의 실시간 연동이 중요한 경우 유용하고, Flux는 복잡한 상태를 예측 가능하게 관리하는 데 적합합니다.

<hr/>

## 개념 설명

## MVVM 패턴

**MVVM**은 데이터 바인딩을 사용하여 View와 ViewModel 간에 **양방향 데이터 흐름**을 유지합니다. 

![MVVMPattern svg](https://github.com/user-attachments/assets/e0623a9c-8f81-4863-91be-83e6ffe4d7c9)

### MVVM의 구성 요소 (Vue.js 관점)

1. **Model**: 애플리케이션의 데이터를 담당합니다. (`data` 속성)
2. **View**: 사용자 인터페이스(UI), 즉 HTML 템플릿입니다. (`template` 속성)
3. **ViewModel**: View와 Model을 연결하는 중간 계층으로, Vue의 인스턴스 자체입니다.

### 대표적인 사용 예시

**Angular, Vue.js**

## **Vue.js에서 MVVM 패턴의 동작 순서**

1. **사용자가 View(UI)에서 이벤트를 발생시킵니다.**
    - 사용자는 버튼을 클릭하거나, 입력 필드에 값을 입력하는 등의 동작을 합니다.
    - Vue에서는 `v-on` 디렉티브 (`@click`, `@input` 등)를 통해 이벤트를 감지합니다.

2. **View는 이벤트를 ViewModel(Vue 인스턴스)로 전달합니다.**

3. **ViewModel은 Model(API, Store 등)에게 데이터를 요청합니다.**
    - `fetchData` 메서드는 서버(API) 요청을 보내거나, Pinia/Vuex에서 데이터를 가져옵니다.
    - 이 과정에서 `axios`, `fetch` 같은 HTTP 클라이언트를 사용할 수 있습니다.

4. **Model은 ViewModel에게 데이터를 응답합니다.**
    - API가 데이터를 응답하거나, Store에서 필요한 데이터를 반환합니다.
    - 예를 들어, 서버에서 사용자의 정보를 JSON 형태로 응답하면, 이를 받아서 가공할 수 있습니다.

5. **ViewModel은 받은 데이터를 가공하여 상태(State)에 저장합니다.**

6. **View는 Data Binding을 통해 UI를 자동으로 갱신합니다.**
    - Vue의 반응형 시스템 덕분에, `{{ message }}` 같은 템플릿 바인딩이 자동으로 업데이트됩니다.
    - `v-bind`, `v-model`, `computed` 등의 기능을 통해 UI가 변경된 상태를 반영합니다.

<br/>

## Flux 패턴

**Flux**는 **단방향 데이터 흐름**을 사용하여 **Action -> Dispatcher -> Store -> View**의 방향으로 데이터가 흐릅니다.

<img width="702" alt="image" src="https://github.com/user-attachments/assets/724fb7d1-3d86-439e-b1e2-68f9cdd9c957" />

### Flux의 구성 요소 (React 관점)

1. **Action**: 상태 변경을 요청하는 객체로, 어떤 동작이 발생했는지를 설명합니다.
    - 예: `{ type: 'ADD_TODO', payload: '새로운 할 일' }`
2. **Dispatcher**: 모든 Action을 받아서 Store로 전달하는 중앙 허브 역할을 합니다.
3. **Store**: 애플리케이션의 상태를 저장하고 관리하는 곳으로, 상태 변경 로직을 포함합니다.
4. **View**: React 컴포넌트로, Store에서 변경된 상태를 받아 렌더링합니다.

**Flux**는 **단방향 데이터 흐름**과 [**단일 책임 원칙**](https://github.com/FE-CITYR0CK/React-Docs/blob/main/02_State%20%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0/03_%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%20%EA%B0%84%20State%20%EA%B3%B5%EC%9C%A0%ED%95%98%EA%B8%B0.md#react%EC%9D%98-%EB%8B%A8%EC%9D%BC-%EC%A7%84%EB%A6%AC%EC%9D%98-%EC%9B%90%EC%B2%9Csingle-source-of-truth)을 따르므로 테스트하기 쉬운 코드를 작성하기 더 쉽습니다.

**Flux**의 **Store와** React 상태 관리 라이브러리에서 사용되는 **Store**는 ****기본적으로 **같은 개념을 따릅니다.**

### 대표적인 사용 예시

**React, Redux**

## React에서 Flux 패턴의 동작 순서

1. **사용자가 View(React 컴포넌트)에서 이벤트를 발생시킵니다.**
    - 사용자가 버튼을 클릭하거나, 입력 필드에 값을 입력하는 등의 동작을 합니다.
    - React에서는 `onClick`, `onChange` 등의 이벤트 핸들러를 사용하여 이벤트를 감지합니다.

2. **View는 Action을 생성하여 Dispatcher에 전달합니다.**
    - 이벤트 핸들러에서 `dispatch({ type: 'ADD_TODO', payload: '새로운 할 일' })` 같은 Action을 발생시킵니다.
    - Action은 상태를 변경하기 위한 정보를 담고 있으며, `type`과 `payload`를 포함합니다.

3. **Dispatcher는 Action을 Store로 전달합니다.**
    - Dispatcher는 모든 Action을 중앙에서 관리하고, 해당 Action을 Store로 전달합니다.
    - Redux에서는 `dispatch(action)`을 호출하면 Reducer를 통해 상태가 변경됩니다.

4. **Store는 Action을 받아 상태를 업데이트합니다.**
    - Reducer 함수가 실행되며, 기존 상태와 Action을 기반으로 새로운 상태를 생성합니다.
    - Redux에서는 `useReducer` 또는 `createStore`를 통해 Store를 정의합니다.

5. **View(React 컴포넌트)는 Store의 변경된 상태를 구독하여 UI를 갱신합니다.**
    - 상태가 변경되면, Store를 구독하고 있는 컴포넌트가 다시 렌더링됩니다.
    - Redux의 경우 `useSelector()`를 사용하여 Store의 상태를 가져옵니다.