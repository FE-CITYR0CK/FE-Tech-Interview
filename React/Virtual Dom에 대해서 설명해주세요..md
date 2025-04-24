# Virtual DOM에 대해 설명해주세요.

<br/>

## 면접용

가상돔이란 실제 돔의 가벼운 복사본으로 메모리 상에 존재하며, js 객체로 존재합니다. 리액트는 렌더링 이전의 화면 구조에 대한 가상돔, 그리고 렌더링 이후의 화면 구조에 대한 가상 돔 이렇게 두 개의 가상 돔을 가지고 있습니다. 이 두 가상돔을 diffing 알고리즘을 활용해 비교하며 변경된 부분만 실제 DOM에 반영하게 됩니다.

만일 가상돔을 사용하지 않는다고 한다면, 변경이 일어난 요소, 그리고 그 요소의 자식요소들에 대해 모두 리플로우가 진행됩니다. 리플로우는 브라우저 렌더링 과정에서 가장 비용이 큰 부분이기때문에, 이러한 비용 즉 성능을 최적화하기 위해 가상돔이라는 개념이 나오게 되었습니다.

<br/>
<hr/>
<br/>

## 개념 설명

오늘날 대부분의 기기는 60hz로 초당 60번 새로고침합니다. 각 새로고침은 눈에 보이는 시각적 출력을 생성하며 일반적으로 프레임이라 합니다.

초당 60번 새로고침을 한다는 것은, 1프레임당 16.66밀리초가 걸린다는 것입니다. 그러나 실제 브라우저에 각 프레임에 대한 오버헤드가 있으므로 모든 작업을 10밀리초 이내에 완료해야 유저는 버벅임을 느끼지 않을 것 입니다.

### 픽셀-화면 파이프라인

![image.png](attachment:eff67aff-1f88-40dc-9006-87cd8f1660b4:image.png)

- **JavaScript:** 사용자 인터페이스의 시각적 변경사항을 초래하는 작업을 처리하는 데 사용됩니다.
- **스타일 계산:** 일치하는 선택자를 기반으로 어떤 CSS 규칙이 어떤 HTML 요소에 적용되는지 파악하는 프로세스입니다.
- **레이아웃:** 요소가 브라우저에 차기하는 공간, 화면에 표시되는 위치 등을 계산합니다. 브라우저 렌더링 과정에서 레이아웃이 가장 비용이 많이 드는 과정입니다. 예를 들어 부모 요소의 크기가 자식 요소의 크기에 영향을 미치는 경우 (자식 요소의 width가 80%로 설정) 브라우저는 부모 요소부터 차례대로 레이아웃을 계산해야 하기에 비용이 큰 연산입니다.
- **페인트:** 페인팅은 픽셀을 채우는 프로세스입니다. 레이아웃이 계산된 후 텍스트, 색상, 이미지, 테두르 등 기본적으로 요소의 모든 시각적 측면을 그리는 작업을 합니다.
- **합성:** 페이지의 일부가 여러 레이어에 그려질 수 있으므로 페이지가 예상대로 렌더링되도록 올바른 순서로 화면에 적용해야 합니다. 만일 다른 요소와 겹치게 될 경우, 실수로 한 요소가 다른 요소 위에 잘못 표시될 수 있기 때문에 주의해야합니다.

### 변경사항이 생긴다면 ?

1. **리플로우(Layout 요소 변경)**

![image.png](attachment:268e5041-4c93-4763-9e89-45f97093b99e:image.png)

너비, 높이, 위치와 같은 요소의 도형을 변경하는 속성을 변경하게 되면 브라우저는 다른 모든 요소를 확인하고 페이지를 **‘리플로우’** 처리해야 합니다. 영향을 받는 영역은 리플로우 후 리페인트 및 재합성 과정을 거쳐야합니다.

아래 div를

```jsx
<div id="div" style="background: red; width: 150px; height: 50px">
  디브입니다요
</div>
```

아래와 같은 JavaScript 코드를 통해 width를 변경시킨다면 ?

```jsx
<script>
  const test = document.getElementById("div"); test.style.width = "500px";
</script>
```

이렇게 Layout이 다시 발생하는 Reflow 과정이 일어나고, 이후 Paint 과정도 다시 발생한 것을 알 수 있습니다.

![image.png](attachment:4e31be72-264e-4703-9c02-d7c8c5548be8:image.png)

1. **리페인트 (페인트 전용 속성 변경)**

![image.png](attachment:c3b9a75c-035c-4118-b95b-0c8f1a0c0d78:image.png)

페인트 전용 속성에 해당하는 background-image, color, box-shadow와 같은 속성을 변경할 경우 페이지에 시각적 업데이트를 커밋하는 데 레이아웃 단계가 필요하지 않습니다.

만일 위 div태그에 대해서 아래처럼 리플로우를 유발하지 않는 속성을 변경한다면 어떻게 될까요 ?

```jsx
<script>
  const test = document.getElementById("div"); test.style.background = "yellow";
</script>
```

위에서 봤던 Layout은 다시 발생하지 않고, Paint만 다시 실행되는 것을 알 수 있습니다.

![image.png](attachment:4d34e499-a810-430d-9636-fdaa5cdc0b50:image.png)

1. **레이아웃, 페인트가 필요하지 않은 속성이 변경**

![image.png](attachment:a88d7f94-a40f-4a03-8496-2ec1e2dec0cf:image.png)

만일 리플로우, 리페인트를 유발하지 않는 속성이 변경된다면 바로 컴포시션 단계로 이동하게 됩니다. 예를 들면 애니메이션, 스크롤 등 성능에 민감한 작업에서 활용될 수 있는 최적화된 방법입니다.

### 가상돔 등장 배경

앞서 이야기했듯, 렌더링 과정 중 가장 비용이 큰 것은 Layout 단계입니다. 그러나 날이 갈 수록 DOM 조작에 대한 복잡도는 높아지고 있고, 이로 인해 렌더트리를 재생성하고 레이아웃을 만들고 페인팅을 하는 과정이 반복되고 있습니다.

이를 해결하기 위해 등장한 개념이 가상돔입니다.

뷰에 변화가 있을 때, 구 가상돔과 새 가상돔을 비교하여 변경된 내용만 DOM에 적용함으로써 브라우저 내에서 발생하는 연산의 양을 줄이고, 성능을 최적화하는 기법입니다.

![image.png](attachment:6877a047-670e-4c0d-85ab-03594c9c5cca:image.png)

### 가상돔 동작원리

가상돔은 메모리 상에 저장된 JavaScript 객체입니다.

예를 들어 아래와 같은 HTML 파일이 있다고 가정하면

```jsx
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style></style>
  </head>
  <body>
    <div id="div" style="background: red; width: 150px; height: 50px">
      디브입니다요
    </div>
  </body>
</html>
```

가상돔은 아래와 같이 중첩된 객체의 형태로 저장된다는 뜻이지요.

```jsx
const virtualDOM = {
  type: "div",
  props: {
    id: "div",
    style: {
      background: "red",
      width: "150px",
      height: "50px",
    },
    children: [
      {
        type: "text",
        value: "디브입니다요",
      },
    ],
  },
};
```

### Diffing

위의 가상돔 코드에서 width를 `200px` 로 변경한다고 가정해봅시다.

React는 Reconciliation 과정을 통해 변경된 가상 DOM과, 기존 가상 DOM을 비교해 최소한의 업데이트를 수행할 것입니다. 그리고 이를 위해 Diff 알고리즘을 활용해 업데이트된 정보를 알아냅니다.

```jsx
const virtualDOMBefore = {
  type: "div",
  props: {
    id: "div",
    style: {
      background: "red",
      width: "150px",
      height: "50px",
    },
    children: [
      {
        type: "text",
        value: "디브입니다요",
      },
    ],
  },
};

const virtualDOMAfter = {
  type: "div",
  props: {
    id: "div",
    style: {
      background: "red",
      width: "200px",
      height: "50px",
    },
    children: [
      {
        type: "text",
        value: "디브입니다요",
      },
    ],
  },
};
```

1. **루트 노드 비교 (Element Type이 같은가?)**

   type은 div로 동일하네요. 그러면 속성을 비교해봅시다.

2. **Props 비교 (Style 속성 포함)**

   id: 변경 없음

   style.background, style.height: 변경 없음

   **style.height: 150px → 200px로 변경됨**

   ⇒ React는 width가 변경되었음을 감지하고, 해당 속성만 업데이트합니다. 즉, DOM의 노드를 다시 생성하지 않고 `div.style.width=”200px”` 만을 수행합니다.

3. **자식요소 비교**

   자식 요소는 변경 없음

### Fiber Reconciliation

React 2017년부터 기존의 Stack Reconciliation을 대체하여 Fiber Reconciliation을 도입했습니다. 이로 인해 UI 업데이트가 부드러워지고, 렌더링 성능이 최적화되었습니다.

이 부분에 대해선 다른 챕터 (React Fiber에 대해서 아시나요?)에서 다룰 예정이니 참고할 수 있는 글만 남기고 이번 문항에 대한 설명을 마무리하도록 하겠습니다.

https://junilhwang.github.io/TIL/Javascript/Design/Vanilla-JS-Virtual-DOM/#_4-diff-%E1%84%8B%E1%85%A1%E1%86%AF%E1%84%80%E1%85%A9%E1%84%85%E1%85%B5%E1%84%8C%E1%85%B3%E1%86%B7-%E1%84%8C%E1%85%A5%E1%86%A8%E1%84%8B%E1%85%AD%E1%86%BC

https://medium.com/stayfolio-tech/react%EA%B0%80-0-016%EC%B4%88%EB%A7%88%EB%8B%A4-%ED%95%98%EB%8A%94-%EC%9D%BC-feat-fiber-1b9c3839675a

**참고문헌**

https://web.dev/articles/rendering-performance?hl=ko#a_note_on_device_refresh_rates

[https://ui.toast.com/fe-guide/ko_PERFORMANCE#레이아웃과-리페인트](https://ui.toast.com/fe-guide/ko_PERFORMANCE#%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83%EA%B3%BC-%EB%A6%AC%ED%8E%98%EC%9D%B8%ED%8A%B8)

https://github.com/JunilHwang/simple-virtual-dom/blob/master/05-diff/src/main.js
