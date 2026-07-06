# 🌈 Wonder World

A single-page, browser-based learning arcade for kids. From one home screen,
pick from nine bite-sized mini-games and earn stars as you play:

- 🔢 **Math Pop** — quick arithmetic questions
- 🧠 **Memory Match** — classic pairs/matching card game
- 🔺 **Patterns** — spot what comes next in a sequence
- 🔍 **Odd One Out** — pick the item that doesn't belong
- 🇫🇷 **French Fun** — learn French colours, numbers, and animals, with audio
- 🎹 **Music Studio** — a simple playable virtual keyboard
- 🧪 **Science Quiz** — kid-friendly science questions
- 🎨 **Art Pad** — free-draw canvas
- ♻️ **Maker Lab** — a creative/making activity

Stars earned across games are tracked in the top bar.

## How to open it

Wonder World is a single self-contained HTML file with no build step and no
dependencies — everything (markup, styles, and game logic) lives in
`index.html`.

1. Open `index.html` directly in any modern browser (double-click it, or
   drag it into a browser window), **or**
2. Serve it locally for a nicer experience (some audio features work better
   over `http://` than `file://`):
   ```bash
   npx serve .
   # or
   python -m http.server 8000
   ```
   then visit the printed URL (e.g. `http://localhost:8000`).

## How to play

1. From the home screen, tap any tile to jump into that game.
2. Use the **← Home** button on each screen to return and try another game.
3. Correct answers and completed activities earn ⭐ stars, shown in the top
   bar — the goal is simply to explore and collect as many as you can.

No account, install, or internet connection is required (French Fun's audio
uses the browser's built-in speech synthesis, so a connection is only
needed if your browser downloads voices on first use).
