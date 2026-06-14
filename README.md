# sns_nai

RisuAI 플러그인 — SNS/메신저 롤플레이 시뮬레이터 + NAI 이미지 생성. **Asset Mommy**와 연동됩니다.

## 설치 (RisuAI)

1. RisuAI → 설정 → 플러그인 → 플러그인 가져오기
2. URL: `https://raw.githubusercontent.com/aredsea/sns-nai/main/sns_nai.js`
3. import 후 새로고침

자동 업데이트는 내장 업데이터가 6시간마다 위 URL을 확인합니다 (notify/off/auto, 기본 notify). `@sr-changelog`로 변경사항이 토스트에 표시됩니다.

## 버전 규칙

`MAJOR.MINOR.PATCH` — 매 릴리스마다 PATCH를 올리고, PATCH가 10이 되면 MINOR로 올림(PATCH는 0으로).
예) `0.8.8 → 0.8.9 → 0.9.0`. Asset Mommy와 동일한 규칙.

## 주요 기능

- SNS(트위터/인스타그램)·메신저(X DM)·데이트 모드 롤플레이 시뮬레이션
- 캐릭터 SNS 계정·프로필 자동 생성, NAI 이미지 생성(프로필/배경/장면)
- 로어북 분석 → 캐릭터 프로필·관계 그래프 구축
- 갤러리 앱 (생성 이미지 모아보기·정리)

## 아이폰 LLM 연동 (0.8.8)

LLM Provider를 **`vertex`**로 선택하면 플러그인이 직접 Vertex를 호출하지 않고 **RisuAI 본체 LLM 파이프라인**(`runLLMModel`)으로 라우팅합니다. 아이폰(iOS)에서 플러그인 직접 호출이 실패하는 문제를 우회합니다.

- 별도 service account JSON 입력 **불필요**
- 본체 메인 모델 → `main` 티어, 본체 보조 모델 → `light`/`verylight` 티어로 매핑
- 사용법: RisuAI 본체에서 메인·보조 모델을 작동 확인된 Vertex 모델로 설정 → 플러그인 LLM 설정에서 provider만 `vertex` 선택 → 연결 테스트
- 트레이드오프: 플러그인의 temperature/max tokens는 vertex 티어에서 무시되고 본체 설정을 따름. 안전차단 자동 재시도(jailbreak prefill)는 이 경로에서 우회됨. 멀티모달은 텍스트로 강제.
- **Auto-Update Mode를 `off`로** 두면 패치가 보존됩니다.

## Asset Mommy 연동

```
Asset Mommy: 외견 추출 → lb-xnai.lb.extra (### 다온비 (Da Onbi))
        ↓
sns_nai: 이미지 태그 사전 스캔 → resolve("다온비") 매칭 → profile.imageTags 갱신
        ↓
sns_nai: SNS 계정 프로필/배경 → NAI 이미지 생성
```

이름 매칭 개선: 헤딩의 괄호 안 이름(영문)과 base 이름(한글)을 모두 분리해 양방향 토큰으로 매칭 — 한글/영문 이중표기 캐릭터가 정상 매칭됩니다.

## 버전 이력

- `0.9.2`: ★NAI 이미지 생성 핵심 수정★ NAI 호출 전송 계층을 `nativeFetch`→`risuFetch`(=globalFetch)로 교체. 본체 "기타 봇>이미지 생성"이 NAI를 정상 호출하는 경로가 바로 globalFetch이며 CORS 우회·프록시 지원으로 아이폰(iOS)에서도 동작. nativeFetch는 iOS RisuAI WebView에서 바이너리 전송이 깨져 이미지 생성이 실패했음.
- `0.9.1`: NAI 엔드포인트 URL 직접 입력칸 추가 (본체 NAIImgUrl을 못 읽는 문제 우회)
- `0.9.0`: NAI 에러 표면화 (401/402/429 힌트)
- `0.8.8`: 아이폰 LLM 연동 (Vertex → 본체 파이프라인)
- `0.8.7`: GitHub 자동 버전관리 + lb-xnai 매칭 개선 + Asset Mommy 연동
