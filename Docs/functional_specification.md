-----

## 백엔드 기능 명세서: 온라인 매뉴얼 플랫폼

이 문서는 '온라인 매뉴얼 플랫폼' PRD(v1.0)에 명시된 기능들을 구현하기 위한 백엔드 설계를 정의합니다.

-----

## 1\. 매뉴얼 콘텐츠 조회 (PRD: MAN-103, MAN-201)

### 1.1. 사용자 시나리오 (User Scenario)

사용자(예: 신입 상담사)가 특정 매뉴얼 페이지(예: '국민취업지원제도 가이드')에 접속합니다. 프론트엔드 애플리케이션은 사이드바에 렌더링할 **목차(TOC) 데이터**와 메인 영역에 렌더링할 **구조화된 본문 콘텐츠**를 백엔드에 요청해야 합니다.

### 1.2. API 명세 (API Specification)

* **Endpoint:** `GET /api/v1/manuals/{manual_slug}`
* **Method:** `GET`
* **Description:** 특정 `slug`를 가진 매뉴얼의 전체 콘텐츠(목차, 본문)를 조회합니다.

**Request:**

* **Path Parameters:**
    * `manual_slug` (string, required): 매뉴얼을 식별하는 고유 슬러그 (예: `national-employment-support-guide`)
* **Headers:**
    * `Accept: application/json`

**Response (Success: 200 OK):**

> `ManualDocs.json`의 구조를 기반으로 하며, 이미지 경로는 백엔드에서 완성된 URL로 변환하여 제공합니다.

```json
{
  "documentTitle": "국민취업지원제도 신입상담사를 위한 프로세스 가이드",
  "slug": "national-employment-support-guide",
  "lastUpdatedAt": "2025-11-10T09:00:00Z",
  "tableOfContents": {
    "title": "목 차",
    "items": [
      { "text": "1. 읽기 전 주의사항" },
      {
        "text": "2. 참여자 배정",
        "subItems": [{ "text": "1. . 참여자 관리 사이트 수정" }]
      }
      // ... (ManualDocs.json의 'tableOfContents'와 동일)
    ]
  },
  "mainContent": [
    {
      "title": "0. 읽기 전 주의사항",
      "content": [
        {
          "type": "listitem",
          "text": "해당 국민취업지원제도 상담진행 프로세스 설명은..."
        }
      ],
      "subsections": []
    },
    {
      "title": "1. 참여자 배정",
      "content": [
        {
          "type": "listitem",
          "text": "참여자 배정시 참여자의 배정일에 맞춰 인정통지일 수정(배정일=인정통지일 일치화)\n[image01.png]"
        },
        // PRD MAN-203(이미지 렌더링) 요구사항에 따라, 백엔드는 이미지 태그를 완성된 URL로 변환합니다.
        {
          "type": "image",
          "src": "https://<your-cdn-domain>/manuals/national-employment-support-guide/image01.png",
          "alt": "참여자 관리 사이트 인정통지일 수정 예시"
        }
      ],
      "subsections": [
        {
          "title": "1-1. 참여자 관리 사이트 수정",
          "content": [
            {
              "type": "listitem",
              "text": "배정받은 참여자는 http://jobmoaa.mycafe24.com/login.do [잡모아 참여자 관리 사이트]에 [신규 참여자]에서 등록 진행\n[image02.png]"
            },
            {
              "type": "image",
              "src": "https://<your-cdn-domain>/manuals/national-employment-support-guide/image02.png",
              "alt": "잡모아 참여자 관리 사이트 신규 등록"
            }
            // ... (ManualDocs.json의 'mainContent'와 동일하되, 'image' 타입은 URL로 변환)
          ]
        }
      ]
    }
  }
}
```

### 1.3. 핵심 비즈니스 로직 (Core Business Logic)

1.  요청된 `{manual_slug}`를 수신하고 이스케이프 처리(sanitize)합니다.
2.  `Manuals` 컬렉션(또는 테이블)에서 `slug` 필드가 일치하는 문서를 조회합니다.
3.  문서를 찾으면, 해당 문서의 `mainContent` 배열을 순회(iterate)합니다.
4.  `type`이 `image`인 객체를 찾으면, `src` 값(예: `[image01.png]`)을 기반으로 정적 자산(S3/CDN)의 전체 URL을 생성합니다. (예: `config.CDN_BASE_URL + /manuals/ + manual_slug + / + imageName`)
    * *참고: 이미지 `alt` 텍스트는 콘텐츠 관리 시 별도 필드로 관리하거나, 파일명에서 추론하는 로직이 필요할 수 있습니다. 위 예시에서는 임의의 alt 태그를 추가했습니다.*
5.  이미지 경로가 변환된 최종 JSON 객체를 클라이언트에 반환합니다.

### 1.4. 데이터 모델 (Data Model)

**Collection: `Manuals` (MongoDB 또는 유사 NoSQL 권장)**

| 필드명 | 데이터 타입 | 설명 | 인덱스 |
| :--- | :--- | :--- | :--- |
| `_id` | `ObjectId` | 문서 고유 ID | (PK) |
| `slug` | `String` | URL 식별자 (예: "national-employment-support-guide") | **Unique** |
| `documentTitle` | `String` | 매뉴얼의 공식 제목 | |
| `tableOfContents` | `Object` | `ManualDocs.json`에 정의된 목차 구조 | |
| `mainContent` | `Array` | `ManualDocs.json`에 정의된 본문 콘텐츠 블록 배열 | |
| `searchText` | `String` | 본문의 모든 텍스트를 추출하여 저장 (검색용) | **Text** |
| `createdAt` | `Timestamp` | 생성 일시 | |
| `updatedAt` | `Timestamp` | 마지막 수정 일시 | |

### 1.5. 예외 처리 (Exception Handling)

| HTTP 상태 | 에러 코드 | 설명 |
| :--- | :--- | :--- |
| `404 Not Found` | `MANUAL_NOT_FOUND` | 요청한 `manual_slug`에 해당하는 매뉴얼이 없을 경우 |
| `500 Internal Server Error` | `DATABASE_ERROR` | 데이터베이스 조회 중 오류 발생 시 |

-----

## 2\. 콘텐츠 검색 (PRD: MAN-304)

### 2.1. 사용자 시나리오 (User Scenario)

사용자가 헤더의 검색창에 키워드(예: "IAP 수립")를 입력하고 Enter 키를 누릅니다. 사용자는 입력한 키워드가 포함된 매뉴얼의 섹션 목록을 즉시 확인하기를 기대합니다.

### 2.2. API 명세 (API Specification)

* **Endpoint:** `GET /api/v1/search`
* **Method:** `GET`
* **Description:** 모든 매뉴얼 또는 특정 매뉴얼 내에서 키워드 검색을 수행합니다.

**Request:**

* **Query Parameters:**
    * `q` (string, required): 사용자가 입력한 검색 키워드.
    * `manual_slug` (string, optional): 검색 범위를 특정 매뉴얼로 한정할 경우 사용.
* **Headers:**
    * `Accept: application/json`

**Response (Success: 200 OK):**

```json
{
  "query": "IAP 수립",
  "results": [
    {
      "manualSlug": "national-employment-support-guide",
      "manualTitle": "국민취업지원제도 신입상담사를 위한 프로세스 가이드",
      "sectionTitle": "4. IAP 수립(3번째 방문)",
      "snippet": "... 3번째 방문 상담시 IAP 등록 및 취업활동계획 수립위한 희망직무 등 변경 필요 ...",
      "sectionUrl": "/national-employment-support-guide#4-iap-수립(3번째-방문)"
    },
    {
      "manualSlug": "national-employment-support-guide",
      "manualTitle": "국민취업지원제도 신입상담사를 위한 프로세스 가이드",
      "sectionTitle": "4-1. 1유형 취업활동계획 수립(IAP 수립)",
      "snippet": "... 1유형 취업활동계획 수립(IAP 수립) -[IAP등록/관리]-[지급계획]에서 ...",
      "sectionUrl": "/national-employment-support-guide#4-1-1유형-취업활동계획-수립(iap-수립)"
    }
  ]
}
```

### 2.3. 핵심 비즈니스 로직 (Core Business Logic)

1.  `q` 쿼리 파라미터를 수신하고, 최소 검색어 길이를 검증합니다 (예: 2자 이상).
2.  `Manuals` 컬렉션의 `searchText` 필드(데이터 모델 참조)에 대해 Full-Text Search를 수행합니다.
3.  `manual_slug` 파라미터가 제공된 경우, 검색 쿼리에 필터(filter) 조건을 추가합니다.
4.  검색 결과로 반환된 각 `Manual` 문서에 대해, `mainContent`를 순회하며 키워드(`q`)가 포함된 섹션(`title` 또는 `content`)을 찾습니다.
5.  일치하는 각 섹션에 대해 `snippet` (키워드 주변 텍스트)을 생성하고, 프론트엔드 라우팅에 사용할 `sectionUrl` (slug + hash)을 조합합니다.
6.  결과 객체 배열을 생성하여 반환합니다.

### 2.4. 데이터 모델 (Data Model)

* `Manuals` 컬렉션의 `searchText` 필드와 해당 필드에 대한 **Text Index**를 활용합니다. (1.4 데이터 모델 섹션 참조)
* `searchText` 필드 로직: `Manual` 문서가 생성되거나 수정될 때, `mainContent` 배열의 모든 `text` 속성 값을 추출하여 하나의 긴 문자열로 병합 후 `searchText` 필드에 저장하는 로직(Trigger/Hook)이 필요합니다.

### 1.5. 예외 처리 (Exception Handling)

| HTTP 상태 | 에러 코드 | 설명 |
| :--- | :--- | :--- |
| `400 Bad Request` | `QUERY_TOO_SHORT` | `q` 파라미터가 없거나 최소 길이(예: 2자) 미만일 경우 |
| `500 Internal Server Error` | `SEARCH_ENGINE_ERROR` | Full-Text Search 엔진 또는 DB 쿼리 실패 시 |

-----

## 3\. 디자인 시스템 토큰 조회 (PRD: MAN-301, MAN-202 등)

### 3.1. 사용자 시나리오 (User Scenario)

프론트엔드 애플리케이션이 처음 로드될 때, 사이트 전체의 스타일(색상, 폰트, 간격)을 일관되게 적용하고 다크/라이트 모드 전환을 준비하기 위해 디자인 시스템 토큰(JSON)이 필요합니다.

### 3.2. API 명세 (API Specification)

* **Endpoint:** `GET /api/v1/design-system/tokens`
* **Method:** `GET`
* **Description:** 현재 활성화된 디자인 시스템의 모든 토큰(JSON)을 반환합니다.

**Request:**

* **Headers:**
    * `Accept: application/json`
    * `If-None-Match`: (Optional) 이전에 수신한 ETag 값을 전송하여 캐시된 버전을 확인합니다.

**Response (Success: 200 OK):**

> `wayfinding_design_system.json`의 내용과 동일한 구조를 반환합니다.

```json
{
  "meta": {
    "title": "Port Authority Wayfinding Manual - Design System JSON",
    "version": "1.0",
    "lastUpdated": "2025-11-10"
  },
  "theme": {
    "currentMode": "dark",
    "modes": ["dark", "light"],
    "storageKey": "wayfinding-theme"
  },
  "colors": {
    "dark": {
      "primary": {
        "background": {
          "main": "#161616",
          "rgb": "rgb(22, 22, 22)"
        },
        // ... (wayfinding_design_system.json의 'colors'와 동일)
      }
    },
    // ... (wayfinding_design_system.json의 모든 키와 동일)
  }
}
```

**Response (Cached: 304 Not Modified):**

* 클라이언트가 유효한 `If-None-Match` ETag를 보냈고, 서버의 데이터가 변경되지 않았을 경우, 빈 Body와 함께 304 상태 코드를 반환합니다.

### 3.3. 핵심 비즈니스 로직 (Core Business Logic)

1.  요청을 수신합니다.
2.  `DesignSystems` 컬렉션에서 `isActive: true`인 문서를 조회합니다. (이 데이터는 자주 변경되지 않으므로 Redis 등에 **강력하게 캐시**되어야 합니다.)
3.  조회된 문서의 ETag(예: `updatedAt` 타임스탬프 또는 콘텐츠 hash)를 생성합니다.
4.  요청 헤더의 `If-None-Match` 값과 서버의 ETag를 비교합니다.
    * **일치하면:** `304 Not Modified`를 응답합니다.
    * **불일치하면:** `200 OK`와 함께 `tokens` 필드의 전체 JSON 객체를 Body에 담아 반환하고, `ETag` 헤더를 응답에 포함시킵니다.

### 3.4. 데이터 모델 (Data Model)

**Collection: `DesignSystems`**

| 필드명 | 데이터 타입 | 설명 | 인덱스 |
| :--- | :--- | :--- | :--- |
| `_id` | `ObjectId` | 문서 고유 ID | (PK) |
| `name` | `String` | 디자인 시스템 이름 (예: "Wayfinding") | |
| `version` | `String` | 버전 (예: "1.0") | |
| `isActive` | `Boolean` | 현재 프로덕션에서 사용 중인 토큰인지 여부 | **Index** |
| `tokens` | `Object` | `wayfinding_design_system.json`의 전체 구조 | |
| `createdAt` | `Timestamp` | 생성 일시 | |
| `updatedAt` | `Timestamp` | 마지막 수정 일시 | |

### 1.5. 예외 처리 (Exception Handling)

| HTTP 상태 | 에러 코드 | 설명 |
| :--- | :--- | :--- |
| `404 Not Found` | `NO_ACTIVE_DESIGN_SYSTEM` | `isActive: true`인 디자인 시스템 문서를 찾을 수 없을 경우 |
| `500 Internal Server Error` | `CACHE_ERROR` | 캐시(Redis) 또는 DB 조회 중 오류 발생 시 |

-----

## 4\. 백엔드 제외 대상 (Out of Scope)

PRD에 명시되어 있으나 백엔드 API 호출이 필요 없는 순수 프론트엔드 기능은 다음과 같습니다.

* **MAN-301/302 (테마 모드 전환 및 유지):** PRD에 명시된 대로 `localStorage`를 사용합니다. 백엔드는 토큰(`colors.dark`, `colors.light`)을 제공할 뿐, 사용자의 현재 테마 상태를 저장하지 않습니다.
* **MAN-104 (활성 섹션 하이라이트):** 프론트엔드의 `useActiveSection.js` 훅과 같은 스크롤 감지 로직으로 처리됩니다.
* **MAN-101, 102 (레이아웃), MAN-202, 204 (스타일 적용):** 백엔드에서 제공된 데이터와 디자인 토큰을 기반으로 프론트엔드(React, CSS)가 렌더링을 책임집니다.