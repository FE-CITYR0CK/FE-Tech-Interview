# React의 라이프 사이클에 대해 설명해 주세요. 

<br/>

## 면접용

리액트의 라이프사이클은 각 컴포넌트에 존재하는 마운트, 업데이트, 언마운트로 이루어진 주기를 뜻합니다. 라이프사이클은 다양한 메서드들로 관리할 수 있습니다.

클래스형 컴포넌트에서는 componentDidMount, componentDidUpdate, componentWillUnmount 메서드를 사용하여 상태 관리 및 리소스를 정리할 수 있습니다. 

함수형 컴포넌트에서는 16.8 버전부터 추가된 hook을 이용하여 같은 기능을 사용할 수 있습니다. useState와 useEffect 훅을 사용해 마운트, 업데이트, 언마운트에 해당하는 작업을 처리할 수 있습니다.

<br/>
<hr/>
<br/>

## 개념설명

리액트의 주요 특징 중 하나는 컴포넌트 기반 아키텍쳐라는 점입니다. 각 컴포넌트는 자체적인 라이프사이클을 다음처럼 가지고 있습니다.

> 1. 컴포넌트 마운트(생성)
>2. 컴포넌트 업데이트
>3. 컴포넌트 언마운트(제거) 

라이프사이클을 통해 컴포넌트가 언제 어떻게 동작하는지 예측할 수 있으며, 라이프사이클 API를 활용해 컴포넌트의 상태와 동작을 제어할 수 있습니다.

라이프사이클 메서드는 클래스형과 함수형 컴포넌트에서 다소 차이가 있습니다. React 16.8 버전 이후부터 Hooks 업데이트가 추가되면서 함수형 방식에서도 라이프사이클을 다룰 수 있게 되었습니다.

### 클래스형 컴포넌트에서의 라이프사이클 메서드

![image](https://github.com/user-attachments/assets/5f1c458b-e7e0-450d-9897-2a87c52e341d)


리액트 컴포넌트가 생겨나는 **Mount 단계**에서는 다음과 같은 라이프사이클 메서드가 생성되고, 실행됩니다.

- `constructor()`: 생성자 메서드
- `static getDerivedStateFromProps()`: 부모 컴포넌트에서 전달되는 props가 변경될 때마다 state를 업데이트해주는 메서드
    - 함수형 컴포넌트에서 useState의 set함수를 렌더링 과정 중에 사용하는 것
    
    ```jsx
    /** 자식 컴포넌트 */
    class Counter extends Component {
      constructor(props) {
        super(props);
        // 초기 state 설정
        this.state = {
          count: 0
        };
      }
    
      // static getDerivedStateFromProps 메서드
      static getDerivedStateFromProps(nextProps, prevState) {
        // 부모에서 전달되는 count 값이 변경될 때마다 상태 업데이트
        if (nextProps.count !== prevState.count) {
          return {
            count: nextProps.count
          };
        }
        // 변경 사항이 없으면 null을 반환하여 상태를 변경하지 않음
        return null;
      }
    
      render() {
        return (
          <div>
            <h1>Current Count: {this.state.count}</h1>
          </div>
        );
      }
    }
    
    /** 부모 컴포넌트 */
    class Parent extends Component {
      constructor(props) {
        super(props);
        this.state = {
          count: 0
        };
      }
    
      increment = () => {
        this.setState({ count: this.state.count + 1 });
      };
    
      render() {
        return (
          <div>
            <button onClick={this.increment}>Increment</button>
            <Counter count={this.state.count} />
          </div>
        );
      }
    }
    
    ```
    
- `render()`: JSX를 리턴하면 HTML로 컴파일되어 보여지게 되는 라이플 사이클 메서드입니다.
    - state, props가 변화되었을 때 재렌더링됩니다.
- `componentDidMount()`: 컴포넌트가 생성되면 트리거되는 메서드입니다.
    - 컴포넌트가 생겼을 때 한번만 실행되며, 변화가 일어나도 재실행되지 않습니다.

다음으로, 컴포넌트 **Update 단계**에서 실행되는 메서드는 다음과 같습니다.

- static getDerivedStateFromProps()
- `shouldComponentUpdate()`: return 하는 값(boolean)에 따라 컴포넌트의 렌더링 여부가 결정됩니다.
    
    ```jsx
    shouldComponentUpdate(nextProps, nextState){
    	if (this.state !== nextState) {
    		return true;
    	}
    	return false;
    }
    ```
    
- render()
- `getSnapshotBeforeUpdate()`: render 메서드가 호출된 후에, 컴포넌트가 업데이트되기 직전 실행됩니다.
    - 업데이트 전 DOM의 상태를 스냅샷으로 캡처할 수 있습니다.
    - return값은 componentDidUpdate()의 인자가 됩니다.
    - ex) 스크롤 위치 추적
    - 함수형 컴포넌트에서 사용할 수 없습니다.
- `componentDidUpdate()`: 재랜더링시 실행되는 메서드입니다.
    - 컴포넌트가 업데이트된 후 마지막으로 실행됩니다.
    - props, state가 변경되어 → 컴포넌트 렌더링이 끝나고 → DOM이 업데이트된 뒤에 → 실행됩니다.
    - DOM 업데이트 후에 추가적인 작업을 할 때 유용합니다.
    
    ```jsx
    class ScrollTracker extends Component {
      state = { scrollY: 0 };
    
      // getSnapshotBeforeUpdate: 업데이트 직전에 호출되어 스크롤 위치 캡처
      getSnapshotBeforeUpdate(prevProps, prevState) {
        return window.scrollY; // 현재 스크롤 위치 반환
      }
    
      // componentDidUpdate: getSnapshotBeforeUpdate에서 반환된 값(snapshot) 사용
      componentDidUpdate(prevProps, prevState, snapshot) {
        window.scrollTo(0, snapshot); // 이전 스크롤 위치로 복원
      }
    
      render() {
        return (
          <div>
            <h1>Scroll Tracker</h1>
            <div style={{ height: '1500px' }}>
              <p>Scroll down to track your scroll position.</p>
            </div>
          </div>
        );
      }
    }
    ```
    

 

마지막으로 컴포넌트 **Unmount 단계**에서 생성되고, 실행되는 메서드는 다음과 같습니다.

- `componentWillUnmount()`: 컴포넌트가 사라질 때 실행되는 메서드입니다.
    - 기존 컴포넌트에 할당해둔 것을 해제할 때 사용합니다.
    - Chart.js와 같은 외부 라이브러리를 사용할 때, 컴포넌트가 제거되기 전에 인스턴스를 해제하여 메모리 누수를 막을 수 있습니다.
    
    ```jsx
    componentWillUnmount() {
      // 차트 인스턴스를 파괴하여 메모리 해제
      if (this.chart) {
        this.chart.destroy();
      }
    }
    ```
    

### 함수형 컴포넌트에서의 라이프사이클 메서드

![image 1](https://github.com/user-attachments/assets/0a26a0cd-94d0-4af9-b303-2f451b6db484)


함수형 컴포넌트에서는 클래스형보다 적은 개수의 메서드를 사용합니다. 또한 기존에 사용되는 메서드들을 hook을 통해 구현할 수 있습니다.

컴포넌트 마운트 시 실행되는 함수들을 hook으로 어떻게 대체할 수 있는지 살펴보겠습니다.

- constructor
    - constructor() 메서드를 사용하는 클래스형과 달리 useState hook을 사용합니다.
    
    ```jsx
    //class
    class Example extends React.Component {
    	constructor(props) {
          	super(props);
          	this.state = { count: 0 } ;
        }
    }
    
    // function => Hooks
    const Example = () => {
      const [count,setCount] = useState(0);
    }
    ```
    
- render()
    - render 메서드를 명시적으로 쓰지 않고도 렌더링할 수 있습니다.
- componentDidMount()
    - useEffect hook을 사용하여 해당 메서드를 구현합니다.
    - 빈 의존성 배열을 사용해야 같은 기능을 구현할 수 있습니다.
    
    ```jsx
    // Class
    class Example extends React.Component {
        componentDidMount() {
            ...
        }
    }
    
    // Hooks
    const Example = () => {
        useEffect(() => {
            ...
        }, []);
    }
    ```
    

---

++ `static getDerivedStateFromProps()`

- useState의 set 함수가 렌더링 중에 호출될 때와 같은 방식으로 동작합니다.

```jsx
const [count, setCount] = useState(0);
const [isClicked, setIsClicked] = useState(false);

// 렌더링 중 상태를 업데이트
useEffect(() => {
  if (!isClicked) {
    setIsClicked(true); // 상태 업데이트로 컴포넌트가 리렌더링 될 것
    setCount(10); // 상태 업데이트로 count도 변경
  }
}, []); // 마운트 시에만 실행되도록 빈 배열
```

<br/>

다음으로 **컴포넌트 업데이트 시** 실행되는 함수들을 hook으로 대체한 모습입니다.

- render()
- componentDidUpdate()
    - useEffect를 사용하여 동일하게 동작할 수 있습니다.
    
    ```jsx
    // class
    constructor(props) {
      super(props);
      this.state = { count: 0 };
    }
    
    // componentDidUpdate는 props나 state가 변경된 후에 호출됩니다.
    componentDidUpdate(prevProps, prevState) {
      if (prevState.count !== this.state.count) {
        console.log('Count has updated to: ', this.state.count);
      }
    }
    
    // function
    const [count, setCount] = useState(0);
    
    // useEffect는 컴포넌트가 렌더링된 후에 실행됩니다.
    useEffect(() => {
      console.log('Count has updated to: ', count);
    }, [count]); // count가 변경될 때마다 실행됩니다.
    ```
    

마지막으로 **컴포넌트 언마운트 단계**에서 실행되는 함수들을 hook으로 대체한 모습입니다.

- componentWillUnmount
    - useEffect, CleanUp 함수를 통해 구현할 수 있습니다.
    
    ```jsx
    const chartRef = useRef(null); // Chart.js 차트를 그릴 canvas 요소를 참조
    
    useEffect(() => {
      // 차트 생성
      const chart = new Chart(chartRef.current, {
    		...
      });
    
      // 컴포넌트가 언마운트될 때 차트 인스턴스를 파괴하는 클린업 함수
      return () => {
        if (chart) {
          chart.destroy(); // 차트 인스턴스 정리
        }
      };
    }, []); // 빈 배열로 설정하면 컴포넌트 마운트 시 한 번만 실행되고 언마운트 시 클린업 함수 실행
    
    ```