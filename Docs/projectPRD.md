# PRD: (가제) 온라인 매뉴얼 플랫폼

| 문서 버전 | 1.0 |
| --- | --- |
| **작성일** | 2025년 11월 10일 |
| **작성자** | (시니어 PM) |
| **최종 목표** | 텍스트 기반의 복잡한 매뉴얼을 명확하고, 접근성 높으며, 탐색하기 쉬운 반응형 웹사이트로 제공하는 플랫폼을 구축합니다. |

## 1\. 프로젝트 제품 개요

### 1.1. 배경 및 문제 정의

현재 다수의 조직 내부 매뉴얼, 가이드라인, 프로세스 문서(예: `ManualDocs.json`의 '국민취업지원제도 프로세스 가이드')가 정적인 파일(PDF, Word) 또는 구조화되지 않은 텍스트로 존재합니다. 이로 인해 다음과 같은 문제가 발생합니다.

* **탐색의 어려움:** 사용자가 원하는 정보를 신속하게 찾는 데 한계가 있습니다.
* **유지보수 비효율:** 콘텐츠 업데이트 및 버전 관리가 어렵고, 변경 이력을 추적하기 힘듭니다.
* **낮은 접근성 및 가독성:** 모바일 환경에서의 가독성이 떨어지며, 다크 모드 등 사용자 편의 기능이 부재합니다.

### 1.2. 제품 목표

본 프로젝트는 `wayfinding_design_system.json` 과 같은 정교한 디자인 시스템을 기반으로, 구조화된 매뉴얼 콘텐츠(`ManualDocs.json` 등)를 동적으로 렌더링하는 **확장 가능하고 사용자 친화적인 온라인 매뉴얼 플랫폼**을 구축하는 것을 목표로 합니다.

**핵심 가치:**

1.  **명확성 (Clarity):** 일관된 디자인 시스템(타이포그래피, 레이아웃)을 적용하여 콘텐츠 가독성을 극대화합니다.
2.  **탐색용이성 (Navigability):** 강력한 사이드바 목차(TOC)와 검색 기능을 통해 정보 접근 속도를 향상시킵니다.
3.  **접근성 (Accessibility):** 다크/라이트 모드, 키보드 네비게이션, WCAG AA 표준을 준수하여 모든 사용자의 접근성을 보장합니다.
4.  **반응형 (Responsive):** 데스크톱, 태블릿, 모바일 등 모든 기기에서 최적화된 뷰를 제공합니다.

-----

## 2\. 핵심 고객/사용자 정의

| 사용자 페르소나 | 핵심 니즈 (Needs) |
| --- | --- |
| **콘텐츠 소비자** <br> (예: 신입 상담사, 디자이너, 개발자) | \* 업무 수행에 필요한 정보를 **빠르게** 찾고 싶어 한다. <br> \* 복잡한 절차를 **이해하기 쉽게** 확인하고 싶어 한다. <br> \* 모바일 등 다양한 환경에서 **편안하게** 문서를 읽고 싶어 한다. |
| **콘텐츠 관리자** <br> (예: 선임 상담사, 기획자, 기술 작가) | \* 매뉴얼 내용을 **쉽게** 수정하고 배포하고 싶어 한다. <br> \* 변경 사항이 모든 사용자에게 **즉시** 적용되기를 원한다. <br> \* (향후) 콘텐츠 작성 및 관리를 위한 CMS를 필요로 한다. |

*(본 PRD는 주로 '콘텐츠 소비자'의 경험(UX)에 초점을 맞추어 정의합니다.)*

-----

## 3\. 기능적 요구사항

### 3.1. 레이아웃 및 네비게이션

| 기능 ID | 기능명 | 상세 설명 | 우선순위 |
| :--- | :--- | :--- | :--- |
| **MAN-101** | **글로벌 레이아웃** | `wayfinding_design_system.json`의 레이아웃 정의 준수. <br> \* Header, Left Sidebar (목차), Main Content, Footer로 구성. <br> \* 데스크톱 기준 사이드바 (212px), 메인 콘텐츠 (731px) 너비 적용. | **최상** |
| **MAN-102** | **반응형 레이아웃** | `layout.responsive` 정의에 따라 브레이크포인트(1024px, 768px)별 레이아웃 변경. <br> \* Tablet: 사이드바를 Drawer(서랍) 메뉴로 전환. <br> \* Mobile: 1단 컬럼 레이아웃으로 변경. | **최상** |
| **MAN-103** | **사이드바 목차(TOC)** | `ManualDocs.json`의 `tableOfContents`와 같은 계층형 구조를 렌더링. <br> \* `navigation.tableOfContents`의 다중 레벨(예: 2. 참여자 배정 \> 1. 참여자 관리...) 지원. <br> \* `components.navigation.sidebarMenu` 스타일 적용. | **최상** |
| **MAN-104** | **활성 섹션 하이라이트** | 사용자가 스크롤하거나 링크를 클릭 시, 현재 보고 있는 섹션에 해당하는 사이드바 항목을 하이라이트(Active 상태). <br> \* (예: `sidebarMenu.item.states.active` 스타일 적용) | **상** |
| **MAN-105** | **헤더 및 푸터** | 헤더: 로고, 검색창(`MAN-201`), 글로벌 링크(Glossary, Help) 포함. <br> 푸터: 저작권, 관련 링크, 테마 토글(`MAN-301`) 포함. | **상** |

### 3.2. 콘텐츠 렌더링

| 기능 ID | 기능명 | 상세 설명 | 우선순위 |
| :--- | :--- | :--- | :--- |
| **MAN-201** | **구조화된 콘텐츠 렌더링** | `ManualDocs.json`의 `mainContent` 배열과 같이 정의된 구조(title, paragraph, listitem, blockquote, image 등)를 HTML 태그로 변환하여 렌더링. | **최상** |
| **MAN-202** | **디자인 시스템 폰트 적용** | `typography.fonts` (HelveticaNowText-Regular, Noto Sans KR) 및 `typography.scales` (h1, h2, body 등)를 콘텐츠에 정확히 적용. | **최상** |
| **MAN-203** | **이미지 렌더링** | 콘텐츠 내 이미지 참조(예: `[image01.png]`)를 실제 이미지 경로로 매핑하여 출력. <br> \* `images.content` 스타일(최대 너비 100%, 그림자 등) 적용. | **중** |
| **MAN-204** | **컴포넌트 스타일 적용** | 정의된 컴포넌트 스타일 적용 (예: 링크 호버 시 `#FFE508` 색상 변경, 버튼 스타일 등). | **상** |

### 3.3. 인터랙션 및 접근성

| 기능 ID | 기능명 | 상세 설명 | 우선순위 |
| :--- | :--- | :--- | :--- |
| **MAN-301** | **다크/라이트 모드 전환** | `theme` 정의에 따라 다크 모드(기본값)와 라이트 모드 간 전환 기능 제공. <br> \* `cssVariables`를 활용하여 테마 색상 동적 변경. | **상** |
| **MAN-302** | **테마 상태 유지** | 사용자가 선택한 테마 모드(`dark`/`light`)를 `localStorage`에 저장하여 재방문 시에도 유지. (`theme.storageKey` 참조) | **상** |
| **MAN-303** | **접근성(A11y) 준수** | `accessibility` 정의 준수. <br> \* **포커스:** 키보드 포커스 시 명확한 아웃라인(`2px solid #FFE508`) 표시. <br> \* **대비:** WCAG AA 레벨의 텍스트/배경 대비 준수. <br> \* **시맨틱:** 모든 인터랙티브 요소에 시맨틱 HTML 적용. | **최상** |
| **MAN-304** | **검색 기능 (기본)** | 헤더에 위치한 검색창(`input.search` 스타일 적용). <br> \* (1단계) 매뉴얼 제목 및 본문 내 키워드 검색 기능. <br> \* (2단계) 검색 결과 페이지 또는 드롭다운으로 결과 표시. | **상** |

-----

## 4\. 상세 프로젝트 구조 (예상)

본 플랫폼은 React, Vue 등 모던 프론트엔드 프레임워크 사용을 전제로 하며, 다음과 같은 폴더/컴포넌트 구조를 제안합니다.

* `/public`
    * `/fonts` (HelveticaNowText-Regular, Noto Sans KR 폰트 파일)
* `/src`
    * `/components` (재사용 가능한 최소 단위 UI 컴포넌트)
        * `/ui`
            * `Button.jsx` (Primary, Secondary, Outline 버튼)
            * `Card.jsx` (Default, Highlighted 카드)
            * `Link.jsx` (Default 링크 스타일)
            * `SearchInput.jsx`
            * `ThemeToggle.jsx` (테마 전환 버튼)
        * `/layout`
            * `Header.jsx`
            * `Footer.jsx`
            * `Sidebar.jsx`
            * `MainContent.jsx`
        * `/navigation`
            * `TableOfContents.jsx` (사이드바 목차 루트)
            * `TocItem.jsx` (재귀적(recursive)으로 하위 목차 렌더링)
        * `/core`
            * `ContentRenderer.jsx` (**핵심:** `ManualDocs.json`의 JSON 구조를 해석하여 HTML 컴포넌트(Paragraph, List, Blockquote 등)로 매핑)
    * `/styles` (또는 `styled-components`, `tailwind.config.js`)
        * `theme.js` (`wayfinding_design_system.json`의 `colors`, `typography`, `spacing` 등을 JS 객체로 변환)
        * `global.css` (기본 폰트, 배경색, CSS 변수 선언)
    * `/contexts`
        * `ThemeContext.jsx` (다크/라이트 모드 상태 및 토글 함수 전역 관리)
    * `/hooks`
        * `useActiveSection.js` (스크롤 위치를 감지하여 활성 목차 ID를 반환하는 훅)
    * `/pages` (또는 `/app` in Next.js)
        * `/[...slug].jsx` (모든 매뉴얼 페이지를 동적으로 처리하는 Catch-all 라우트)
    * `/data` (또는 CMS/API)
        * `manualContent.json` (제공된 `ManualDocs.json` 콘텐츠)
        * `designSystemTokens.json` (제공된 `wayfinding_design_system.json` 원본)

-----

이 PRD를 기반으로 디자이너는 와이어프레임과 UI 디자인을 구체화하고, 개발자는 기술 스택을 확정하고 컴포넌트 개발을 시작할 수 있습니다.

혹시 이 구조를 바탕으로 특정 컴포넌트(예: 'ContentRenderer')의 동작 방식을 더 자세히 정의해 드릴까요?