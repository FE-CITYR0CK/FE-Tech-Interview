# CORS에 대처하는 방법과 우회하는 방법에 대해 설명해 주세요.

<br/>

## **면접용**

브라우저는 보안상의 이유로 출처가 다른 도메인으로 요청을 보낼 때 CORS 정책을 따릅니다. 이로 인해 서버가 명시적으로 허용하지 않으면 요청이 차단됩니다.

CORS에 대처하는 방법은 주로 서버에서 `Access-Control-Allow-Origin` 헤더를 설정해 클라이언트의 요청을 허용하는 방식입니다. 만약 서버를 직접 수정할 수 없는 상황이라면, 프록시 서버를 두고 동일 출처처럼 요청을 중계하거나, 개발 환경에서는 브라우저 플러그인이나 로컬 프록시 설정을 통해 우회하기도 합니다.

<br/>

<hr/>
<br/>

## **개념 설명**

### CORS는 무엇일까요?

- **Cross-Origin Resource Sharing**의 약자입니다.
- 브라우저가 **다른 출처(origin)의 리소스를 요청할 때**, 서버가 허용한 경우에만 응답을 허용하는 보안 메커니즘입니다.
- **같은 출처란?**
→ 프로토콜, 도메인, 포트까지 모두 같아야 합니다. ( **Origin : Protocol + Host + Port** )

```html
https://domain.com:3000/user?query=name&page=1#first
```

- Protocol(Scheme) : http, https ( https:// )
- Host : 사이트 도메인 ( www.domain.com )
- Port : 포트 번호 ( :3000 )
- Path : 사이트 내부 경로 ( /user )
- Query string : 요청의 key와 value값 ( ?query=name&page=1 )
- Fragment : 해시 태크 ( #first )


1. **`<img>`, `<video>`, `<script>`, `<link>` 태그 등은 기본적으로 Cross-Origin 정책을 지원합니다.**

- `<link>` 태그의 href 에서 다른 사이트의 .css 리소스에 접근하는 것이 가능
- `<img>` 태그의 src 에서 다른 사이트의 .png, .jpg 등의 리소스에 접근하는 것이 가능
- `<script>` 태그의 src 에서 다른 사이트의 .js 리소스에 접근하는 것이 가능 (`type="module"` 속성은 제외)


2. **XMLHttpRequest, Fetch API 스크립트는 기본적으로 Same-Origin 정책을 지원합니다.**

- 다른 도메인의 소스에 대해 자바스크립트 ajax 요청 API 호출시
- 웹 폰트 CSS 파일 내 @font-face에서 다른 도메인의 폰트 사용 시

자바스크립트에서의 요청은 기본적으로서로 다른 도메인에 대한 요청을 보안상 제한합니다.
따라서  `http://localhost:3000`에서 `https://api.example.com`으로 데이터를 요청할 때,
브라우저는 **출처가 다르다고 판단하고**, 서버가 이를 허용하지 않으면 요청을 막습니다.

### **대처 방법**

**서버에서 CORS 헤더 설정**

```
Access-Control-Allow-Origin: https://example.com
```

- 가장 확실하고 권장되는 방법.
- 백엔드 서버에서 응답 시 위 헤더를 포함하면, 해당 출처의 요청을 허용합니다.
- `Access-Control-Allow-Methods`, `Access-Control-Allow-Headers` 등도 함께 설정될 수 있습니다.

### **우회 방법**

**1. 프록시 서버 사용**

- 클라이언트 → 프록시 → 실제 API로 요청이 가도록 하여 **같은 출처처럼 보이게 만드는 방식**
- 개발 시 `vite.config.js`, `webpack` 등에서 프록시 설정 가능

```
// Vite
server: {
  proxy: {
    '/api': {
      target: 'https://api.example.com',
      changeOrigin: true,
      rewrite: path => path.replace(/^\/api/, '')
    }
  }
}
```

**2. 브라우저 확장 프로그램 사용**

- 개발 단계에서만 사용. 예: CORS Unblock, Allow CORS 플러그인 등
( 브라우저에 **`Access-Control-Allow-Origin: *`** 헤더가 있다고 속임 )
- 실제 의미있는 결과를 응답받을 수 없어서 응답이 발생하는지 확인만 하고싶을 때는 사용.
- 보안상 실제 배포에서는 사용하지 않습니다.