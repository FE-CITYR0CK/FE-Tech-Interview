# null, undefined, undeclared, NaN 에 대해 설명해주세요.

<br/>

## 면접용

null은 변수에 값이 없다는 것을 의도적으로 명시할 때 사용합니다. 변수에 null을 할당하면 변수가 이전에 참조하던 값을 더 이상 참조하지 않겠다는 의미로, 자바스크립트 엔진이 가비지 컬렉션을 수행하게 됩니다.

undefined는 개발자가 의도적으로 할당하기 위한 값이 아닌, 자바스크립트 엔진이 변수를 초기화할 때 사용하는 값입니다. 변수를 참조했을때 undefined가 반환된다면 참조한 변수가 선언 이후 초기화되지 않았다는 것을 뜻합니다.

undeclared는 접근 가능한 스코프에 변수 선언조차 되어있지 않은 상태를 의미합니다.

NaN은 Not-a-Number의 약어로 표현 불가능한 수치형의 결과를 의미합니다. 주로 수학적 연산이 실패했을 때 반환되는데, 예를 들어 `console.log(1 * 'String')` 의 결과가 NaN입니다.

<br/>
<hr/>
<br/>

## 개념 설명

### null

null 타입의 값은 null이 유일합니다. 프로그래밍 언어에서 null은 변수에 값이 없다는 것을 의도적으로 명시할 때 사용합니다. 때문에 이전에 참조하던 값을 더이상 참조하지 않겠다는 의미로, 이전에 할당되어있던 값에 대한 참조를 명시적으로 제거하겠다는 뜻입니다.

함수가 유효한 값을 반환할 수 없는 경우, 명시적으로 null을 반환하기도 합니다.

### undefined

undefined 타입의 값은 undefined가 유일합니다. var 키워드로 선언한 변수는 암묵적으로 undefined로 초기화합니다. 즉, 변수 선언에 의해 확보된 **메모리 공간을 처음 할당이 이뤄질 때까지 빈 상태로 내버려두지 않고 자바스크립트 엔진이 undefined로 초기화**합니다.

```jsx
var foo;
console.log(foo); // undefined
```

undefined는 직역하자면 “정의되지 않은”입니다. 자바스크립트의 undefined에서 말하는 정의란 변수에 값을 할당하여 변수의 실체를 명확히하는 것을 말합니다.

따라서 변수를 초기화할 때 개발자가 의도적으로 undefined를 사용하는 것은 혼란을 유발할 수 있습니다. 때문에 비어있다는 것을 개발자가 명시적으로 표현했다고 알 수 있는 null을 할당하는 것이 권장됩니다.

### undeclard

undeclared는 ‘선언되지 않은’ 이라는 뜻입니다. 말 그래도 현재 접근가능한 스코프에 없는, 선언되지 않은 식별자에 접근하려 한다면 오류가 발생할 것입니다.

```jsx
console.log(myVar); // ReferenceError: myVar is not defined
console.log(typeof myVar); // undeclared
```

`“use strict”` 를 사용하게 된다면 암묵적 전역변수 (undeclare 변수)를 방지할 수 있습니다.

```jsx
function test() {
  myVar = 20; // ❌ 원래는 실행 가능하지만, use strict 모드에서는 오류 발생!
  console.log(myVar);
}

test();
console.log(myVar); // 함수 밖에서도 접근 가능 (암묵적 전역)
```

### NaN

NaN은 Not-a-Number의 약자로, 숫자가 아닌 결과를 나타내는 특수한 값입니다. 주로 수학적 연산이 실패했을 때 반환됩니다.

```jsx
console.log(Number("Hello")); // NaN
console.log(parseInt("ABC")); // NaN
console.log(undefined + 10);
console.log(0 / 0);
```

**NaN의 특징**

1. NaN은 자기 자신과도 같지 않다

   NaN은 어떠한 값과도 같지 않은 유일한 값입니다. NaN이 정의할 수 없는 숫자이기 때문에 어떤 값과 비교해도 항상 false가 되도록 설계되어있습니다.

   ```jsx
   console.log(NaN === NaN); // false
   console.log(NaN == NaN); // false
   ```

1. isNaN()을 활용한 검사

   만일 NaN인지 확인하고자 한다면 isNaN 함수를 활용하면 됩니다.

   하지만 isNaN()은 타입 변환을 하기 때문에 주의가 필요합니다.

   ```jsx
   console.log(isNaN(NaN)); // true
   console.log(isNaN(10)); // false
   console.log(isNaN("Hello")); // true (문자열은 숫자가 아니므로 NaN 취급)
   ```

1. Number.isNaN()

   Number.isNaN()은 값이 실제로 NaN인지 정확히 검사합니다.

   ```jsx
   console.log(Number.isNaN(NaN)); // true
   console.log(Number.isNaN("Hello")); // false (문자열 자체는 NaN이 아님)
   console.log(Number.isNaN(10 / "A")); // true (10 / "A"는 NaN)
   ```
