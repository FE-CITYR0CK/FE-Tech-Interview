# REST API에 대해 설명해주세요.

## 면접용

**REST API**는 `REST 원칙`을 적용하여 설계된 API로, `HTTP 기반`에서 `자원`을 효율적으로 관리하고 상호작용하도록 설계된 인터페이스입니다.

- **REST**는 웹 상에서 자원을 주고받는 데 사용되는 웹 아키텍처 스타일로, HTTP URL을 통해 자원을 명시하며 HTTP 메서드를 통해 자원에 대한 CRUD를 수행합니다.
- **API**는 기존 애플리케이션의 기능을 외부에서 활용할 수 있도록 설계된 인터페이스입니다.

<br>

## 개념 설명

### REST API란?

**REST**
- REpresentational State Transfer의 약자입니다.
- 웹 상에서 자원을 주고받는 데 사용되는 웹 아키텍처 스타일입니다.
- 자원을 이름(URI)으로 구분하고, 자원의 상태를 주고받는 모든 행위를 규정합니다.
- HTTP URL을 통해 자원을 명시하며, HTTP 메서드(GET, POST, PUT, DELETE 등)를 통해 자원에 대한 CRUD(생성, 조회, 수정, 삭제)를 수행합니다.

**API**
- Application Programming Interface의 약자
- 프로그램 간 데이터 제공 및 기능 사용을 위한 인터페이스 또는 규격입니다.
- 기존 애플리케이션의 기능을 외부에서 활용할 수 있도록 설계됩니다.

**따라서, REST API는 REST 원칙을 적용하여 설계된 API로, HTTP 기반에서 자원을 효율적으로 관리하고 상호작용하도록 설계된 인터페이스입니다.**

---

➕CRUD
- Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말입니다.
- 대부분의 컴퓨터 소프트웨어가 가지는 기본 데이터 처리 기능입니다.
- 사용자 인터페이스가 갖추어야 할 기능(정보의 참조, 검색, 갱신)을 가리키는 용어로도 사용합니다.

<br>

### REST의 특징들

1. **균등한 인터페이스 (Uniform Interface) ⭐️**
- 플랫폼, 언어의 제약이 없습니다.
    - HTTP의 표준 프로토콜에 따르는 모든 플랫폼에서 사용가능합니다.
- REST API를 정의할 때 방식
    - JSON(JavaScript Object Notation) → 가장 많이 사용
    - XML(eXtensible Markup Language)
2. **무상태성 (Stateless) ⭐️**
- 서버는 API 요청에 대해서만 처리하고, 클라이언트의 상황은 무시합니다.
    - 이를 **상태가 없다**고 표현합니다.
- 클라이언트를 고려하지 않아 구현이 간결해집니다.
3. **캐싱 가능(Cacheable) ⭐️**
- HTTP 기반이므로, HTTP의 특징인 캐싱을 사용할 수 있습니다.
- 캐싱 사용 방법 : `GET 메소드`를 `Last-Modified` 값과 함께 전송
- 캐싱 결과 : 컨텐츠의 변화가 없을 때 캐시된 값을 사용
- 장점
    - 네트워크 응답시간이 빠릅니다.
    - API 서버에 요청을 발생시키지 않아 부담이 덜합니다.
4. **서버-클라이언트 구조 (Server-Client) ⭐️**
- `Server`(자원이 있는 쪽) - `Client` (자원을 요청하는 쪽) 구조를 가집니다.
- 영역을 분리하면 서버의 처리 부담이 줄어들고, 다양한 플랫폼과 UI 형태로 확장하기 쉬워집니다.
    - 한 데이터 → 웹, 앱, … 등 여러 플랫폼으로 확장가능합니다.
5. **자체 표현성 (Self-Descriptiveness)**
- REST API 요청 자체로 어떤 것을 표현하는지 알아보기 쉽습니다.
    - REST API의 자원을 명시하는 규칙은 그 자체로 의미를 가집니다.
- 따라서 **좋은 REST API**란 **요청하는 방식만으로 어떤 의미인지 명확하게 나타내는 것**입니다!
6. **계층화 (Layered Ststem)**
- REST Server는 순수 비즈니스 로직에 로드밸런싱, 암호화, 사용자 인증 등을 추가하여 다중 계층으로 구성할 수 있습니다.
- REST Client는 위와 상관없이 REST API만 호출합니다.

<br>

### URI, URL, URN 에 대해서

![0206_REST_1](https://github.com/user-attachments/assets/bf742dcc-e3fe-4cfa-95b7-2cb4f3e43053)

**URI**
- Uniform Resource Identifier의 약자입니다.
- **자원에 접근할 수 있는 유일한 식별자**라는 뜻입니다.
- 표현 방식으로 `URL` , `URN` 이 있습니다.

**URL**
- Uniform Resource Locater의 약자입니다.
- **자원에 접근할 수 있는 유일한 식별자(의 경로)**라는 뜻입니다.
- URL을 수신하는 기기는 해당 URL에 맞게 데이터를 반환합니다.
- URI를 표현하는 주요 방식으로 쓰입니다.

**URN**
- Uniform Resource Name의 약자입니다.
- 리소스를 이름(Name)으로 식별하는 방법입니다.
- 예시로 ISBN(책 분류 코드) 등이 있습니다.

<br>

---

### 참고자료

[rest-api.md - Esoolgnah](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/rest-api.md)

[rest-api.md - baeharam](https://github.com/baeharam/Must-Know-About-Frontend/blob/main/Notes/network/rest-api.md)

[URI, URL, URN 의 차이 한 번에 정리하기 : 리소스 구분 관점에서 보는 URI, URL, URN의 차이 - 조세영의 Kotlin World](https://kotlinworld.com/96)