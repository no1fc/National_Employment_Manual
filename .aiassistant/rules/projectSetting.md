---
적용: 항상
---

# ProjectSetting.md: 온라인 매뉴얼 플랫폼

**문서 버전: 1.0**
**작성자: Tech Lead**
**최종 업데이트: 2025년 11월 10일**

---

이 문서는 '온라인 매뉴얼 플랫폼' 프로젝트의 절대적인 기술 표준, 아키텍처, 코딩 규칙을 정의합니다. 프로젝트에 참여하는 **모든 개발자 및 AI 코딩 어시스턴트(Claude Code 등)는 이 문서의 규칙을 예외 없이 반드시 준수해야 합니다.**

이 문서는 PRD의 요구사항과 디자인 시스템의 제약 조건을 충족시키기 위해 설계되었습니다. AI 어시스턴트는 코드 생성 시 이 문서를 최우선 순위의 지침(System Prompt)으로 삼아야 하며, 특히 '절대 사용하지 말 것' 섹션에 명시된 모든 항목을 원천적으로 차단해야 합니다.

---

## 1. 핵심 기술 스택 (Core Technology Stack)

본 프로젝트는 PRD의 요구사항(반응형, 동적 콘텐츠 렌더링, 테마 변경)을 충족시키기 위해 모던 타입스크립트 기반의 React 생태계를 사용합니다.

| 카테고리 (Category) | 기술명 (Technology) | 버전 (Version) | 핵심 사유 (Reason) |
| :--- | :--- | :--- | :--- |
| **언어 (Language)** | **TypeScript** | `5.x+` | JSON 데이터 구조의 안정적인 처리 및 코드 품질 보장 |
| **프레임워크 (Framework)** | **Next.js** (App Router) | `14.x+` | 파일 기반 라우팅 (`[...slug]`), 레이아웃, SSR/SSG 지원 |
| **UI 라이브러리** | **React** | `18.x+` | PRD가 제안한 컴포넌트 기반 아키텍처 구현 |
| **스타일링 (Styling)** | **Styled-Components** | `5.x+` | 동적 테마(Dark/Light) 적용 및 컴포넌트 스코프 스타일링 |
| **상태 관리 (State)** | **React Context API** | (React 내장) | 전역 테마 상태(ThemeContext) 관리에 사용 (PRD `MAN-301`) |
| **Linter / Formatter** | **ESLint** & **Prettier** | 최신 | 일관된 코드 스타일 및 규칙 강제 |
| **데이터 소스 (Data)** | **Static JSON** | `N/A` | `ManualDocs.json` 파일을 `fetch` 또는 `import`하여 사용 |

---

## 2. 개발 환경 (Development Environment)

모든 팀원은 동일한 개발 환경을 사용하여 일관성을 유지합니다.

| 구분 (Category) | 도구 (Tool) | 표준 (Standard) | 비고 (Note) |
| :--- | :--- | :--- | :--- |
| **IDE** | **Visual Studio Code** | `최신 버전` | 권장 확장 프로그램: ESLint, Prettier, Styled-Components |
| **버전 관리 (VCS)** | **Git** | `N/A` | `main` 브랜치 보호. `feature/` 브랜치에서 작업 후 PR. |
| **저장소 (Repository)** | **GitHub** | `N/A` | PR 시 Lint 및 Type Check 자동 실행 (GitHub Actions) |
| **패키지 매니저** | **Yarn** | `1.x` | `yarn.lock` 파일을 통한 의존성 고정 |
| **Node.js 버전** | **Node.js** | `20.x (LTS)` | `nvm` 또는 `corepack`을 통한 버전 고정 |

---

## 3. 코딩 규칙 및 아키텍처 원칙

AI 어시스턴트는 아래 명시된 규칙을 **절대적으로** 준수하여 코드를 생성해야 합니다.

### 3.1. 핵심 아키텍처 원칙 (Core Principles)

1.  **모듈화 (Modularity) 및 단일 책임 원칙 (SRP)**
    * 모든 컴포넌트와 함수는 하나의 명확한 기능(책임)만 가져야 합니다.
    * PRD의 `/src/components` 구조를 따르며, 재사용 가능한 최소 단위(`ui`)와 조합된 단위(`layout`, `navigation`)를 명확히 분리합니다.

2.  **재사용성 (Reusability) 및 낮은 결합도 (Loose Coupling)**
    * 컴포넌트는 특정 컨텍스트에 종속되지 않도록 `props`를 통해 데이터를 주입받아야 합니다.
    * 비즈니스 로직은 UI 컴포넌트에서 분리하여 커스텀 훅 (`/hooks`) 또는 유틸리티 함수 (`/utils`)로 추출합니다.

3.  **데이터 흐름 (Data Flow)**
    * 데이터는 부모에서 자식으로 (Props Down) 단방향으로 흐릅니다.
    * 자식의 상태 변경은 부모가 전달한 콜백 함수 (Events Up)를 통해 처리합니다.

4.  **디자인 시스템 우선 (Design System First)**
    * 모든 시각적 요소(색상, 폰트, 간격)는 `wayfinding_design_system.json`에서 파생된 `theme.js` 파일을 참조해야 합니다. (PRD `MAN-202`)

### 3.2. 반드시 사용할 것 (MUST USE)

* **TypeScript (Strict Mode):** `tsconfig.json`에서 `strict: true`를 사용합니다.
* **Next.js App Router:** 모든 라우팅 및 레이아웃 구성은 App Router (`/app` 디렉토리) 규칙을 따릅니다.
* **React Functional Components & Hooks:** 모든 컴포넌트는 함수형 컴포넌트와 훅(`useState`, `useEffect`, `useContext`)을 사용해 작성합니다.
* **Named Exports:** 컴포넌트와 유틸리티는 `export const MyComponent`와 같이 **Named Export**를 사용합니다. (Default Export 금지)
* **Styled-Components:** 모든 스타일링은 `styled-components`를 사용합니다.
    * `ThemeProvider`를 `ThemeContext`와 연동하여 다크/라이트 모드를 구현합니다. (PRD `MAN-301`)
    * 스타일 내부에서는 `props.theme`를 참조하여 디자인 토큰을 사용합니다. (예: `color: ${props => props.theme.colors.text.main};`)
* **타입/인터페이스 정의:**
    * 모든 `props`에 대해 `interface` 또는 `type`을 정의합니다.
    * `ManualDocs.json`의 스키마( `mainContent` 배열 등)에 대한 명확한 타입을 `/types` 디렉토리에 정의하고 `import`하여 사용합니다.
* **컴포넌트 구조:**
    * **`ContentRenderer.tsx` (핵심):** PRD의 `MAN-201` 요구사항을 구현하는 **재귀(Recursive) 컴포넌트**를 작성해야 합니다. 이 컴포넌트는 JSON 배열을 `map` 돌며 `type` (e.g., `paragraph`, `listitem`, `blockquote`)에 따라 적절한 Styled Component (e.g., `<StyledP>`, `<StyledLi>`)를 렌더링해야 합니다.
    * **시맨틱 HTML:** `Header`, `Footer`는 각각 `<header>`, `<footer>`를, `Sidebar`는 `<aside>` 또는 `<nav>`를, `MainContent`는 `<main>` 태그를 사용합니다. (PRD `MAN-303`)
* **접근성 (A11y):**
    * 모든 `<img>` 태그에는 `alt` 속성을 제공합니다.
    * 모든 인터랙티브 요소(버튼, 링크)는 키보드 포커스가 가능해야 하며, `accessibility.focus` 정의에 따라 `focus-visible` 상태를 스타일링합니다. (PRD `MAN-303`)

### 3.3. 절대 사용하지 말 것 (MUST NOT USE)

* **`any` 타입:** **`any` 타입의 사용을 절대 금지합니다.** 타입을 알 수 없는 경우 `unknown`을 사용 후 타입 가드를 수행합니다.
* **JavaScript 파일:** `.js` 또는 `.jsx` 파일 생성을 금지합니다. 모든 코드는 **`.ts` 또는 `.tsx`** 여야 합니다.
* **React Class Components:** **클래스형 컴포넌트의 사용을 절대 금지합니다.**
* **Next.js Pages Router:** `/pages` 디렉토리를 사용하는 구형 Pages Router 방식을 금지합니다.
* **인라인 스타일 (Inline Styles):** `style={{ color: 'red' }}`와 같은 인라인 스타일 사용을 **절대 금지합니다.** (애니메이션 등 예외적인 경우 제외)
* **일반 CSS/Sass:** `*.css` 또는 `*.scss` 파일을 생성하여 `import`하는 방식을 금지합니다. (예외: `global.css`의 폰트 정의 및 CSS Reset)
* **스타일링 대안:** **Tailwind CSS, Emotion, CSS Modules** 등 `styled-components` 외의 다른 스타일링 라이브러리 사용을 금지합니다.
* **하드코딩된 디자인 값:**
    * `color: '#161616'`, `font-size: '16px'`와 같이 디자인 시스템 토큰을 참조하지 않는 하드코딩된 값을 **절대 금지합니다.**
    * `Theme` 객체를 통해서만 값을 사용해야 합니다.
* **대규모 상태 관리 라이브러리:** **Redux, Zustand, MobX** 등의 라이브러리 사용을 금지합니다. 전역 상태는 `ThemeContext`로 충분합니다.
* **Default Exports:** 코드 일관성을 위해 `export default` 사용을 금지합니다.