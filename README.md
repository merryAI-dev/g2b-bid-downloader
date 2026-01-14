# 나라장터 입찰공고 검색 스킬 (G2B Bid Downloader)

Claude Code용 나라장터(g2b.go.kr) 입찰공고 자동 검색, 파일 다운로드, SWOT 분석 스킬입니다.

## 주요 기능

- **입찰공고 검색**: 나라장터 크롤링을 통한 실시간 검색 (기본 90일)
- **파일 자동 다운로드**: Playwright 기반 k00 토큰 캡처 + Raonkupload 다운로드
- **HWP 파싱**: 나라장터 특수 HWP 포맷 지원 (olefile + zlib)
- **SWOT 분석**: Claude 기반 입찰 적합도 자동 분석
- **필요서류 추출**: HWP에서 제출서류 자동 추출 + Opus 검증
- **슬랙 친화적 출력**: 이모지 없는 깔끔한 포맷

## 설치

### 1. 사전 요구사항

- Python 3.8 이상
- Node.js 18 이상
- Claude Code CLI

### 2. Claude Code 설치

```bash
npm install -g @anthropic-ai/claude-code
```

### 3. 스킬 설치

```bash
# 스킬 폴더로 이동
cd ~/.claude/skills

# 이 레포지토리 클론
git clone https://github.com/YOUR_USERNAME/g2b-bid-downloader.git

# Python 의존성 설치
cd g2b-bid-downloader
pip install -r requirements.txt

# Playwright 브라우저 설치
playwright install chromium
```

## 사용법

### Claude Code에서 실행

```bash
claude
```

대화창에서:

```
/g2b-bid-downloader 액셀러레이팅
```

또는 자연어로:

```
나라장터에서 창업지원 입찰 찾아줘
```

### 직접 스크립트 실행

**검색만 (목록 보기)**

```bash
python scripts/g2b_file_downloader.py "액셀러레이팅" --list-only
```

**파일 다운로드**

```bash
python scripts/g2b_file_downloader.py "액셀러레이팅" --indices 1,2,3
```

## 검색 키워드 예시

| 키워드 | 설명 |
|--------|------|
| 액셀러레이팅 | 액셀러레이터 관련 사업 |
| 창업지원 | 창업 지원 프로그램 |
| 스타트업 | 스타트업 관련 용역 |
| 컨설팅 | 경영/기술 컨설팅 |
| 인공지능 | AI 관련 사업 |

## 워크플로우

```
1. 관심 분야 선택
   ↓
2. 나라장터 검색 (크롤링)
   ↓
3. 공고 목록 제시 (테이블)
   ↓
4. 파일 다운로드 (Playwright)
   ↓
5. HWP 파싱 (olefile + zlib)
   ↓
6. 필요서류 추출 (Claude Opus 검증)
   ↓
7. SWOT 분석 (회사 역량 기반)
   ↓
8. 슬랙용 포맷 출력
```

## 출력 예시

```
나라장터 입찰공고 상세 (2026.01.14 기준)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. [기보벤처캠프 액셀러레이팅]

[기본 정보]
• 사업명: (긴급) 2026년 기보벤처캠프 액셀러레이팅 위탁운영 용역
• 발주기관: 기술보증기금 벤처혁신금융부
• 추정가격: 6.8억원
• 마감: 2026.1.8.(목) 18:00 (D-2)

[SWOT 분석]
• S 강점: TIPS+등록AC 자격 충족 / AC 운영경험 풍부
• W 약점: 부산 발주처(물리적 거리) / 기보 네트워크 부족
• O 기회: 최대 규모(6.8억) / 연간 반복사업
• T 위협: D-2 마감 / 기존 수행사 경쟁

적합도: 5/5
```

## 기술 스택

- **크롤링**: requests + 나라장터 POST API
- **파일 다운로드**: Playwright (Chromium) + Raonkupload k00 토큰
- **HWP 파싱**: olefile + zlib (나라장터 특수 포맷 지원)
- **검증/분석**: Claude Opus 4 (Anthropic API)

### HWP 파싱 주의사항

나라장터 HWP 파일은 `hwp5txt` 라이브러리와 호환되지 않습니다 (ColumnSet AssertionError 발생).
본 스킬은 `olefile + zlib` 방식으로 텍스트를 추출하며, 자동으로 폴백 처리됩니다.

## 문제 해결

### Q. "command not found: claude"

```bash
npm install -g @anthropic-ai/claude-code
```

### Q. "playwright chromium not found"

```bash
playwright install chromium
```

### Q. 검색 결과가 없음

나라장터 서버 상태 확인 후 재시도하거나, 검색 기간을 늘려보세요 (기본 90일).

### Q. HWP 파싱 실패

`hwp5txt` 사용 시 에러가 정상입니다. `olefile` 방식으로 자동 폴백됩니다.

## 라이선스

MIT License

## 기여

이슈 및 PR 환영합니다!

## 제작

MYSC (메이커스 소셜 컴퍼니)
