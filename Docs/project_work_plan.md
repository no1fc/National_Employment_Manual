# 프로젝트 작업 계획서 (Project Work Plan)

| 항목 | 내용 |
|------|------|
| **프로젝트명** | 온라인 매뉴얼 플랫폼 (국민취업지원제도 가이드) |
| **문서 버전** | 1.0 |
| **작성일** | 2025년 11월 10일 |
| **기반 문서** | projectPRD.md, functional_specification.md, wayfinding_design_guide.md |

---

## 📋 목차

1. [프로젝트 개요](#1-프로젝트-개요)
2. [기술 스택](#2-기술-스택)
3. [개발 단계별 작업 계획](#3-개발-단계별-작업-계획)
4. [세부 작업 항목](#4-세부-작업-항목)
5. [일정 및 마일스톤](#5-일정-및-마일스톤)
6. [품질 관리](#6-품질-관리)
7. [리스크 관리](#7-리스크-관리)

---

## 1. 프로젝트 개요

### 1.1 목표
텍스트 기반의 복잡한 매뉴얼(국민취업지원제도 프로세스 가이드)을 명확하고 접근성 높으며 탐색하기 쉬운 반응형 웹사이트로 제공하는 플랫폼 구축

### 1.2 핵심 가치
- **명확성**: 일관된 디자인 시스템으로 가독성 극대화
- **탐색용이성**: 강력한 TOC와 검색 기능
- **접근성**: 다크/라이트 모드, 키보드 네비게이션, WCAG AA 준수
- **반응형**: 모든 기기 최적화

### 1.3 주요 사용자
- **콘텐츠 소비자**: 신입 상담사, 업무 담당자
- **콘텐츠 관리자**: 선임 상담사, 기획자

---

## 2. 기술 스택

### 2.1 프론트엔드
- **프레임워크**: Next.js 15 (App Router)
- **언어**: TypeScript
- **스타일링**: Tailwind CSS + CSS Variables
- **상태관리**: React Context API
- **폰트**: HelveticaNowText-Regular, Noto Sans KR

### 2.2 백엔드
- **런타임**: Node.js
- **데이터베이스**: MongoDB (Manuals, DesignSystems 컬렉션)
- **캐싱**: Redis (디자인 토큰 캐싱)
- **검색**: Full-Text Search Index

### 2.3 인프라
- **호스팅**: Vercel (프론트엔드)
- **스토리지**: AWS S3/CloudFront (이미지, 정적 자산)
- **버전관리**: Git

---

## 3. 개발 단계별 작업 계획

### Phase 1: 환경 설정 및 기본 구조 (1-2주)
**목표**: 프로젝트 초기 설정 및 디자인 시스템 구축

#### 작업 항목:
1. **프로젝트 초기화**
   - Next.js 프로젝트 생성
   - TypeScript, ESLint, Prettier 설정
   - 폴더 구조 설정 (PRD 섹션 4 참조)

2. **디자인 시스템 구현**
   - `wayfinding_design_system.json` 파싱 및 타입 정의
   - CSS Variables 생성 (`theme.js`, `global.css`)
   - 색상, 타이포그래피, 간격 시스템 적용

3. **기본 레이아웃 구조**
   - Header 컴포넌트
   - Sidebar 컴포넌트 (TOC)
   - Main Content 영역
   - Footer 컴포넌트

**완료 기준**:
- ✅ 기본 레이아웃이 다크/라이트 모드에서 정상 렌더링
- ✅ 디자인 토큰이 CSS 변수로 정상 적용

---

### Phase 2: 핵심 UI 컴포넌트 개발 (2-3주)
**목표**: 재사용 가능한 UI 컴포넌트 라이브러리 구축

#### 작업 항목:

**2.1 기본 컴포넌트 (`/components/ui`)**
- [ ] Button (Primary, Secondary, Outline)
- [ ] Link (Default, Active 상태)
- [ ] Card (Default, Highlighted)
- [ ] SearchInput
- [ ] ThemeToggle

**2.2 레이아웃 컴포넌트 (`/components/layout`)**
- [ ] Header (로고, 검색창, 네비게이션)
- [ ] Footer (저작권, 링크, 테마 토글)
- [ ] Sidebar (TOC 렌더링)
- [ ] MainContent (콘텐츠 래퍼)

**2.3 네비게이션 컴포넌트 (`/components/navigation`)**
- [ ] TableOfContents (계층형 목차)
- [ ] TocItem (재귀 렌더링, 활성 상태 처리)

**완료 기준**:
- ✅ 모든 컴포넌트가 Storybook에서 확인 가능
- ✅ 반응형 동작 검증 (Desktop, Tablet, Mobile)
- ✅ 접근성 검증 (키보드 네비게이션, 포커스)

---

### Phase 3: 콘텐츠 렌더링 시스템 (2-3주)
**목표**: JSON 기반 매뉴얼 콘텐츠를 동적으로 렌더링

#### 작업 항목:

**3.1 ContentRenderer 구현** (PRD: MAN-201)
- [ ] JSON 파서 (title, paragraph, listitem, blockquote, image 타입 처리)
- [ ] Heading 렌더링 (h1, h2, h3)
- [ ] Paragraph 렌더링
- [ ] List 렌더링 (ordered, unordered)
- [ ] Blockquote 렌더링
- [ ] Image 렌더링 (이미지 경로 매핑)

**3.2 타이포그래피 적용** (PRD: MAN-202)
- [ ] `typography.scales` 적용 (h1, h2, body 등)
- [ ] 폰트 로딩 최적화 (Preload, Font Display)
- [ ] 한글 폰트 지원 (Noto Sans KR)

**3.3 이미지 처리** (PRD: MAN-203)
- [ ] 이미지 참조 파싱 (`[image01.png]` → URL 변환)
- [ ] 이미지 스타일 적용 (그림자, 최대 너비)
- [ ] Lazy Loading 구현

**완료 기준**:
- ✅ `ManualDocs.json`의 모든 콘텐츠 타입 정상 렌더링
- ✅ 이미지 로딩 최적화 확인
- ✅ 모바일 환경에서 가독성 검증

---

### Phase 4: 네비게이션 및 인터랙션 (2주)
**목표**: 사용자 경험 향상을 위한 인터랙션 구현

#### 작업 항목:

**4.1 TOC 인터랙션** (PRD: MAN-103, MAN-104)
- [ ] 계층형 목차 렌더링 (다중 레벨 지원)
- [ ] 스크롤 위치 감지 (`useActiveSection` 훅)
- [ ] 활성 섹션 하이라이트 (Active 상태 적용)
- [ ] 목차 클릭 시 부드러운 스크롤

**4.2 반응형 네비게이션** (PRD: MAN-102)
- [ ] Desktop: 고정 사이드바
- [ ] Tablet: Drawer 메뉴
- [ ] Mobile: 햄버거 메뉴

**4.3 키보드 네비게이션** (PRD: MAN-303)
- [ ] Tab 키로 모든 인터랙티브 요소 접근
- [ ] 포커스 아웃라인 표시 (`2px solid #FFE508`)
- [ ] Skip Links 구현

**완료 기준**:
- ✅ 스크롤 시 TOC 활성 항목 자동 업데이트
- ✅ 모든 브레이크포인트에서 네비게이션 정상 동작
- ✅ WCAG AA 접근성 기준 통과

---

### Phase 5: 테마 및 접근성 (1-2주)
**목표**: 다크/라이트 모드 전환 및 접근성 강화

#### 작업 항목:

**5.1 테마 시스템** (PRD: MAN-301, MAN-302)
- [ ] ThemeContext 구현 (다크/라이트 모드 상태 관리)
- [ ] ThemeToggle 컴포넌트 (Footer 배치)
- [ ] localStorage 연동 (테마 선택 유지)
- [ ] CSS Variables 동적 전환

**5.2 접근성 강화** (PRD: MAN-303)
- [ ] 색상 대비 검증 (WCAG AA: 4.5:1)
- [ ] 포커스 관리 (모든 인터랙티브 요소)
- [ ] 시맨틱 HTML 적용 (nav, main, article)
- [ ] ARIA 속성 추가 (aria-label, aria-current)
- [ ] 스크린 리더 테스트

**완료 기준**:
- ✅ 테마 전환 시 즉시 반영 및 새로고침 후 유지
- ✅ axe DevTools 검사 통과 (0개 위반)
- ✅ 키보드 단독 사용으로 모든 기능 접근 가능

---

### Phase 6: 검색 기능 (2-3주)
**목표**: 매뉴얼 내 키워드 검색 기능 구현

#### 작업 항목:

**6.1 프론트엔드 검색 UI** (PRD: MAN-304)
- [ ] 헤더 검색창 UI (`input.search` 스타일)
- [ ] 검색 결과 페이지 또는 드롭다운
- [ ] 검색어 하이라이팅
- [ ] 최근 검색어 기록 (localStorage)

**6.2 백엔드 검색 API** (functional_specification.md 섹션 2)
- [ ] `GET /api/v1/search` 엔드포인트
- [ ] Full-Text Search 쿼리 (searchText 필드)
- [ ] 검색 결과 스니펫 생성
- [ ] 섹션 URL 생성 (slug + hash)

**6.3 검색 최적화**
- [ ] 디바운싱 (300ms)
- [ ] 최소 검색어 길이 검증 (2자)
- [ ] 검색 결과 페이지네이션

**완료 기준**:
- ✅ 키워드 입력 시 0.5초 이내 결과 표시
- ✅ 검색 결과 클릭 시 해당 섹션으로 이동
- ✅ 모바일에서 검색 UI 정상 동작

---

### Phase 7: 백엔드 API 개발 (2-3주)
**목표**: 콘텐츠 및 디자인 토큰 제공 API 구축

#### 작업 항목:

**7.1 매뉴얼 콘텐츠 API** (functional_specification.md 섹션 1)
- [ ] `GET /api/v1/manuals/{manual_slug}` 구현
- [ ] MongoDB 연동 (Manuals 컬렉션)
- [ ] 이미지 경로 변환 로직
- [ ] 에러 처리 (404, 500)

**7.2 디자인 시스템 API** (functional_specification.md 섹션 3)
- [ ] `GET /api/v1/design-system/tokens` 구현
- [ ] Redis 캐싱 (ETag 기반)
- [ ] 304 Not Modified 응답 처리

**7.3 데이터베이스 설정**
- [ ] MongoDB Atlas 클러스터 생성
- [ ] Manuals 컬렉션 스키마 정의
- [ ] DesignSystems 컬렉션 스키마 정의
- [ ] Text Index 생성 (searchText 필드)
- [ ] 초기 데이터 시딩

**완료 기준**:
- ✅ 모든 API 엔드포인트 정상 응답 (200 OK)
- ✅ Postman 테스트 통과
- ✅ 응답 시간 < 500ms

---

### Phase 8: 통합 및 데이터 연동 (1-2주)
**목표**: 프론트엔드와 백엔드 연동

#### 작업 항목:

**8.1 API 클라이언트 구현**
- [ ] Fetch/Axios 래퍼 함수
- [ ] 에러 핸들링 (네트워크 오류, 타임아웃)
- [ ] 로딩 상태 관리

**8.2 데이터 페칭**
- [ ] 매뉴얼 콘텐츠 페칭 (getStaticProps 또는 SSR)
- [ ] 디자인 토큰 페칭 (앱 초기화 시)
- [ ] 검색 API 연동

**8.3 이미지 CDN 설정**
- [ ] S3 버킷 생성 및 CloudFront 배포
- [ ] 이미지 업로드 및 경로 매핑
- [ ] 이미지 최적화 (WebP, 다중 해상도)

**완료 기준**:
- ✅ 실제 API에서 데이터 페칭 성공
- ✅ 이미지 CDN에서 정상 로딩
- ✅ 에러 상태 UI 정상 표시

---

### Phase 9: 테스트 및 QA (2주)
**목표**: 품질 보증 및 버그 수정

#### 작업 항목:

**9.1 단위 테스트**
- [ ] 컴포넌트 테스트 (Jest, React Testing Library)
- [ ] 유틸리티 함수 테스트
- [ ] 테스트 커버리지 > 80%

**9.2 통합 테스트**
- [ ] E2E 테스트 (Playwright)
- [ ] API 통합 테스트
- [ ] 사용자 시나리오 테스트

**9.3 접근성 테스트**
- [ ] axe DevTools 자동 검사
- [ ] 스크린 리더 수동 테스트 (NVDA, JAWS)
- [ ] 키보드 네비게이션 테스트

**9.4 성능 테스트**
- [ ] Lighthouse 검사 (90+ 점수)
- [ ] Core Web Vitals 측정
- [ ] 네트워크 성능 최적화

**9.5 크로스 브라우저 테스트**
- [ ] Chrome 90+
- [ ] Firefox 88+
- [ ] Safari 14+
- [ ] Edge 90+

**완료 기준**:
- ✅ 모든 테스트 통과
- ✅ Lighthouse Performance > 90
- ✅ 크리티컬 버그 0개

---

### Phase 10: 배포 및 모니터링 (1주)
**목표**: 프로덕션 환경 배포 및 모니터링 설정

#### 작업 항목:

**10.1 배포 설정**
- [ ] Vercel 프로젝트 생성 및 연동
- [ ] 환경변수 설정 (API URL, CDN URL)
- [ ] CI/CD 파이프라인 (GitHub Actions)
- [ ] 도메인 연결

**10.2 모니터링 설정**
- [ ] Vercel Analytics 활성화
- [ ] 에러 트래킹 (Sentry)
- [ ] 로그 수집 (CloudWatch)
- [ ] 성능 모니터링 (Web Vitals)

**10.3 문서화**
- [ ] README 작성
- [ ] API 문서 (Swagger/OpenAPI)
- [ ] 컴포넌트 문서 (Storybook)
- [ ] 배포 가이드

**완료 기준**:
- ✅ 프로덕션 배포 성공
- ✅ 모니터링 대시보드 정상 동작
- ✅ 문서화 완료

---

## 4. 세부 작업 항목

### 4.1 우선순위별 기능 목록

#### 최상 우선순위 (P0) - Phase 1-4
| 기능 ID | 기능명 | 상세 설명 |
|---------|--------|-----------|
| MAN-101 | 글로벌 레이아웃 | Header, Sidebar, Main, Footer 구조 |
| MAN-102 | 반응형 레이아웃 | 브레이크포인트별 레이아웃 변경 |
| MAN-103 | 사이드바 목차 | 계층형 TOC 렌더링 |
| MAN-201 | 구조화된 콘텐츠 렌더링 | JSON → HTML 변환 |
| MAN-202 | 디자인 시스템 폰트 적용 | 타이포그래피 스케일 적용 |
| MAN-303 | 접근성 준수 | WCAG AA 레벨 준수 |

#### 상 우선순위 (P1) - Phase 5-6
| 기능 ID | 기능명 | 상세 설명 |
|---------|--------|-----------|
| MAN-104 | 활성 섹션 하이라이트 | 스크롤 위치 감지 및 TOC 업데이트 |
| MAN-105 | 헤더 및 푸터 | 로고, 검색창, 테마 토글 |
| MAN-204 | 컴포넌트 스타일 적용 | 링크, 버튼 호버 상태 |
| MAN-301 | 다크/라이트 모드 전환 | 테마 토글 기능 |
| MAN-302 | 테마 상태 유지 | localStorage 저장 |
| MAN-304 | 검색 기능 | 키워드 검색 및 결과 표시 |

#### 중 우선순위 (P2) - Phase 7-8
| 기능 ID | 기능명 | 상세 설명 |
|---------|--------|-----------|
| MAN-203 | 이미지 렌더링 | 이미지 경로 매핑 및 스타일 적용 |
| BE-101 | 매뉴얼 콘텐츠 API | GET /api/v1/manuals/{slug} |
| BE-102 | 검색 API | GET /api/v1/search |
| BE-103 | 디자인 토큰 API | GET /api/v1/design-system/tokens |

---

## 5. 일정 및 마일스톤

### 5.1 전체 일정 (16-20주)

```
Week 1-2   : Phase 1 - 환경 설정 및 기본 구조
Week 3-5   : Phase 2 - UI 컴포넌트 개발
Week 6-8   : Phase 3 - 콘텐츠 렌더링 시스템
Week 9-10  : Phase 4 - 네비게이션 및 인터랙션
Week 11-12 : Phase 5 - 테마 및 접근성
Week 13-15 : Phase 6 - 검색 기능
Week 13-15 : Phase 7 - 백엔드 API 개발 (병렬)
Week 16-17 : Phase 8 - 통합 및 데이터 연동
Week 18-19 : Phase 9 - 테스트 및 QA
Week 20    : Phase 10 - 배포 및 모니터링
```

### 5.2 주요 마일스톤

| 마일스톤 | 완료 예정 | 완료 기준 |
|----------|-----------|-----------|
| M1: 프로토타입 | Week 5 | 기본 레이아웃 + UI 컴포넌트 완성 |
| M2: 콘텐츠 렌더링 | Week 8 | ManualDocs.json 전체 렌더링 |
| M3: 인터랙션 완성 | Week 12 | TOC, 테마 전환, 접근성 통과 |
| M4: 검색 기능 | Week 15 | 검색 API 및 UI 완성 |
| M5: 통합 완료 | Week 17 | 프론트엔드-백엔드 통합 |
| M6: 프로덕션 배포 | Week 20 | 배포 및 모니터링 완료 |

---

## 6. 품질 관리

### 6.1 코드 품질 기준
- **ESLint**: Airbnb 스타일 가이드
- **Prettier**: 코드 포맷팅 자동화
- **TypeScript**: Strict 모드
- **커밋 컨벤션**: Conventional Commits

### 6.2 테스트 목표
- **단위 테스트 커버리지**: > 80%
- **E2E 테스트**: 주요 사용자 시나리오 커버
- **접근성**: axe DevTools 0개 위반
- **성능**: Lighthouse Performance > 90

### 6.3 코드 리뷰 프로세스
1. 모든 PR은 최소 1명의 승인 필요
2. CI 테스트 통과 필수
3. 접근성 및 성능 영향 검토

---

## 7. 리스크 관리

### 7.1 주요 리스크

| 리스크 | 영향도 | 발생 확률 | 대응 방안 |
|--------|--------|-----------|-----------|
| 디자인 시스템 복잡도 | 높음 | 중간 | Phase 1에서 조기 검증, 점진적 적용 |
| 검색 성능 저하 | 중간 | 중간 | Full-Text Index 최적화, 페이지네이션 |
| 접근성 미준수 | 높음 | 낮음 | 개발 단계부터 접근성 테스트 병행 |
| 브라우저 호환성 이슈 | 중간 | 낮음 | Polyfill 적용, 크로스 브라우저 테스트 |
| 이미지 로딩 속도 | 중간 | 중간 | CDN 사용, Lazy Loading, WebP 포맷 |

### 7.2 의존성 리스크
- **폰트 로딩 실패**: System Font Stack 폴백
- **API 응답 지연**: 로딩 상태 UI, 타임아웃 처리
- **MongoDB 다운타임**: 에러 페이지 표시, 재시도 로직

---

## 8. 향후 확장 계획 (Out of Scope - v2.0)

### 8.1 Phase 11: CMS 구축
- 콘텐츠 관리자를 위한 관리 페이지
- WYSIWYG 에디터
- 버전 관리 및 배포 워크플로우

### 8.2 Phase 12: 고급 검색
- 필터 기능 (섹션, 날짜)
- 자동완성 (Autocomplete)
- 검색 분석 (Search Analytics)

### 8.3 Phase 13: 다국어 지원
- i18n 라이브러리 통합
- 언어별 콘텐츠 관리
- URL 구조 변경 (/ko, /en)

### 8.4 Phase 14: 사용자 인증
- 로그인/로그아웃 기능
- 역할 기반 접근 제어 (RBAC)
- 즐겨찾기, 히스토리 기능

---

## 9. 참고 문서

- **PRD**: `Docs/projectPRD.md`
- **기능 명세서**: `Docs/functional_specification.md`
- **디자인 가이드**: `Docs/wayfinding_design_guide.md`
- **디자인 시스템 JSON**: `Docs/wayfinding_design_system.json`

---

## 10. 변경 이력

| 버전 | 날짜 | 변경 내용 | 작성자 |
|------|------|-----------|--------|
| 1.0 | 2025-11-10 | 초기 작성 | PM |

---

**마지막 업데이트**: 2025년 11월 10일
**다음 리뷰 예정일**: Phase 완료 시 업데이트
