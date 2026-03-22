# browse

Browser automation CLI with autonomous agent, multi-tab, cookies, and CAPTCHA solving. One Python script, connects to existing Chrome via CDP or launches headless browser.

```
$ browse task "Go to google.com and search for weather in Berlin"

mode: cdp
task: Go to google.com and search for weather in Berlin
max_steps: 30

[step 1] thinking... input 5 weather in Berlin
[step 2] thinking... click 9
[step 3] thinking... done The current temperature in Berlin is 6 C.

Task complete: The current temperature in Berlin is 6 C.
```

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/buildingopen/browse/main/browse -o ~/.local/bin/browse
chmod +x ~/.local/bin/browse
```

Requires: Python 3.8+, `playwright` (`pip install playwright`)

## Commands

| Command | Description |
|---------|-------------|
| `browse open <url>` | Navigate to URL |
| `browse state` | Show numbered clickable elements |
| `browse click <n>` | Click element by index |
| `browse input <n> <text>` | Clear field and type |
| `browse select <n> <value>` | Select dropdown option |
| `browse type <text>` | Type into focused element |
| `browse screenshot [file]` | Take screenshot |
| `browse extract` | Extract page text |
| `browse tabs` | List all open tabs |
| `browse tab <n>` | Switch to tab |
| `browse newtab <url>` | Open URL in new tab |
| `browse closetab [n]` | Close tab |
| `browse cookies save [file]` | Save cookies to file |
| `browse cookies load [file]` | Restore cookies |
| `browse solve` | Solve CAPTCHA via 2captcha |
| `browse task "<goal>"` | Autonomous agent mode |
| `browse close` | Close browser |

## Autonomous Agent

Give it a goal, it figures out what to click:

```bash
browse task "Search Google for flights from NYC to London"
browse task "Fill out the signup form with name John and email john@test.com"
browse task "Go to GitHub trending and list the top 3 repos" --max-steps 10
```

The agent uses **vision + DOM**: sends both a screenshot and the element list to Gemini, so it understands visual layout, images, and dynamic content. Includes error recovery (retries on failures, stops after 3 consecutive errors).

## Multi-Tab

```bash
browse newtab https://example.com    # Open in new tab
browse tabs                          # List tabs with indices
browse tab 2                         # Switch to tab
browse closetab 0                    # Close tab
```

## Cookie Persistence

```bash
browse cookies save                  # Save to default file
browse cookies save ~/my-cookies.json
browse cookies load                  # Restore cookies
```

## CAPTCHA Solving

```bash
export TWOCAPTCHA_KEY=your_key
browse solve                         # Solve reCAPTCHA/hCaptcha on current page
```

Detects reCAPTCHA and hCaptcha via `data-sitekey`, submits to 2captcha, polls for solution, injects the token.

## Connection

Tries CDP first (`BROWSE_CDP_URL`, default `http://localhost:9222`), falls back to headless browser. On a server with Chrome running, it connects automatically.

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `BROWSE_CDP_URL` | `http://localhost:9222` | CDP endpoint |
| `GEMINI_API_KEY` | - | For autonomous agent |
| `GOOGLE_API_KEY` | - | Alternative to GEMINI_API_KEY |
| `TWOCAPTCHA_KEY` | - | For CAPTCHA solving |

## Why

[browser-use](https://github.com/browser-use/browser-use) (82k stars) has the same concept but its BrowserSession watchdog layer [fails on headless Linux](https://github.com/browser-use/browser-use/issues/4471). browse uses Playwright directly.

## License

MIT
