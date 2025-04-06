# require와 import의 차이점에 대해 설명해 주세요.

<br/>

## 면접용

두 키워드 모두 자바스크립트 개발에서 모듈을 불러오는 데 쓰입니다. 

require은 Node.js에서 사용되고 있는 CommonJS 모듈 시스템의 키워드이고, import는 ES6에서 도입된 ESM의 모듈 시스템 키워드입니다.

require은 동적 구문으로 사용하며 동기적으로 작동합니다. 따라서 모듈이 완전히 로드된 후에 코드가 실행됩니다. 모듈을 내보낼 때는 주로 module.exports 또는 exports 키워드를 사용하여 내보냅니다.

import는 정적 구문으로 사용하며, 모듈을 명시적으로 불러옵니다. 모듈을 비동기적으로 처리할 수 있어 성능과 메모리 측면에서 유리하고, 브라우저 환경에서 더 적합합니다. export default 또는 named export 키워드를 사용하여 모듈을 내보내며, 이를 통해 트리 쉐이킹 등의 최적화가 가능합니다.

<br/>
<hr/>
<br/>

## 개념설명
자바스크립트 개발에서 모듈을 불러오는 문법은 2가지가 있습니다.

- require (내보내는 건 exports)
- import (내보내는 건 export)


>**모듈(Module)이란?**
>코드의 독립적이고 재사용 가능한 단위입니다. 즉, 모듈화란 큰 프로그램을 관리하기 쉬운 작은 코드로 나누는 것입니다.

require은 Node.js에서 사용되고 있는 CommonJS 모듈 시스템의 키워드이고, import는 ES6에서 도입된 ESM의 모듈 시스템 키워드입니다.

### CommonJS 모듈 불러오기/내보내기

CommonJS 환경에서 require를 사용하여 모듈을 불러올 때 다음과 같이 사용할 수 있습니다.

```jsx
const express = require("express");
```

반대로 모듈을 내보낼 때는 exports와 module.exports의 2가지 방식이 있습니다. 특정 변수 자체나, 특정 변수의 속성에 내보낼 객체를 할당하는 방식으로 모듈을 내보냅니다.

1. 하나의 객체를 내보낼 경우 module.exports 변수 자체에 할당합니다.

```jsx
// 하나의 객체 내보내기
// converter.js

const currencyConverter = {};

currencyConverter.convertUsdToKrw = function (usdAmount) {
  return roundTwoDecimals(usdAmount * usdToKrwRate);
};

currencyConverter.convertKrwToUsd = function (krwAmount) {
  return roundTwoDecimals(krwAmount / usdToKrwRate);
};

module.exports = currencyConverter;

// 하나의 객체 불러오기

const converter = require("./converter");
console.log(converter.convertUsdToKrw(50));  // 예: 50 USD를 KRW로 변환
console.log(converter.convertKrwToUsd(30000)); // 예: 30,000 KRW를 USD로 변환
```

2. 여러 개의 객체를 내보낼 경우, exports 변수의 속성에 할당합니다.

```jsx
// USD와 KRW 변환 함수 내보내기 // 예시 환율: 1 USD = 1200 KRW

function roundToTwoDecimals(amount) {
  return Math.round(amount * 100) / 100;
}

const convertUsdToKrw = function(usdAmount) {
  return roundToTwoDecimals(usdAmount * usdToKrwRate);
};

function convertKrwToUsd(krwAmount) {
  return roundToTwoDecimals(krwAmount / usdToKrwRate);
}

exports.convertUsdToKrw = convertUsdToKrw;
exports.convertKrwToUsd = convertKrwToUsd;

// USD와 KRW 변환 함수 불러오기
const currencyConverter = require("./currency-functions");
console.log(currencyConverter.convertUsdToKrw(50)); // 50 USD를 KRW로 변환
console.log(currencyConverter.convertKrwToUsd(30000)); // 30,000 KRW를 USD로 변환
```

### ES 모듈 불러오기/내보내기

ES6 모듈 시스템에서는 import 키워드를 통해 모듈을 불러올 수 있습니다. 이때 불러오는 모듈에서 몇 개의 객체를 내보냈는가에 따라 사용 방식이 약간 다르기 때문에, 내보내는 코드와 함께 설명드리겠습니다.

하나의 모듈에서 하나의 객체를 내보낼 때는 `export default` 키워드를 사용해서 명시적으로 선언합니다.  

이때 하나의 객체를 불러올 때는 다음처럼 불러올 수 있습니다.

```jsx
// currency-object.js
// 내보내기
function roundTwoDecimals(amount) {
  return Math.round(amount * 100) / 100;
}

export default roundTwoDecimals;

// test_import.js
// 불러오기
import roundTwoDecimals from "./currency-object";
console.log(roundTwoDecimals(50));
```

만약 이름을 지정하지 않고 내보낸다면, 불러올 때도 아무 이름이나 사용할 수 있습니다. 내보내는 객체는 Default Export 라고도 부릅니다.

```jsx
// 내보내기 (currency-object.js)
export default {
  convertUsdToKrw(usdAmount) {
    return roundTwoDecimals(usdAmount * usdToKrwRate);
  },

  convertKrwToUsd(krwAmount) {
    return roundTwoDecimals(krwAmount / usdToKrwRate);
  },
};

// 아무 이름이나 주고 객체를 통해 속성에 접근
// 불러오기
import currency from "./currency-object";
console.log(currency.convertUsdToKrw(50)); 
console.log(currency.convertKrwToUsd(30000));
```

하나의 모듈에서 여러 객체를 내보낼 때도 마찬가지로 export 키워드를 사용하는데, 이때 내보내는 변수나 함수의 이름이 불러올 때 그대로 사용되기 때문에 Named Exports라고 합니다.

```jsx
// 내보내기 1
export function convertUsdToKrw(usdAmount) {
  return roundTwoDecimals(usdAmount * usdToKrwRate);
}

// 내보내기 2
const convertKrwToUsd = function (krwAmount) {
  return roundTwoDecimals(krwAmount / usdToKrwRate);
};
export { convertKrwToUsd, convertUsdToKrw };
```

여러 객체를 불러올 때는 ES6의 구조 분해 할당(destructing) 문법을 사용해서 필요한 객체만 선택적으로 사용하거나, 별명(alias)를 붙여 모든 객체에 접근할 수 있습니다.

```jsx
// Destructuring 방식
import { convertUsdToKrw } from "./currency-functions";

console.log("100 US dollars equals to KRW:");
console.log(convertUsdToKrw(100));

// Alias 방식
import * as currency from "./currency-functions";

console.log("20000 KRW equals to US dollars:");
console.log(currency.convertKrwToUsd(20000));
```

### require과 import 비교

require은 동적 구문을 사용하며 동기적으로 작동합니다. 따라서 모듈이 완전히 로드된 후에 코드가 실행됩니다.

node.js 환경에서 주로 사용하며, node.js가 기본적으로 commonjs 모듈 시스템을 채택하고 있어 기본적으로 사용됩니다.

import는 정적 구문으로 사용하며, 모듈을 명시적으로 불러옵니다. 모듈을 비동기적으로 처리할 수 있어 브라우저에서 유리합니다.

import 키워드를 사용하는 ESM은 정적 분석을 통해 의존성을 추적할 수 있으며, 코드 최적화 및 트리 쉐이킹을 지원합니다.


>**정적 분석이란?**
>프로그램의 소스 코드를 실행하지 않고 코드의 구조, 성능 최적화 가능성 등을 분석하는 과정입니다.
>
>정적 분석은 주로 컴파일 시점에 이루어집니다. 컴파일러 등의 도구가 실행 전에 소스 코드를 분석하여 문법 오류 등을 체크하기 때문에 런타임 에러를 사전에 방지할 수 있습니다.
>
>정적 분석 도구에는 ESLint, TypeScript의 정적 분석 등이 있습니다.


동기식 모듈 시스템인 CommonJS와 달리, ESM은 비동기 방식으로 작동하며 모듈에서 실제로 쓰이는 부분만 불러옵니다. 

- 이는 ESM이 Top-level Await를 지원하기 때문입니다.


>**Top-level Await란?**
>Node.js에서 ESM을 사용할 때 최상위 수준에서 await를 사용할 수 있게 해주는 기능입니다.
>
>이를 통해 비동기 함수 안에서만 사용 가능했던 await를 모듈 최상위 레벨에서 직접 사용할 수 있게 되어, import 구문도 비동기적으로 처리할 수 있습니다.

```jsx
import { readFile } from 'fs/promises';

const data = await readFile('example.txt', 'utf8');
console.log(data);

// 기존 방식 
import { readFile } from 'fs/promises';

async function readAndPrintFile() {
    const data = await readFile('example.txt', 'utf8');
    console.log(data);
}

readAndPrintFile();
```

</aside>

### 추가 : CommonJS와 ESM의 등장 배경

초기에는 JavaScript에서 모듈의 개념이 없었고, 다른 파일에 있는 객체에 접근하기 위해 전역 객체(window)를 사용했습니다. 또한 index.html의 `<script>` 태그에 모든 파일을 포함시키고, 브라우저에서 실행시켰습니다.

```jsx
// A.js
var functionA = function(){
  console.log("모듈이 없었던 시절엔 어떻게 살았나?");
}

// B.js
window.functionA();

// index.html
<script src="./A.js"></script>
<script src="./B.js"></script>
```

하지만 이런 방식은 대부분의 코드가 같은 스코프에 존재하기 때문에 예상치 못한 오류를 일으킬 수 있었으며, 재사용성도 떨어졌습니다.

시간이 지나면서 js로 작성되는 스크립트의 크기가 커져 모듈화의 필요성이 대두되었습니다. 특히 서버 측에서 더 많은 외부 라이브러리와 패키지를 사용했기 때문에, 의존성들을 모듈화하여 관리할 필요가 있었습니다.

이를 위해 Node.js에서 서버 사이드 애플리케이션에서 사용하기 위해 CommonJS 모듈 시스템을 만들었습니다. 이 모듈의 가장 큰 특징은 동기적으로 동작한다는 점입니다.

- 모든 파일이 로컬에 있고, 필요할 때 바로 불러올 수 있는 상황인 서버 사이드 js 환경에서 사용하기 위해 만들어졌습니다.

하지만 이 방식은 메인 스레드의 실행 시간을 최소화해야 하는 브라우저에서 문제가 되었습니다. 

그래서 ECMAScript에서 지원하는 Javascript 공식 모듈인 ESM이 등장했습니다. ESM은 모듈 로더를 비동기 환경에서 실행하기 때문에 브라우저에 적합합니다. ESM은 다음처럼 동작합니다.

1. 구성 : import문을 따라가며 모듈의 종속성 트리를 형성합니다. 모듈의 위치를 찾고, 필요한 파일을 비동기적으로 다운로드하여 파싱을 준비합니다.
2. 인스턴스화 : 초기화되지 않은 상태의 모듈을 메모리에 할당합니다. 
3. 평가: 인스턴스화된 모듈에 실제 값을 할당합니다.

ESM은 현재 대부분의 js코드에서 사용되고 있으며, Node.js에서도 12버전부터 CommonJS와 ESM을 동시에 지원하고 있습니다.


>**require 문법, 왜 알아둬야 하지?**
>많은 프로젝트에서 ES6 모듈을 널리 사용하고 있지만, 하위 호환성을 위해 Node.js의 기본 모듈 시스템으로 CommonJS를 채택하고 있습니다. 
>ES6 코드를 변환해주는 Babel 같은 도구를 사용할 수 없다면 require 키워드를 반드시 사용해야 합니다.
