# sns_nai

RisuAI 플러그인 — **SocialRisu**(by EVSkyFall) 파생 fork. SNS/메신저 롤플레이 시뮬레이터에 NAI 이미지 생성 연동.
**Asset Mommy**와 연동되도록 `lb-xnai.lb.extra` 이미지태그 사전 매칭을 개선했습니다.

## 설치 (RisuAI)

1. RisuAI → 설정 → 플러그인 → 플러그인 가져오기
2. URL: `https://raw.githubusercontent.com/aredsea/sns-nai/main/sns_nai.js`
3. import 후 새로고침

자동 업데이트는 내장 SrUpdater가 6시간마다 위 URL을 확인합니다 (notify/off/auto, 기본 notify). `@sr-changelog`로 변경사항이 토스트에 표시됩니다.

## SocialRisu 0.8.6 → sns_nai 0.8.7 변경

- **리네임**: display-name `SocialRisu` → `sns_nai` (내부 `//@name socialrisu` 유지 — 자동업데이트 호환). `//@update-url`을 이 GitHub raw로 변경.
- **lb-xnai 매칭 개선** (핵심): 이미지 태그 사전(`lb-xnai.lb.extra`)을 캐릭터 프로필에 매칭할 때 — 기존엔 `### Da Onbi`(영문) 헤딩이 프로필 realname `다온비`(한글)와 normalize 후에도 안 맞아 **미매칭**됐음.
  - `_srSplitAliases`: 괄호 안 이름도 별도 alias로 분리. `다온비 (Da Onbi)` → `["다온비", "Da Onbi"]`
  - `ProfileManager.resolve`: 양방향 토큰 폴백 매칭 — 쿼리/프로필 양쪽을 괄호·콤마·슬래시로 분리 후 토큰 교집합으로 매칭. `다온비`(쿼리) ↔ `다온비 (Da Onbi)`(realname) 양방향 성립.
- **Asset Mommy 연동**: Asset Mommy 1.0.25가 extra 로어북 헤딩을 `char.name` 전체(`다온비 (Da Onbi)`)로 강제 출력 → sns_nai가 한글 realname으로 매칭 성공 → 프로필 imageTags 갱신 → SNS 프로필/배경 NAI 이미지 생성 동작.

## 연동 플로우

```
Asset Mommy: 외견 추출 → lb-xnai.lb.extra (### 다온비 (Da Onbi))
        ↓
sns_nai: 이미지 태그 사전 스캔 → resolve("다온비") 매칭 ✓ → profile.imageTags 갱신
        ↓
sns_nai: SNS 계정 프로필/배경 → NAI 이미지 생성
```

## 원본 attribution

- 원저자: **EVSkyFall** (SocialRisu)
- 이 fork는 개인 사용 목적의 리네임 + 매칭/연동 패치. 원저자 정식 업데이트가 나오면 그쪽을 권장.

## 버전

- `0.8.7` (2026-06-14): 초기 fork. 리네임 + lb-xnai 매칭 개선 + Asset Mommy 연동.
