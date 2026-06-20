# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Wonder World is a self-contained children's educational web app — a single `index.html` file with no build system, no package manager, and no external JavaScript dependencies. Google Fonts is the only external resource (loaded via CDN `<link>`).

To run: open `index.html` directly in a browser. There are no commands to build, lint, or test.

## Architecture

The entire app lives in `index.html` with three sections:

1. **Inline CSS** (minified, ~100 lines) — uses CSS custom properties on `:root` for the color palette (`--coral`, `--sky`, `--sun`, `--mint`, `--purple`, `--ink`, etc.)
2. **HTML** — nine `<div class="screen">` panels that are shown/hidden by toggling `.active`
3. **Inline JavaScript** — module-style sections, each prefixed with a comment like `/* ══ 1. MATH POP ══ */`

### Screen routing

`show(id)` strips `.active` from all `.screen` elements and adds it to the target. There is no router — navigation is purely CSS class toggling.

### Shared quiz engine

The global `Q` object and `startQuiz(questions, opts)` are shared by Math Pop, Patterns, Odd One Out, Science Quiz, and French Quiz. Each game builds its own `questions` array of this shape:

```js
{
  big: '🐱🐱🐱',     // large display above question
  text: 'How many?', // question label
  answer: '3',       // must match one entry in opts exactly
  opts: ['3','4','2'], // will be shuffled before display
  speak: 'trois',    // optional: spoken aloud via Web Speech API on load
  lang: 'fr-FR',     // optional: lang for speak
  rate: 0.7          // optional: speech rate
}
```

`finishQuiz()` shows the result overlay; `opts.onDone` is called when the player hits "Again".

### Touch/click handling

All interactive elements use `addTap(el, fn)` instead of raw event listeners. This prevents double-firing on mobile by tracking a `t` flag with a 500 ms timeout.

```js
function addTap(el, fn) { /* touchstart (preventDefault) + click guard */ }
```

Never attach `addEventListener('click', ...)` directly on tappable elements.

### Audio

- **Speech**: `say(text, rate, pitch, lang)` wraps `SpeechSynthesisUtterance`. Silently no-ops if `speechSynthesis` is unavailable.
- **Music**: The piano uses Web Audio API (`AudioContext`, triangle oscillator). `ensureAudio()` must be called from a user gesture to resume a suspended context. The `AC` global holds the shared `AudioContext`.

### State globals

| Variable | Owns |
|----------|------|
| `Q` | Current quiz state (list, index, correct count) |
| `M` | Memory Match game state (open cards, moves, pairs) |
| `E` | Echo/Simon game state (sequence, position, level length) |
| `ART` | Art Pad state (canvas context, color, size, drawing flag) |
| `FR` | French vocabulary data (col/num/ani arrays) |
| `frTab` | Currently active French tab (`'col'`, `'num'`, `'ani'`) |
| `mkIdx` | Current Maker Lab card index |
| `stars` | Global star count displayed in topbar |

### Adding a new activity

1. Add a tile to the `#sHome` grid.
2. Add a new `<div class="screen" id="sXxx">` panel.
3. Wire navigation with `addTap(document.getElementById('g-xxx'), function() { show('sXxx'); ... })` and a back button.
4. To use the shared quiz engine, build a questions array and call `startQuiz(qs, { onDone: startXxx })`.

### Confetti

`confetti()` renders a canvas animation on `#cfx` (fixed, full-screen, pointer-events none). It auto-clears after 120 frames (~2 s).
