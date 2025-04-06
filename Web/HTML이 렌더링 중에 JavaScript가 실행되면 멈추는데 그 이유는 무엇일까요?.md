# HTML이 렌더링 중에 JavaScript가 실행되면 멈추는데 그 이유는 무엇일까요?

<br/>

## 면접용

HTML이 렌더링되는 도중에 JavaScript가 실행되면 브라우저는 렌더링을 멈추고 JavaScript 코드를 먼저 실행합니다. 그 이유는 브라우저가 HTML을 해석하고 화면을 그리는 과정(렌더링)과 JavaScript 실행이 동일한 메인 스레드에서 처리되기 때문입니다.

자바스크립트는 기본적으로 싱글 스레드 환경에서 동작하기 때문에, 실행 중인 코드가 끝나야 다음 작업을 진행할 수 있습니다. 그래서 HTML을 파싱하는 도중 <script> 태그를 만나면, 브라우저는 먼저 해당 JavaScript를 실행하고, 실행이 끝난 후에야 다시 HTML 파싱을 이어갑니다.

이 때문에 스크립트 실행이 오래 걸리면 화면 렌더링이 지연될 수 있습니다. 이를 방지하기 위해 defer 또는 async 속성을 사용하면 브라우저가 HTML을 계속 파싱하면서 JavaScript를 비동기적으로 실행할 수 있습니다.

<br/>

<hr/>
<br/>

## 개념 설명

### **JavaScript 실행과 HTML 파싱의 관계**

브라우저는 기본적으로 **싱글 스레드(Single Thread)**에서 동작합니다. 즉, 한 번에 하나의 작업만 실행할 수 있습니다. 이 때문에 HTML을 파싱하는 작업과 JavaScript를 실행하는 작업이 같은 메인 스레드에서 처리됩니다.

브라우저가 HTML을 읽는 도중 `<script>` 태그를 만나면, **HTML 파싱을 멈추고 해당 JavaScript를 먼저 다운받고 실행합니다.** JavaScript 실행이 완료된 후에야 다시 HTML을 해석하고 화면을 렌더링할 수 있습니다. 이 과정에서 JavaScript 실행 시간이 길어지면 렌더링이 지연될 수 있으며, 사용자가 빈 화면을 보게 되는 경우도 발생할 수 있습니다.

이런 브라우저의 동작 방식은 두 가지 중요한 이슈를 만듭니다.

1. 스크립트에서는 스크립트 아래에 있는 DOM 요소에 접근할 수 없습니다. 따라서 DOM 요소에 핸들러를 추가하는 것과 같은 여러 행위가 불가능해집니다.
    
    ```html
    <!DOCTYPE html>
    <html lang="ko">
    <head>
        <title>DOM 접근 문제</title>
        <script>
            document.getElementById("myButton").addEventListener("click", function() {
                alert("버튼 클릭됨!");
            });
        </script>
    </head>
    <body>
    		<!<script>가 실행될 때 아직 <button> 요소가 생성되지 않았기 때문에 오류 발생>
        <button id="myButton">클릭하세요</button>
    </body>
    </html>
    ```
    
2. 페이지 위쪽에 용량이 큰 스크립트가 있는 경우 스크립트가 페이지를 ‘막아버립니다’. 페이지에 접속하는 사용자들은 스크립트를 다운받고 실행할 때까지 스크립트 아래쪽 페이지를 볼 수 없게 됩니다.

### **해결 방법**

이러한 문제를 방지하기 위해 `<script>` 태그에 `async` 또는 `defer` 속성을 추가하면 JavaScript가 HTML 파싱을 막지 않도록 조정할 수 있습니다. Javascript 다운은 네트워크 스레드가 실행하기 때문에 병렬로 다운받을 수 있습니다. 기존에는 script.js를 만나면 다운받고 실행해서 다운받을 js 용량이 크거나 하면 성능 저하가 있을 수 있었는데, async, defer는 script.js를 병렬로 다운받아 문제를 해결합니다.

- **`async`**
    
    JavaScript 파일을 HTML과 **병렬로 다운로드**한 후, 다운로드가 완료되는 즉시 실행합니다. 하지만 실행 순서는 보장되지 않으므로, 스크립트 간의 의존성이 있을 경우에는 주의해야 합니다.
    
    ```jsx
    <!DOCTYPE html>
    <html lang="ko">
    <head>
        <meta charset="UTF-8">
        <title>JavaScript 실행 테스트</title>
        <script src="script.js" async></script> <!-- HTML과 병렬로 다운로드, 다운로드 후 즉시 실행 -->
    </head>
    <body>
        <h1>안녕하세요</h1>
        <p>이 문장은 script 실행과 관계없이 렌더링될 수 있습니다.</p>
    </body>
    </html>
    
    ```
    
- **`defer`**
    
    JavaScript 파일을 HTML과 **병렬로 다운로드**하면서도, HTML 파싱이 **완전히 끝난 후** 실행됩니다. 따라서 DOM을 조작하는 코드가 있을 때 적합하며, 여러 개의 `defer` 스크립트는 **작성된 순서대로 실행됩니다.**
    
    ```html
    <!DOCTYPE html>
    <html lang="ko">
    <head>
        <meta charset="UTF-8">
        <title>JavaScript 실행 테스트</title>
        <script src="script.js" defer></script> <!-- HTML과 병렬로 다운로드, HTML 파싱 후 실행 -->
    </head>
    <body>
        <h1>안녕하세요</h1>
        <p>이 문장은 script 실행 전에 이미 렌더링될 수 있습니다.</p>
    </body>
    </html>
    
    ```
    

- **`DOMContentLoaded`** 이벤트
    
    `defer` 속성과 유사한 방식으로, **HTML이 완전히 파싱된 후 JavaScript를 실행**하고 싶다면 `DOMContentLoaded`이벤트를 사용할 수도 있습니다.
    
    HTML이 모두 로드되고, `defer` 스크립트까지 실행된 후 `DOMContentLoaded` 이벤트가 발생합니다.
    
    ```html
    <!DOCTYPE html>
    <html lang="ko">
    <head>
        <meta charset="UTF-8">
        <title>JavaScript 실행 테스트</title>
    </head>
    <body>
        <h1>안녕하세요</h1>
        <script>
            document.addEventListener("DOMContentLoaded", function() {
                document.getElementById("myButton").addEventListener("click", function() {
                    alert("버튼 클릭됨!");
                });
            });
        </script>
        
        <button id="myButton">클릭하세요</button>
    </body>
    </html>
    ```