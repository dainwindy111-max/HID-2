# 자라는 정리 — Vercel 배포 가이드

쓰려는 걸 적으면 AI가 답하고, 정리하면 핵심만 추려지며, 줄을 눌러 더 파고들거나 이어서 물으며 계속 뻗어가는 프로토타입.

## 구성
- `index.html` — 화면 전체 (정적)
- `api/ask.js` — Claude에게 답을 받아오는 서버리스 함수 (API 키를 숨겨줌)
- `package.json` — ESM 설정

브라우저에서 Anthropic API를 직접 부르면 키가 노출되고 막히기 때문에, 답변은 항상 `api/ask.js`를 거칩니다.

## 배포 순서

### 1) Anthropic API 키 발급
https://console.anthropic.com 에서 API 키를 만듭니다. (사용한 만큼 과금됩니다.)

### 2) GitHub에 올리기
이 폴더 안의 항목(`index.html`, `package.json`, `README.md`, 그리고 `api` 폴더)을 저장소 맨 위에 올립니다.
- 중요: `ask.js`는 반드시 `api` 폴더 안에 있어야 합니다(`api/ask.js`).
- 웹에서 폴더 드래그가 안 되면, `Add file → Create new file`로 이름 칸에 `api/ask.js`를 입력하고 `api/ask.js`의 내용을 붙여넣으면 폴더가 자동으로 만들어집니다.

### 3) Vercel에 연결
1. vercel.com 에 GitHub 계정으로 로그인.
2. `Add New → Project` → 이 저장소 `Import`.
3. 빌드 설정은 그대로 두고, `Environment Variables`에 추가:
   - Name: `ANTHROPIC_API_KEY`
   - Value: 발급받은 키
4. `Deploy`.

환경 변수를 빠뜨리고 먼저 배포했다면, Settings에서 추가한 뒤 Deployments 탭에서 Redeploy 하면 적용됩니다.

## 참고
- 모델은 `api/ask.js`의 `model: "claude-sonnet-4-6"`에서 바꿀 수 있습니다.
- 키는 서버에만 있고 브라우저로 노출되지 않습니다.
