# Industry Section Telegram Bot

This bot opens selected industry news sections in Chromium, sends each page screenshot to Telegram, then sends the extracted article titles and links.

## Sections

- Yonhap heavy chemistry: `https://www.yna.co.kr/industry/heavy-chemistry`
- The Guru industry: `https://www.theguru.co.kr/news/section.html?sec_no=108`
- Maeil Business chemical: `https://www.mk.co.kr/news/business/chemical`
- Business Post industry: `https://www.businesspost.co.kr/BP?command=sub&sub=2`
- Hankyung ship marine: `https://www.hankyung.com/industry/ship-marine`
- Hankyung construction machinery: `https://www.hankyung.com/industry/build-machinery`

## Local Run

```powershell
cd .\industry_section_telegram_bot
pip install -r .\requirements.txt
python -m playwright install chromium
python .\industry_section_telegram_bot.py --once
```

For Windows Task Scheduler, run `run_industry_section_bot.ps1` every hour. The wrapper runs one check and exits.

## GitHub Actions

The cloud schedule is defined at the repository root:

```text
.github/workflows/industry-section-bot.yml
```

Add these GitHub repository secrets:

- `TELEGRAM_BOT_TOKEN`
- `TELEGRAM_CHAT_ID`

Then run `Industry Section Bot` manually once from the Actions tab, or wait for the hourly schedule.

## Colab

See `COLAB_INDUSTRY_BOT.md`.

## Options

Set these as environment variables if needed:

- `INDUSTRY_ARTICLE_LIMIT=15`
- `INDUSTRY_VIEWPORT_WIDTH=1440`
- `INDUSTRY_VIEWPORT_HEIGHT=1800`
- `INDUSTRY_FULL_PAGE_SCREENSHOT=false`
- `INDUSTRY_HEADLESS=true`
