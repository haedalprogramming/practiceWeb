# 06. REST API란?

웹 애플리케이션에서 가장 많이 사용되는 API 형식은 <strong>REST(Representational State Transfer)</strong>입니다. 이번 강의에서는 REST의 개념부터 REST API 설계‧사용 방법까지 알아봅니다.

## 1) REST란 무엇인가?

REST는 2000년대 초반 **Roy Fielding**의 박사 논문에서 제안된 **웹 아키텍처 스타일**입니다. "웹(HTTP)을 그대로 활용해 **자원(Resource)** 을 효율적으로 전송(Transfer)하도록 한다"는 철학을 따릅니다.

REST를 구성하는 핵심 제약 조건은 다음과 같습니다.

| 제약 조건 | 설명 |
|-----------|-------|
| 1. 클라이언트‧서버 구조 | 사용자 인터페이스(클라이언트)와 데이터 저장‧처리(서버)를 분리합니다.<br>👉 1일차 "[클라이언트–서버 구조](../day1/02-Client-Server-Architecture.md)" 리마인드 |
| 2. 무상태(Stateless) | 서버는 <strong>요청 간</strong> 클라이언트의 상태를 저장하지 않습니다. 모든 요청은 독립적이어야 합니다. |
| 3. 캐시 가능(Cacheable) | 응답에 캐시 가능 여부를 명시해, 클라이언트·중간 프록시가 데이터를 재사용할 수 있게 합니다. |
| 4. 계층형 시스템(Layered System) | 클라이언트는 중간 계층(프록시, 로드 밸런서 등)의 존재를 알 필요가 없습니다. |
| 5. 균일한 인터페이스(Uniform Interface) | URI, HTTP 메서드 등을 일관된 규칙으로 사용합니다. |
| 6. 필요 시 코드 온 디맨드(Code on Demand) | 선택적으로 서버가 코드를 전송(Javascript 등)해 클라이언트가 실행할 수 있도록 합니다. (실제로는 자주 사용되지 않습니다) |

> [!NOTE]
> REST는 "<strong>제약 조건</strong>" 집합에 가깝습니다. 위 조건을 최대한 만족하도록 API를 설계하면 "RESTful"하다고 표현합니다.

## 2) 자원(Resource)과 URL 설계

REST에서는 **모든 데이터**를 <strong>자원(Resource)</strong>으로 취급합니다. 자원마다 고유한 <strong>URL(Uniform Resource Locator)</strong>을 부여해 식별합니다.

### 2-1) URL 작성 규칙

1. **명사 사용**: 동사가 아닌 <strong>복수형 명사</strong>로 표현합니다.  
   `/users`, `/posts/123/comments`
2. **소문자 사용 + 띄어쓰기 대신 하이픈(-)**:  
   `/user-profiles` (띄어쓰기 X, 언더스코어 X)
3. **계층 구조 활용**: 관계를 URL 경로로 나타냅니다.  
   `/posts/123/comments/7`

> [!TIP]
> 자원을 URI(경로)로 식별하고, 동작은 HTTP <strong>메서드</strong>로 표현합니다. 예컨대 "사용자 조회"는 `GET /users/1`, "새 글 작성"은 `POST /posts`처럼 구성합니다.

## 3) HTTP 메서드와 CRUD 매핑

| HTTP 메서드 | 역할 | 예시 |
|-------------|------|------|
| GET | 자원 조회(Read) | `GET /users/1` – ID가 1인 사용자 정보 가져오기 |
| POST | 자원 생성(Create) | `POST /users` – 새 사용자 등록 |
| PUT | 자원 **전체** 수정(Update) | `PUT /users/1` – 사용자 전체 정보 교체 |
| PATCH | 자원 **부분** 수정(Update) | `PATCH /users/1` – 사용자 이름만 변경 |
| DELETE | 자원 삭제(Delete) | `DELETE /users/1` – 사용자 삭제 |

> [!IMPORTANT]
> 1일차 "[HTTP](../day1/03-HTTP.md)" 강의에서 배운 HTTP 메서드를 실제로 어떻게 활용하는지 확인해 보세요.

## 4) 요청(Request)과 응답(Response) 구조

### 4-1) 요청 예시

```
GET /users/1 HTTP/1.1
Host: api.example.com
Accept: application/json
```

*   최초 줄: **메서드, 경로, 프로토콜**
*   헤더(Header): 추가 정보(요청 본문 형식, 인증 토큰 등)
*   본문(Body): `POST`, `PUT` 등에서 전송할 데이터를 JSON 등으로 포함

### 4-2) 응답 예시

```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 1,
  "name": "홍길동",
  "email": "hong@example.com"
}
```

| 상태 코드 | 의미 |
|-----------|------|
| 200 OK | 정상 처리 |
| 201 Created | 자원 생성 성공 (POST) |
| 204 No Content | 성공했지만 응답 본문 없음 (DELETE 등) |
| 400 Bad Request | 잘못된 요청(필드 누락 등) |
| 401 Unauthorized | 인증 필요 |
| 404 Not Found | 자원 없음 |
| 500 Internal Server Error | 서버 오류 |

> [!CAUTION]
> API를 설계할 때 <strong>상태 코드</strong>를 올바르게 사용해야 클라이언트가 상황을 정확히 파악할 수 있습니다.

## 5) 간단한 블로그 REST API 설계 예시

| 기능 | HTTP | URL | 설명 |
|------|------|-----|------|
| 글 목록 조회 | GET | `/posts` | 글 전체 조회 |
| 글 상세 조회 | GET | `/posts/{postId}` | 특정 글 조회 |
| 글 작성 | POST | `/posts` | 새 글 생성 |
| 글 수정 | PUT | `/posts/{postId}` | 글 전체 수정 |
| 글 부분 수정 | PATCH | `/posts/{postId}` | 예) 제목만 변경 |
| 글 삭제 | DELETE | `/posts/{postId}` | 글 삭제 |
| 글 댓글 목록 | GET | `/posts/{postId}/comments` | 해당 글의 댓글 조회 |
| 댓글 작성 | POST | `/posts/{postId}/comments` | 댓글 추가 |

### 5-1) 예시 요청: 글 작성

```http
POST /posts HTTP/1.1
Content-Type: application/json

{
  "title": "첫 글입니다",
  "content": "안녕하세요, REST API 예제입니다.",
  "author": "hong"
}
```

### 5-2) 예시 응답

```http
HTTP/1.1 201 Created
Location: /posts/101
Content-Type: application/json

{
  "id": 101,
  "title": "첫 글입니다",
  "content": "안녕하세요, REST API 예제입니다.",
  "author": "hong",
  "createdAt": "2025-07-09T12:34:56Z"
}
```

## 6) 버전 관리와 확장성

실제 서비스에서는 시간이 지나면서 API가 변경될 수 있습니다. **하위 호환성**을 유지하려면 버전을 관리합니다.

1. **URL 버전**: `/v1/users`, `/v2/users`  
2. **헤더 버전**: `Accept: application/vnd.example.v1+json`

> [!WARNING]
> 버전이 늘어날수록 코드가 복잡해지므로, 기존 엔드포인트를 깨뜨리는 변경은 신중히 결정해야 합니다.

## 7) REST API의 장단점

| 장점 | 단점 |
|------|------|
| HTTP 표준만 지키면 구현 언어·플랫폼에 구애받지 않습니다. | 자원마다 여러 엔드포인트가 필요해 URL이 많아질 수 있습니다. |
| 캐시를 활용해 성능을 높일 수 있습니다. | 복잡한 관계 데이터를 한 번에 가져오기 어렵습니다. |
| 클라이언트와 서버의 결합도가 낮아, 독립적으로 개선할 수 있습니다. | 엄격한 REST 원칙을 지키기 어렵거나 과도하게 제한적일 수 있습니다. |

## 8) 정리

*   REST는 **웹(HTTP)** 의 장점을 최대한 활용하도록 고안된 아키텍처 스타일입니다.
*   자원을 **URL**로 식별하고, **HTTP 메서드**로 동작을 구분합니다.
*   요청‧응답은 **JSON** 형식을 주로 사용하며, **상태 코드**로 결과를 표현합니다.
*   올바른 REST API 설계는 애플리케이션의 확장성과 유지 보수성을 높여 줍니다.

---

- [목차로 돌아가기](./README.md)
- [이전 강의로 이동](./05-API-Integration.md)
- [다음 강의로 이동](./07-API-Mocking-MSW.md)
