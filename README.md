# 🌴 Family Trip · Da Nang 2026.05

> **2026년 5월 20일(수) ~ 24일(일) · 다낭 4박 5일**
> 부부(43/41) + 딸 9세·5세 · 미카즈키(2박) → 푸라마(2박)

베트남 다낭 가족여행 계획·리서치·인터랙티브 일정 사이트.

---

## 📁 파일 안내

먼저 [`00-INDEX.md`](./00-INDEX.md)부터 읽으세요. 마스터 인덱스 + Top 10 핵심 인사이트가 정리되어 있습니다.

### 리서치 (도메인별)
| 파일 | 내용 |
|---|---|
| [`01-research-master.md`](./01-research-master.md) | 1차 리서치 (호텔·날씨·교통·기본 명소·맛집·예산) |
| [`02-banahills-deep-research.md`](./02-banahills-deep-research.md) | 바나힐 심층 리서치 (이번 여행에선 제외 결정 — 보존용) |
| [`03-vietnam-essentials.md`](./03-vietnam-essentials.md) | 베트남·다낭 기본 컨텍스트 (역사·문화·매너·안전·비자·환전·5월 특이) |
| [`04-danang-major-places.md`](./04-danang-major-places.md) | 메이저 명소 Top 20 + 호이안 심층 + 외곽 평가 + 인스타 명소 |
| [`05-danang-hidden-gems.md`](./05-danang-hidden-gems.md) | 숨은 명소·로컬 핫플·2025-2026 트렌드 |
| [`06-danang-food.md`](./06-danang-food.md) | 음식 정체성 + 시그니처 12 + 식당 + 카페 + 어린이 안전망 |
| [`07-danang-shopping-experience.md`](./07-danang-shopping-experience.md) | 쇼핑 18선 + 흥정 + 약국 + 문화체험 + 짐 체크리스트 |

### 인터랙티브 페이지
| 파일 | 내용 |
|---|---|
| [`itinerary.html`](./itinerary.html) | 단일 HTML — LNB + 날짜 탭 + 한 줄 타임라인 + Leaflet 지도 + 핀번호·이동시간 |
| [`design-system.md`](./design-system.md) | 일정 사이트 디자인 시스템 룰셋 (색상·타이포·컴포넌트·데이터 스키마) |

---

## 🎯 핵심 결정 (현 시점)

- ✅ **호이안**: 5/23(토) 저녁 야경 본격
- ✅ **용다리 불쇼**: 5/23(토) 21:00
- ✅ **마사지 2회**: 5/22 트리스 스파 + 5/24 노아 스파
- ✅ **푸라마 레이트 체크아웃**: 5/24 시도
- ⛔ **바나힐**: 패스 (5세 무리)

---

## 🛠️ 페이지 사용법

```bash
# 로컬에서 일정 페이지 열기
open itinerary.html
```

또는 GitHub Pages 활성화 후 `https://first6.github.io/family-trip-2605/itinerary.html`로 접속.

---

## 📝 작업 흐름

리서치는 sub-agent 병렬 dispatch로 수행. 일정 데이터는 `itinerary.html` 상단의 `PLACES` / `ITINERARY` / `TRAVEL_TIMES` 3개 객체만 수정하면 디자인은 자동 적용 (`design-system.md` 참조).
