---
name: browse
description: >
  Browser automation CLI with autonomous agent mode. Use when: navigating
  websites, filling forms, taking screenshots, extracting web content, or
  completing multi-step browser tasks. Triggers: "browse", "open website",
  "fill form", "take screenshot of page", "browser task", "go to URL",
  "web automation", or any task requiring browser interaction.
---

# Browse CLI

## Commands

```bash
browse open <url>               # Navigate
browse state                    # Show numbered clickable elements
browse click <index>            # Click element by index
browse input <index> <text>     # Clear + type into specific element
browse type <text>              # Type into focused element
browse screenshot [file]        # Take screenshot
browse extract                  # Extract page text
browse task "<goal>"            # Autonomous: AI completes the goal
browse close                    # Close
```

## Workflow

1. `browse open <url>` to navigate
2. `browse state` to see numbered elements
3. `browse click <n>` or `browse input <n> <text>` to interact
4. `browse screenshot` to verify
5. Repeat as needed

## Autonomous Mode

For complex multi-step tasks, use `browse task`:

```bash
browse task "Search for flights from NYC to London on Google Flights"
browse task "Fill out the signup form with test data" --max-steps 15
```

The AI agent handles navigation, clicking, typing, and scrolling autonomously.

## Connection

Connects to Chrome via CDP (default port 9222) or launches headless Chromium.
Set `BROWSE_CDP_URL` to change the CDP endpoint.
