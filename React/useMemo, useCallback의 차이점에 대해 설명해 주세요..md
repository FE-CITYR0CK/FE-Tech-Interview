# useMemo, useCallback의 차이점에 대해 설명해 주세요.

<br/>

## 면접용
useMemo와 useCallback은 성능 최적화를 위해 사용되는 React 훅입니다. 

useMemo는 **값의 계산 결과**를 메모이제이션하여 **불필요한 계산을 방지**하고, useCallback은 **함수**를 메모이제이션하여 **불필요한 함수 재생성을 방지**합니다. 

또한, 이 hook들은 **의존성 배열**에 있는 값이나 함수가 변경될 때만 다시 계산하거나 생성됩니다.

하지만 불필요한 hook 사용은 메모리를 낭비할 수 있기 때문에, 사용하기 전에 계산이나 함수가 자주 변경되는지 여부를 고려해야 합니다.

- 꼬리질문 1: 만약 함수들을 모두 다 useCallback으로 감싸면 더 좋은가요?
    - 자주 호출되지 않거나, 계산이 간단한 함수 같은 경우에는 사용하면 불필요합니다.
    - 메모이제이션 비용 발생, 코드 가독성 저하같은 문제점이 발생할 수 있습니다.
    - ex) 토글함수
<br/>
<hr/>
<br/>

## 개념설명
`useMemo`와 `useCallback`은 React에서 **성능 최적화**를 위해 자주 사용되는 hook입니다. 즉 **컴포넌트의 불필요한 렌더링을 방지하고 성능을 개선**하기 위해 사용됩니다.

두 hook은 기능은 비슷하지만, 어떤 것을 메모이제이션하고, 어떤 상황에서 사용하는지가 다릅니다. 


>**메모이제이션이란?**
>이전에 계산한 결과를 메모리에 저장하여, 동일한 계산을 반복하지 않도록 하는 기술.
>이전에 계산한 값을 재사용하여 애플리케이션의 속도를 높일 수 있다. ⇒ 성능을 향상시킬 수 있다.
>ex) DP


`useMemo`는 **특정 값의 계산 결과**를 메모이제이션합니다. 의존성이 변경되지 않는 한 동일한 값을 반환합니다. 따라서 **자주 변경되지 않는 값**에 대해 그 값을 메모이제이션하여 **불필요한 계산을 방지**하는 데 유용합니다.

`useCallback`은 **특정 함수**를 메모이제이션합니다. 의존성이 변경되지 않는 한 동일한 함수를 반환합니다. 따라서 **정의가 자주 변경되지 않는 함수를 재사용하여** **불필요한 함수 재생성을 방지**합니다.

각 Hook의 작동원리와 사용하는 상황에 대해 더 알아보겠습니다.

### useMemo의 정의

useMemo는 재랜더링 사이에 계산 결과를 메모이제이션할 수 있게 해주는  React Hook입니다.

```jsx
const cachedValue = useMemo(calculateValue, dependencies)
```

- calculateValue: 메모이제이션하려는 값을 계산하는 함수.
    - 순수 함수이며, 인자를 받지 않음 ⇒ 콜백 함수
    - 초기 렌더링에서 함수를 호출함.
    - 다음 렌더링에서 `dependencies`가 변경되지 않으면 이전에 계산된 값을 반환
    - 변경되면 calculateValue를 재호출
- dependencies: calculateValue 코드 내에서 참조된 모든 반응형 값들의 목록
    - 인라인 형태로 작성되어야 함 → [dep1, dep2, … ]
    - React는 `Object.js` 비교를 통해 의존성 배열에 있는 값들을 이전 값과 비교함
    
    >**Object.js 비교란?**
    >=== (엄격 동등 연산자) 와 비슷하지만 약간 다릅니다.
    >|  | === | Object.js |
    >| ---- | --- | --- |
    >| **NaN === NaN** | false | true |
    >| **+0 === -0** | true | false |
    >
    ><img width="610" alt="image" src="https://github.com/user-attachments/assets/6abd8b46-2f87-49d7-ab2a-2568e3b56451" />
    
    </aside>
    
- cachedValue (반환값)
    - 초기 렌더링에서 calculateValue를 호출한 결과를 반환
    - 다음 렌더링에서 종속성을 확인하고
        - 저장된 값을 반환하거나
        - calculateValue를 재호출한 값을 반환
- 사용 예시

```jsx
import { useMemo } from 'react';

function TodoList({ todos, tab }) {
  const visibleTodos = useMemo(
    () => filterTodos(todos, tab),
    [todos, tab]
  );
  // ...
}
```

### useMemo를 사용하는 상황

useMemo를 이용해서 성능을 개선하는 과정을 보겠습니다. 다음 코드에서는 상위 컴포넌트인 App에서 사용자의 입력 값이 변경될 때마다 PrimeNumbers 컴포넌트에 calculatePrimeNumbers 함수가 재실행됩니다. 

```jsx
const calculatePrimeNumbers = (max) => {
  let primes = [];
  for (let i = 2; i <= max; i++) {
    let isPrime = true;
    for (let j = 2; j * j <= i; j++) {
      if (i % j === 0) {
        isPrime = false;
        break;
      }
    }
    if (isPrime) primes.push(i);
  }

  return primes;
};

function PrimeNumbers ({ max }) {
  const primes = calculatePrimeNumbers(max);
  return (
    <div>
      <p>{max} 이하의 소수: {primes.join(', ')}</p>
    </div>
  );
};

function App() {
  const [max, setMax] = useState(10);
  return (
    <div>
      <input
        type="number"
        value={max}
        onChange={(e) => setMax(Number(e.target.value))}
      />
      <PrimeNumbers max={max} />
    </div>
  );
};

export default App;
```

만약 동일한 입력 값이 들어올 경우에도 함수는 재실행되어 불필요한 계산을 할 것입니다. 이런 상황에서 useMemo를 적용할 수 있습니다.

```jsx
const calculatePrimeNumbers = (max) => {
  // 소수 계산 로직은 동일
};

const PrimeNumbers = ({ max }) => {
  const primes = useMemo(() => calculatePrimeNumbers(max), [max]);
  return (
    <div>
      <p>{max} 이하의 소수: {primes.join(', ')}</p>
    </div>
  );
};
```

PrimeNumbers에서 useMemo의 인자로 콜백함수와 max가 담긴 배열을 넘겨주면, 입력 값이 변하지 않는 한 이전에 계산된 소수 목록을 재사용합니다. 이를 통해 성능을 향상시킬 수 있습니다.

다만 useMemo는 재랜더링에 대해서 불필요한 작업을 생략하여 성능을 최적화해주는 것이지, 처음 렌더링을 더 빠르게 만들어주는 것은 아닙니다. 그래서 상호작용이 잦은 경우 (페이지나 전체 섹션이 바뀌는 경우) 에는 불필요합니다.

만약 값이 자주 변경되지 않거나 계산이 간단한 경우에도 사용한다면 메모이제이션 자체가 비용을 초과할 수 있습니다.

useMemo로 최적화하는 것이 유용한 경우는 다음과 같습니다.

- useMemo에 입력하려는 계산이 매우 느리고, 의존성이 거의 변경되지 않는 경우
    - 위와 같은 경우입니다.
- 메모이제이션한 값을 특정 Hook의 의존성으로 이용할 경우
    - 의존성 값이 자주 변경되지 않아 계산을 최소화할 수 있습니다.
    
    ```jsx
    import React, { useState, useMemo, useEffect } from 'react';
    
    function Parent() {
      const [num, setNum] = useState(1);
    
      // useMemo로 계산된 값
      const memoizedValue = useMemo(() => {
        console.log('값을 계산 중...');
        return num * 1000;
      }, [num]);
    
      // memoizedValue를 useEffect의 의존성으로 사용
      useEffect(() => {
        console.log('memoizedValue가 변경되었습니다:', memoizedValue);
      }, [memoizedValue]);
    
      return (
        <div>
          <div>{memoizedValue}</div>
          <button onClick={() => setNum(num + 1)}>num 증가</button>
        </div>
      );
    }
    
    export default Parent;
    ```
    
- React.memo로 감싸진 컴포넌트에 props로 전달할 경우

### useCallback의 정의

useCallback은 리렌더링 간에 함수 정의를 메모이제이션하는 React Hook입니다.

```jsx
const cachedFn = useCallback(fn, dependencies)
```

- fn: 캐싱할 함수값으로, 어떤 인자나 반환값도 가질 수 있음
    - 첫 렌더링에서 이 함수를 반환함 → 함수 메모이제이션
    - 다음 렌더링에서 dependencies 값이 이전과 같다면 같은 함수 반환
    - dependencies 값이 변경되었다면, 이번 렌더링에서 전달한 함수를 반환 → 함수 메모이제이션
- `dependencies`: fn 내에서 참조되는 모든 반응형 값의 목록
    - useMemo와 동일한 형태, 비교방법을 가집니다.
- 사용 예시

```jsx
import { useCallback } from 'react';

function ProductPage({ productId, referrer, theme }) {
// 리렌더링 간에 메모이제이션할 함수 정의와 의존성 배열을 인자로 넘겨줍니다.
  const handleSubmit = useCallback((orderDetails) => {
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }, [productId, referrer]);
  // ...
```

### useCallback을 사용하는 상황

다음으로 useCallback을 이용해서 성능을 개선하는 과정을 보겠습니다. 

아래 코드처럼 자식 컴포넌트에 handleClick 함수를 props로 전달한다고 해봅시다. 부모 컴포넌트가 렌더링될 때마다 새로운 함수 객체가 생성되고, 자식 컴포넌트는 불필요하게 재렌더링될 수 있습니다.

```jsx
import React, { useState } from 'react';

function Child({ onClick }) {
  return <button onClick={onClick}>클릭</button>;
}

function Parent() {
  const [count, setCount] = useState(0);
	// Parent 컴포넌트가 렌더링될 때마다 새로 생성됨
  const handleClick = () => {
    console.log('버튼 클릭');
  };

  return (
    <div>
      <p>클릭 수: {count}</p>
      // 그에 따른 불필요한 리렌더링 발생
      <Child onClick={handleClick} />
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
}

export default Parent;
```

이런 상황에서 useCallback을 사용한다면, useCallback의 의존성 배열이 비어있기 때문에 handleClick 함수는 마운트 시 한 번만 생성되고, 그 이후로는 변경되지 않습니다. 

따라서 Parent 컴포넌트가 리렌더링되더라도 동일한 함수가 재사용되며, 자식 컴포넌트에서의 불필요한 재랜더링이 일어나지 않습니다.

```jsx
import React, { useState, useCallback } from 'react';

function Child({ onClick }) {
  return <button onClick={onClick}>클릭</button>;
}

function Parent() {
  const [count, setCount] = useState(0);

  // useCallback을 사용하여 handleClick 함수의 재생성을 방지
  const handleClick = useCallback(() => {
    console.log('버튼 클릭');
  }, []);

  return (
    <div>
      <p>클릭 수: {count}</p>
      <Child onClick={handleClick} />
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
}

export default Parent;
```

useCallback 역시 useMemo 처럼 상호작용이 잦은 상황에는 불필요합니다. 간단하거나 자주 변경되는 함수에 useCallback을 사용하면 불필요한 메모리 사용을 일으킬 수 있습니다.

다음과 같은 경우에 유용하게 사용할 수 있습니다.

- memo로 감싸진 컴포넌트에 props로 넘길 때
- 넘긴 함수가 어떤 Hook의 의존성으로 사용될 때
    
    ```jsx
    import React, { useState, useEffect, useCallback } from 'react';
    
    function Parent() {
      const [count, setCount] = useState(0);
      
      const handleClick = useCallback(() => {
        console.log('버튼 클릭됨');
      }, []); 
    
      // useEffect에서 handleClick을 의존성으로 사용
      useEffect(() => {
        console.log('컴포넌트가 마운트되었습니다.');
      }, [handleClick]); // handleClick이 변경될 때만 실행
    
      return (
        <div>
          <p>클릭 수: {count}</p>
          <button onClick={handleClick}>클릭</button>
          <button onClick={() => setCount(count + 1)}>증가</button>
        </div>
      );
    }
    
    export default Parent;
    ```
    

### 어떤게 비용이 높은 로직일까?

useMemo와 (useCallback)을 통해 최적화하는 것이 필요한 상황은 비용이 높은 로직을 다시 계산하는 것을 생략하고 싶을 때입니다.

코드의 비용에 대해 확인하고 싶다면, 해당 코드를 콘솔 로그로 감싸서 소요 시간을 측정할 수 있습니다.

예를 들어 기록된 시간이 클 때 (공식문서에서는 1ms 이상) 기존 코드에 useMemo, useCallback을 적용하여 소요 시간을 비교하고, 최적화된 방법을 찾을 수 있습니다.

```jsx
// useMemo를 사용하지 않았을 때
console.time('filter array');
const visibleTodos = filterTodos(todos, tab); 
console.timeEnd('filter array');

// useMemo를 사용했을 때
console.time('filter array');
const visibleTodos = useMemo(() => {
  return filterTodos(todos, tab); // todo와 tab이 변경되지 않은 경우 건너뜁니다.
}, [todos, tab]);
console.timeEnd('filter array');

// useCallback을 사용했을 때
const filterTodos = useCallback((todos, tab) => {
  console.time('filter array');
  const visible = todos.filter(todo => todo.tab === tab);
  console.timeEnd('filter array');
  return visible;
}, [todos, tab]);
```