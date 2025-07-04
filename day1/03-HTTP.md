# 03. HTTP (HyperText Transfer Protocol)

이 문서는 웹의 핵심 통신 규약인 HTTP에 대해 심도 있게 설명합니다. 클라이언트와 서버가 어떻게 메시지를 주고받는지, 그리고 웹 애플리케이션의 상태를 어떻게 관리하는지에 대한 기본 원리를 이해하는 것을 목표로 합니다.

## 목차

1. [HTTP란?](#1-http란)
2. [HTTP 메시지 구조](#2-http-메시지-구조)
   - [2-1) HTTP 요청 (Request)](#2-1-http-요청-request)
   - [2-2) HTTP 응답 (Response)](#2-2-http-응답-response)
3. [HTTP 요청 메서드](#3-http-요청-메서드)
4. [HTTP 상태 코드](#4-http-상태-코드)
5. [HTTPS: 보안이 강화된 HTTP](#5-https-보안이-강화된-http)
6. [상태 관리: 쿠키와 세션](#6-상태-관리-쿠키와-세션)
   - [6-1) 쿠키 (Cookie)](#6-1-쿠키-cookie)
   - [6-2) 세션 (Session)](#6-2-세션-session)
7. [HTTP 기반 인증 (Authentication)](#7-http-기반-인증-authentication)
8. [최신 HTTP 동향](#8-최신-http-동향)

---

## 1) HTTP란?

HTTP(HyperText Transfer Protocol)는 웹에서 클라이언트(예: 웹 브라우저)와 서버 간에 데이터를 주고받기 위한 <strong>통신 규칙(Protocol)</strong>입니다. 웹의 기반이 되는 이 프로토콜은 HTML 문서, 이미지, 동영상 등 다양한 종류의 데이터를 전송하는 데 사용됩니다.

### HTTP의 주요 특징

1.  **클라이언트-서버 구조**: 요청(Request)을 보내는 클라이언트와 응답(Response)을 반환하는 서버를 기반으로 동작합니다.
2.  **무상태(Stateless)**: 각 요청은 독립적으로 처리되며, 서버는 이전 요청에 대한 정보를 기억하지 않습니다. 이로 인해 서버 설계가 단순해지지만, 로그인 상태 유지 등을 위해 쿠키나 세션 같은 별도의 기술이 필요합니다.
3.  **비연결성(Connectionless)**: 클라이언트가 요청을 보내고 서버가 응답을 반환하면 연결을 끊습니다. 이는 서버 자원을 효율적으로 사용하는 데 도움이 되지만, 매번 새로운 연결을 맺어야 하는 단점이 있습니다. (HTTP/1.1의 Keep-Alive 기능으로 일부 개선됨)

---

## 2) HTTP 메시지 구조

HTTP 통신은 <strong>요청(Request)</strong>과 <strong>응답(Response)</strong> 메시지를 통해 이루어집니다. 두 메시지는 비슷한 구조를 가집니다.

### 2-1) HTTP 요청 (Request)

클라이언트가 서버에 보내는 메시지입니다.

```
GET /users/1 HTTP/1.1
Host: api.example.com
User-Agent: Chrome/91.0.4472.124
Accept: application/json
Authorization: Bearer abc123token

{
  "key": "value"
}
```

-   **시작 줄 (Start Line)**: `GET /users/1 HTTP/1.1`
    -   `GET`: **HTTP 메서드** (요청의 목적)
    -   `/users/1`: **요청 대상 (Path)** (자원의 경로)
    -   `HTTP/1.1`: **HTTP 버전**
-   **헤더 (Headers)**: 요청에 대한 부가 정보
    -   `Host`: 요청을 보내는 서버의 주소
    -   `User-Agent`: 요청을 보낸 클라이언트(브라우저) 정보
    -   `Accept`: 클라이언트가 이해할 수 있는 데이터 형식
    -   `Authorization`: 인증 토큰
-   **본문 (Body)**: 전송할 실제 데이터 (주로 `POST`, `PUT` 요청에 사용됨)

### 2-2) HTTP 응답 (Response)

서버가 클라이언트에 보내는 메시지입니다.

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 123
Cache-Control: max-age=3600
Set-Cookie: sessionId=xyz789; HttpOnly

{
  "id": 1,
  "name": "Alice",
  "email": "alice@example.com"
}
```

-   **상태 줄 (Status Line)**: `HTTP/1.1 200 OK`
    -   `HTTP/1.1`: **HTTP 버전**
    -   `200`: **상태 코드** (요청 처리 결과)
    -   `OK`: **상태 메시지** (상태 코드에 대한 설명)
-   **헤더 (Headers)**: 응답에 대한 부가 정보
    -   `Content-Type`: 응답 본문의 데이터 형식
    -   `Content-Length`: 본문의 크기 (바이트 단위)
    -   `Cache-Control`: 캐시 정책
    -   `Set-Cookie`: 클라이언트에 쿠키를 설정하라는 명령
-   **본문 (Body)**: 클라이언트에 전달할 실제 데이터 (예: HTML, JSON)

---

## 3) HTTP 요청 메서드

요청 메서드는 클라이언트가 서버에 **어떤 행동(Action)**을 원하는지 나타냅니다.

| 메서드   | 목적             | 특징                                   |
| :------- | :--------------- | :------------------------------------- |
| **GET**  | 자원 **조회**    | 서버 데이터 변경 없음 (Safe), 캐시 가능  |
| **POST** | 자원 **생성**    | 서버 데이터 변경 발생, 본문에 데이터 포함 |
| **PUT**  | 자원 **전체 수정** | 자원 전체를 새로운 데이터로 덮어씀     |
| **PATCH**| 자원 **부분 수정** | 자원의 일부만 수정                     |
| **DELETE**| 자원 **삭제**    | 지정된 자원을 삭제                     |

-   **안전한 메서드 (Safe Methods)**: `GET`, `HEAD`, `OPTIONS`처럼 서버의 상태를 변경하지 않는 메서드입니다.
-   **멱등성 (Idempotent)**: `GET`, `PUT`, `DELETE`처럼 동일한 요청을 여러 번 보내도 결과가 항상 같은 메서드입니다. `POST`나 `PATCH`는 여러 번 호출 시 결과가 달라질 수 있으므로 멱등성이 보장되지 않습니다.

---

## 4) HTTP 상태 코드

상태 코드는 서버가 클라이언트의 요청을 어떻게 처리했는지를 나타내는 **세 자리 숫자**입니다.

### 주요 상태 코드 그룹

-   **1xx (Informational)**: 요청을 받았으며, 프로세스를 계속 진행 중입니다.
-   **2xx (Successful)**: 요청이 성공적으로 처리되었습니다.
    -   `200 OK`: 요청 성공.
    -   `201 Created`: 새로운 자원이 성공적으로 생성됨 (`POST`, `PUT` 응답).
    -   `204 No Content`: 요청은 성공했으나 반환할 데이터가 없음 (`DELETE` 응답).
-   **3xx (Redirection)**: 요청을 완료하려면 추가적인 동작이 필요합니다.
    -   `301 Moved Permanently`: 요청한 리소스의 URI가 영구적으로 변경됨.
    -   `302 Found`: 일시적인 리다이렉션.
-   **4xx (Client Error)**: 클라이언트의 요청에 오류가 있습니다.
    -   `400 Bad Request`: 요청 문법이 잘못됨.
    -   `401 Unauthorized`: 인증이 필요함 (로그인 필요).
    -   `403 Forbidden`: 접근 권한이 없음 (인증은 되었으나 권한 부족).
    -   `404 Not Found`: 요청한 리소스를 찾을 수 없음.
-   **5xx (Server Error)**: 서버가 요청을 처리하는 데 실패했습니다.
    -   `500 Internal Server Error`: 서버 내부에서 오류 발생.
    -   `503 Service Unavailable`: 서버가 일시적으로 요청을 처리할 수 없음 (과부하, 점검 등).

---

## 5) HTTPS: 보안이 강화된 HTTP

HTTPS(HTTP Secure)는 HTTP에 **SSL/TLS(Secure Sockets Layer/Transport Layer Security)** 프로토콜을 결합하여 데이터를 암호화하는 방식입니다.

### 왜 HTTPS를 사용해야 하는가?

1.  **암호화 (Encryption)**: 클라이언트와 서버 간에 주고받는 데이터를 암호화하여 중간에서 가로채더라도 내용을 알 수 없게 합니다.
2.  **무결성 (Integrity)**: 데이터가 전송 중에 위변조되지 않았음을 보장합니다.
3.  **인증 (Authentication)**: 접속하려는 서버가 신뢰할 수 있는 서버임을 증명합니다.

로그인, 결제 등 민감한 정보를 다루는 모든 웹사이트는 반드시 HTTPS를 사용해야 합니다. 오늘날에는 모든 웹사이트에 HTTPS를 적용하는 것이 표준입니다.

---

## 6) 상태 관리: 쿠키와 세션

HTTP는 무상태(Stateless) 프로토콜이므로, 서버는 클라이언트의 이전 요청을 기억하지 못합니다. 하지만 로그인 상태 유지, 장바구니 기능 등을 구현하려면 사용자의 상태를 기억해야 합니다. 이를 위해 <strong>쿠키(Cookie)</strong>와 <strong>세션(Session)</strong>을 사용합니다.

### 6-1) 쿠키 (Cookie)

-   **정의**: 서버가 클라이언트(브라우저)에 저장하는 작은 데이터 조각.
-   **동작 방식**:
    1.  서버가 응답 헤더에 `Set-Cookie`를 담아 클라이언트에 쿠키를 보냅니다.
    2.  브라우저는 이 쿠키를 저장하고, 이후 동일한 서버에 요청을 보낼 때마다 요청 헤더에 `Cookie`를 담아 자동으로 전송합니다.
-   **단점**: 클라이언트에 저장되므로 보안에 취약할 수 있으며, 저장 용량에 제한이 있습니다.

### 6-2) 세션 (Session)

-   **정의**: 사용자의 상태 정보를 서버 측에 저장하고 관리하는 방식.
-   **동작 방식**:
    1.  사용자가 로그인하면 서버는 고유한 **세션 ID**를 생성하고, 이 ID와 사용자 정보를 서버에 저장합니다.
    2.  서버는 세션 ID를 쿠키에 담아 클라이언트에 보냅니다.
    3.  클라이언트는 이후 요청 시 세션 ID가 담긴 쿠키를 전송합니다.
    4.  서버는 세션 ID를 통해 사용자를 식별하고 상태 정보를 참조합니다.
-   **장점**: 중요한 정보(개인정보 등)를 서버에 저장하므로 쿠키 방식보다 안전합니다.

보통 **세션 ID를 저장하는 매개체로 쿠키를 사용**하는 방식이 널리 쓰입니다.

---

## 7) HTTP 기반 인증 (Authentication)

웹 애플리케이션에서 사용자가 누구인지 확인하는 과정입니다.

-   **Authorization 헤더**: 클라이언트가 자신의 인증 정보를 서버에 보낼 때 사용하는 표준 헤더입니다.
-   **Bearer Authentication**: 가장 널리 사용되는 방식으로, `Authorization: Bearer <token>` 형태를 가집니다. 여기서 토큰(예: JWT - JSON Web Token)은 사용자의 신원을 증명하는 암호화된 문자열입니다.

---

## 8) 최신 HTTP 동향

-   **HTTP/1.1**: 현재 가장 널리 사용되는 버전으로, Keep-Alive, 파이프라이닝 등의 기능이 있습니다.
-   **HTTP/2**: 여러 요청과 응답을 동시에 처리(Multiplexing)하고, 헤더를 압축하여 성능을 크게 향상시켰습니다.
-   **HTTP/3**: TCP 대신 UDP 기반의 QUIC 프로토콜을 사용하여 연결 설정 시간을 줄이고, 네트워크 변경에 더 강한 모습을 보입니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](02-Client-Server-Architecture.md)
- [다음 강의로 이동](04-Git-Fundamentals.md)
