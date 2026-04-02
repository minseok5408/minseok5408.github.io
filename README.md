# 석류의 개발일지

> **kimseokryu**의 개발 블로그

---

## 목차

1. [프로젝트 개요](#프로젝트-개요)
2. [디렉토리 구조](#디렉토리-구조)
3. [블로그 표시 항목](#블로그-표시-항목)
4. [설치 요구사항](#설치-요구사항)
5. [로컬 개발 환경 구성](#로컬-개발-환경-구성)
6. [포스트 작성 방법](#포스트-작성-방법)
7. [배포](#배포)

---

## 프로젝트 개요

| 항목               | 내용                                                                           |
| ------------------ | ------------------------------------------------------------------------------ |
| 테마               | [jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) v7.3.1 |
| 정적 사이트 생성기 | Jekyll 4.4.1                                                                   |
| 호스팅             | GitHub Pages                                                                   |
| 언어 설정          | 한국어 (`ko-KR`)                                                               |
| 시간대             | `Asia/Seoul`                                                                   |
| 배포 방식          | `main` 브랜치 push 시 GitHub Actions 자동 빌드·배포                            |

---

## 디렉토리 구조

```
minseok5408.github.io/
│
├── _config.yml              # 사이트 전역 설정 (제목, 언어, 소셜 링크 등)
├── jekyll-theme-chirpy.gemspec  # 테마 gem 명세 (의존 라이브러리 버전 정의)
├── Gemfile                  # Ruby gem 의존성 선언
├── Gemfile.lock             # gem 버전 잠금 파일
├── package.json             # Node.js 빌드 도구 의존성
│
├── _posts/                  # ★ 블로그 포스트 마크다운 파일 저장 위치
│   └── YYYY-MM-DD-title.md  # 파일명 형식: 날짜-제목.md
│
├── _drafts/                 # 미발행 초안 (빌드 시 제외, --drafts 옵션으로 미리보기 가능)
│
├── _tabs/                   # 사이드바 탭 페이지 (about, archives, categories, tags)
│   ├── about.md             # 소개 페이지
│   ├── archives.md          # 전체 포스트 시간순 목록
│   ├── categories.md        # 카테고리별 포스트 목록
│   └── tags.md              # 태그별 포스트 목록
│
├── _data/                   # 사이트 전역 데이터 파일
│   ├── authors.yml          # 포스트 작성자 정보
│   ├── contact.yml          # 소셜 연락처 아이콘 설정
│   ├── share.yml            # 포스트 공유 버튼 설정
│   ├── media.yml            # 미디어 소스 설정
│   └── locales/             # 36개 언어 UI 번역 파일 (ko-KR.yml 포함)
│
├── _layouts/                # 페이지 레이아웃 템플릿 (HTML + Liquid)
│   ├── default.html         # 최상위 기본 레이아웃
│   ├── home.html            # 홈 페이지 레이아웃
│   ├── post.html            # 포스트 상세 페이지 레이아웃
│   ├── page.html            # 일반 페이지 레이아웃
│   ├── archives.html        # 아카이브 페이지
│   ├── categories.html      # 전체 카테고리 목록 레이아웃
│   ├── category.html        # 개별 카테고리 페이지 레이아웃
│   ├── tags.html            # 전체 태그 목록 레이아웃
│   └── tag.html             # 개별 태그 페이지 레이아웃
│
├── _includes/               # 재사용 가능한 HTML 부분 템플릿 (레이아웃에서 include)
│   ├── sidebar.html         # 사이드바 (프로필, 내비게이션)
│   ├── topbar.html          # 상단바 (검색, 테마 토글)
│   ├── footer.html          # 하단 저작권 정보
│   ├── head.html            # HTML <head> 메타데이터
│   ├── toc.html             # 목차(Table of Contents)
│   ├── post-sharing.html    # 포스트 공유 버튼
│   ├── related-posts.html   # 관련 포스트 추천
│   ├── comments/            # 댓글 시스템 (Disqus, Utterances, Giscus)
│   └── analytics/           # 웹 분석 코드 (Google Analytics 등)
│
├── _sass/                   # SCSS 스타일시트 소스
│   ├── abstracts/           # 변수, 믹스인, 브레이크포인트
│   ├── base/                # 기본 리셋, 타이포그래피, 코드 하이라이팅
│   ├── components/          # 버튼, 팝업 등 UI 컴포넌트
│   ├── layout/              # 사이드바, 상단바, 푸터 레이아웃
│   ├── pages/               # 홈, 포스트, 아카이브, 검색 페이지별 스타일
│   ├── themes/              # 라이트/다크 테마 색상 정의
│   └── vendors/             # Bootstrap 5 통합
│
├── _javascript/             # JavaScript 소스 (ES6 모듈)
│   ├── theme.js             # 라이트/다크 테마 전환
│   ├── post.js              # 포스트 페이지 기능 (TOC 스크롤 등)
│   ├── home.js              # 홈 페이지 기능
│   ├── commons.js           # 공통 유틸리티
│   ├── categories.js        # 카테고리 페이지
│   ├── misc.js              # 기타 기능
│   ├── modules/             # 재사용 가능한 JS 모듈
│   └── pwa/                 # PWA(Progressive Web App) 서비스 워커
│
├── assets/                  # 정적 자산 파일
│   ├── img/
│   │   ├── avatar.jpeg      # 사이드바 프로필 사진
│   │   └── favicons/        # 파비콘 (브라우저 탭 아이콘)
│   ├── css/
│   │   └── jekyll-theme-chirpy.scss  # SCSS 진입점 (빌드 후 .css 생성)
│   └── js/
│       └── dist/            # 번들링·압축된 JS 파일 (빌드 산출물)
│
├── _plugins/                # Jekyll 커스텀 플러그인
│   └── posts-lastmod-hook.rb  # 포스트 최종 수정일 자동 주입
│
├── _site/                   # ★ Jekyll 빌드 산출물 (자동 생성, 커밋 불필요)
│
├── .github/
│   └── workflows/
│       └── jekyll.yml       # GitHub Actions: main 브랜치 push 시 자동 빌드·배포
│
├── tools/                   # 빌드·릴리즈 스크립트 (개발용)
└── docs/                    # 테마 문서 (CHANGELOG, CONTRIBUTING 등)
```

---

## 블로그 표시 항목

### 사이드바 (항상 표시)

- 프로필 사진 (`assets/img/avatar.jpeg`)
- 사이트 제목 및 부제목 (`_config.yml` → `title`, `tagline`)
- 내비게이션 탭: 홈, 아카이브, 카테고리, 태그, 소개
- 소셜 링크 아이콘 (`_data/contact.yml`)
- 라이트/다크 테마 전환 버튼

### 홈 페이지

- 최신 포스트 목록 (페이지당 10개, `_config.yml` → `paginate`)
- 포스트 제목, 날짜, 카테고리, 태그, 미리보기 텍스트

### 포스트 상세 페이지

- 제목, 날짜, 작성자, 카테고리, 태그
- 예상 읽기 시간
- 목차(TOC) — 오른쪽 사이드에 고정
- 본문 (Markdown → HTML, 코드 하이라이팅 포함)
- 포스트 공유 버튼
- 이전/다음 포스트 네비게이션
- 관련 포스트 추천

### 아카이브 (`/archives/`)

- 전체 포스트를 연도·월 기준으로 시간순 정렬

### 카테고리 (`/categories/`)

- 카테고리별 포스트 수 및 목록

### 태그 (`/tags/`)

- 태그 클라우드 및 태그별 포스트 목록

### 소개 (`/about/`)

- `_tabs/about.md` 파일 내용

---

## 설치 요구사항

### 필수 런타임

| 도구        | 버전                            | 설치 방법                                                           |
| ----------- | ------------------------------- | ------------------------------------------------------------------- |
| **Ruby**    | `~> 3.1` (3.1.x 이상, 4.x 미만) | [RubyInstaller](https://rubyinstaller.org/) (Windows) / rbenv / rvm |
| **Bundler** | `2.3.27` 이상                   | `gem install bundler`                                               |
| **Jekyll**  | `4.4.1` (Gemfile.lock 고정)     | Bundler가 자동 설치                                                 |
| **Node.js** | `18.x` 이상                     | [nodejs.org](https://nodejs.org/)                                   |
| **npm**     | `9.x` 이상                      | Node.js에 포함                                                      |

### 주요 Ruby Gem 버전 (Gemfile.lock 기준)

| Gem                  | 버전                 |
| -------------------- | -------------------- |
| jekyll               | 4.4.1                |
| jekyll-paginate      | 1.1.0                |
| jekyll-seo-tag       | 2.8.0                |
| jekyll-archives      | 2.3.0                |
| jekyll-sitemap       | 1.4.0                |
| jekyll-include-cache | 0.2.1                |
| kramdown             | 2.5.1                |
| rouge                | 4.6.0                |
| sass-embedded        | 1.90.0               |
| html-proofer         | 5.0.10 (테스트 전용) |

### 주요 npm 패키지 버전 (package.json 기준)

| 패키지         | 버전     | 용도                   |
| -------------- | -------- | ---------------------- |
| bootstrap      | ^5.3.6   | UI 프레임워크          |
| @popperjs/core | ^2.11.8  | 드롭다운/툴팁 포지셔닝 |
| rollup         | ^4.41.0  | JS 번들러              |
| @babel/core    | ^7.27.x  | JS 트랜스파일러        |
| eslint         | ^9.27.0  | JS 린터                |
| stylelint      | ^16.19.1 | SCSS 린터              |
| purgecss       | ^7.0.2   | CSS 최적화             |

---

## 로컬 개발 환경 구성

### 1단계: 저장소 클론

```bash
git clone https://github.com/minseok5408/minseok5408.github.io.git
cd minseok5408.github.io
```

### 2단계: Ruby Gem 설치

```bash
bundle install
```

### 3단계: Node.js 패키지 설치

```bash
npm install
```

### 4단계: 로컬 서버 실행

```bash
bundle exec jekyll serve
```

브라우저에서 `http://localhost:4000` 접속

> **초안 포함 미리보기:**
>
> ```bash
> bundle exec jekyll serve --drafts
> ```

> **라이브 리로드 (파일 변경 시 자동 새로고침):**
>
> ```bash
> bundle exec jekyll serve --livereload
> ```

---

## 포스트 작성 방법

`_posts/` 디렉토리에 아래 형식으로 파일을 생성합니다.

**파일명 형식:** `YYYY-MM-DD-제목.md`

**예시:** `_posts/2026-04-02-my-first-post.md`

```markdown
---
title: 포스트 제목
date: 2026-04-02 09:00:00 +0900
categories: [상위카테고리, 하위카테고리]
tags: [태그1, 태그2, 태그3]
author: kimseokryu
toc: true # 목차 표시 여부 (기본값: true)
comments: true # 댓글 표시 여부 (기본값: true)
image:
  path: /assets/img/posts/썸네일이미지.png
  alt: 이미지 설명
---

포스트 본문을 여기에 마크다운으로 작성합니다.
```

---

## 배포

`main` 브랜치에 push하면 `.github/workflows/jekyll.yml` GitHub Actions 워크플로우가 자동으로 실행됩니다.

```
push to main
  → GitHub Actions 트리거
  → bundle exec jekyll build (Ubuntu 환경, Ruby 3.1)
  → _site/ 빌드 산출물 → GitHub Pages 배포
```

**직접 배포가 필요한 경우 (로컬 빌드):**

```bash
JEKYLL_ENV=production bundle exec jekyll build
```

산출물은 `_site/` 디렉토리에 생성됩니다. 이 디렉토리는 git에 커밋하지 않습니다.
