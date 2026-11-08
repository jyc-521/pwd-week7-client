# Ajou Campus Foodmap

아주대학교 주변 맛집을 한눈에 찾아볼 수 있는 웹 서비스의 프론트엔드입니다.

맛집 목록을 카테고리별로 살펴보고, 맛집의 상세 정보와 평점이 높은 인기 맛집을 확인할 수 있습니다. 새로운 맛집을 제보하는 기능도 있습니다. 맛집 데이터는 Express API 서버(pwd-week7)에서 받아 오며, 서버는 Supabase의 PostgreSQL 데이터베이스에 맛집을 저장합니다.

실전웹서비스개발 7주차 과제로 진행했습니다.

## 주요 화면

| 경로 | 화면 | 설명 |
| --- | --- | --- |
| / | 홈 | 서비스 소개와 각 화면으로 이동하는 메뉴 |
| /list | 맛집 목록 | 전체 맛집 목록과 카테고리 필터 |
| /restaurant/:id | 맛집 상세 | 위치, 가격대, 평점, 소개, 추천 메뉴 |
| /popular | 인기 맛집 | 평점이 높은 순서의 맛집 목록 |
| /submit | 맛집 제보 | 새로운 맛집 정보 제보 |

## 동작 구조

맛집 목록, 상세, 인기 맛집은 서버의 API를 호출해서 가져옵니다. 서버는 Supabase 데이터베이스에서 데이터를 읽어 응답합니다.

```text
React 클라이언트 (Vercel) → Express API 서버 (Render) → Supabase PostgreSQL
```

서버 API 호출은 `src/services/api.ts`에서 처리합니다. 맛집 제보와 좋아요는 서버를 거치지 않고 브라우저의 LocalStorage에 저장하며, 이 기능은 `src/services/api.local.ts`에 있습니다. 현재 서버는 맛집 수정과 삭제 API를 제공하지 않으므로 클라이언트에서도 해당 기능을 사용하지 않습니다.

## 배포 주소

| 구분 | 주소 |
| --- | --- |
| 클라이언트 GitHub | https://github.com/jyc-521/pwd-week7-client |
| 서버 GitHub | https://github.com/jyc-521/pwd-week7 |
| Vercel 서비스 | 배포 후 입력 |
| Render API | 배포 후 입력 |

## 사용 기술

React 19와 TypeScript로 작성했고, Vite로 개발 서버를 실행하고 빌드합니다. 화면 이동은 React Router, 서버 데이터 요청은 axios와 TanStack Query를 사용합니다. 스타일은 Emotion으로 작성했습니다. 배포는 Vercel을 사용합니다.

## 프로젝트 구조

```text
src/
├── App.tsx
├── main.tsx
├── pages/
│   ├── HomePage.tsx
│   ├── ListPage.tsx
│   ├── DetailPage.tsx
│   ├── PopularPage.tsx
│   └── SubmitPage.tsx
├── components/
├── services/
│   ├── api.ts
│   ├── api.local.ts
│   ├── storage.ts
│   └── validation.ts
├── styles/
└── types/
```

## 실행 방법

서버 주소는 환경변수 `VITE_API_BASE_URL`로 설정합니다. 로컬에서는 `.env.example`을 복사해 `.env.local` 파일을 만들고, 로컬 서버 주소를 입력합니다.

```bash
cp .env.example .env.local
npm ci --include=dev
npm run dev
```

```dotenv
VITE_API_BASE_URL=http://localhost:3000
```

서버 주소에는 `/api/restaurants`를 붙이지 않고 기본 주소만 입력합니다. 로컬에서 확인할 때는 pwd-week7 서버를 먼저 실행해야 합니다.

그 밖의 명령은 다음과 같습니다.

| 명령 | 설명 |
| --- | --- |
| npm run typecheck | 타입 검사 |
| npm run lint | 코드 검사 |
| npm run build | 배포용 빌드 |

## 배포

Vercel 프로젝트에 이 저장소를 연결해서 배포합니다. 빌드 명령은 `npm run build`, 결과 폴더는 `dist`이며 Node.js 버전은 24.x를 사용합니다.

Vercel의 Environment Variables에 `VITE_API_BASE_URL`을 Render 서버 주소로 등록합니다. 이 값은 빌드할 때 코드에 들어가므로, 값을 바꾼 뒤에는 Redeploy를 해야 반영됩니다.
