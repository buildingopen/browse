# browse

Browser automation CLI with autonomous agent mode. One Python script, connects to existing Chrome via CDP or launches headless Chromium.

```
$ browse task "Go to hacker news and tell me the top story"

mode: cdp
task: Go to hacker news and tell me the top story
max_steps: 30

[step 1] thinking... navigate https://news.ycombinator.com
[step 2] thinking... done The top story is "PC Gamer recommends RSS readers"

Task complete: The top story is "PC Gamer recommends RSS readers"
```

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/buildingopen/browse/main/browse -o ~/.local/bin/browse
chmod +x ~/.local/bin/browse
```

Requires: Python 3.8+, `playwright` (`pip install playwright && playwright install chromium`)

For autonomous mode: set `GEMINI_API_KEY` or `GOOGLE_API_KEY`

## Commands

```bash
browse open <url>               # Navigate to URL
browse state                    # Show clickable elements with indices
browse click <index>            # Click element by index
browse input <index> <text>     # Clear field and type into element
browse type <text>              # Type into focused element
browse screenshot [file]        # Take screenshot
browse extract                  # Extract page text
browse task "<goal>"            # Autonomous: AI completes the task
browse close                    # Close browser
```

## Autonomous Agent

Give it a goal in plain English:

```bash
browse task "Search Google for 'best coffee shops in Berlin'"
browse task "Fill out the contact form on example.com with name John and email john@test.com"
browse task "Go to GitHub trending and list the top 3 repos" --max-steps 10
```

The agent:
1. Takes a screenshot + reads page elements
2. Sends to Gemini: "here's the page, here's the goal, what action next?"
3. Executes the action (click, type, navigate, scroll)
4. Repeats until done or max steps reached

## How it connects

1. Tries CDP connection (`BROWSE_CDP_URL`, default `http://localhost:9222`)
2. Falls back to launching headless Chromium

On a server with Chrome already running (e.g., via `google-chrome --remote-debugging-port=9222`), it connects automatically. No new browser launched.

## Environment variables

| Variable | Default | Description |
|----------|---------|-------------|
| `BROWSE_CDP_URL` | `http://localhost:9222` | CDP endpoint for existing Chrome |
| `GEMINI_API_KEY` | - | Required for `task` command |
| `GOOGLE_API_KEY` | - | Alternative to GEMINI_API_KEY |

## Why

[browser-use](https://github.com/browser-use/browser-use) (82k stars) has the same concept but its `BrowserSession` watchdog layer [fails on headless Linux](https://github.com/browser-use/browser-use/issues/4471) (timeout on launch, WebSocket reconnection loop on CDP). This script uses Playwright directly, skipping the broken layer.

## License

MIT
