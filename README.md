# Industry Section Telegram Bot (Claude 개조판)

산업 뉴스 섹션을 돌면서 텔레그램으로 전송한다. 원본(88.codex)과 달리 **변경 감지**가 붙어 있다.

## 동작

각 섹션마다:

1. 상위 5개 기사(제목/링크)를 추출한다. (`INDUSTRY_ARTICLE_LIMIT`, 기본 5)
2. **상위 2개** 기사 URL로 지문(fingerprint)을 만들어 직전 실행값과 비교한다. (`INDUSTRY_DETECT_TOP_N`, 기본 2 — 표시는 5개지만 변경 판단은 상단 2개만 본다.) 상태는 `industry_section_state.json`에 섹션별로 저장된다.
3. **바뀌었으면** → 페이지 스크린샷 + 상위 5개 리스트 전송.
4. **안 바뀌었으면** → 스크린샷 **생략**, `🔁 새로운 뉴스 없음 (상위 5개 동일)` 표시 + 상위 5개 리스트만 텍스트로 전송.
5. 기사 추출이 0건이면(파싱 실패 가능성) 안전하게 스크린샷을 전송한다.

> 첫 실행은 저장된 상태가 없으므로 모든 섹션이 "변경"으로 처리되어 스크린샷이 전송된다.

## 섹션

- Yonhap heavy chemistry: `https://www.yna.co.kr/industry/heavy-chemistry`
- The Guru industry: `https://www.theguru.co.kr/news/section.html?sec_no=108`
- Maeil Business chemical: `https://www.mk.co.kr/news/business/chemical`
- EBN industry: `https://www.ebn.co.kr/news/articleList.html?sc_section_code=S1N5&view_type=sm`
- Chosun Biz shipbuilding: `https://biz.chosun.com/tag/shipbuilding/`
- Hankyung ship marine: `https://www.hankyung.com/industry/ship-marine`
- Hankyung construction machinery: `https://www.hankyung.com/industry/build-machinery`

## 로컬 실행

```powershell
cd .\industry_section_telegram_bot
pip install -r .\requirements.txt
python -m playwright install chromium
python .\industry_section_telegram_bot.py --once
```

Windows 작업 스케줄러에서는 `run_industry_section_bot.ps1`을 매시간 돌리면 된다(1회 실행 후 종료).

## 상태 파일 (중요)

변경 감지는 `industry_section_state.json`이 실행 사이에 **유지되어야** 동작한다.

- 로컬 / 작업 스케줄러 / VPS: 파일이 그대로 남으므로 문제 없음.
- **GitHub Actions**: 매 실행이 새 러너라 이 파일이 사라진다 → 매번 "변경"으로 잡힌다. actions cache나 아티팩트/커밋으로 `industry_section_state.json`을 유지해야 스크린샷 생략이 동작한다.

## 옵션 (환경변수)

- `INDUSTRY_ARTICLE_LIMIT=5` — 표시할 상위 기사 수
- `INDUSTRY_DETECT_TOP_N=2` — 변경 판단에 쓰는 상단 기사 수 (표시 개수와 별개)
- `INDUSTRY_MAIN_COLUMN_RATIO=0.66` — 본문 열 판정 폭 비율. 오른쪽 사이드바(많이 본 뉴스/베스트 클릭)를 제외하기 위해 `left < 화면폭 × 이 값`인 링크만 기사로 취급
- `INDUSTRY_VIEWPORT_WIDTH=1440`
- `INDUSTRY_VIEWPORT_HEIGHT=1800`
- `INDUSTRY_FULL_PAGE_SCREENSHOT=false`
- `INDUSTRY_HEADLESS=true`
- `TELEGRAM_NOTIFY_ERRORS=false`

## Colab

`COLAB_INDUSTRY_BOT.md` 참고.
