# Colab Industry Section Telegram Bot

Colab can run this bot while the notebook runtime is alive. It is useful for testing or temporary monitoring, but it is not a reliable always-on scheduler because Colab can disconnect idle or long-running sessions.

For a more reliable 1-hour schedule, use Windows Task Scheduler, GitHub Actions, a small VPS, or another always-on machine.

## 1. Install Playwright in Colab

Run this in the first Colab cell:

```python
!pip -q install playwright
!python -m playwright install --with-deps chromium
```

## 2. Upload the bot file

Upload `industry_section_telegram_bot.py` to Colab, or mount Google Drive and copy it into the current working directory.

Simple upload cell:

```python
from google.colab import files
files.upload()  # choose industry_section_telegram_bot.py
```

## 3. Set Telegram secrets

Run this cell and paste your values when prompted:

```python
import getpass
import os

os.environ["TELEGRAM_BOT_TOKEN"] = getpass.getpass("Telegram bot token: ")
os.environ["TELEGRAM_CHAT_ID"] = getpass.getpass("Telegram chat id: ")

os.environ["RUN_MODE"] = "once"
os.environ["INDUSTRY_ARTICLE_LIMIT"] = "5"
os.environ["INDUSTRY_VIEWPORT_WIDTH"] = "1440"
os.environ["INDUSTRY_VIEWPORT_HEIGHT"] = "1800"
os.environ["INDUSTRY_FULL_PAGE_SCREENSHOT"] = "false"
os.environ["INDUSTRY_HEADLESS"] = "true"
```

## 4. Run one test

```python
!python industry_section_telegram_bot.py --once
```

## 5. Run hourly while Colab stays alive

```python
!python industry_section_telegram_bot.py --loop
```

Stop the cell when you want to end monitoring. If Colab disconnects, run the setup cells again.

## Notes

- Screenshots are saved to `industry_section_captures/` in the Colab runtime.
- Change detection state is stored in `industry_section_state.json`. Colab runtimes are ephemeral, so this file is lost when the session ends; every fresh session will treat the first run as "changed". Mount Google Drive and keep the file there if you want change detection to persist.
- When a section is unchanged, only the top-5 text list is sent and the screenshot is skipped.
