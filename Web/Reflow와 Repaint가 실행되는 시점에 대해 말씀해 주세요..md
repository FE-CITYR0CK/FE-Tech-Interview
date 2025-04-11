# Reflow와 Repaint가 실행되는 시점에 대해 말씀해 주세요.
<br/>

## 면접용
Reflow는 기존에 생성된 DOM 노드의 레이아웃이 변경되었을 때, 관련된 모든 노드의 수치를 다시 계산하여 렌더 트리를 다시 생성하는 과정입니다. 

Reflow가 발생되는 시점은 다음과 같습니다.

- DOM 요소의 기하학적 속성(width, height 등)이 변경될 때
- 브라우저 사이즈가 변할 때

Repaint는 변경된 요소를 다시 화면에 그려주는 작업입니다. 리플로우와 마찬가지로 첫 렌더링 이후에 변경된 요소를 그리는 작업에 해당하며, 발생 시점은 다음과 같습니다.

- 리플로우 발생
- 요소의 스타일(색상 등) 변경
    - 레이아웃의 수치를 변화시키지 않는 스타일의 변경
    - opacity, visbility 변경 등

Reflow와 Repaint는 브라우저 렌더링에서 비용이 많이 드는 작업이므로 발생 횟수를 줄이는 것이 매우 중요합니다.

<br/>
<hr/>
<br/>

## 개념설명

![image](https://github.com/user-attachments/assets/8e2af136-0678-4973-bb48-4dbfb307c551)



### Reflow와 실행 시점

Reflow는 기존에 생성된 DOM 노드의 레이아웃이 변경되었을 때, 관련된 모든 노드의 수치를 다시 계산하여 렌더 트리를 다시 생성하는 과정입니다.

브라우저 렌더링 과정 중 `레이아웃(Layout)` 에서 **실행 시점**을 기준으로 세분화한 과정입니다.

- `레이아웃(Layout)` : 브라우저가 첫 렌더링 시 실행되는 과정
- `Reflow`: 초기 렌더링 후 변경 사항이 생길 때 실행되는 과정

Reflow가 발생되는 시점은 다음과 같습니다.

- DOM 요소의 기하학적 속성(width, height 등)이 변경될 때
- 브라우저 사이즈가 변할 때

DOM 요소에서 리플로우가 발생하면 주변 요소에도 모두 영향을 줍니다. 

![image 1](https://github.com/user-attachments/assets/4f3e26f2-16e9-4c9c-b765-7b41a4101ce3)

이미지처럼 특정 요소의 너비가 변경된다면 그와 인접한 다른 요소의 위치도 변경이 됩니다. 이렇게 요소 하나만 변화해도 주변 요소의 위치나 크기에 영향을 줘서 DOM트리를 다시 계산하고 렌더 트리를 생성하게 됩니다. 

위와 같이 CSS 스타일을 변경하는 것 이외에도 애니메이션, 트랜지션 등의 스타일 요소에서 모든 프레임에 리플로우가 발생합니다. 유저 인터랙션으로 발생하는 효과(hover, 텍스트 입력 등등)에서도 트리거할 수 있습니다. 

또한 자바스크립트에서 특정 메서드나 속성을 접근할 때도 리플로우가 발생합니다. 요소의 계산된 스타일에 접근하는 offsetWidth, offsetHeight 등의 메서드를 사용하면, 요소의 최신값을 알아내기 위해 리플로우가 발생합니다.

[gist-What forces layout/reflow](https://gist.github.com/paulirish/5d52fb081b3570c81e3a)

```jsx
window.scrollX, window.scrollY
window.innerHeight, window.innerWidth

elem.offsetLeft, elem.offsetTop, elem.offsetWidth, elem.offsetHeight
```

이렇듯 여러 상황에서 reflow가 발생할 수 있고, 당연히 많은 비용이 발생하게 됩니다.

### Repaint와 실행 시점

리페인트는 변경된 요소를 다시 화면에 그려주는 작업입니다. 리플로우와 마찬가지로 첫 렌더링 이후에 변경된 요소를 그리는 작업에 해당하며, 발생 시점은 다음과 같습니다.

- 리플로우 발생
- 요소의 스타일(색상 등) 변경
    - 레이아웃의 수치를 변화시키지 않는 스타일의 변경
    - opacity, visbility 변경 등

<details>
<summary>그럼 `display:none` 도 repaint만 발생할까?</summary>

- 해당 속성이 적용된 요소는 영역이 사라집니다. 즉, 렌더 트리에서 제외됩니다.

- 주변 요소의 크기 및 위치에도 영향을 주므로 reflow, repaint 모두 발생합니다.
</details>

<br/>

리페인트의 경우 변경된 레이아웃을 다시 계산하고 트리를 구성하는 작업이 없기 때문에, 리페인트만 발생한다면 성능 면에서 더 나은 점이 있습니다.

### 리플로우와 리페인트 직접 확인하기

1. 기록을 통한 확인
- 개발자도구 > 성능 > 기록
- 기록이 되는 동안 css요소를 변경
- 기록 중단 후 결과를 보면 리플로우(레이아웃), 리페인트(페인트)가 발생한 것을 확인 가능!

![%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-02-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8 59 03](https://github.com/user-attachments/assets/a63510ee-1117-4956-918c-3ba400701d84)

2. 렌더링을 통한 확인
- 개발자도구 > 도구 더보기 > 렌더링
- 페인트 플래시를 활성화 → 페인트가 발생하는 영역 하이라이트
- 레이아웃 변경 지역을 활성화 → 변경 영역에 하이라이트

![%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-02-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9 11 41](https://github.com/user-attachments/assets/0394c9e0-bf95-4854-9cf0-2955b84d64df)


### Reflow, Repaint 줄이기

1. 애니메이션 요소에 position: absolute 적용하기
- 애니메이션으로 요소의 위치를 변경하면 프레임마다 리플로우가 여러번 발생하며, 주변 요소에게도 영향을 주어 함께 변경됩니다.
- 따라서 해당 요소의 위치를 absolute로 설정하여 영향을 주지 않게 합니다.

```jsx
.animation-target {
  position: absolute;
  animation: moveTo 2s;
}
```

2. **display: none 사용하기**
- 특정 요소의 스타일을 여러 번 수정해야 한다면, `display:none` 을 설정하고 스타일 변경 후 `display: block` 을 할 수 있습니다.

```jsx
div.style.display = "none";   // (A) 렌더트리에서 제외시키기

// 스타일 수정

div.style.display = "block";  // (B) 스타일 수정 후 렌더트리에 추가하기
```

3. DOM 속성 변경 코드 모으기
- 브라우저는 변경할 요소가 있을 때 큐에 저장하고 일정 시간 후에 리플로우를 실행합니다.
- 여러 DOM 속성을 변경할 때, 변경하는 코드를 모아두면 리플로우 횟수를 줄일 수 있습니다.

```jsx
// BAD
const el1 = document.querySelector('.target-first');
el1.style.width = '10px';

const el2 = document.querySelector('.target-second');
el2.style.width = '10px';

const el3 = document.querySelector('.target-third');
el3.style.width = '10px';

// GOOD
const el1 = document.querySelector('.target-first');
const el2 = document.querySelector('.target-second');
const el3 = document.querySelector('.target-third');

// dom의 스타일 변경 코드를 한 곳으로 모아둠
el1.style.width = '10px';
el2.style.width = '10px';
el3.style.width = '10px';
```

- 마찬가지로 여러 스타일을 변경할 때도 한번에 변경하는 것이 좋습니다.

```jsx
// BAD
el.style.width = "10px";
el.style.height = "10px";
el.style.borderRadius = "5px";
el.style.backgroundColor = "red";
el.style.left = "20px";

// GOOD
<body>
  <script>
    el.className = 'small';
  </script>
</body>

<style>
.small {
  width: 10px;
  height: 10px;
  border-radius: 5px;
  background-color: red;
  left: 20px;
}
</style>
```

4. 가상 DOM 사용하기

기존의 리페인트, 리플로우 과정에서는 전체 레이아웃을 다시 계산하기 때문에 성능의 문제가 있었습니다. 이를 위해 React, Vue에서는 가상DOM을 사용하여 실제 DOM과의 상호작용을 최소화하고, 주변 요소에 영향을 주지 않고 최소한의 변경을 할 수 있습니다.

### React 관점에서의 Reflow/Repaint

복잡한 SPA에서는 DOM 조작이 여러번 발생합니다. 위에서 설명한 것처럼 DOM 조작은 행동 하나하나가 리플로우를 트리거하여 레이아웃 변화, 트리 변화, 리렌더링을 발생시킵니다.

이를 Virtual DOM으로 불필요한 조작을 막을 수 있습니다.

- 컴포넌트의 상태나 props가 변경되면, 리액트는 먼저 **가상 DOM을 업데이트**합니다.
    - **변경을 실시간으로 화면에 렌더링하지 않고 메모리 상에서** 가상 DOM 트리만 변경합니다.
- 가상 DOM에서 변화가 일어난 후, 리액트는 **가상 DOM과 이전의 가상 DOM을 비교**합니다.
    - 이 과정은 **“diffing”** 알고리즘을 통해 이루어집니다.
- 변경된 부분만 실제 DOM에 업데이트되어 브라우저 화면에 반영됩니다.
    - 변화를 한번에 묶어서 적용시키기 때문에 연산의 횟수를 크게 줄일 수 있습니다.
    - 단, 레이아웃 계산과 리렌더링의 규모는 다소 커질 수 있습니다.