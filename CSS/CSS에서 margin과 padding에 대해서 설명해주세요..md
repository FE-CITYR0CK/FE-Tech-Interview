# CSS에서 margin과 padding에 대해서 설명해주세요.

<br/>

## 면접용

margin은 요소 외부의 여백을 설정합니다. 보통 요소와 요소 사이의 간격을 조정하는 데 사용됩니다.

padding은 요소 내부의 여백을 설정합니다. 이는 요소의 컨텐츠와 요소의 테두리 사이의 간격을 조정합니다.

## 개념설명

<br/>
<hr/>
<br/>

### CSS 기본 박스 모델

![image (31)](https://github.com/user-attachments/assets/83ec1bfd-2797-4b4c-8722-93762bdbb399)

문서의 레이아웃을 계산할 때, 브라우저 렌더링 엔진은 표준 CSS 기본 박스 모델에 따라 각각의 요소를 사각형 박스로 표현합니다. CSS는 박스의 크기, 위치, 속성을 결정합니다.

하나의 박스는 네 부분의 영역으로 이루어집니다. 각 영역을 콘텐츠 영역, 안쪽 여백(padding), 테두리 영역(border), 그리고 바깥 영역(margin)이라 부릅니다.

### margin

**`margin`** CSS 속성은 요소의 네 방향 [바깥 여백 영역](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_box_model/Introduction_to_the_CSS_box_model)을 설정합니다. [`margin-top`](https://developer.mozilla.org/ko/docs/Web/CSS/margin-top), [`margin-right`](https://developer.mozilla.org/ko/docs/Web/CSS/margin-right), [`margin-bottom`](https://developer.mozilla.org/ko/docs/Web/CSS/margin-bottom), [`margin-left`](https://developer.mozilla.org/ko/docs/Web/CSS/margin-left)의 단축 속성입니다.

<img width="660" alt="image (32)" src="https://github.com/user-attachments/assets/0909cd39-d8dc-4c0e-bc80-9ab7716bca4e" />

**구문**

```css
margin: 5%; /* 모두 5% */

margin: 10px; /* 모두 10px */

margin: 1.6em 20px;
/* 상하: 1.6em */
/* 좌우: 20px  */

margin: 10px 3% -1em;
/* 상: 10px */
/* 좌우: 3% */
/* 하: -1em */

margin: 10px 3px 30px 5px;
/* 상: 10px */
/* 우:  3px */
/* 하: 30px */
/* 좌:  5px */

margin: 2em auto;
/* 상하: 2em */
/* 수평 중앙정렬 */

margin: auto;
/* 상하: 0 */
/* 수평 중앙정렬 */
```

`margin` 속성은 한 개, 두 개, 세 개, 혹은 네 개의 값으로 지정할 수 있습니다. 각 값은 [`<length>`](https://developer.mozilla.org/ko/docs/Web/CSS/length), [`<percentage>`](https://developer.mozilla.org/ko/docs/Web/CSS/percentage) 또는 키워드 [`auto`](https://developer.mozilla.org/ko/docs/Web/CSS/margin#auto) 중 하나입니다. 음수 값은 요소와 이웃의 거리가 더 가까워지도록 합니다.

- **한 개의 값**은 모든 네 면의 여백을 설정합니다.
- **두 개의 값**을 지정하면 첫 번째는 **위와 아래**, 두 번째는 **왼쪽과 오른쪽** 여백을 설정합니다.
- **세 개의 값**을 지정하면 첫 번째는 **위**, 두 번째는 **왼쪽과 오른쪽,** 세 번째 값은 **아래** 여백을 설정합니다.
- **네 개의 값**을 지정하면 각각 **상, 우, 하, 좌** 순서로 여백을 지정합니다. (시계방향)

### padding

**`padding`** [CSS](https://developer.mozilla.org/ko/docs/Web/CSS) 속성은 요소의 네 방향 [안쪽 여백 영역](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_box_model/Introduction_to_the_CSS_box_model#padding-area)을 설정합니다. [`padding-top`](https://developer.mozilla.org/ko/docs/Web/CSS/padding-top), [`padding-right`](https://developer.mozilla.org/ko/docs/Web/CSS/padding-right), [`padding-bottom`](https://developer.mozilla.org/ko/docs/Web/CSS/padding-bottom), [`padding-left`](https://developer.mozilla.org/ko/docs/Web/CSS/padding-left)의 단축 속성입니다.

<img width="1165" alt="image (33)" src="https://github.com/user-attachments/assets/efe91bea-6257-4dd0-bf4f-cf77da5de928" />

**구문**

```css
padding: 5%; /* 모두 5% */

padding: 10px; /* 모두 10px */

padding: 10px 20px;
/* 상하: 10px */
/* 좌우: 20px */

padding: 10px 3% 20px;
/* 상: 10px */
/* 좌우: 3% */
/* 하: 20px */

padding: 1em 3px 30px 5px;
/* 상:  1em */
/* 우:  3px */
/* 하: 30px */
/* 좌:  5px */
```

padding 속성은 한 개, 두 개, 세 개, 혹은 네 개의 값으로 지정할 수 있습니다. 각 값은 [`<length>`](https://developer.mozilla.org/ko/docs/Web/CSS/length), [`<percentage>`](https://developer.mozilla.org/ko/docs/Web/CSS/percentage) 중 하나로, 음수 값은 유효하지 않습니다.

- **한 개의 값**은 모든 네 면의 여백을 설정합니다.
- **두 개의 값**을 지정하면 첫 번째는 **위와 아래**, 두 번째는 **왼쪽과 오른쪽** 여백을 설정합니다.
- **세 개의 값**을 지정하면 첫 번째는 **위**, 두 번째는 **왼쪽과 오른쪽,** 세 번째 값은 **아래** 여백을 설정합니다.
- **네 개의 값**을 지정하면 각각 **상, 우, 하, 좌** 순서로 여백을 지정합니다. (시계방향)

### 콘텐츠 영역

콘텐츠 영역은 콘텐츠 경계가 갑싼 영역으로, 글이나 이미지, 비디오 등 요소의 실제 내용을 포함합니다. 콘텐츠 영역의 크기는 톤켄츠 너비, 콘텐츠 높이 입니다. 배경색과 배경 이미지를 가지고 있기도 합니다.

[`box-sizing`](https://developer.mozilla.org/ko/docs/Web/CSS/box-sizing) 속성의 값이 기본 값인 `content-box`이며 요소가 블록 레벨 요소인 경우, 콘텐츠 영역의 크기를 [`width`](https://developer.mozilla.org/ko/docs/Web/CSS/width), [`min-width`](https://developer.mozilla.org/ko/docs/Web/CSS/min-width), [`max-width`](https://developer.mozilla.org/ko/docs/Web/CSS/max-width), [`height`](https://developer.mozilla.org/ko/docs/Web/CSS/height), [`min-height`](https://developer.mozilla.org/ko/docs/Web/CSS/min-height), [`max-height`](https://developer.mozilla.org/ko/docs/Web/CSS/max-height) 속성을 사용해 사용해 명시적으로 설정할 수 있습니다.

### 테두리 영역

테두리 영역은 테두리 경계가 감싼 영역으로, 안쪽 여백 영역을 요소의 테두리까지 포함하는 크기로 확장합니다. 영역의 크기는 테두리 박스 너비와 테두리 박스 높이입니다.

테두리의 두께는 [`border-width`](https://developer.mozilla.org/ko/docs/Web/CSS/border-width)와 단축 속성인 [`border`](https://developer.mozilla.org/ko/docs/Web/CSS/border)가 결정합니다. [`box-sizing`](https://developer.mozilla.org/ko/docs/Web/CSS/box-sizing) 속성의 값이 `border-box`라면 테두리 영역의 크기를 [`width`](https://developer.mozilla.org/ko/docs/Web/CSS/width), [`min-width`](https://developer.mozilla.org/ko/docs/Web/CSS/min-width), [`max-width`](https://developer.mozilla.org/ko/docs/Web/CSS/max-width), [`height`](https://developer.mozilla.org/ko/docs/Web/CSS/height), [`min-height`](https://developer.mozilla.org/ko/docs/Web/CSS/min-height), [`max-height`](https://developer.mozilla.org/ko/docs/Web/CSS/max-height) 속성을 사용해 명시적으로 설정할 수 있습니다. 박스의 배경([`background-color`](https://developer.mozilla.org/ko/docs/Web/CSS/background-color) 또는 [`background-image`](https://developer.mozilla.org/ko/docs/Web/CSS/background-image))은 테두리의 바깥 경계까지 늘어나고, 그릴 땐 테두리에 가려집니다. 이 기본동작 방식은 [`background-clip`](https://developer.mozilla.org/ko/docs/Web/CSS/background-clip) 속성으로 바꿀 수 있습니다.
