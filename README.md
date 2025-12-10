# Type 200 ⚡

Type 200 is a modern typing speed test built with Nuxt 3.
Unlike static typing games, Type 200 dynamically fetches its content—real code snippets,
historical facts, and philosophical quotes—via server-side API routes,
ensuring a fresh challenge every time you play.

Tagline: Fetch data. Race the render.

## 🚀 The Concept

The name "Type 200" is a nod to the HTTP Status Code 200 OK.

The "200" (Backend): Nuxt Server Routes act as a proxy to fetch, sanitize,
and prepare text from external public APIs.

The "Type" (Frontend): A high-performance typing engine powered by Vue Composables
handles real-time validation.

## 🛠 Tech Stack

- Framework: Nuxt 3 (SSR + Server Routes)
- Styling: CSS / SCSS
- Icons: Nuxt Icon
- State Management: useState / Composables

## AI Usage

AI is used in the project to generate designs, ideas, and some basic code-reviews. The code is
manually handwritten (albeit some autocompletion with AI may still happen)

## Design

[Link](https://gemini.google.com/share/504d32011056)

**Typography:**

- Primary Font (UI & Headers)
  - Family: System Sans-Serif (`Inter`, `-apple-system`, `BlinkMacSystemFont`, `Segoe UI`, `Roboto`, `sans-serif`)
  - Usage: Navigation, Modals, Results, Buttons.
- Code Font (The Game Area)
  - Family: Monospace (`JetBrains Mono`, `Fira Code`, `Consolas`, `Monaco`, `monospace`)
  - Usage: The typing text, live stats numbers.
  - Critical: Must use `font-variant-ligatures: none;` to prevent confusion during typing.

**Color Palette:**

Backgrounds

- `#1a1b26` (Deep Blue-Black - Main background)
- `#24283b` (Lighter Blue-Black - Cards/Modals)
- `#414868` (Selection highlight)

Typography

- `#c0caf5` (Pale Blue-White - Main text)
- `#565f89` (Muted Blue-Grey - Untyped text / Labels)
- `#7aa2f7` (Bright Blue - Links / Focus)

Feedback / State

- `#9ece6a` (Soft Green - Correct character)
- `#f7768e` (Soft Red - Incorrect character)
- `#e0af68` (Orange/Yellow - The caret)

## 🎮 Game Mechanics & Behavior

**The Core Loop**

1. Selection: The user arrives and selects a "Track" (Mode).

2. The Handshake: The browser requests the snippet via Nuxt (/api/snippet?mode=...). 
   The server fetches, cleans, and returns the text. 
3. The Race:
  - The text renders in a specific "monospaced" area. 
  - The timer starts automatically upon the first keystroke. 
  - The user must type the text exactly as displayed. 
  - Visual Feedback: Correct characters turn Green; Incorrect characters turn Red.
  The cursor moves forward only on correct input (or we can allow backspacing—game design choice).
4. The Result: Upon completing the final character, the game ends immediately. A modal appears displaying:
   - WPM (Words Per Minute): (Total Characters / 5) / Time in Minutes 
   - Accuracy: (Correct Keystrokes / Total Keystrokes) * 100

**Specific Behaviors**

- Restart Strategy: A "Quick Restart" button (or Tab key) immediately re-fetches a new snippet
  from the same mode without reloading the page.
- Error Handling: If the user mistypes a character, the cursor should visually "stumble" or
  block progress until corrected (Hard Mode) or allow typing but mark it red (Speed Mode).
  We will default to "Stop on Error" (Hard Mode) to encourage accuracy. 
- Sanitization: The backend must strip \n (newlines) from Quotes to make them a continuous stream,
  but preserve formatting for Code snippets if possible (or flatten them for easier typing).

## 📡 External Data Sources (APIs)

The application relies on Nuxt Server Routes to proxy requests to these public endpoints. No API keys are stored on the client.

**Mode 1: "The Philosopher" (Quotes)**
- Source: ZenQuotes API 
- Endpoint: https://zenquotes.io/api/random
- Data Structure: Returns a JSON array. We extract q (quote text) and a (author). 
- Behavior: Fetch one random quote. If the quote is too short (< 50 chars), fetch again.

**Mode 2: "The Historian" (Wikipedia)**
- Source: Wikipedia API
- Endpoint: https://en.wikipedia.org/w/api.php
- Query: ?action=query&format=json&list=random&rnnamespace=0&rnlimit=1 (Get random article title), then fetch the summary.
- Behavior: Get a random article summary. Strip all HTML tags (<p>, <b>) before sending to the frontend.