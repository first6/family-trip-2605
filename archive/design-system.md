# 다낭 가족여행 일정 사이트 — 디자인 시스템 룰셋

> 이 문서는 `itinerary.html`의 디자인 컨셉과 데이터 스키마를 정의합니다.
> 일정이 바뀌어도 데이터(`ITINERARY`, `PLACES`, `TRAVEL_TIMES`)만 수정하면 디자인이 자동 적용됩니다.

---

## 1. 디자인 철학

| 원칙 | 의미 |
|---|---|
| **여유로운 페이스, 밀도 있는 정보** | 한 줄에 한 일정. 가족여행이 빡빡하지 않게 보이되, 정보는 빠짐없이 |
| **장소 중심 동기화** | 일정표의 핀 번호 = 지도 핀 번호 = 동선 시작점. 세 곳이 항상 일치 |
| **시간 흐름의 시각화** | 좌측 LNB(불변 정보) → 탭(시간 단위) → 타임라인(시간 순) → 지도(공간) 4단 구조 |
| **모바일·반쪽화면 친화** | 240/220/0 px 3단계 LNB, 800px 미만에선 LNB 자동 접힘 |
| **색은 의미를 가진다** | 5일 = 5색. 일자별 일관성 유지. 임의 색상 추가 ❌ |

---

## 2. 컬러 시스템

### 2-1. 의미 기반 토큰 (CSS variables)

```css
--bg            #fafaf7   페이지 배경
--panel         #ffffff   카드/패널 배경
--ink           #1c1c1e   본문 텍스트
--ink-mute      #6b6b70   보조 텍스트
--ink-soft      #9a9a9f   라벨·캡션
--line          #e8e6e0   경계선
--line-soft     #f1efe9   분할선 (얕은)
--accent        #d4644a   브랜드 (활성·강조)
```

**룰**:
- `--accent` 는 활성 상태 표시에만 (active row의 좌측 보더, "전체" tab hover 등)
- `--ink` 활성 탭 배경, `--panel` 비활성 탭 배경
- 그라이션은 `linear-gradient(to right, var(--day-tint), transparent 60%)` 만 사용 (day header)

### 2-2. 일자별 색상 (DAY_COLORS 배열)

```js
const DAY_COLORS = [
  { hex: "#e8a87c", tint: "#fbe9d7" },  // Day 1 — 코랄
  { hex: "#65a3c8", tint: "#dde9f1" },  // Day 2 — 블루
  { hex: "#6fb583", tint: "#dfece4" },  // Day 3 — 그린
  { hex: "#a86fb5", tint: "#ebe1ee" },  // Day 4 — 퍼플
  { hex: "#e8a233", tint: "#f9e7c8" },  // Day 5 — 오렌지
  { hex: "#5fb1ad", tint: "#dceae9" },  // Day 6 — 틸 (예비)
  { hex: "#c87a7a", tint: "#f0d9d9" }   // Day 7 — 로즈 (예비)
];
```

**룰**:
- 채도 낮은 어스톤 팔레트 (베트남·해변·여유 분위기)
- 배열 인덱스 = `ITINERARY[i]`의 day index. 데이터에 명시 안 하면 자동 할당
- 일정이 7박을 넘으면 배열을 순환 (modulo)
- 새 색을 추가할 때는 같은 명도·채도 톤 유지 (HSL: L≈70, S≈40 부근)

### 2-3. 태그 컬러

태그는 7가지로만 한정. 새 카테고리 추가 시 색상도 같은 톤으로 추가.

| Tag | 라벨 | 배경 | 텍스트 |
|---|---|---|---|
| `flight` | 항공 | `#e8eef9` | `#3a5da3` |
| `hotel` | 숙소 | `#efe9f6` | `#6f4ea3` |
| `meal` | 식사 | `#fbeee6` | `#c66e3e` |
| `activity` | 액티 | `#e6f3ec` | `#4a8c6a` |
| `transport` | 이동 | `#f1ecdf` | `#897045` |
| `spa` | 스파 | `#fae3eb` | `#b04d72` |
| `event` | 이벤트 | `#fdebda` | `#b87530` |

---

## 3. 타이포그래피

```
font-family : -apple-system, BlinkMacSystemFont, "SF Pro Text",
              "Pretendard", "Apple SD Gothic Neo", "Noto Sans KR"

LNB title    16px / 700 / -0.01em
탭          12px / 600
일정 제목    13px / 600
일정 시간    11px / 500 (tabular-nums)
일정 설명    11.5px / 400 / mute
태그·번호    9.5–10.5px / 700 / uppercase
```

**룰**:
- 절대 폰트 크기 13px 이상은 본문 제목에만 (위계 보호)
- `tabular-nums`는 시간/숫자에만 (`.ti-time`)
- 한글·영문 혼용 시 `letter-spacing: -0.005em`

---

## 4. 컴포넌트 룰

### 4-1. LNB (Left Navigation Bar)
- 너비 240px (1100px+) → 220px (~1099px) → 0 (~800px)
- 섹션은 `<div class="lnb-section">`로 감싸고 `--line-soft`로 구분
- 헤더 `.lnb-h`: UPPERCASE + 0.06em letter-spacing + 10px
- 토글 ☰ 버튼은 탭바 좌측에 고정

### 4-2. 탭 (Tabs)
- "전체"가 항상 첫 번째
- 각 일자 탭에 `.tab-dot` (7px 원형, 일자 색)
- 활성 탭 = 다크 배경 (`--ink`) + 흰 텍스트
- 가로 스크롤 허용, 스크롤바 숨김

### 4-3. 타임라인 행 (`.ti-row`)
**고정 그리드**: `50px 22px 1fr auto`
- `시간` `아이콘` `제목·설명` `태그+장소`
- 한 줄로 끝낸다 (`text-overflow: ellipsis`)
- 좁은 폭에서 `.ti-desc` 자동 숨김 (≤ 1099px)
- 더 좁으면 `.ti-tag` 도 숨김 (≤ 600px)

**룰**:
- 일정 제목은 13자 이내 권장 (한 줄 유지)
- 설명은 30자 이내 권장 (truncate 방지)
- 활성 행은 좌측 2px 일자 색 보더 + 따뜻한 톤 배경

### 4-4. 핀 번호 배지 (`.ti-pin-num`)
- 17×17px 라운드 4px
- 배경은 그 일정의 `day.colorHex`
- 흰 텍스트, 9.5px / 700
- **장소 칩(`.ti-place`) 좌측에 인라인 배치** — 핀 번호와 장소명이 한 묶음

### 4-5. 지도 마커 (`.marker-pin`)
- 28×38 핀 모양, 첫 방문 일자 색
- 라벨 = 핀 번호 (placeOrderMap 기반, 1-indexed)
- 영구 툴팁(`.marker-name-tip`)으로 장소 단축명 표시
- 같은 장소 여러 번 방문 시 **첫 등장 순서 번호 1개**만 표시

### 4-6. 이동시간 칩 (`.travel-time-pill`)
- 노선 폴리라인 중간점에 배치
- `🚐 30분` 형태, 9.5px / 600 / `--ink-mute`
- `interactive: false` (클릭 무시, 마커와 충돌 없음)
- 데이터 없는 구간은 표시 안 함 (강제 표시 ❌)

### 4-7. 폴리라인
- 스타일 고정: `weight: 3, opacity: 0.6, dashArray: '6, 6'`
- 색상은 그 일정의 `day.colorHex`
- 같은 장소 연속 방문 시 합쳐 한 점으로 처리

---

## 5. 간격 시스템

```
xs   4px    chip 내부 패딩
sm   6–8px  요소 간 일반 간격
md   10–14px 행 패딩, 칩 묶음
lg   16–22px 섹션 패딩
xl   28px+   섹션 간 여백 (보통 미사용)
```

LNB 섹션 padding: `14px 16px`
타임라인 행 padding: `8px 14px`
탭바 padding: `10px 14px`

---

## 6. 데이터 스키마

### 6-1. `PLACES`
```js
KEY_UPPER: {
  name: "정식 명칭",            // 팝업 제목·LNB
  short: "지도라벨",            // 8자 이내 권장
  coords: [위도, 경도],
  detail: "한 줄 설명 (30~60자)"
}
```

### 6-2. `ITINERARY`
```js
{
  day: "5/20",
  weekday: "수",
  colorHex: "#...",   // 생략 가능 (자동 할당)
  tint: "#...",       // 생략 가능 (자동 할당)
  title: "그날의 컨셉 한 줄",
  items: [
    {
      time: "HH:mm",
      icon: "이모지 1개",
      title: "13자 이내",
      place: "PLACES_KEY",   // 또는 생략
      tag: "flight|hotel|meal|activity|transport|spa|event",
      desc: "30자 이내 (선택)"
    }
  ]
}
```

### 6-3. `TRAVEL_TIMES`
```js
"FROM_KEY-TO_KEY": "🚐 30분"
```
양방향 자동 매칭 (A→B 정의하면 B→A 자동 인식). 다음 구간만 표시:
- 의미 있는 이동 (호텔→관광지, 시내→교외 등)
- 동일 단지 내 이동(미카즈키 풀↔레스토랑 같은) 정의 ❌

---

## 7. 일정 추가/변경 워크플로우

### 새 장소 추가
1. `PLACES`에 `KEY: { name, short, coords, detail }` 추가
2. 필요한 이동시간을 `TRAVEL_TIMES`에 추가
3. `ITINERARY`의 항목에서 `place: "KEY"` 참조

### 새 일정 항목 추가
1. 해당 day의 `items` 배열에 객체 추가 (스키마 준수)
2. 시간 순서대로 정렬
3. 새 장소면 위 워크플로우도 함께

### 새 일자 추가
1. `ITINERARY` 배열에 새 day 객체 추가
2. `colorHex`/`tint` 생략하면 `DAY_COLORS` 배열에서 자동 할당
3. 6박 이상이면 `DAY_COLORS` 배열에 색 추가 (같은 톤 유지)

### 검증 (자동)
페이지 로드 시 `validateData()`가 다음을 검사 — 콘솔 경고:
- 누락된 `time`/`title`
- 존재하지 않는 `place` 참조
- 알 수 없는 `tag`
- 정의되지 않은 인접 이동시간

---

## 8. 절대 룰 (Don'ts)

- ❌ HTML에 인라인 스타일로 색상 직접 지정 (CSS 변수 또는 `day.colorHex` 사용)
- ❌ 한 일정 행을 여러 줄로 쓰기 (`white-space: nowrap` 유지)
- ❌ 새로운 색상 팔레트 추가 (DAY_COLORS / 태그 컬러로 한정)
- ❌ 핀 번호와 장소를 분리 (항상 묶음)
- ❌ 좌표 없는 장소를 PLACES에 등록 (지도와 동기화 깨짐)
- ❌ 임의 폰트·임의 폰트 크기 추가 (시스템 폰트 + 정의된 크기만)

---

## 9. 파일 구조

```
family-trip/
├── itinerary.html              # 단일 페이지 (데이터 + 디자인 + 로직)
├── design-system.md            # 이 문서
├── 01-research-master.md       # 리서치 마스터
└── 02-banahills-deep-research.md  # 바나힐 (제외) 보존
```

데이터를 수정할 때는 `itinerary.html`의 상단 3개 객체 (`PLACES`, `ITINERARY`, `TRAVEL_TIMES`) 만 건드리면 됩니다. 나머지는 자동.
