# 주소창에 naver.com을 입력하면 일어나는 일에 대해 말씀해주세요.

## 면접용

사용자가 naver.com을 입력하면 브라우저가 url을 파싱합니다. 파싱 이후 dns 조회에 들어가는데 보통 브라우저 캐시, 로컬 dns, tld, authoritative dns 순서로 조회하게 됩니다.

조회 결과 반환받은 ip주소로 3way handshake로 연결을 맺고, Http 요청을 보냅니다. 이후 html 응답을 받으면 해당 응답을 토대로 브라우저 렌더링 과정을 거치게 됩니다.

<hr/>

## 면접용

사용자가 naver.com을 입력하면 브라우저가 url을 파싱합니다. 파싱 이후 dns 조회에 들어가는데 보통 브라우저 캐시, 로컬 dns, tld, authoritative dns 순서로 조회하게 됩니다.

조회 결과 반환받은 ip주소로 3way handshake로 연결을 맺고, Http 요청을 보냅니다. 이후 html 응답을 받으면 해당 응답을 토대로 브라우저 렌더링 과정을 거치게 됩니다.

<br/>
<hr/>
<br/>

## 개념 설명

우리가 naver.com을 치면 브라우저는 보통 DNS(Domain Name Service)를 체크하지만, 현대 브라우저는 여러 캐시를 거치게 됩니다.

### 1. 캐시 체크

브라우저 캐시 → OS 캐시 → 라우터 캐시 → ISP 캐시 순서로 캐시 체크를 합니다.

**브라우저 캐시**

기존에 naver.com을 방문했었다면, 구글에 빠르게 접근할 수 있는 내용들이 브라우저 캐시에 있습니다.

크롬에서는 `chrome://net-internals/#dns` 에서 DNS 캐시 상태를 볼 수 있습니다.

**OS 캐시**

브라우저 캐시에서 기록을 찾을 수 없다면 OS 캐시를 찾아보게 됩니다.

Mac에서는 `nslookup naver.com` 을 터미널에 입력하면 알 수 있는데, `naver.com`에 대한 ip주소는 `142.250.206.206` 으로, 실제 주소창에 해당 ip를 검색하면 `naver.com`으로 리다이렉션 되는 것을 알 수 있습니다.

![image (10)](https://github.com/user-attachments/assets/97287be9-e2a4-4e23-9689-99265f3cfd43)

**라우터 캐시**

OS 캐시에도 발견하지 못하면 라우터 캐시를 확인하는데, 여기서 라우터는 집에서 사용하는 공유기라고 생각하면 됩니다.

**ISP 캐시**

마지막으로는 ISP(Internet Service Provider) 캐시를 확인하는데, 한국에서는 인터넷을 제공하는 회사라고 생각하면 됩니다.

### 2. DNS(Domain Name Service)로 IP 주소 획득

만일 ISP 캐시까지 체크를 했지만 IP주소를 찾지 못한다면 ISP의 DNS 서버에 DNS 쿼리를 보내야 합니다.

**DNS ?**

![image (11)](https://github.com/user-attachments/assets/ad69010f-b2ac-4029-84e5-50ffe9260836)

DNS는 도메인네임과 IP를 매칭해서, 도메인네임으로 쿼리를 보냈을 때 IP로 변환해서 네트워크 통신을 할 수 있게 해주는 시스템입니다.

<img width="869" alt="image (12)" src="https://github.com/user-attachments/assets/a21fa6c4-bad2-4f26-bcea-7207ed836432" />

전 세계에 13개의 Root DNS 서버가 있고, 한국에는 없지만 미러 서버 3대를 가지고 있습니다.

**DNS 질의 방식**

최상위 DNS에서 시작해서 아래 딸린 node DNS 서버까지 차례차례 물어보는 구조로 되어있습니다. 이를 **Recursive Resolver**라고 합니다.

![image (13)](https://github.com/user-attachments/assets/43d3c20d-5c3c-4c41-9590-6c6c36c096f5)

**DNS 종류**

- **Local DNS 서버 (기지국 DNS 서버)**
  인터넷을 설치할 때 각각의 통신사가 있고, 각각의 통신사마다 DNS 서버가 존재합니다.
  KT 인터넷을 설치하면 KT DNS가 되고, U+를 사용하면 LG U+ DNS가 자동으로 세팅됩니다.

<img width="975" alt="image (14)" src="https://github.com/user-attachments/assets/137e7c09-db89-4ae6-b0f9-21234674cf62" />    
    www.naver.com.
    
- **Root DNS**
    
![image (15)](https://github.com/user-attachments/assets/ac7f16c1-f2be-4e03-8d13-3d0773c2c6a4)
    
    Local DNS 서버에 해당 도메인에 대한 IP 주소가 없을때 Root DNS 서버에게 물어보게 됩니다.
    
    Root DNS는 트리 구조입니다. 
    
- **TLD(Top-Level Domain) DNS**
    
    최상위 도메인을 관리하는 DNS서버.
    
    `.com` `.net` `.org` `.kr`  등의 최상위 도메인을 관리하는 역할을 합니다.
    
    TLD DNS는 특정 도메인의 네임서버를 저장하고 있으며, 도메인의 Authoritative DNS / Second-Level DNS 서버를 알려주는 역할을 합니다.
    
- **Second-Level DNS**
    
    Second-Level DNS는 TLD 바로 아래에 위치하는 도메인으로, 브랜드 및 웹 사이트 주소로 사용합니다.
    
    DNS 계층 구조의 일부입니다.
    
- **Sub-Level DNS**
    
    이 안에 `mail.naver.com` , `cafe.naver.com` ,`blog.naver.com` 등등의 Sub DNS 서버가 가지고 있는 것이죵

<img width="217" alt="image (16)" src="https://github.com/user-attachments/assets/ed8e4544-b45e-4ca5-bedf-be46a15b1be0" />
![image.png](attachment:8a4da83b-93b2-4e05-81ad-ffbcfabe9a4a:image.png)

<img width="213" alt="image (17)" src="https://github.com/user-attachments/assets/2cb3bdde-2f11-4bbd-8a34-6616c51fa05e" />

**그래서 사실은..**

<img width="826" alt="image (18)" src="https://github.com/user-attachments/assets/eeca1659-f527-4156-b409-3288c4e55902" />

우리의 도메인은 그냥 url처럼 보였지만 위 구조로 구성되어있는 것을 알 수 있습니다.

**주소 획득까지의 일련의 과정**

<img width="989" alt="image (19)" src="https://github.com/user-attachments/assets/2d4f3801-6bb2-4ca8-8b82-612b394f52fd" />

**dig 명령어를 써서 DNS 검색 과정 찾아보기**

아래 명령어를 쓰면 `www.naver.com` 의 ip주소를 찾기 위한 DNS 조회 과정의 전체 경로를 확인할 수 있습니다.

```bash
dig [www.naver.com](http://www.naver.com/) +trace
```

1. **Root DNS 조회 (.)**

![image (20)](https://github.com/user-attachments/assets/346da3cf-4acf-4a45-97fc-ea2ee6976b23)

1. **TLD 네임서버 조회 (com.)**

![image (21)](https://github.com/user-attachments/assets/2fa2acdc-6535-4cdb-9b54-f902010617d8)

2. **naver.com의 Authoritative DNS 서버 조회 (naver.com)**

![image (22)](https://github.com/user-attachments/assets/0dd2d209-d8cc-4a47-af10-c61a8f762a73)

3. **최종적으로 www.naver.com의 IP주소 조회**

![image (23)](https://github.com/user-attachments/assets/9aea71ee-582e-4b2e-b600-a1e48bf6bc2f)

**Authoritative DNS**

Authoritative DNS란 특정 도메인에 대해 최종적인 DNS 정보를 저장하고 관리하는 서버입니다. Recursive Resolver가 최종 IP 주소를 얻기 위해 참조하는 서버입니다.

사용자가 웹 사이트에 접속할 때, 이 서버에서 최종 IP정보를 제공합니다.

- **역할**
  - 도메인의 IP주소 저장 및 응답
  - DNS 레코드 관리
  - Recursive Resolver의 질의에 대한 최종 응답 제공

**Authoritative DNS 확인해보기**

아래 명령어를 입력하면 naver.com에 대한 네임서버를 조회할 수 있습니다.

```bash
dig [naver.com](http://naver.com) NS
```

![image (24)](https://github.com/user-attachments/assets/7b055c80-95d9-4dea-bda0-1b99458a903d)

결과를 보면 [`ns1.naver.com`](http://ns1.naver.com), `ns2.naver.com` 가 naver.com의 Authoritative DNS인 것을 알 수 있습니다.

**DNS 레코드 종류**

<img width="817" alt="image (25)" src="https://github.com/user-attachments/assets/51ea10f6-7304-4c65-a05e-a95d23f9fad1" />

우리는 보통 A 레코드와 CNAME 레코드가 좀 친숙할테니 이 두 개를 중점으로 설명해보겠습니다.

**A레코드**

DNS에 저장되는 정보의 타입으로 도메인 주소와 서버의 IP 주소가 직접 매핑시키는 방법입니다.

- 도메인 naver.com을 예로 든다면, A레코드는 “네이버의 IP 주소는 123.123.123.123에 연결됨” 이라고 말하는 역할입니다.

> 도메인 매핑 설정에 따라 일대다 / 다대일이 될 수도 있습니다.

실제로 naver.com의 A 레코드를 조회하였을 때 223.130.200.104 / 223.130.200.107 / 223.130.195.95 / 223.130.195.200의 IP주소가 매핑 되어있는 것을 볼 수 있습니다.

**CNAME레코드(Canonical Name record)**

CName 레코드는 도메인 별명 레코드라고 부르며, 도메인 주소를 또 다른 도메인 주소로 이중 매핑시키는 형태의 DNS 레코드 타입입니다.

쉽게 말하면 도메인 주소로 연결한 부분에 다시 한번 도메인 주소로 연결하는 방식입니다.

`daum.net` 도메인이 있다고 했을때, 도메인의 CNAME을 `daum2.net`으로 정해, 브라우저에 `daum2.net` 을 입력하면 `daum.net` 로 접근되는 형식입니다.

그러고 `daum.net` 에 매핑된 (A레코드) IP 주소 203.133.167.81을 얻어 최종적으로 서비스에 접속되는 방식입니다.

<img width="968" alt="image (27)" src="https://github.com/user-attachments/assets/0176f1f3-f0c4-4962-b6fa-2165d40ddef4" />

**A레코드 VS CNAME레코드**

|         | 장점                                                       | 단점                                                                         |
| ------- | ---------------------------------------------------------- | ---------------------------------------------------------------------------- |
| A레코드 | 도메인이 바뀌어도 IP는 그대로이므로 유지가 된다            | 서버 이전등의 문제로 IP가 변동될시에 일일히 변경해야한다.                    |
| CNAME   | 서버 이전등의 문제로 IP가 변동될시에 변경하지 않아도 된다. | 도메인이 바뀌면 변경해야 한다. 여러번 요청이 될 경우 성능 저하가 날 수 있다. |

### 3. TCP 3-way handshake

![image (26)](https://github.com/user-attachments/assets/45fc116b-8eb3-42a1-b3ce-4b7609db1381)

1. 클라이언트가 서버로 `SYN 세그먼트`를 보냅니다. (`SYN 세그먼트`는 헤더의 `SYN` 필드의 비트가 1로 설정된 세그먼트, 데이터 포함 X)
2. `SYN 세그먼트`를 받은 서버는 `SYN_RCVD` 상태가 되고, 해당 클라이언트와의 TCP 통신에서 사용할 버퍼 및 변수(e.g. 윈도우 크기)등을 초기화합니다.
3. `SYN 세그먼트` 에 대한 응답으로 헤더의 `SYN`과 `ACK` 필드의 비트가 1로 설정된 `SYNACK 세그먼트`를 클라이언트에 전송합니다.
4. `SYNACK 세그먼트` 를 받은 클라이언트는 해당 서버와의 TCP 통신에서 사용할 버퍼와 변수를 초기화하고, `SYNACK 세그먼트`를 통해 받은 값에 1을 더한 값을 ack 값으로 해서 `ACK 세그먼트`에 포함하여 서버에 전송합니다.
5. 클라이언트의 상태가 `ESTABLISHED`로 바뀌고, 이제부터는 TCP 연결이 생성된 상태이므로 `ACK 세그먼트`를 서버에 전송할 때 애플리케이션의 데이터도 포함하여 전송할 수 있습니다.

### 4. HTTP 요청 & 브라우저 렌더링

이제 TCP연결이 생성되었으니 HTTP 요청을 보낼 수 있게 됩니다.

HTTP 요청으로 `GET /` 요청을 보내고, 응답으로 받은 HTML을 바탕으로 브라우저 렌더링 과정을 거치게 됩니다.

[브라우저의 렌더링 원리에 대해 설명해보세요.](https://www.notion.so/36c128fb753a48569a5aabc8d8e948a5?pvs=21)
