# async/await에 대해 설명해주세요.

<br/>

## 면접용

async/await은 JavaScript에서 비동기 작업을 처리하는 데 사용되는 문법입니다. 기본적으로 Promise를 기반으로 동작하며, 비동기 작업을 동기 작업처럼 간단하고 직관적으로 표현할 수 있게 해줍니다.

async 키워드는 함수가 항상 Promise를 반환하도록 만듭니다. 그리고 그 함수 내부에서 await 키워드를 사용하면 Promise가 처리될 때까지 실행을 잠시 멈추고 결과를 기다릴 수 있습니다.

예를 들어, 서버에서 데이터를 가져오는 작업을 처리한다고 가정해보겠습니다. 이전에는 콜백 함수나 .then() 체인을 사용해서 처리했는데, 이 방식은 코드가 복잡하고 가독성이 떨어지는 문제가 있었습니다. 하지만 async/await을 사용하면 작업이 완료될 때까지 기다렸다가 다음 작업을 진행할 수 있기 때문에 코드의 흐름을 더 쉽게 이해할 수 있습니다.

이를 통해 비동기 작업의 실행 순서를 명확히 표현할 수 있고, 에러 처리도 try-catch 구문을 사용해 더 구조적으로 할 수 있습니다. 다만 await은 async 함수 내부에서만 사용할 수 있고, 병렬로 여러 작업을 처리하려면 Promise.all 같은 방법을 함께 사용해야 한다는 제한점이 있습니다.

<br/>

<hr/>

<br/>

## 개념 설명

ES6에서는 비동기 처리를 위한 패턴으로 Promise를 도입했습니다. 

Promise 생성자 함수를 new 연산자와 함께 호출하면 Promise 객체를 생성합니다. 이는 비동기 처리를 수행할 콜백함수를 인수로 전달받는데, 이 콜백 함수는 resolve와 reject함수를 인수로 전달받습니다.

```tsx
const promiseGet = (url) => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();

    xhr.open("GET", url, true); // 비동기 GET 요청 설정
    xhr.send(); // 요청 전송

    xhr.onload = () => {
      if (xhr.status === 200) {
        resolve(JSON.parse(xhr.response)); // 성공 시 JSON으로 변환 후 resolve
      } else {
        reject(new Error(`Error: ${xhr.status}`)); // 실패 시 상태 코드와 함께 reject
      }
    };

    xhr.onerror = () => {
      reject(new Error("Network Error")); // 네트워크 에러 처리
    };
  });
};

// 사용 예시
const apiURL = "https://jsonplaceholder.typicode.com/posts";

promiseGet(apiURL)
  .then((data) => console.log("데이터 요청 성공:", data))
  .catch((error) => console.error("데이터 요청 실패:", error));

```

프로미스는 다음과 같은 3가지 상태 정보를 갖습니다.

- Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태
- Fulfilled(이행) : 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태
- Rejected(실패) : 비동기 처리가 실패하거나 오류가 발생한 상태

즉 프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체로, 이 전 코드에서는 비동기 작업을 원하는대로 처리하기 어려웠던 단점을 보완해줍니다.

<br/>

이러한 **프로미스를 활용해 비동기 작업을 동기 작업처럼 처리해 주는 것**이 ayncs/await 문법입니다. async/await는 프로미스를 간결하게 사용할 수 있도록 해 비동기 작업을 도와줍니다.

사용법 또한 간단한데, 함수명 앞에 async를 붙여주고, 비동기 작업이지만 동기 작업처럼 처리하고 싶은 부분 앞에 await를 붙여주면 됩니다.

```tsx
// 기존 Promise.then() 형식
function main() {
  delay(1000)
      .then(() => {
        return delay(2000);
      })
      .then(() => {
        return Promise.resolve('끝');
      })
      .then(result => {
        console.log(result);
      });
}

// 메인 함수 호출
main();
```

<br/>

delay가 순차적으로 처리되게 됩니다. 하지만 **async 함수 자체는 비동기적으로 처리되어 다른 함수들은 실행되고 있습니다.**

```tsx
// async/await 방식
async function main() {
  await delay(1000);
  await delay(2000);
  const result = await Promise.resolve('끝');
  console.log(result);
}

// 메인 함수 호출
main();
```
<br/>

async 키워드는 함수가 항상 Promise를 반환하도록 만듭니다. 그리고 그 함수 내부에서 await 키워드를 사용하면 Promise가 처리될 때까지 **실행을 잠시 멈추고 결과를 기다립니다.**

<img width="651" alt="스크린샷 2025-01-22 오후 8 32 05" src="https://github.com/user-attachments/assets/6766c623-7686-4ea0-8304-8e562dce7659" />

또한 await 키워드는 Promise 객체가 완료될 때까지 코드 실행을 일시 중지하므로, try-catch 블록 안에서 사용하여 에러 처리를 할 수 있습니다. fetch에서 네트워크 에러가 발생할 경우, await 이후의 코드는 실행되지 않으며, catch 블록으로 제어가 넘어가 에러를 처리할 수 있습니다.

<br/>
