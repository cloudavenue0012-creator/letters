# 편지 페이지 제작 가이드 (A to Z)

개인 편지를 웹 페이지로 만들어 QR 책갈피 카드로 전달하는 전체 플로우.

---

## 개요

- 편지 내용을 HTML로 만들어 GitHub Pages로 배포
- 수신자별 고유 URL 생성
- QR코드가 인쇄된 책갈피 카드로 전달

---

## 1. 레포지토리 전략

### 원칙
- **수신자 1명 전용** 편지 → 단독 레포 (예: `letter-wayne`)
- **여러 명** 편지를 확장할 경우 → 통합 레포 (예: `letters`)
- 한 번 배포된 URL은 바꾸지 않는다. 새 레포를 열어 확장한다.

### 파일 구조
```
letters/
├── index.html          # 목록 페이지 (편지 링크 모음)
├── toSD.html           # 수신자별 편지 (이름 약자 또는 별칭)
├── toJonny.html
├── bookmarks.html      # 인쇄용 QR 책갈피 카드
└── HOW_TO_MAKE_LETTER.md
```

### 파일 네이밍
- `to{이름약자 또는 영문별칭}.html`
- 예: `toSD.html` (임승대), `toJonny.html` (김은웅), `toWayne.html`

---

## 2. GitHub 레포 생성

1. GitHub → **New repository**
2. Repository name: `letters` (또는 원하는 이름)
3. **Public** 설정 (GitHub Pages 사용 조건)
4. Create repository

---

## 3. 로컬 셋업

```powershell
mkdir "경로\letters"
cd "경로\letters"
git init
git remote add origin https://github.com/{유저명}/letters.git
```

---

## 4. 편지 HTML 제작

### 디자인 베이스
- 배경: 크래프트지 느낌 (`#e8dfd0 → #d9cdb8` 그라디언트)
- 편지지: 미색 (`#faf6ec`)
- 폰트: Nanum Myeongjo (본문), Gaegu (서명), Gowun Batang (날짜)
- 커버(봉투) → 터치하면 열리는 애니메이션

### 편지 구성 요소
| 요소 | 내용 |
|------|------|
| cover | 수신자 이름, FROM, "화면을 터치해주세요" |
| ornament | 상단 장식 (SVG 꽃 문양 + 라인) |
| salutation | 수신자 호칭 (예: "임승대 대표님께,") |
| paragraph | 본문 (단락별 fadeUp 애니메이션) |
| closing | 서명 ("— 윤태원 드림") |
| date | 날짜 (예: "2026 . 05 . 29") |
| footer-ornament | 하단 점 장식 |

### 애니메이션 딜레이 구조
```
ornament     0.3s
salutation   0.6s
paragraph 1  0.9s
paragraph 2  1.4s
paragraph 3  1.9s
paragraph 4  2.4s
closing      2.9s
date         3.3s
footer       3.5s
```
단락이 늘어나면 0.5s씩 추가, closing 이후도 맞춰 밀기.

---

## 5. 편지 문체 가이드

### 원칙
- 담백하게. 수식어 없이.
- 사실의 나열 → 자연스럽게 감사로 수렴
- 오글거리는 표현, 과한 비유 금지
- 마지막은 짧고 단호하게 ("잘 가십시오. 기도하겠습니다.")

### 구조 (참고)
1. 함께한 시간에 대한 한 줄
2. 기억에 남는 구체적인 장면/행동
3. 그것이 남긴 것 (배운 것, 느낀 것)
4. 마무리 인사 + 연락 약속

---

## 6. index.html (목록 페이지)

- 편지가 2개 이상이면 목록 페이지 필요
- 카드 형태로 수신자 나열, 클릭 시 각 편지로 이동
- 동일한 디자인 시스템 사용 (같은 배경, 폰트)

---

## 7. GitHub Pages 배포

1. 레포 → **Settings → Pages**
2. Source: `Deploy from a branch`
3. Branch: `main` / `/ (root)` → **Save**
4. 배포 완료 후 URL:
   ```
   https://{유저명}.github.io/{레포명}/
   https://{유저명}.github.io/{레포명}/toSD.html
   ```

> 배포까지 1~2분 소요될 수 있음.

---

## 8. QR 책갈피 카드 (bookmarks.html)

### 카드 스펙
- 크기: **54 × 148mm** (책갈피 비율)
- 흑백 출력 기준
- QR 라이브러리: `qrcodejs` (CDN)

### QR 코드 생성 코드
```javascript
new QRCode(document.getElementById('qr-id'), {
  text: 'https://{유저명}.github.io/{레포명}/{파일명}.html',
  width: 121,
  height: 121,
  colorDark: '#1a1a1a',
  colorLight: '#ffffff',
  correctLevel: QRCode.CorrectLevel.M
});
```

### 카드 구성
- FROM. 윤태원
- TO. + 수신자 이름
- QR코드 + "편지 열기" 힌트
- 날짜

### 출력 방법
1. 브라우저에서 `bookmarks.html` 열기
2. **인쇄하기** 버튼 클릭
3. 용지: A4 / 여백: 최소
4. 출력 후 가위로 재단

---

## 9. 수정 이력 및 시행착오

| 항목 | 초기값 | 최종값 | 이유 |
|------|--------|--------|------|
| 레포 구조 | 단일 레포 확장 검토 | 기존 레포 유지 + 신규 `letters` 레포 분리 | 기존 URL 보존 |
| 파일명 | imd.html, keu.html 검토 | toSD.html, toJonny.html | 본인 요청 |
| 날짜 | 2026.05.28 | 2026.05.29 | 전달일 기준으로 수정 |
| 편지 문체 | 시적·은유적 서술 | 담백한 사실 나열 | "오글거린다" 피드백 |
| 편지 구성 | 단순 나열 | 장면 중심 + 짧은 마무리 | 본인 어조·톤 반영 |

---

## 10. 전체 플로우 요약

```
1. 편지 내용 정리 (구어체로 핵심만)
      ↓
2. GitHub 레포 생성 (Public)
      ↓
3. 로컬 폴더 생성 + git init + remote 연결
      ↓
4. toXX.html 작성 (기존 파일 복사 후 내용·수신자 교체)
      ↓
5. index.html 작성 (편지 2개 이상일 때)
      ↓
6. bookmarks.html 작성 (QR + 책갈피 카드)
      ↓
7. git add → commit → push
      ↓
8. GitHub Pages 활성화 (Settings → Pages)
      ↓
9. URL 확인 후 bookmarks.html 출력
      ↓
10. 카드 재단 → 전달
```

---

## 현재 배포 현황

| 수신자 | 파일 | URL |
|--------|------|-----|
| Wayne 이사님 | index.html | https://cloudavenue0012-creator.github.io/letter-wayne/ |
| 임승대 대표님 | toSD.html | https://cloudavenue0012-creator.github.io/letters/toSD.html |
| 김은웅 실장님 | toJonny.html | https://cloudavenue0012-creator.github.io/letters/toJonny.html |
