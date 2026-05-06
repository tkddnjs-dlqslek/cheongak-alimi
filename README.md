# 신청빼고 다 해주는 청약 알리미

> 한국 청약 공고 발견·자격 판정·일정 관리·알림까지 — **신청 버튼 누르는 것만 빼고 모두 자동화**합니다.
>
> Claude Code · OpenAI Codex 양쪽에서 동일하게 작동하는 Agent Skill.

---

## ✨ 사용자가 할 수 있는 모든 것

### 📢 공고 발견·필터링
- **8개 채널 통합 조회** — APT 일반 / 오피스텔·도시형 / LH 공공분양 / APT 잔여세대 / 공공지원민간임대 / 임의공급 / **SH(서울)** / **GH(경기)**
- **지역·구/군 필터** — 17개 광역 + 자치구 단위 (강남구·송파구 등)
- **세대수·시공사 필터** — 대단지(500세대+)·1군 브랜드(삼성·현대·GS·대우 등)
- **조회 기간** — 최근 1~12개월 조절
- **인접 지역 자동 확장** — 0건일 때 광주 → 전남·전북 같은 매핑 17개 제안

### 📊 자격·가점 판정 (결정론 계산, LLM 환각 X)
- **가점 84점 만점 정확 계산** — 무주택(32) + 부양가족(35) + 통장(17)
- **미성년 통장 한도 자동 차감** — 2024.7.1. 시행 5년 / 그 이전 2년
- **1순위 자격 판정** — 지역별 납입횟수 (투기과열 24회 / 수도권 12회 / 기타 6회)
- **특별공급 5종 자격** — 신혼부부·생애최초·다자녀·노부모부양·청년
- **공고-프로필 적합도 매칭** — high(3/3) / medium(2/3) / low 3단계
- **가점대별 현실 전략** — 0~20점, 20~40점, 40~60점, 60~75점, 75점+ 차등 안내
- **1주택자 갈아타기 경로** — 추첨제·잔여세대·임의공급 안내

### 📄 공고 해석·매칭
- **모집공고 원문 자동 추출** — 청약홈·LH·SH·GH 4개 사이트 본문 파싱
- **섹션 자동 분리** — 자격 / 공급일정 / 공급금액 / 유의사항 / 공급대상
- **LLM 기반 자연어 해석** — 자격 요건·일정·가격을 사용자 프로필 맥락에 맞춰 요약

### 🎲 경쟁률 (3단 폴백)
1. **청약홈 실제 결과** — 발표 끝난 공고
2. **지역 12개월 평균** — 같은 지역 실제 데이터 (`?history=true`)
3. **통계 추정치** — 폴백 (서울 투기과열 소형 160:1, 경기 일반 소형 28:1 등)

### 📅 일정 관리
- **D-day 자동 계산·색상 분기** — 🔴 D-1 이하 / 🟡 D-2~3 / 🟢 D-4+
- **캘린더 ICS 다운로드** — 구글/아이폰/아웃룩 캘린더 즉시 임포트
- **4종 리마인더** — D-3 임박 / D-1 초긴급 / 당첨발표 임박 / 계약 임박

### 🔔 알림 발송
- **Slack** — Block Kit 풀 포맷 (헤더·divider·section·버튼)
- **Telegram** — HTML parse_mode + 링크 미리보기 차단
- **양 채널 동시** — 한쪽 실패해도 다른쪽 정상 전달
- **서버측 7일 자동 dedup** — 같은 공고 중복 발송 방지
- **Webhook/Token 자동 감지·저장** — 채팅창에 그냥 붙여넣기만

### 🆕 공고 변동 추적
- **신규 공고 등장** — "어제 대비 새로 뜬 거 있어?"
- **변경 감지** — 분양가·일정·세대수 등 12개 필드 자동 추적
- **삭제·마감** — 사라진 공고 알림
- **30일 이력 보관** — 언제든 조회

### 👤 프로필·즐겨찾기
- **12개 항목 대화형 setup** — 출생연도·지역·가구·통장·소득·평형 등
- **프로필 부분 업데이트** — "혼인신고일만 수정" 자연어 인식
- **프로필 갱신 알림** — 90일·365일 경과 시 자동
- **즐겨찾기 공고** — 추가/제거/목록 + 변동 체크

### 🔄 자동 스케줄 (4가지 옵션)
| 옵션 | 비고 |
|---|---|
| 즉시 1회 | 그 시점에만 |
| `/loop 24h` | 세션 열어둔 동안 |
| **GitHub Actions** ⭐ | PC 꺼도 작동, 가장 안정적 |
| 로컬 cron / Task Scheduler | PC 켜둔 동안 |

자연어 시간 입력(`"저녁 7시"`, `"주말만"`) → cron 자동 변환.

---

## 🚀 설치 (1분)

### Claude Code
```bash
mkdir -p ~/.claude/skills && \
git clone https://github.com/tkddnjs-dlqslek/cheongak-alimi.git \
  ~/.claude/skills/korea-apt-alert
```

### OpenAI Codex CLI
```bash
mkdir -p ~/.agents/skills && \
git clone https://github.com/tkddnjs-dlqslek/cheongak-alimi.git \
  ~/.agents/skills/korea-apt-alert
```

### Windows PowerShell
```powershell
$dst = "$env:USERPROFILE\.claude\skills\korea-apt-alert"
New-Item -ItemType Directory -Force -Path (Split-Path $dst) | Out-Null
git clone https://github.com/tkddnjs-dlqslek/cheongak-alimi.git $dst
```

> **중요:** 폴더명을 반드시 `korea-apt-alert`로 유지 (SKILL.md frontmatter의 `name` 일치).

런타임 재시작 후:
```
/korea-apt-alert 청약이 뭐야?
```
→ 초보 가이드 응답이 나오면 설치 성공.

---

## 💬 사용 예시

```
/korea-apt-alert                          # 전체 공고 간결 조회
/korea-apt-alert setup                    # 12개 항목 프로필 설정
/korea-apt-alert 내 조건에 맞는 청약       # 프로필 매칭 + Top 3
/korea-apt-alert 서울 강남구 대단지만      # 지역+자치구+세대수 필터
/korea-apt-alert 내 가점 몇 점이야?        # 가점 + 가점대별 전략
/korea-apt-alert 1순위 돼?                # 1순위 자격 판정
/korea-apt-alert 이 공고 자격 분석해줘     # 모집공고 원문 LLM 해석
/korea-apt-alert 경쟁률 어때?             # 3단 폴백 조회
/korea-apt-alert 캘린더에 추가             # ICS 다운로드 링크
/korea-apt-alert 새로 뜬 공고              # 어제 대비 변동
/korea-apt-alert 알림 보내줘               # Slack/Telegram 발송
```

---

## 🔌 작동 방식

```
사용자 채팅
   ↓
Claude/Codex가 SKILL.md 따라서
   ↓
공용 프록시 서버 (https://k-apt-alert-proxy.onrender.com)
   ↓
공공데이터포털 API (6종) + SH·GH HTML 크롤링
   ↓
가점·1순위·매칭 결정론 계산 + LLM 자연어 해석
   ↓
사용자에게 답변
```

**개인정보는 로컬에만 저장**됩니다 (`~/.config/k-skill/*.json`). 프록시에는 지역·평형·세대수·시공사 키워드만 전달.

---

## 📦 알림 설정 (선택)

### Slack
```bash
# 1. Slack에서 Incoming Webhook URL 발급
# 2. 채팅창에 URL 그대로 붙여넣기 → Claude/Codex가 자동 저장
```

### Telegram
```bash
# 1. @BotFather에서 봇 생성 → Token 발급
# 2. 그룹 만들고 봇 초대 → Chat ID 확보
# 3. 채팅창에 Token + Chat ID 차례로 붙여넣기 → 자동 저장
```

### 매일 자동 발송 (GitHub Actions, 5분 셋업)
1. 본인 GitHub 계정에 빈 repo 1개 생성
2. `.github/workflows/notify.yml` 1개 추가 (Claude가 생성해줌)
3. Settings → Secrets → `SLACK_WEBHOOK` 등록 후 push

---

## 🚫 안 되는 것 (의도적·기술적 제약)

- **실제 청약 신청** — 청약홈 본인 인증 절차 거쳐야 해서 자동화 불가
- **실시간 경쟁률** — 청약홈이 공개 안 함 → 통계·과거이력 폴백
- **소득 자격 자동 확정** — 공고별 기준 다름 → 정성 판정만
- **납입 관리** — 은행 앱에서 직접

---

## 🔗 관련 링크

- **백엔드 코드**: [tkddnjs-dlqslek/k-apt-alert](https://github.com/tkddnjs-dlqslek/k-apt-alert)
- **운영 프록시**: https://k-apt-alert-proxy.onrender.com
- **공공데이터포털**: https://www.data.go.kr (백엔드가 사용)

---

## License

MIT
