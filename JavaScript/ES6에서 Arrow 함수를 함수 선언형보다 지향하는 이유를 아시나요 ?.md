# ES6에서 Arrow 함수를 함수 선언형보다 지향하는 이유를 아시나요 ?

<br/>

## 면접용

화살표 함수를 기존의 함수 선언식보다 더 지향하는 이유에는 3가지 장점이 있습니다.

첫째, 간결한 문법과 가독성 향상입니다. 화살표 함수는 함수 표현식보다 더 간결하게 함수를 작성할 수 있습니다.

둘째, this 바인딩의 일관성입니다. 화살표 함수는 자신만의 this를 생성하지 않고, 외부 스코프의 this를 그대로 사용합니다.

셋째, 콜백 함수에서의 활용입니다. 화살표 함수를 사용하면 콜백 함수 내에서 this를 명시적으로 바인딩하지 않아도 되어 코드가 간결해집니다.

<br/>
<br/>

## 개념설명

### 화살표 함수란?

화살표 함수 표현식은 함수 표현식에 대한 간결한 대안으로, 약간의 의미적 차이와 의도적인 사용성의 제한을 가지고 있습니다.

```jsx
let func = (arg1, arg2, ...argN) => expression;
```

이렇게 코드를 작성하면 인자 arg1 … argN을 받는 함수 func이 만들어집니다. 함수 func은 화살표 우측의 표현식을 평가하고, 평가 결과를 반환합니다.

아래 함수의 축약 버전이라고 할 수 있습니다.

```jsx
let func = function (arg1, arg2, ...argN) {
  return expression;
};
```

### 일반 함수와 비교

1. this 바인딩 방식의 차이

   **일반함수 선언 (function)**

   - `호출되는 시점`에서의 호출 문맥에 따라 this가 결정됨.
   - 메서드 내부 콜백이나 이벤트 핸들러에서 this가 달라져 혼란을 줄 수 있음
     ```jsx
     const person = {
       name: "철수",
       sayHi: function () {
         setTimeout(function () {
           console.log(`안녕! 나는 ${this.name}`);
         }, 1000);
       },
     };
     this.name = "효린";
     person.sayHi(); // 결과: 안녕! 나는 undefined
     ```

   **화살표 함수**

   - 정의 시점 기준으로 this 바인딩을 가짐
   - 외부 스코프의 this를 그대로 사용함
     ```jsx
     const person = {
       name: "철수",
       sayHi: function () {
         setTimeout(() => {
           console.log(`안녕! 나는 ${this.name}`);
         }, 1000);
       },
     };
     person.sayHi(); // 결과: 안녕! 나는 철수
     ```

   별개의 this가 만들어지는 것을 원하지 않고, 외부 컨텍스트에 있는 this를 이용하고 싶은 경우 화살표 함수가 유용합니다.

2. [\*\*a](http://new.target)rguments, new 바인딩\*\*

   **일반 함수**

   - 생성자로써 인스턴스 생성 가능
   - arguments 객체 사용 가능

   **화살표 함수**

   - 생성자 함수 사용 불가, new와 함께 사용 시 에러 발생
   - arguments 객체 사용 불가, 대신 `…rest` 사용

   ```jsx
   function normalFunc() {
     console.log(arguments); // 유사 배열 [1, 2, 3]
   }
   normalFunc(1, 2, 3);

   const arrowFunc = () => {
     console.log(arguments); // ❌ ReferenceError
   };
   arrowFunc(1, 2, 3);
   ```

   ```jsx
   // 일반 함수 – 생성자 O
   function Person(name) {
     this.name = name;
   }
   const p = new Person("철수"); // OK

   // 화살표 함수 – 생성자 X
   const PersonArrow = (name) => {
     this.name = name;
   };
   const p2 = new PersonArrow("영희"); // ❌ TypeError
   ```
