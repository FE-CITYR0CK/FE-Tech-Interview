# SASS(SCSS)와 기존 CSS를 비교할 때, SASS의 장점은 무엇인가요?

<br/>

## 면접용

SASS 혹은 SCSS는 기존 CSS에 비해 변수, 중첩, 믹스인, 상속 등의 기능을 재공해 코드의 재사용성과 유지보수성을 높인, CSS를 더 편하고 강력하게 쓸 수 있는 도구입니다. 기존의 CSS는 스타일을 일일히 반복해서 써야하고, 값이 바뀌면 하나하나 다 고쳐야하지만 SCSS는 이런 귀찮은 부분을 변수, 함수, 상속, 중첩 등의 기능으로 훨씬 쉽게 해결해줍니다.

<br/>

<br/>

## 개념 설명

### SCSS

SCSS는 SASS의 문법 중 하나로, CSS를 더 강력하게 만들어주는 전처리기입니다.

SCSS는 `.scss` 확장자를 사용하고, 우리가 작성한 SCSS 파일들은 CSS로 변환되는 과정을 거쳐야합니다.

### 왜 더 편할까?

1. **변수**

   색상, 여백을 변수로 관리하기 때문에, 나중에 값이 바뀌어도 한 줄만 고치면 됩니다.

   ```jsx
   $primary-color: #3498db;
   $padding: 16px;

   .button {
     background-color: $primary-color;
     padding: $padding;
   }
   ```

1. **중첩**

   계층 관계를 명확히 볼 수 있어 가독성이 좋습니다.

   ```jsx
   //SCSS
   .nav {
     ul {
       list-style: none;
     }
     li {
       display: inline-block;
     }
   }

   //CSS
   .nav ul{list-style:none}
   .nav li{display: inline-block;}
   ```

1. **믹스인 + include**

   자주 사용하는 스타일 묶음은 함수처럼 만들어서 재사용이 가능합니다.

   ```jsx
   @mixin flex-center {
     display: flex;
     justify-content: center;
     align-items: center;
   }

   .box {
     @include flex-center;
     height: 100px;
   }
   ```

   ### 변환 방법

   SCSS는 웹에서 직접 동작할 수 없습니다. 따라서 SCSS를 CSS로 컴파일해야합니다.

   1. Webpack

      Webpack에서는 style-loader, css-loader, scss-loader을 통해 코드를 변환해줍니다.

   2. Vite

      Vite는 sass를 설치하면 추가적으로 구성하지 않고도 사용할 수 있습니다.

   ### cf) CSS-in-JS vs SCSS

   | **항목**        | **SCSS**                                   | **CSS-in-JS**                                      |
   | --------------- | ------------------------------------------ | -------------------------------------------------- |
   | **기본 개념**   | CSS를 확장한 전처리기                      | JS 안에서 CSS를 작성하는 방식                      |
   | **파일 분리**   | .scss 파일에서 스타일 관리                 | JS/TS 안에 스타일 포함 (ex. styled-components)     |
   | **스코프**      | 기본적으로 글로벌, BEM/네임스페이스로 관리 | 기본적으로 컴포넌트 단위로 스코프가 자동 분리      |
   | **동적 스타일** | 불가능하거나 제한적 (@if, @mixin으로 우회) | JS 변수/props 사용해서 완전히 동적                 |
   | **런타임 비용** | 없음 (빌드 시 SCSS → CSS 변환)             | 있음 (런타임에 스타일 생성)                        |
   | **재사용성**    | @mixin, @extend 등                         | JS의 함수, 객체, props를 이용한 고급 재사용        |
   | **도구 의존성** | Sass 컴파일러만 있으면 됨                  | 라이브러리 필요 (styled-components, Emotion 등)    |
   | **디버깅**      | CSS class 기준 디버깅                      | 자동 생성된 class명으로 디버깅 다소 어려울 수 있음 |
   | **생산성**      | 익숙한 사람에게 빠르고 명확                | JS와 통합되어 로직과 함께 다룰 수 있음             |
   | **학습 곡선**   | CSS에 익숙하면 쉬움                        | JS/React 개념에 익숙해야 자연스럽게 사용 가능      |
