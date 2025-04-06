# this의 용법에 대해 아는 대로 설명해 주세요.

<br/>

## 면접용
자바스크립트에서 this는 **함수가 호출되는 방식**에 따라 가리키는 대상이 다르게 설정됩니다. 

먼저 **일반 함수**로 호출될 때 this는 전역 객체를 참조하고, **메서드로 호출**될 때 this는 해당 메서드를 호출한 객체를 가리킵니다. **생성자 함수**에서는 this가 새로 생성된 객체를 참조하며, **특정 메서드(call, apply, bind)**를 사용하면 this를 원하는 객체로 명시적으로 지정할 수 있습니다. 

- 꼬리질문 1: 일반 함수 호출에서 this가 참조하는 **전역 객체**에 대해 설명해주세요.
    - 전역객체는 환경에 따라 다른데, 브라우저에서는 window, Node.js 환경에서는 global을 가리킵니다.
    - window는 브라우저에서 제공하는 전역 객체로 브라우저의 탭이나 창에 해당하며, window.alert()과 같이 여러 브라우저 API를 호출할 수 있는 함수들을 제공합니다.
- 꼬리 질문 2: **call, apply, bind 메서드의 차이점**에 대해 설명해주세요.
    - call과 apply는 함수를 즉시 호출하고 bind는 새로운 함수를 반환하여 나중에 호출할 수 있게 해줍니다.
    - call과 apply는 인수 전달 방식에서 개별적으로 전달하거나 배열 형태로 전달하는 차이가 있습니다.

<br/>
<hr/>
<br/>

## 개념설명

### 자바스크립트에서의 this

여러 언어(cpp, java, …) 에서 사용되는 `this` 키워드는 참조 변수입니다. 해당 참조 변수는 객체 자신(instance)을 가리킵니다. 

```java
public Class Person {

  private String name;

// 매개변수와 객체의 멤버변수의 이름이 같을 경우 구분하기 위해 사용합니다.
  public Person(String name) {
    this.name = name;
  }
}
```

반면 **자바스크립트에서의 this**는 좀 더 다양한 객체를 가리킬 수 있습니다. 

자바스크립트에서는 **this를 담은 함수가 호출되는 방식에 따라 가리키는 대상이 다르게 설정**됩니다. 방식은 다음의 4가지가 있습니다.

1. **일반 함수로 호출될 경우**
2. **메서드로 호출될 경우**
3. **생성자 함수로 호출**
4. **특정 메서드(call, apply 등)로 호출**

### 일반 함수 호출에서의 this 동작

다음 코드를 실행해보면 아래 이미지와 같은 결과가 나옵니다.

```java
function foo() {
	console.log(this);
}

foo(); // 일반 함수로 호출 -> 전역 객체를 가리킴
```

![819c7c6a-f11f-4ad6-9f87-922d3ae3f0a2](https://github.com/user-attachments/assets/9fdd1333-0a9d-4f0f-bb8c-0acab070f44a)


일반 함수로 호출 시 this가 가리키는 객체는 전역 객체입니다. ⇒ window 객체를 가리킵니다.


>**window 객체란?**
>- 브라우저에서 제공하는 전역 객체 → 브라우저의 탭, 창
>- 브라우저 환경에서는 window, Node.js 환경에서는 global
>- 모든 전역 변수와 전역 함수가 속하는 객체 → 자바스크립트에서 변수/함수 전역 선언 시 window 객체의 속성이 됨
>- 여러 브라우저 API와 UI 기능을 다룰 수 있는 메서드 제공 (애플리케이션 전역에서 접근 가능)
>    - window.alert()
>    - window.setTimeout()

위의 예시와 같이 일반 전역함수의 경우와 더불어 내부함수, 메소드의 내부함수, 콜백함수의 경우에도 this는 전역 객체에 바인딩됩니다. 즉, 다시 말하면 내부 함수는 어디에서 선언되든 전역객체가 this에 바인딩 된다라고 말할 수 있습니다. 

```jsx
// 1. 내부함수
function foo() {
  console.log("foo's this: ",  this);  // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}

foo();

// 2. 메서드의 내부함수
var value = 1;

var obj = {
  value: 100,
  foo: function() {
    console.log("foo's this: ",  this);  // obj
    console.log("foo's this.value: ",  this.value); // 100
    function bar() {
      console.log("bar's this: ",  this); // window
      console.log("bar's this.value: ", this.value); // 1
    }
    bar();
  }
};

obj.foo();

// 3. 콜백함수
var value = 1;

var obj = {
  value: 100,
  foo: function() {
    console.log("foo's this: ",  this);  // obj
    console.log("foo's this.value: ",  this.value); // 100
    function bar() {
      console.log("bar's this: ",  this); // window
      console.log("bar's this.value: ", this.value); // 1
    }
    bar();
  }
};

obj.foo();

```

### this에 전역객체가 바인딩 될 때의 문제점과 해결방안

전역객체가 this에 바인딩 될 때, 의도하지 않은 동작이나 버그가 발생할 수 있습니다. 

아래의 코드에서 this는 전역 객체를 참조하고 있는데, 함수를 실행하면 someVar가 전역 객체의 속성으로 추가될 수 있습니다. 이는 다른 코드에서 해당 값을 의도치 않게 참조하는 문제점이 될 수 있습니다.

```jsx
function setGlobalVar() {
  this.someVar = 100;
}
setGlobalVar();
console.log(someVar);  // 100 (전역 객체에 속성이 추가됨)
```

다음 코드에서도 setTimeout 내의 콜백 함수인 function이 전역 객체를 참조하기 때문에, this.name은 Alice가 아닌 전역 객체를 참조합니다. 따라서 해당 콘솔은 undefined를 출력합니다.

```jsx
const obj = {
  name: 'Alice',
  greet: function() {
    setTimeout(function() {
      console.log(this.name);  // this는 전역 객체를 참조하므로 undefined
    }, 1000);
  }
};

obj.greet();
```

만약 this가 전역객체를 참조하여 예상치 못한 동작이 일어나는 것을 피하고 싶다면 다음과 같은 방법이 있습니다.

1. **객체의 this를 변수에 저장해서 사용하기**

this를 다른 변수에 저장하고, 해당 변수를 사용하면 전역객체를 참조하는 것을 피할 수 있습니다. setTimeout 안의 콜백함수에서 this는 전역 객체를 참조하지만, this를 self와 같은 변수에 저장

```jsx
const obj = {
  name: 'Alice',
  greet: function() {
    // this를 변수에 저장 -> obj를 참조하게 됨
    const self = this;
    setTimeout(function() {
      console.log('Hello, ' + self.name); // 'Alice'
    }, 1000);
  }
};

obj.greet();
```

2. **화살표 함수 사용하기**

화살표 함수는 자신만의 this를 가지지 않고, 외부 함수에서 상속받아 사용합니다.

setTimeout에 전달된 화살표 함수는 외부 함수 greet의 this를 사용합니다. 해당 this는 obj를 가리키므로, 화살표 함수에서도 this는 obj를 참조합니다.

```jsx
const obj = {
  name: 'Bob',
  greet: function() {
    setTimeout(() => {
      console.log('Hello, ' + this.name); // 'Bob'
    }, 1000);
  }3
};

obj.greet();
```

3. **call, bind, apply로 this 설정하기**

3번의 경우는 밑에서 동작을 더 자세히 보겠습니다.

### 메서드 호출에서의 this 동작

객체(인스턴스)의 메서드로 호출 시, this가 가리키는 객체는 해당 메소드를 호출한 객체입니다.

```jsx
const obj = {
	foo: foo
};

obj.foo(); // obj 객체 메서드로 호출 -> obj 객체를 가리킴
```

![13de773c-1a7d-4995-b0d6-c10652728ea2](https://github.com/user-attachments/assets/0511aa31-4fa2-432b-a4f4-b2722712f71e)

<br/>

![image](https://github.com/user-attachments/assets/9aaf6ad2-85eb-4116-a3d0-ea84dbc16189)


만약 해당 객체가 프로토타입 객체이고, 프로토타입 객체의 메소드로 this를 호출하면 어떻게 될까요?

마찬가지로 해당 메소드를 호출한 객체에 바인딩됩니다.

```jsx
function Person(name) {
	this.name = name;
}

Person.prototype.getName = function() {
	return this.name;
}

Person.prototype.name = 'Kim';
// 프로토타입 객체에서 name 프로퍼티를 찾아 반환
console.log(Person.prototype.getName()); // Kim
```

![image 1](https://github.com/user-attachments/assets/c7fc880a-371a-426b-832c-a8b2bcda5cef)

### 생성자 함수로 호출

javascript에서 생성자 함수는 일반 함수에 new를 붙여 호출하고, 새로운 객체를 생성할 때 사용됩니다. 생성자 함수 내부에서 this는 새로 생성될 객체를 가리킵니다.

생성자 함수에서 반환문이 없는 경우, this에 바인딩 된 객체(새로 생성된 객체)가 반환됩니다.

```jsx
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// this는 새로 만들어진 person1 객체를 참조한다.
const person1 = new Person('Alice', 25);
console.log(person1.name);  // 'Alice'
console.log(person1.age);   // 25

// 만약 new 키워드를 사용하지 않을 경우 this는 전역 객체를 참조하므로(1번) 주의!
const person2 = Person('Bob', 30); // new를 사용하지 않음
console.log(name);  // 'Bob' - 전역 객체의 name 속성에 값이 할당됨
console.log(age);   // '30' - 전역 객체의 age 속성에 값이 할당됨
```

만약 생성자 함수가 return문을 통해 명시적으로 반환하는 객체가 있다면, 해당 객체가 this로 바인딩된 객체 대신 반환됩니다.

```jsx
function Person(name, age) {
  this.name = name;
  this.age = age;
  return { name: 'Custom Name' };  // 새로운 객체를 반환
}

const person3 = new Person('Charlie', 35);
console.log(person3.name);  // 'Custom Name' (새로운 객체가 반환됨)
```

### 특정 메서드(call, apply, bind)로 호출

위의 3가지 예시에서 보았듯이 this에 바인딩될 객체는 함수를 어디에서 호출하느냐에 따라 결정되며, 이는 자바스크립트 엔진이 암묵적으로 수행합니다. ⇒ `암묵적 this 바인딩`

암묵적 this 바인딩 이외에 apply, call, bind 메소드를 통해 this를 특정 객체에 명시적으로 바인딩할 수 있습니다.

- 이 메서드들은 모든 함수 객체의 프로토타입 객체인 Function.prototype 객체의 메서드입니다.
    - Function.prototype.apply, Function.prototype.call, Function.prototype.bind
    - 호출하는 주체는 함수이므로 본질적인 기능은 함수 호출입니다.

call과 apply 메소드는 함수를 호출할 때 this 값을 지정하여 호출할 수 있게 해줍니다. 즉, 즉시 호출되는 함수입니다. 두 메서드는 동작은 비슷하지만 인수를 어떻게 전달하는지가 차이점입니다.

call 메서드의 경우 인수를 쉼표로 구분지어진 개별 인수로 전달합니다. 

```jsx
func.call(thisArg, arg1, arg2, ...);
```

- thisArg: this로 사용할 객체
- arg1, arg2, … : 함수에 전달할 인수들

```jsx
function greet(name, age) {
  console.log(`Hello, my name is ${name} and I am ${age} years old.`);
}

const person = { name: 'Alice' };

// greet 함수를 호출하면서 this를 person 객체로 설정하고, 2개의 인수를 개별적으로 전달함
greet.call(person, 'Alice', 25);  // Hello, my name is Alice and I am 25 years old.
```

apply 메서드의 경우 인수를 배열 형태로 전달합니다.

```jsx
func.apply(thisArg, [argsArray]);
```

- thisArg: this로 사용할 객체
- argsArray: 함수에 전달할 인수 배열

```jsx
function greet(name, age) {
  console.log(`Hello, my name is ${name} and I am ${age} years old.`);
}

const person = { name: 'Bob' };

// greet 함수를 호출하면서 this를 person 객체로 설정하고, 인수는 배열로 전달
greet.apply(person, ['Bob', 30]);  // Hello, my name is Bob and I am 30 years old.
```

ES5에 추가된 bind 메서드의 경우 위의 두 메서드와 동작이 다릅니다. 

- 함수를 호출하지 않고 새로운 함수를 반환합니다 → 반환된 함수를 나중에 호출할 수 있도록 해줍니다.
    - 함수가 즉시 호출되지 않습니다.
- 원본 함수가 호출될 때 this와 인수를 미리 바인딩합니다. → 바인딩 된 새로운 함수를 반환합니다.

```jsx
const boundFunc = func.bind(thisArg, arg1, arg2, ...);
```

- thisArg: this로 사용할 객체
- arg1, arg2, ...: 함수에 전달할 인수들
- boundFunc 객체에 bind를 통해 반환된 새로운 함수가 입력됨

```jsx
function greet(name, age) {
  console.log(`Hello, my name is ${name} and I am ${age} years old.`);
}

const person = { name: 'Charlie' };

// 'this'를 person 객체로 설정하고, 인수를 미리 바인딩
// name과 age를 미리 바인딩한 새로운 함수를 반환 -> boundGreet()
const boundGreet = greet.bind(person, 'Charlie', 35);

boundGreet();  // Hello, my name is Charlie and I am 35 years old.
```