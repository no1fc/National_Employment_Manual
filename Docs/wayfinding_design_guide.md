# Port Authority Wayfinding Manual - 디자인 가이드

## 개요

Port Authority Wayfinding Manual은 공항 표지판 및 안내 시스템을 위한 종합 온라인 매뉴얼입니다. 이 가이드는 사이트의 시각적 디자인, 레이아웃, 타이포그래피, 컬러 시스템을 정의합니다.

**사이트 URL**: https://wayfinding.panynj.gov/  
**문서 제목**: Port Authority Wayfinding Manual  
**현재 테마**: Dark Mode (Light 모드 전환 가능)

---

## 1. 시스템 구조

### 1.1 페이지 구성

사이트는 다음 주요 섹션으로 구성됩니다:

- **헤더 (Header)**: 로고, 검색창, 네비게이션 (Glossary, Help)
- **좌측 사이드바 (Left Sidebar)**: 목차 네비게이션 (10개 주요 섹션)
- **메인 컨텐츠 영역**: Dashboard 및 주요 콘텐츠
- **푸터 (Footer)**: 저작권, 링크, 테마 전환 버튼

### 1.2 주요 레이아웃 치수

| 요소 | 크기 |
|------|------|
| 사이드바 너비 | 212px |
| 컨테이너 너비 | 943px |
| 전체 페이지 너비 | 1155px (추정) |
| 배경 색상 | Dark (#161616) |

---

## 2. 컬러 팔레트

### 2.1 핵심 색상

#### Dark Theme (기본)

| 색상명 | RGB | HEX | 사용처 |
|--------|-----|-----|--------|
| Background Dark | rgb(22, 22, 22) | #161616 | 페이지 배경 |
| Text Primary | rgb(245, 245, 245) | #F5F5F5 | 주 텍스트 |
| Text Secondary | rgb(118, 118, 118) | #767676 | 보조 텍스트 |
| Accent Yellow | rgb(255, 229, 8) | #FFE508 | 강조색 (다운로드 버튼, 박스) |
| Border/Divider | rgb(100, 100, 100) | #646464 | 테두리, 구분선 |
| Neutral Gray | rgb(60, 60, 60) | #3C3C3C | 버튼 배경, 호버 상태 |

#### Light Theme (선택 가능)

라이트 테마는 다크 테마의 역상입니다:
- 배경: 흰색 (rgb(240, 240, 240))
- 텍스트: 검은색 (rgb(0, 0, 0))
- 강조색: 동일 (rgb(255, 229, 8))

### 2.2 기능별 컬러 사용

**Primary Background**: #161616  
**Primary Text**: #F5F5F5  
**Accent/Highlight**: #FFE508 (노란색 - 중요 요소 강조)  
**Hover/Active**: #3C3C3C (밝은 회색)  
**Disabled/Muted**: #767676 (보조 텍스트)

---

## 3. 타이포그래피

### 3.1 폰트 패밀리

#### 기본 폰트
- **Primary Font**: HelveticaNowText-Regular (본문, UI)
- **Secondary Font**: "Noto Sans KR" (한글 지원)
- **Fallback**: System fonts (sans-serif)

### 3.2 폰트 크기 및 계층

| 요소 | 크기 | 두께 | 사용처 |
|------|------|------|--------|
| 페이지 기본 | 16px | Regular | 본문 텍스트 |
| 제목 (Dashboard) | 32px+ | Bold/Semi-bold | 페이지 제목 |
| 소제목 | 20-24px | Semi-bold | 섹션 제목 |
| 본문 | 14-16px | Regular | 일반 텍스트 |
| 캡션/Date | 12-13px | Regular | 날짜, 보조 정보 |
| 메뉴 항목 | 14px | Regular | 네비게이션 |

### 3.3 라인 높이
- **기본 라인 높이**: 1.5-1.6 (텍스트 가독성)
- **타이트 라인 높이**: 1.2 (제목용)
- **루즈 라인 높이**: 1.8 (긴 텍스트용)

---

## 4. 컴포넌트 디자인

### 4.1 버튼

#### 다운로드 버튼 스타일
```
배경색: rgb(255, 229, 8) (#FFE508)
텍스트색: Black (#000000)
패딩: 12px 20px
보더 라디우스: 4px
폰트: Bold, 14px
호버 상태: 밝기 감소 (-10%)
```

#### 액션 버튼 (Load Older 등)
```
배경색: rgb(60, 60, 60) (#3C3C3C)
텍스트색: rgb(245, 245, 245) (#F5F5F5)
패딩: 10px 16px
보더 라디우스: 4px
폰트: Regular, 13px
호버 상태: 배경 밝게 (rgb(100, 100, 100))
```

### 4.2 링크

**기본 링크 스타일**
```
색상: rgb(245, 245, 245) (#F5F5F5)
텍스트 데코레이션: None
호버 상태: 색상 변경 (rgb(255, 229, 8)) 또는 언더라인
```

**내부 링크** (목차, 네비게이션)
```
색상: rgb(245, 245, 245) (#F5F5F5)
클릭 시: 활성화 표시 (배경색 변경)
```

### 4.3 입력 필드

**검색 박스**
```
배경색: rgba(22, 22, 22, 0.9) (반투명 검정)
보더: 1px solid rgb(100, 100, 100)
텍스트색: rgb(245, 245, 245)
패딩: 10px 12px
보더 라디우스: 4px
플레이스홀더 색상: rgb(118, 118, 118)
```

### 4.4 카드/박스

**콘텐츠 박스**
```
배경색: rgb(60, 60, 60) (#3C3C3C) 또는 투명
보더: 1px solid rgb(100, 100, 100)
패딩: 20px
보더 라디우스: 6px
그림자: 0 4px 6px rgba(0, 0, 0, 0.3)
```

**강조 박스 (다운로드 영역)**
```
배경색: rgb(255, 229, 8) (#FFE508) - 노란색
패딩: 30px
보더 라디우스: 4px
텍스트색: Black
마진: 20px 0
```

### 4.5 테이블/네비게이션

**사이드바 목차**
```
배경색: 투명 또는 rgb(22, 22, 22)
보더: 1px solid rgb(100, 100, 100)
행 높이: 40px
호버 상태: 배경 rgb(60, 60, 60)
활성 항목: 배경 rgb(100, 100, 100)
```

**테이블 스타일**
```
보더: 1px solid rgb(100, 100, 100)
헤더 배경: rgb(60, 60, 60)
행 호버: rgb(40, 40, 40)
패딩: 12px 16px
```

---

## 5. 레이아웃 패턴

### 5.1 그리드 시스템

**컨테이너**
- 최대 너비: 943px
- 패딩: 20px (양쪽)
- 마진: 0 auto (중앙 정렬)

**컬럼 레이아웃**
- 3단 구성: Sidebar (212px) + Main (731px)
- 갭: 20px
- Responsive: 768px 이하에서 단일 컬럼

### 5.2 공백 및 마진

| 요소 | 마진 | 패딩 |
|------|------|------|
| 섹션 | 40px bottom | 20px all |
| 제목 | 20px bottom | 0 |
| 단락 | 16px bottom | 0 |
| 리스트 항목 | 8px bottom | 12px left |
| 카드 | 16px bottom | 20px all |

### 5.3 일반적인 레이아웃 블록

```
┌─────────────────────────────────────────┐
│ 헤더 (로고, 검색, 네비)                   │
├──────────────┬──────────────────────────┤
│              │                          │
│  사이드바     │  메인 콘텐츠 영역         │
│  (목차)      │  - Dashboard             │
│  212px       │  - Quick Links           │
│              │  - Updates & News        │
│              │  - Change Log            │
│              │  731px                   │
│              │                          │
├──────────────┴──────────────────────────┤
│ 푸터 (저작권, 링크, 테마)                 │
└─────────────────────────────────────────┘
```

---

## 6. 인터랙션 및 상태

### 6.1 호버 상태

**링크 호버**
```
색상 변경: #F5F5F5 → #FFE508
텍스트 데코레이션: Underline 추가
트랜지션: 0.2s ease-in-out
```

**버튼 호버**
```
배경색 변경: -10% 밝기
그림자 추가: 0 4px 12px rgba(0, 0, 0, 0.4)
트랜지션: 0.2s ease
```

**네비게이션 항목 호버**
```
배경색 변경: #3C3C3C → #646464
텍스트색: 유지
트랜지션: 0.15s ease
```

### 6.2 활성 상태

**현재 페이지/섹션**
```
배경색: rgb(100, 100, 100) (#646464)
왼쪽 보더: 4px solid rgb(255, 229, 8)
텍스트 굵기: Bold로 표시
```

### 6.3 비활성 상태

```
색상: rgb(118, 118, 118) (#767676)
포인터: not-allowed
투명도: 0.6
```

### 6.4 로딩 상태

```
투명도: 0.5-0.7
커서: loading
애니메이션: Fade or Spinner (구체 사항 확인 필요)
```

---

## 7. 반응형 디자인 (Responsive)

### 7.1 브레이크포인트

| 기기 | 너비 | 레이아웃 |
|------|------|---------|
| Desktop | 1024px+ | 사이드바 + 메인 콘텐츠 |
| Tablet | 768px-1023px | 축소된 사이드바 또는 드로어 |
| Mobile | <768px | 풀 너비 단일 컬럼 |

### 7.2 모바일 적응

- 사이드바: 해머거 메뉴로 변환
- 테이블: 수평 스크롤 또는 카드 형태
- 다운로드 박스: 전체 너비
- 폰트 크기: 14px (기본 16px에서 축소)
- 마진/패딩: 15px (기본 20px에서 축소)

---

## 8. 접근성 (Accessibility)

### 8.1 색상 대비

- **WCAG AA 기준**: 최소 4.5:1 (일반 텍스트)
- **WCAG AAA 기준**: 최소 7:1 (권장)

**검증된 대비**
- 텍스트 (#F5F5F5) vs 배경 (#161616): 높음 ✓
- 강조색 (#FFE508) vs 배경 (#161616): 높음 ✓

### 8.2 포커스 상태

```
포커스 아웃라인: 2px solid #FFE508
포커스 오프셋: 2px
초점 링: 모든 인터랙티브 요소에 표시
```

### 8.3 기타

- 모든 이미지: alt 태그 필수
- 폼 필드: label 요소 필수
- 키보드 네비게이션: Tab 키로 모든 요소 접근 가능
- 스크린 리더: 의미론적 HTML 사용

---

## 9. 다크/라이트 모드 전환

### 9.1 테마 토글

위치: 페이지 하단 오른쪽  
버튼: "Light" / "Dark" 선택

### 9.2 각 테마의 색상 매핑

**Dark Mode (기본)**
```
배경: #161616
텍스트: #F5F5F5
강조: #FFE508
```

**Light Mode**
```
배경: #F0F0F0
텍스트: #000000
강조: #FFE508 (변화 없음)
```

### 9.3 저장 방식

- localStorage 사용
- 사용자 선택 기억
- 시스템 설정과 동기화 (선택)

---

## 10. 아이콘 및 이미지

### 10.1 아이콘 스타일

- **크기**: 16px, 24px, 32px (표준)
- **색상**: 텍스트 색상 상속 (#F5F5F5)
- **스트로크**: 1-2px
- **형식**: SVG 선호, PNG 대체

### 10.2 이미지

**로고**
- 포맷: SVG 또는 PNG
- 배경색: 투명 또는 조화로운 색상
- 호버 효과: 없음 (정적)

**콘텐츠 이미지**
- 최대 너비: 100% (컨테이너 내)
- 박스 섀도우: 0 2px 8px rgba(0, 0, 0, 0.2)
- 보더 라디우스: 4px

**다운로드 미리보기**
- 높이: 300px-400px
- 종횡비: 유지 (aspect-ratio)
- 가운데 정렬

---

## 11. 애니메이션 및 트랜지션

### 11.1 일반 규칙

- **기본 기간**: 0.2-0.3s
- **이징 함수**: ease-in-out
- **CPU 효율**: transform, opacity 사용

### 11.2 일반적인 애니메이션

**페이드 인/아웃**
```css
opacity: 0 → 1 (0.3s ease-in-out)
```

**슬라이드**
```css
transform: translateX(-20px) → translateX(0) (0.3s ease-out)
```

**호버 이펙트**
```css
transform: scale(1.05) (0.2s ease)
```

**로딩 애니메이션**
```css
animation: spin 1s linear infinite
```

---

## 12. 브라우저 호환성

### 12.1 지원 브라우저

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

### 12.2 폴백

- CSS Grid: Fallback flexbox
- CSS Custom Properties: Precompiled 값
- Transform: Fallback position change

---

## 13. 성능 최적화

### 13.1 로딩

- 폰트: Preload 또는 System font 사용
- 이미지: Lazy loading 적용
- CSS: 최소화 및 번들링

### 13.2 렌더링

- Paint: 최소화 (transform 사용)
- Layout: Reflow 최소화
- Composite: GPU 가속화

---

## 14. 코딩 표준

### 14.1 CSS 네이밍

```
BEM (Block Element Modifier)
.dashboard__section
.button--primary
.card__header-title
```

### 14.2 CSS 변수 (Custom Properties)

```css
--color-bg-dark: #161616;
--color-text-primary: #F5F5F5;
--color-accent: #FFE508;
--spacing-small: 8px;
--spacing-medium: 16px;
--spacing-large: 24px;
--font-primary: 'HelveticaNowText-Regular', sans-serif;
--radius-small: 4px;
--radius-medium: 6px;
--shadow-default: 0 4px 6px rgba(0, 0, 0, 0.3);
```

---

## 15. 추가 리소스

### 15.1 관련 문서

- Port Authority 공식 가이드라인
- WCAG 2.1 접근성 표준
- Material Design 원칙 (참고)

### 15.2 디자인 도구

- Figma / Adobe XD (프로토타입)
- Storybook (컴포넌트 카탈로그)
- Chrome DevTools (검사 및 디버깅)

---

**마지막 업데이트**: 2025년 11월 10일  
**버전**: 1.0  
**작성자**: Design System Team
